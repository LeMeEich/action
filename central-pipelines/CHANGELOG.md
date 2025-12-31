# 📝 CHANGELOG

Todas as mudanças notáveis neste projeto serão documentadas neste arquivo.

O formato é baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.0.0/),
e este projeto adere ao [Versionamento Semântico](https://semver.org/lang/pt-BR/).

## [1.0.0] - 2025-12-31

### Adicionado
- Workflow reutilizável de build (`reusable-build.yml`)
- Workflow reutilizável de deploy (`reusable-deploy.yml`)
- Workflow reutilizável de pipeline completo (`reusable-pipeline.yml`)
- Composite action para build
- Composite action para deploy
- Composite action para validação de ambiente
- Composite action para geração de versão
- Exemplos de consumo (4 aplicações exemplo)
- Documentação completa (README, QUICKSTART, CHANGELOG)
- Suporte para múltiplos ambientes (dev, pre, prod)
- Geração automática de versões
- Upload/download de artifacts
- Build info detalhado

### Características
- Suporte a Node.js (versões configuráveis)
- Comandos de build customizáveis
- Deploy para múltiplos ambientes
- Validação de ambientes
- Versionamento semântico automático
- Integração com GitHub Environments

---

## [Unreleased]

### Planejado para próximas versões
- Suporte a Docker builds
- Integração com Kubernetes
- Suporte a Python, Java, .NET
- Notificações (Slack, Teams, Email)
- Testes de performance
- Análise de segurança (SAST/DAST)
- Cache inteligente de dependências
- Rollback automático
- Métricas de deploy

---

## Como usar este changelog

### Tipos de mudanças
- **Adicionado** para novas funcionalidades
- **Modificado** para mudanças em funcionalidades existentes
- **Descontinuado** para funcionalidades que serão removidas
- **Removido** para funcionalidades removidas
- **Corrigido** para correções de bugs
- **Segurança** para vulnerabilidades

### Versionamento
- **MAJOR** (X.0.0): Mudanças incompatíveis com versões anteriores
- **MINOR** (0.X.0): Novas funcionalidades compatíveis
- **PATCH** (0.0.X): Correções de bugs

[1.0.0]: https://github.com/sua-org/central-pipelines/releases/tag/v1.0.0
[Unreleased]: https://github.com/sua-org/central-pipelines/compare/v1.0.0...HEAD
