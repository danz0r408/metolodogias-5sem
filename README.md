# Metodologias - 5 Semestre

Este repositorio resume as metodologias usadas no projeto de Big Data apresentado na documentacao.

O trabalho tem como contexto a deteccao de Perdas Nao Tecnicas (PNT) no setor eletrico, causadas por furtos e fraudes de energia. Para organizar o projeto, foram aplicadas metodologias voltadas para negocio, engenharia de dados, ciencia de dados, gestao agil e operacao de pipelines.

As metodologias abordadas sao:

- CRISP-DM
- KDD
- Scrum / Kanban
- DataOps

## CRISP-DM

O CRISP-DM foi usado para garantir o alinhamento entre os objetivos de negocio e a execucao tecnica do projeto.

No documento, ele aparece dividido em seis fases ciclicas:

### 1. Business Understanding

Nesta fase foi definido o objetivo principal do projeto: reduzir as Perdas Nao Tecnicas causadas por furtos e fraudes de energia em larga escala.

O foco foi identificar comportamentos irregulares de consumo para aumentar a assertividade das equipes de fiscalizacao em campo. Com isso, a eficiencia operacional pode ser transformada em recuperacao de receita.

### 2. Data Understanding

Aqui ocorreu a exploracao inicial das bases oficiais da ANEEL e dos dados historicos de faturamento.

Durante essa analise, o histograma de distribuicao de frequencia mostrou a presenca de outliers extremos, com cauda longa a direita. Esse comportamento justificou o uso de abordagens focadas em deteccao de anomalias.

### 3. Data Preparation

Nesta fase foi consolidado o pipeline usando a arquitetura Medallion dentro do Databricks.

Os dados brutos de interrupcoes e perdas passam por limpeza, padronizacao e enriquecimento com variaveis geograficas e climaticas. A camada Bronze guarda os dados brutos, enquanto a camada Silver trata os dados para remover ruidos e valores nulos.

### 4. Modeling

Na etapa de modelagem, o documento apresenta o uso de algoritmos de Machine Learning nao supervisionados no Databricks, como Isolation Forests ou tecnicas de Clustering.

O modelo calcula um score de risco para cada unidade consumidora, considerando a distancia entre o padrao de consumo observado e o comportamento esperado para a regiao.

### 5. Evaluation

A avaliacao valida estatisticamente os alertas de anomalia gerados pelo modelo.

Os resultados sao comparados com bases historicas de inspecoes passadas e fraudes reais ja confirmadas. O objetivo e medir precision e recall, evitando uma quantidade excessiva de falsos positivos.

### 6. Deployment

Na implantacao, as tabelas refinadas da camada Gold do Databricks alimentam dashboards operacionais e sistemas de ordem de servico.

Com isso, o modelo passa a direcionar os fiscais aos alvos com maior probabilidade de irregularidade.

## KDD

Enquanto o CRISP-DM organiza o ciclo de vida de negocio do projeto, o KDD foi aplicado para orientar as etapas tecnicas de engenharia e extracao de conhecimento a partir dos dados consolidados no Databricks.

### Selecao

Nesta etapa foram identificadas e isoladas as variaveis criticas para o modelo de fraude.

O documento cita atributos como:

- historico de consumo faturado;
- registros de interrupcoes de energia;
- coordenadas geograficas dos medidores;
- dados de temperatura media por regiao.

### Pre-processamento

O pre-processamento envolve a ingestao e persistencia das fontes de dados estruturadas e de streaming diretamente na camada Bronze do Databricks.

Essa camada funciona como armazenamento bruto dos dados em seu formato original. A ideia e garantir a integridade do historico e a linhagem dos dados antes de qualquer modificacao tecnica.

### Transformacao

A transformacao acontece na transicao e consolidacao da camada Silver.

Aqui os dados brutos da Bronze passam por limpeza profunda, tratamento de valores nulos e duplicados, filtragem de ruidos operacionais e normalizacao em relacao as variaveis climaticas.

Tambem e feito o calculo do desvio padrao do consumo mensal para preparar o dataset final.

### Data Mining

Na fase de Data Mining sao aplicados algoritmos de Machine Learning, como Isolation Forest ou modelos baseados em agrupamento e densidade.

Esses algoritmos sao processados de forma distribuida nos clusters Spark do Databricks. O objetivo e varrer a base transformada para destacar perfis com comportamento estatisticamente fora do padrao e com alto risco de fraude.

### Interpretacao

Na interpretacao, os padroes descobertos pelo algoritmo sao analisados pela otica do negocio.

Os outliers de cauda longa mapeados no histograma sao avaliados para verificar se a anomalia tecnica corresponde a uma perda comercial causada por furto. Os resultados sao refinados para melhorar o direcionamento das inspecoes.

## Scrum / Kanban

Scrum e Kanban foram usados para organizar a gestao agil do projeto.

## Scrum

No Scrum, a equipe organizou as frentes de trabalho, os papeis e o planejamento das entregas.

### Backlog

O backlog do projeto inclui:

- mapeamento das bases de dados da ANEEL;
- configuracao do ambiente e dos clusters no Databricks;
- desenvolvimento do pipeline de limpeza e normalizacao dos dados nas camadas Bronze e Silver;
- criacao do algoritmo de deteccao de anomalias com Isolation Forest;
- construcao do dashboard de visualizacao no Power BI.

### Sprint 1

A primeira sprint contempla:

- ingestao dos dados brutos e estruturacao do armazenamento bruto na camada Bronze do Databricks;
- tratamento de valores nulos e normalizacao climatica na camada Silver via PySpark;
- extracao do histograma de distribuicao de frequencia das perdas reais para identificacao de outliers.

### Papeis

O Product Owner do projeto e Breno Cunha Michilizzi, responsavel por definir as regras de negocio para identificacao de irregularidades e priorizar o backlog com foco no retorno financeiro.

O Scrum Master e Bruno Ramos, responsavel por garantir a aplicacao das praticas ageis e remover impedimentos tecnicos no uso do Databricks.

O time de desenvolvimento e formado por Daniel Mussoi Marques, Eduardo Yokomizo, Gustavo Camargo Souza e Gustavo Henrique. O time atua na engenharia de dados, modelagem de Machine Learning e visualizacao.

## Kanban

O Kanban foi usado para organizar visualmente o fluxo de desenvolvimento do projeto.

O quadro acompanha a evolucao das tarefas tecnicas e de negocio, garantindo entregas incrementais e transparencia em cada etapa.

### A fazer

Concentra o planejamento das proximas fases tecnicas, incluindo transformacao de dados, definicao do recorte temporal e escolha dos algoritmos de deteccao de anomalias.

Tambem inclui a criacao de indicadores de consumo, alertas automaticos e relatorios finais.

### Fazendo

Representa as frentes em execucao ativa pelo time.

Segundo o documento, essa etapa envolve integracao das bases de dados, remocao de registros duplicados e calculo da variacao de consumo mensal. Em paralelo, o grupo trabalha na definicao da variavel alvo para o modelo.

### Concluido

Reune os marcos iniciais ja finalizados e validados.

Entre eles estao o planejamento geral do projeto, a selecao dos atributos relevantes e a coleta das bases de dados publicas oficiais da ANEEL.

## DataOps

O DataOps foi desenhado para assegurar governanca e agilidade no ciclo de vida dos dados.

No documento, a estrutura de DataOps aparece organizada nas seguintes etapas:

### Coleta de Dados

Ingestao de dados provenientes de fontes diversificadas, incluindo bancos de dados das distribuidoras de energia, APIs regulatorias, arquivos CSV com historicos de consumo e dados externos socioeconomicos e climaticos por regiao.

### Processamento

Etapa realizada via PySpark para limpeza de registros, remocao de duplicados, tratamento de nulidade, transformacao, normalizacao, criacao de indicadores de consumo, codificacao de variaveis e validacao da integridade estatistica dos dados.

### Armazenamento

O armazenamento e centralizado no ecossistema Databricks, usando uma arquitetura de Data Lakehouse.

Essa arquitetura suporta grandes volumes de dados estruturados e semiestruturados por meio de camadas de processamento escalaveis.

### Analise

A analise envolve a aplicacao de modelos preditivos e algoritmos de deteccao de anomalias, como Isolation Forest.

O objetivo e gerar scores de risco por unidade consumidora e identificar padroes de comportamento de consumo irregular.

### Visualizacao

A visualizacao e feita por dashboards operacionais e de monitoramento no Power BI.

Esses dashboards convertem os resultados das analises em indicadores visuais e mapas de calor para direcionar estrategicamente as equipes de campo.

### Automacao

A automacao aparece na orquestracao de pipelines para execucao periodica e atualizacao automatica das bases de dados.

Essa automacao e integrada aos sistemas de alertas de fraude para operacao em tempo quase real.

### Monitoramento

O monitoramento controla continuamente a saude do pipeline e a acuracia do modelo.

O documento cita metricas de Precision e Recall, logs de execucao do sistema, verificacao de dados faltantes e alertas de inconsistencias no fluxo de dados.
