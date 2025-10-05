---
description: 'Melhores práticas para arquivos YAML de Pipeline do Azure DevOps'
applyTo: '\*\*/azure-pipelines.yml, \**/azure-pipelines*.yml, \*\*/\*.pipeline.yml'
---
# Melhores Práticas para YAML de Pipeline do Azure DevOps

## Diretrizes Gerais

  - Use a sintaxe YAML de forma consistente com indentação adequada (2 espaços).
  - Sempre inclua nomes e `displayName` significativos para pipelines, estágios (stages), trabalhos (jobs) e passos (steps).
  - Implemente tratamento de erros adequado e execução condicional.
  - Use variáveis e parâmetros para tornar os pipelines reutilizáveis e fáceis de manter.
  - Siga o princípio do menor privilégio para conexões de serviço e permissões.
  - Inclua logs e diagnósticos abrangentes para a solução de problemas.

## Estrutura do Pipeline

  - Organize pipelines complexos usando estágios (stages) para melhor visualização и controle.
  - Use trabalhos (jobs) para agrupar passos relacionados e habilitar a execução paralela quando possível.
  - Implemente dependências adequadas entre estágios e trabalhos.
  - Use templates para componentes de pipeline reutilizáveis.
  - Mantenha os arquivos de pipeline focados e modulares - divida pipelines grandes em múltiplos arquivos.

## Melhores Práticas de Build

  - Use versões específicas de `agent pool` e imagens de VM para consistência.
  - Armazene em cache as dependências (npm, NuGet, Maven, etc.) para melhorar o desempenho do build.
  - Implemente um gerenciamento de artefatos adequado com nomes significativos e políticas de retenção.
  - Use variáveis de build para números de versão e metadados de build.
  - Inclua validações de qualidade de código (linting, testes, varreduras de segurança).
  - Garanta que os builds sejam reprodutíveis e independentes do ambiente.

## Integração de Testes

  - Execute testes unitários como parte do processo de build.
  - Publique os resultados dos testes em formatos padrão (JUnit, VSTest, etc.).
  - Inclua relatórios de cobertura de código e validações de qualidade.
  - Implemente testes de integração e ponta a ponta (end-to-end) nos estágios apropriados.
  - Use análise de impacto de teste (test impact analysis) quando disponível para otimizar a execução dos testes.
  - Falhe rapidamente em caso de falha nos testes para fornecer feedback rápido.

## Considerações de Segurança

  - Use o Azure Key Vault para configurações e segredos sensíveis.
  - Implemente o gerenciamento adequado de segredos com grupos de variáveis.
  - Use conexões de serviço (service connections) com as permissões mínimas necessárias.
  - Habilite varreduras de segurança (vulnerabilidades de dependência, análise estática).
  - Implemente portões de aprovação (approval gates) para implantações em produção.
  - Use identidades gerenciadas (managed identities) sempre que possível em vez de `service principals`.

## Estratégias de Implantação (Deployment)

  - Implemente a promoção de ambiente adequada (dev → homologação → produção).
  - Use `deployment jobs` com o direcionamento de ambiente correto.
  - Implemente estratégias de implantação blue-green ou canary quando apropriado.
  - Inclua mecanismos de rollback e verificações de saúde (health checks).
  - Use infraestrutura como código (ARM, Bicep, Terraform) para implantações consistentes.
  - Implemente o gerenciamento de configuração adequado por ambiente.

## Gerenciamento de Variáveis e Parâmetros

  - Use grupos de variáveis para configurações compartilhadas entre pipelines.
  - Implemente parâmetros de tempo de execução (runtime parameters) para uma execução flexível do pipeline.
  - Use variáveis condicionais com base em branches ou ambientes.
  - Proteja variáveis sensíveis e marque-as como segredos.
  - Documente o propósito das variáveis e seus valores esperados.
  - Use templates de variáveis para lógicas complexas de variáveis.

## Otimização de Desempenho

  - Use `parallel jobs` e estratégias de `matrix` quando apropriado.
  - Implemente estratégias de cache adequadas para dependências e saídas de build.
  - Use `shallow clone` para operações Git quando o histórico completo não for necessário.
  - Otimize a construção de imagens Docker com `multi-stage builds` e cache de camadas.
  - Monitore o desempenho do pipeline e otimize os gargalos.
  - Use gatilhos de recursos de pipeline (resource triggers) de forma eficiente.

## Monitoramento e Observabilidade

  - Inclua logs abrangentes em todo o pipeline.
  - Use o Azure Monitor e o Application Insights para rastreamento de implantações.
  - Implemente estratégias de notificação adequadas para falhas e sucessos.
  - Inclua verificações de saúde da implantação e gatilhos de rollback automatizados.
  - Use a análise de pipeline (pipeline analytics) para identificar oportunidades de melhoria.
  - Documente o comportamento do pipeline e os passos para solução de problemas.

## Templates e Reutilização

  - Crie templates de pipeline para padrões comuns.
  - Use `extends templates` para herança completa de pipelines.
  - Implemente templates de passos (step templates) для sequências de tarefas reutilizáveis.
  - Use templates de variáveis para lógicas complexas de variáveis.
  - Versione os templates adequadamente para estabilidade.
  - Documente os parâmetros e exemplos de uso dos templates.

## Estratégia de Branch e Gatilho (Trigger)

  - Implemente gatilhos apropriados para diferentes tipos de branch.
  - Use filtros de caminho (path filters) para acionar builds apenas quando arquivos relevantes mudarem.
  - Configure gatilhos de CI/CD adequados para as branches `main`/`master`.
  - Use gatilhos de pull request para validação de código.
  - Implemente gatilhos agendados (scheduled triggers) para tarefas de manutenção.
  - Considere gatilhos de recursos (resource triggers) para cenários com múltiplos repositórios.

## Estrutura de Exemplo

```yaml
# azure-pipelines.yml
trigger:
  branches:
    include:
      - main
      - develop
  paths:
    exclude:
      - docs/*
      - README.md

variables:
  - group: shared-variables
  - name: buildConfiguration
    value: 'Release'

stages:
  - stage: Build
    displayName: 'Compilar e Testar'
    jobs:
      - job: Build
        displayName: 'Compilar Aplicação'
        pool:
          vmImage: 'ubuntu-latest'
        steps:
          - task: UseDotNet@2
            displayName: 'Usar .NET SDK'
            inputs:
              version: '8.x'
          
          - task: DotNetCoreCLI@2
            displayName: 'Restaurar dependências'
            inputs:
              command: 'restore'
              projects: '**/*.csproj'
          
          - task: DotNetCoreCLI@2
            displayName: 'Compilar aplicação'
            inputs:
              command: 'build'
              projects: '**/*.csproj'
              arguments: '--configuration $(buildConfiguration) --no-restore'

  - stage: Deploy
    displayName: 'Implantar em Homologação'
    dependsOn: Build
    condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
    jobs:
      - deployment: DeployToStaging
        displayName: 'Implantar no Ambiente de Homologação'
        environment: 'staging'
        strategy:
          runOnce:
            deploy:
              steps:
                - download: current
                  displayName: 'Baixar artefato drop'
                  artifact: drop
                - task: AzureWebApp@1
                  displayName: 'Implantar no Azure Web App'
                  inputs:
                    azureSubscription: 'staging-service-connection'
                    appType: 'webApp'
                    appName: 'myapp-staging'
                    package: '$(Pipeline.Workspace)/drop/**/*.zip'
```

## Antipadrões Comuns a Evitar

  - Codificar valores sensíveis diretamente nos arquivos YAML.
  - Usar gatilhos muito abrangentes que causam builds desnecessários.
  - Misturar lógica de build e de implantação em um único estágio.
  - Não implementar tratamento de erros e limpeza adequados.
  - Usar versões de tarefas (tasks) depreciadas sem planos de atualização.
  - Criar pipelines monolíticos que são difíceis de manter.
  - Não usar convenções de nomenclatura adequadas para clareza.
  - Ignorar as melhores práticas de segurança de pipeline.

<!-- End of Azure DevOps Pipelines Instructions -->