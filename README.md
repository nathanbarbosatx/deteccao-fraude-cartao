# Detecção de Fraudes em Transações de Cartão

Projeto de Machine Learning desenvolvido para identificar transações fraudulentas em operações com cartão de crédito.

## Sobre o projeto

O objetivo deste projeto é desenvolver e avaliar modelos de Machine Learning capazes de identificar possíveis transações fraudulentas.

O principal desafio do problema é o forte desbalanceamento entre as classes. A grande maioria das transações é legítima, enquanto uma pequena parcela representa fraudes.

Por esse motivo, a avaliação dos modelos não foi baseada apenas em acurácia. Foram utilizadas métricas como Precision, Recall, F1-score, ROC-AUC e Average Precision, além de técnicas de ajuste de threshold e estratégias de balanceamento das classes.

## Dataset

Foi utilizado o dataset de transações de cartão de crédito disponibilizado pelo TensorFlow.

O dataset possui:

- 284.807 transações
- 30 variáveis preditoras
- 1 variável de classe (`Class`)
- 284.315 transações legítimas
- 492 transações fraudulentas

A classe `0` representa transações legítimas e a classe `1` representa transações fraudulentas.

O dataset não é armazenado neste repositório. Ele é carregado diretamente por URL durante a execução do notebook.


## Tecnologias utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- XGBoost
- Imbalanced-learn
- SHAP
- Jupyter Notebook

## Metodologia

O desenvolvimento foi realizado seguindo as seguintes etapas:

### 1. Exploração dos dados

Foram analisadas:

- dimensões do dataset;
- tipos das variáveis;
- valores ausentes;
- estatísticas descritivas;
- distribuição das classes;
- comportamento da variável `Amount`.

Foi identificada uma forte diferença entre a quantidade de transações legítimas e fraudulentas.

### 2. Preparação dos dados

A variável `Amount` recebeu uma transformação logarítmica utilizando `log1p`, reduzindo o impacto de valores muito elevados.

A variável `Class` foi separada como variável alvo:

- `0` → transação legítima
- `1` → transação fraudulenta

Os dados foram divididos utilizando uma divisão estratificada entre treinamento, validação e teste.

A padronização das variáveis foi realizada utilizando `StandardScaler`. O scaler foi ajustado somente nos dados de treinamento e posteriormente aplicado aos dados de validação e teste, evitando vazamento de dados.

### 3. Modelos avaliados

Foram experimentadas diferentes abordagens:

- Regressão Logística com `class_weight="balanced"`;
- Random Forest com `class_weight="balanced"`;
- Random Forest com undersampling;
- Random Forest com oversampling;
- XGBoost utilizando `scale_pos_weight`.

### 4. Ajuste de threshold

Além do threshold padrão de 0,5, foram testados diferentes valores de threshold.

O threshold foi escolhido utilizando o conjunto de validação, tendo o F1-score como referência. Após a escolha, o modelo foi avaliado no conjunto de teste, que permaneceu separado durante esse processo.

### 5. Avaliação

Os modelos foram avaliados utilizando:

- Precision;
- Recall;
- F1-score;
- ROC-AUC;
- Average Precision;
- Matriz de confusão;
- Curva ROC;
- Curva Precision-Recall.

A utilização dessas métricas foi importante devido ao forte desbalanceamento das classes.


## Resultados

Os resultados obtidos no conjunto de teste foram:

| Modelo | Threshold | Precision | Recall | F1 | ROC-AUC | AP |
|---|---:|---:|---:|---:|---:|---:|
| Regressão Logística | 0.99 | 0.53 | 0.85 | 0.65 | 0.971852 | 0.719430 |
| Random Forest | 0.42 | 0.89 | 0.84 | 0.86 | 0.957110 | 0.860970 |
| RF + Undersampling | 0.92 | 0.78 | 0.82 | 0.80 | 0.974599 | 0.718127 |
| RF + Oversampling | 0.30 | 0.87 | 0.85 | 0.86 | 0.952565 | 0.865635 |
| XGBoost | 0.99 | 0.82 | 0.78 | 0.80 | 0.982738 | 0.811209 |


### Análise dos resultados

A Regressão Logística apresentou recall de 0,85, porém apresentou precision de 0,53, indicando uma quantidade maior de falsos positivos.

O Random Forest apresentou precision de 0,89, recall de 0,84 e F1-score de 0,86.

O Random Forest com undersampling apresentou F1-score de 0,80 e Average Precision de 0,7181. Nessa abordagem, a quantidade de exemplos utilizados no treinamento foi significativamente reduzida para equilibrar as classes.

O Random Forest com oversampling apresentou precision de 0,87, recall de 0,85 e F1-score de 0,86. Essa abordagem também apresentou o maior Average Precision entre os modelos avaliados, com 0,8656.

O XGBoost apresentou o maior ROC-AUC, com 0,9827. Entretanto, utilizando o threshold de 0,99 definido durante a validação, apresentou recall de 0,78 e F1-score de 0,80 no conjunto de teste.

Os resultados demonstram que diferentes métricas podem apresentar perspectivas diferentes sobre o desempenho dos modelos. Por isso, a avaliação considerou um conjunto de métricas, em vez de utilizar somente a acurácia.


## Aprendizados

Durante o desenvolvimento do projeto, foi possível praticar conceitos importantes de Machine Learning aplicado à detecção de fraudes, incluindo:

- análise exploratória de dados;
- identificação e tratamento de desbalanceamento de classes;
- separação entre treinamento, validação e teste;
- prevenção de data leakage;
- padronização de variáveis;
- treinamento e comparação de diferentes modelos;
- ajuste de threshold;
- avaliação utilizando Precision, Recall e F1-score;
- análise de ROC-AUC e Average Precision;
- utilização de undersampling e oversampling;
- análise de importância das variáveis;
- interpretação de modelos utilizando SHAP.

Um dos principais aprendizados foi perceber que a acurácia, isoladamente, não é suficiente para avaliar um problema de detecção de fraude altamente desbalanceado. Nesse contexto, é necessário analisar o comportamento do modelo em relação aos falsos positivos e falsos negativos.

## Como executar

### 1. Clone o repositório

```bash
git clone URL_DO_REPOSITORIO
cd deteccao-fraude-cartao