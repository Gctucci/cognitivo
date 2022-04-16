# Cognitivo - Base de dados do Airbnb

## Perguntas
-------------
1. Como foi a definição da sua estratégia de modelagem?

Primeiramente realizei uma análise exploratória dos dados, buscando informações como número de valores distintos de cada variável, valores faltantes, etc. Também li a descrição dos dados disponível no [link](https://docs.google.com/spreadsheets/d/1iWCNJcSutYqpULSQHlNyGInUvHg2BoUGoNRIGa6Szc4/edit#gid=982310896). A seguir, fiz uma análise da distribuição dos dados de forma independente, e com base nessa informação realizei os seguintes passos:
* Uma análise de correlação não-paramétrica entre as variáveis numéricas, removendo as variáveis que apresentam alta correlação entre si (> 0.8)
* Uma análise exploratória das variáveis textuais e categóricas, excluindo aquelas com alto número de valores (como as do tipo "id"), aquelas com alto grau de valores faltantes, ou aquelas que não pareciam pertinentes ao contexto do problema

Com base nessa limpeza dos dados, efetuei a normalização de todos os dados numéricos usando a função Z-Score, bem como substitui valores faltantes nos dados pela média de cada variável. Para o modelo em si, inicialmente criei um modelo simples para utilizar como baseline, e com base nas limitações desse modelo propus um outro mais complexo, que se utiliza de mais variáveis. Também transformei a variável resposta para tipo dummy (One-hot encoding)

O modelo simples proposto usa somente os dados numéricos e booleanos da base, evitando potenciais problemas com a representação de dados tipo texto ou categóricos. Para evitar problemas com balanceamento dos dados, o algoritmo de classificação utilizado fez uso de uma distribuição de pesos para cada classe, proporcional à ocorrência de cada uma delas no conjunto de treino.

O modelo mais completo incluiu também os dados de texto através de sua conversão para embeddings usando modelos pré-treinados. Além disso, uma estratégia de rebalanceamento simples foi utilizada, selecionando a classe com menor amostras e reamostrando-a até atingir um número igual àquela com maior número de amostras (over sampling)
2. Como foi definida a função de custo utilizada?

A função de custo utilizada inicialmente foi a de cross-entropia padrão da biblioteca, mas dado o alto desbalancemento de classes na base de dados, foi utilizada no modelo mais completo uma função de perda específica para dados desbalanceados, denominada Perda Focal (Focal Loss). Foram testados diversos valores de gamma, porém a com melhor resultado teve gamma=5.
4. Qual foi o critério utilizado na seleção do modelo final?

Pelo alto desbalanceamento dos dados, o critério mais utilizado para a avaliação dos modelos foi a métrica F1 **para os dados de validação** de cada classe da variável resposta "room_type", principalmente a diferença dessa métrica entre cada classes (não apenas uma análise de médias).
6. Qual foi o critério utilizado para validação do modelo? Por que escolheu utilizar este
método?
Para a validação do modelo, os dados foram separados de forma aleatória em conjunto de treino (70%) e teste (30%), sendo o modelo avaliado principalmente pelo seu desempenho no conjunto de teste. A escolha desse método em detrimento de outros mais robustos (como Cross validation) se deve ao alto desbalanceamento das classes analisadas, o que poderia gerar problemas na divisão desses dados usando muitas partições, mesmo considerando o uso de técnicas de rebalanceamento como Over sampling. Além disso, essa forma de validação é simples de implementar, o que acelera o processo de análise de dados e permite a seleção mais rápida de modelos e/ou alteração de seus hyperparâmetros.
5. Quais evidências você possui de que seu modelo é suficientemente bom?

Ainda estou longe de estar satisfeito com o modelo proposto, visto que as métricas de precision, recall e F1 ainda encontram-se baixas, apesar do uso de técnicas específicas para lidar com o desbalanceamento dos dados. Nesse contexto, meu principal critério para avaliar a qualidade do modelo proposto é observar a métrica F1 **para os dados de validação** de cada classe da variável resposta "room_type", principalmente a diferença dessa métrica entre as classes.
