# Guia de Início Rápido - Central Pipelines

## 🚀 Setup em 5 minutos

### 1. Criar o repositório central

```bash
# Clone este repositório ou crie um novo
git clone <seu-repo>
cd central-pipelines

# Adicione os arquivos ao seu repositório
git add .
git commit -m "Initial commit: Central Pipelines"
git push origin main
```

### 2. Configurar uma aplicação para usar

No repositório da sua aplicação, crie o arquivo `.github/workflows/pipeline.yml`:

```yaml
name: Pipeline

on:
  push:
    branches: [main, develop]

jobs:
  pipeline:
    uses: sua-org/central-pipelines/.github/workflows/reusable-pipeline.yml@main
    with:
      environments: ${{ github.ref_name == 'main' && '["prod"]' || '["dev"]' }}
      node-version: '18'
```

**Substitua** `sua-org/central-pipelines` pelo caminho real do seu repositório.

### 3. Configurar ambientes no GitHub

1. Vá para **Settings > Environments** no repositório da aplicação
2. Crie os ambientes:
   - `dev`
   - `prod`
3. Configure aprovações em `prod` se necessário

### 4. Testar

```bash
# No repositório da aplicação
git add .github/workflows/pipeline.yml
git commit -m "Add central pipeline"
git push origin develop
```

Vá para **Actions** no GitHub e veja o workflow rodando! 🎉

## 📋 Checklist de configuração

- [ ] Repositório central criado
- [ ] Workflows reutilizáveis commitados
- [ ] Aplicação configurada com workflow
- [ ] Ambientes criados no GitHub
- [ ] Teste executado com sucesso

## 🔧 Próximos passos

1. **Adicionar mais ambientes**: pre-prod, staging, etc.
2. **Configurar secrets**: Deploy tokens, credentials
3. **Adicionar notificações**: Slack, Teams, email
4. **Customizar deploy**: SSH, Kubernetes, Cloud providers
5. **Versionar o central**: Criar tags (v1.0.0)

## 💡 Dicas importantes

- **Repositórios privados**: Configure um token com permissões adequadas
- **Versionamento**: Use tags no repo central para controlar mudanças
- **Testes**: Sempre teste em dev antes de aplicar em prod
- **Documentação**: Documente customizações específicas da sua organização

## 🆘 Problemas comuns

### Erro: "workflow not found"
- Verifique se o caminho está correto: `sua-org/central-pipelines`
- Confirme que o arquivo existe em `.github/workflows/`
- Se o repo for privado, configure token de acesso

### Erro: "environment not found"
- Crie o ambiente em Settings > Environments
- Verifique o nome (case-sensitive)

### Build falha
- Confirme que `package.json` tem scripts `build` e `test`
- Verifique a versão do Node.js
- Veja os logs detalhados no Actions

## 📚 Documentação completa

Veja o [README.md](README.md) principal para documentação completa.
