# Exemplos de Consumo do Central Pipelines

Esta pasta contém exemplos de como diferentes aplicações podem consumir o repositório central de pipelines.

## 📁 Estrutura

```
exemplos-consumo/
├── app1/    # Exemplo com build e deploy separados
├── app2/    # Exemplo com pipeline completo
├── app3/    # Exemplo com composite actions
└── app4/    # Exemplo com validações customizadas
```

## 🎯 Cenários de Uso

### App1: Build e Deploy Separados
**Quando usar**: Quando você precisa de controle fino sobre cada etapa, ou quando deploy depende de aprovações manuais.

**Características**:
- Build e deploy são jobs separados
- Fácil adicionar steps entre build e deploy
- Ideal para ambientes com múltiplas aprovações

### App2: Pipeline Completo
**Quando usar**: Para simplificar ao máximo o workflow da aplicação.

**Características**:
- Um único job cuida de tudo
- Deploy automático para múltiplos ambientes
- Menos código no repositório da aplicação

### App3: Composite Actions
**Quando usar**: Quando você quer flexibilidade máxima mas manter a lógica centralizada.

**Características**:
- Total controle sobre o workflow
- Reutiliza apenas as "ações" individuais
- Permite inserir steps customizados entre as actions

### App4: Com Validações
**Quando usar**: Quando precisa de validações específicas antes do deploy.

**Características**:
- Validação de ambiente antes de iniciar
- Suporte a deploy manual com escolha de ambiente
- Ideal para ambientes regulados

## 🚀 Como testar localmente

Para testar localmente, você pode usar o [act](https://github.com/nektos/act):

```bash
# Instalar act
# Windows (via Chocolatey)
choco install act-cli

# Testar workflow
cd app1
act -l  # Listar workflows
act -j build  # Executar job de build
```

## 📝 Personalizando para sua organização

1. Substitua `sua-org/central-pipelines` pelo nome real do seu repositório central
2. Ajuste os ambientes conforme sua necessidade (dev, qa, staging, prod, etc.)
3. Customize os comandos de build conforme sua stack
4. Adicione secrets necessários para seu ambiente

## 🔧 Configuração mínima necessária

Cada aplicação precisa ter:

1. **package.json** com scripts:
   ```json
   {
     "scripts": {
       "build": "seu-comando-de-build",
       "test": "seu-comando-de-test"
     }
   }
   ```

2. **Ambientes configurados** no GitHub (Settings > Environments):
   - dev
   - pre (opcional)
   - prod

3. **Secrets** (se necessário):
   - `DEPLOY_TOKEN`
   - Outros secrets específicos da aplicação

## 💡 Dicas

1. **Comece simples**: Use o exemplo App2 (pipeline completo) para começar
2. **Evolua conforme necessário**: Migre para App1 se precisar de mais controle
3. **Teste em dev primeiro**: Sempre teste mudanças em ambiente de dev
4. **Use tags**: No repositório central, use tags para versionar suas pipelines
5. **Documente customizações**: Se adicionar steps customizados, documente o porquê

## 🔗 Recursos adicionais

- [GitHub Actions - Reusing Workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows)
- [GitHub Actions - Creating Composite Actions](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action)
- [GitHub Actions - Using Environments](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)
