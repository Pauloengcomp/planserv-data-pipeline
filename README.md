# Planserv Data Pipeline

Pipeline de dados automatizado para ingestão, processamento e disponibilização de tabelas estruturadas, seguindo arquitetura em camadas (RAW → SILVER → GOLD), orquestrado com Apache Airflow.

# Visão Geral

Este projeto implementa um fluxo de dados automatizado responsável por:

Extrair arquivos de uma API externa
Persistir dados brutos (camada RAW)
Aplicar transformações e padronizações (camada SILVER)
Refinar dados para regras de negócio (camada GOLD)
Disponibilizar os dados tratados para consumo analítico e automações
A orquestração é realizada via DAG no Apache Airflow, executando em ambiente Docker.

# Arquitetura

O projeto segue o padrão moderno de engenharia de dados em camadas:

           ┌────────────┐
           │   API      │
           └──────┬─────┘
                  │
                  ▼
            RAW (dados brutos)
                  │
                  ▼
          SILVER (padronização)
                  │
                  ▼
          GOLD (regra de negócio)
                  │
                  ▼
        Consumo / Automação / BI
## RAW

Armazena os arquivos exatamente como recebidos
Mantém histórico
Sem alterações estruturais

## SILVER

Limpeza de dados
Padronização de colunas
Tratamento de encoding
Remoção de inconsistências
Normalização de formatos

## GOLD

Aplicação de regras de negócio
Seleção de campos relevantes
Estrutura pronta para consumo analítico
Base para dashboards ou automações (ex: Slack Bot)

# Stack Tecnológica

Python
Apache Airflow
Docker
Pandas
Requests
Estrutura de controle de estado via JSON

# Estrutura do Projeto
.
├── dags/
│   ├── pipeline.py
│   └── downloads/
│       ├── raw/
│       ├── silver/
│       └── gold/
├── state/
│   └── state.json
├── docker-compose.yml
└── README.md

# Orquestração

##A DAG executa:

Extração de dados da API
Validação de alteração via controle de hash
Processamento incremental
Tratamento de exceções
Persistência em camadas
Finalização condicional

##Configuração típica:

schedule_interval="0 */12 * * *"  (Execução a cada 12 horas)

# Tratamento de Erros

Controle de estado para evitar reprocessamento desnecessário
Validação de resposta HTTP
Retry automático via Airflow
Separação clara de responsabilidades por função

# Possíveis Evoluções

Paralelismo com LocalExecutor ou CeleryExecutor
Persistência em banco relacional
Armazenamento em Data Lake (S3 / GCS)
Monitoramento via logs estruturados
Integração com ferramentas de BI
Testes automatizados (Pytest)

# Como Executar

Subir ambiente: docker-compose up -d
Acessar Airflow: http://localhost:8080
Ativar a DAG
Executar manualmente ou aguardar agendamento

# Objetivo do Projeto

Demonstrar implementação prática de:
Arquitetura de dados em camadas
Orquestração com Airflow
Processamento incremental
Engenharia de dados aplicada a cenário real