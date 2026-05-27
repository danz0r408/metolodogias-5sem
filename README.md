# Projeto de Big Data - Deteccao de Fraudes na Rede Eletrica

Este projeto apresenta uma solucao de Big Data Analytics para ajudar concessionarias de energia eletrica a identificar possiveis fraudes e furtos de energia, tambem conhecidos como Perdas Nao Tecnicas (PNT).

A ideia principal e simples: em vez de mandar equipes de fiscalizacao para lugares aleatorios, o projeto usa dados, processamento em larga escala e Machine Learning para apontar quais unidades consumidoras ou regioes possuem maior risco de irregularidade.

## Problema

O setor eletrico sofre com perdas causadas por ligacoes clandestinas, adulteracao de medidores e outros tipos de fraude. Essas perdas geram prejuizo financeiro para as distribuidoras, aumentam custos para consumidores regulares e ainda podem comprometer a qualidade e a seguranca da rede.

O grande desafio e que esse problema acontece em larga escala. Uma distribuidora pode ter milhoes de registros de consumo, interrupcoes, faturamento, clima e localizacao. Analisar tudo isso manualmente seria lento, caro e pouco eficiente.

Por isso, a pergunta central do projeto e:

> Como identificar padroes de consumo irregular em larga escala para aumentar a assertividade das inspecoes e reduzir perdas comerciais?

## Objetivo

O objetivo do trabalho e construir uma proposta de arquitetura de dados capaz de:

- coletar dados oficiais e operacionais sobre perdas e interrupcoes;
- organizar esses dados em uma estrutura confiavel;
- tratar inconsistencias, valores nulos e registros duplicados;
- identificar comportamentos fora do padrao;
- gerar um score de risco para apoiar a fiscalizacao;
- apresentar os resultados em dashboards para tomada de decisao.

## Bases de dados

O projeto utiliza como referencia dados publicos e oficiais da ANEEL, alem de dados complementares que podem ser integrados ou simulados para enriquecer a analise.

As principais fontes consideradas sao:

- Relatorios de Perdas de Energia Eletrica da ANEEL;
- dados de interrupcoes nas redes de distribuicao;
- indicadores de continuidade e desempenho das distribuidoras;
- historico de consumo e faturamento;
- dados de medidores inteligentes;
- variaveis climaticas e socioeconomicas por regiao.

Essas fontes ajudam a cruzar o comportamento de consumo com falhas na rede, contexto regional e historico de perdas.

## Como o pipeline funciona

O pipeline foi pensado usando uma arquitetura Lakehouse no Databricks, seguindo o modelo Medallion. Esse modelo separa os dados em camadas, o que facilita a organizacao e melhora a confiabilidade do processo.

### Camada Bronze

Na camada Bronze ficam os dados brutos. Aqui os arquivos sao armazenados praticamente como chegaram da fonte original, por exemplo em CSV convertido para Delta.

Essa etapa e importante porque preserva o historico original e permite reprocessar os dados caso alguma regra de negocio mude depois.

### Camada Silver

Na camada Silver os dados passam por limpeza e padronizacao.

Nesta fase entram tarefas como:

- remocao de registros duplicados;
- tratamento de valores nulos;
- padronizacao de campos;
- cruzamento entre bases diferentes;
- normalizacao por variaveis climaticas;
- calculo da variacao mensal de consumo.

Essa camada transforma dados baguncados em dados confiaveis para analise.

### Camada Gold

Na camada Gold ficam os dados prontos para gerar valor para o negocio.

E aqui que entram os indicadores finais, os scores de risco e os resultados do modelo de Machine Learning. Esses dados podem alimentar dashboards, relatorios e sistemas de ordem de servico.

## Analise e Machine Learning

Durante a analise exploratoria, o projeto identificou que as perdas nao seguem uma distribuicao normal. A maior parte dos casos fica em faixas menores, mas existem alguns pontos muito extremos, formando uma cauda longa.

Em termos simples: poucas regioes ou unidades podem concentrar perdas muito altas.

Esse comportamento justifica o uso de tecnicas de deteccao de anomalias. O algoritmo sugerido no trabalho e o Isolation Forest, que ajuda a encontrar registros com comportamento muito diferente do esperado.

O modelo gera um score de risco para indicar quais consumidores ou regioes devem receber mais atencao das equipes de fiscalizacao.

## Visualizacao dos resultados

Os resultados do processamento e da modelagem sao pensados para serem exibidos em dashboards no Power BI.

Esses dashboards podem mostrar:

- mapas de calor de regioes com maior risco;
- indicadores de perdas por distribuidora ou localidade;
- ranking de unidades com maior score de anomalia;
- comparacao entre perdas, interrupcoes e consumo;
- metricas de acompanhamento do modelo.

Assim, a area de negocio consegue entender os dados sem precisar olhar diretamente para codigo ou tabelas tecnicas.

## Metodologias usadas

O projeto combina metodologias de negocio, ciencia de dados, engenharia de dados e gestao agil.

### CRISP-DM

O CRISP-DM guia o ciclo de vida do projeto de dados:

1. entendimento do negocio;
2. entendimento dos dados;
3. preparacao dos dados;
4. modelagem;
5. avaliacao;
6. implantacao.

No projeto, ele ajuda a garantir que a solucao tecnica esteja conectada ao problema real da distribuidora: reduzir perdas e melhorar a fiscalizacao.

### KDD

O KDD orienta a parte mais tecnica de descoberta de conhecimento nos dados.

Ele aparece principalmente nas etapas de selecao das variaveis, pre-processamento, transformacao, mineracao de dados e interpretacao dos resultados encontrados.

### Scrum e Kanban

Scrum e Kanban foram usados para organizar o trabalho do grupo.

O Scrum ajuda na divisao de papeis, backlog e sprints. Ja o Kanban ajuda a visualizar o andamento das tarefas em colunas como "A fazer", "Fazendo" e "Concluido".

### DataOps

DataOps entra para dar mais confiabilidade ao pipeline de dados.

A proposta e automatizar execucoes, acompanhar logs, monitorar qualidade dos dados e garantir que o processo seja repetivel, organizado e facil de manter.

## Tecnologias

As principais tecnologias e ferramentas consideradas no projeto sao:

- Python;
- PySpark;
- Apache Spark;
- Databricks;
- Delta Lake;
- Power BI;
- Machine Learning;
- Isolation Forest;
- arquitetura Lakehouse;
- modelo Medallion.

## Estrutura do repositorio

Atualmente o repositorio contem a documentacao do trabalho e este README explicativo.

```text
.
+-- 10 - Documentacao do Projeto (1).docx
+-- README.md
```

Em uma evolucao futura, o repositorio pode receber tambem notebooks, bases tratadas, imagens do dashboard e arquivos auxiliares.

## Possiveis proximas versoes

Algumas melhorias que podem ser feitas em proximas etapas:

- adicionar notebooks com o pipeline em PySpark;
- incluir uma amostra de dados anonimizados;
- publicar imagens do dashboard em Power BI;
- documentar melhor as metricas do modelo;
- comparar Isolation Forest com outros algoritmos de deteccao de anomalias;
- automatizar a execucao com Databricks Workflows;
- criar um fluxo completo de ingestao, tratamento, modelagem e visualizacao.

## Conclusao

O projeto mostra como Big Data pode ser usado para resolver um problema real do setor eletrico. A deteccao de Perdas Nao Tecnicas nao depende apenas de fiscalizacao em campo, mas tambem de uma boa estrategia de dados.

Com uma arquitetura bem organizada, dados tratados e modelos de anomalia, a distribuidora pode direcionar melhor suas equipes, reduzir custos e aumentar a recuperacao de receita.

Mesmo sendo uma proposta academica, o trabalho representa um cenario bem proximo do mercado: transformar dados brutos em informacao util para decisao.
