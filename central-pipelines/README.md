# 🏗️ Central Pipelines - Repositório Central de CI/CD

Repositório centralizado com workflows e actions reutilizáveis do GitHub Actions para facilitar a manutenção de pipelines CI/CD em múltiplas aplicações.

## 📋 Índice

- [Visão Geral](#visão-geral)
- [Estrutura do Repositório](#estrutura-do-repositório)
- [Workflows Reutilizáveis](#workflows-reutilizáveis)
- [Composite Actions](#composite-actions)
- [Como Usar](#como-usar)
- [Exemplos](#exemplos)
- [Configuração](#configuração)

## 🎯 Visão Geral

Este repositório centraliza toda a lógica de build e deploy, permitindo que múltiplas aplicações reutilizem os mesmos workflows e actions, facilitando:

- ✅ **Manutenção centralizada**: Atualize uma vez, aplica em todas as aplicações
- ✅ **Padronização**: Mesma lógica de pipeline para todos os projetos
- ✅ **Reutilização**: Evita duplicação de código em workflows
- ✅ **Versionamento**: Controle de versão das pipelines
- ✅ **Flexibilidade**: Permite customização por aplicação

## 📁 Estrutura do Repositório

```
central-pipelines/
├── .github/
│   ├── workflows/
│   │   ├── reusable-build.yml        # Workflow de build reutilizável
│   │   ├── reusable-deploy.yml       # Workflow de deploy reutilizável
│   │   └── reusable-pipeline.yml     # Pipeline completo reutilizável
│   └── actions/
│       ├── build/                    # Composite action de build
│       ├── deploy/                   # Composite action de deploy
│       ├── validate-environment/     # Validação de ambientes
│       └── generate-version/         # Geração de versões
└── README.md
```

## 🔄 Workflows Reutilizáveis

### 1. Reusable Build Workflow

Workflow para executar build da aplicação.

**Arquivo**: [.github/workflows/reusable-build.yml](.github/workflows/reusable-build.yml)

**Inputs**:
- `environment`: Ambiente (dev, pre, prod) - padrão: `dev`
- `node-version`: Versão do Node.js - padrão: `18`
- `build-command`: Comando de build - padrão: `npm run build`

**Outputs**:
- `artifact-name`: Nome do artifact gerado
- `version`: Versão do build

**Uso**:
```yaml
jobs:
  build:
    uses: sua-org/central-pipelines/.github/workflows/reusable-build.yml@main
    with:
      environment: 'prod'
      node-version: '18'
      build-command: 'npm run build'
```

### 2. Reusable Deploy Workflow

Workflow para deploy da aplicação.

**Arquivo**: [.github/workflows/reusable-deploy.yml](.github/workflows/reusable-deploy.yml)

**Inputs**:
- `environment` (obrigatório): Ambiente de deploy
- `artifact-name` (obrigatório): Nome do artifact
- `version` (obrigatório): Versão a deployar
- `deploy-url`: URL do ambiente

**Secrets**:
- `deploy-token`: Token para autenticação (opcional)

**Uso**:
```yaml
jobs:
  deploy:
    uses: sua-org/central-pipelines/.github/workflows/reusable-deploy.yml@main
    with:
      environment: 'prod'
      artifact-name: 'build-v1.0.1'
      version: 'v1.0.1'
      deploy-url: 'https://app.example.com'
    secrets:
      deploy-token: ${{ secrets.DEPLOY_TOKEN }}
```

### 3. Reusable Complete Pipeline

Pipeline completo com build e deploy para múltiplos ambientes.

**Arquivo**: [.github/workflows/reusable-pipeline.yml](.github/workflows/reusable-pipeline.yml)

**Inputs**:
- `environments`: Array JSON de ambientes - padrão: `["dev"]`
- `node-version`: Versão do Node.js - padrão: `18`
- `build-command`: Comando de build - padrão: `npm run build`
- `skip-tests`: Pular testes - padrão: `false`

**Uso**:
```yaml
jobs:
  pipeline:
    uses: sua-org/central-pipelines/.github/workflows/reusable-pipeline.yml@main
    with:
      environments: '["dev", "pre", "prod"]'
      node-version: '20'
      skip-tests: false
```

## 🧩 Composite Actions

### 1. Build Action

Action composta para build de aplicações.

**Uso**:
```yaml
- uses: sua-org/central-pipelines/.github/actions/build@main
  with:
    node-version: '18'
    build-command: 'npm run build'
    environment: 'prod'
```

### 2. Deploy Action

Action composta para deploy de aplicações.

**Uso**:
```yaml
- uses: sua-org/central-pipelines/.github/actions/deploy@main
  with:
    environment: 'prod'
    artifact-name: 'build-v1.0.1'
    version: 'v1.0.1'
    deploy-url: 'https://app.example.com'
```

### 3. Validate Environment Action

Valida se o ambiente é permitido.

**Uso**:
```yaml
- uses: sua-org/central-pipelines/.github/actions/validate-environment@main
  with:
    environment: 'prod'
    allowed-environments: 'dev,pre,prod'
```

### 4. Generate Version Action

Gera versão semântica.

**Uso**:
```yaml
- uses: sua-org/central-pipelines/.github/actions/generate-version@main
  with:
    prefix: 'v'
    major: '1'
    minor: '0'
```

## 🚀 Como Usar

### Passo 1: Criar o repositório central

1. Crie um repositório público ou privado (ex: `sua-org/central-pipelines`)
2. Adicione os arquivos deste repositório
3. Commit e push

### Passo 2: Configurar nas aplicações

Nas suas aplicações, crie workflows que referenciam o repositório central:

```yaml
# .github/workflows/pipeline.yml
name: Pipeline

on:
  push:
    branches: [main, develop]

jobs:
  pipeline:
    uses: sua-org/central-pipelines/.github/workflows/reusable-pipeline.yml@main
    with:
      environments: ${{ github.ref_name == 'main' && '["prod"]' || '["dev"]' }}
```

### Passo 3: Configurar permissões (repositórios privados)

Se o repositório central for privado, você precisa configurar permissões:

1. Crie um GitHub App ou Personal Access Token
2. Adicione como secret nas aplicações
3. Configure no workflow:

```yaml
jobs:
  pipeline:
    uses: sua-org/central-pipelines/.github/workflows/reusable-pipeline.yml@main
    secrets:
      token: ${{ secrets.CENTRAL_PIPELINES_TOKEN }}
```

## 📚 Exemplos

Veja os exemplos completos na pasta [exemplos-consumo/](exemplos-consumo/):

### Exemplo 1: Build e Deploy separados
[exemplos-consumo/app1/.github/workflows/pipeline.yml](exemplos-consumo/app1/.github/workflows/pipeline.yml)

Usa workflows reutilizáveis separados para build e deploy, com controle fino sobre cada etapa.

### Exemplo 2: Pipeline completo
[exemplos-consumo/app2/.github/workflows/pipeline.yml](exemplos-consumo/app2/.github/workflows/pipeline.yml)

Usa o workflow de pipeline completo para build + deploy em múltiplos ambientes automaticamente.

### Exemplo 3: Composite Actions
[exemplos-consumo/app3/.github/workflows/pipeline.yml](exemplos-consumo/app3/.github/workflows/pipeline.yml)

Usa composite actions para ter controle total sobre o workflow, mantendo a lógica centralizada.

### Exemplo 4: Com validações customizadas
[exemplos-consumo/app4/.github/workflows/pipeline.yml](exemplos-consumo/app4/.github/workflows/pipeline.yml)

Combina validações customizadas com pipeline reutilizável.

## ⚙️ Configuração

### Ambientes no GitHub

Configure os ambientes no seu repositório (Settings > Environments):

- **dev**: Desenvolvimento
- **pre**: Pré-produção/Staging
- **prod**: Produção (com proteções e aprovações)

### Secrets necessários

Configure secrets conforme necessário:
- `DEPLOY_TOKEN`: Token para autenticação no deploy
- Secrets específicos de cada ambiente

### Customização

#### Versionamento do repositório central

Use tags ou branches para versionar suas pipelines:

```yaml
# Usar versão específica
uses: sua-org/central-pipelines/.github/workflows/reusable-build.yml@v1.0.0

# Usar última versão da main
uses: sua-org/central-pipelines/.github/workflows/reusable-build.yml@main

# Usar branch específica
uses: sua-org/central-pipelines/.github/workflows/reusable-build.yml@develop
```

#### Comandos de build customizados

```yaml
with:
  build-command: 'npm run build:production'
```

#### Múltiplos ambientes

```yaml
with:
  environments: '["dev", "pre", "prod"]'
```

## 🔐 Segurança

- Use **environments** do GitHub para controlar aprovações
- Configure **branch protection rules** 
- Use **secrets** para informações sensíveis
- Versione o repositório central com **tags**
- Configure **CODEOWNERS** para revisar mudanças nas pipelines

## 📝 Manutenção

### Atualizando as pipelines

1. Faça alterações no repositório central
2. Teste em uma aplicação piloto
3. Crie uma tag de versão
4. Atualize as aplicações para usar a nova versão

### Rollback

Se algo der errado, simplesmente aponte para uma tag anterior:

```yaml
uses: sua-org/central-pipelines/.github/workflows/reusable-build.yml@v1.0.0
```

## 🤝 Contribuindo

1. Crie uma branch para sua feature
2. Faça as alterações
3. Teste em uma aplicação de exemplo
4. Crie um Pull Request
5. Após aprovação, crie uma tag de versão

## 📄 Licença

MIT

---

**Desenvolvido para centralizar e facilitar a manutenção de pipelines CI/CD** 🚀
