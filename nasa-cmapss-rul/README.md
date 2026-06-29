# 🔧 Manutenção Preditiva — NASA CMAPSS FD001

Série de notebooks sobre predição de Vida Útil Remanescente (RUL) de motores turbofan, combinando análise exploratória, confiabilidade estatística clássica e machine learning. O projeto usa o dataset CMAPSS da NASA como estudo de caso, mas a metodologia se aplica a qualquer ativo industrial com histórico de sensores e falhas.

## Contexto

Motores aeronáuticos operam sob condições extremas de temperatura, pressão e vibração. Identificar o momento certo de intervir, antes da falha, mas sem manutenção desnecessária, é o problema central da manutenção preditiva.

Este projeto aborda esse problema sob duas óticas complementares: a **confiabilidade estatística clássica** (análise de sobrevivência), que responde perguntas de frota e planejamento, e o **machine learning preditivo** (modelos tabulares e deep learning), que responde perguntas operacionais sobre um ativo específico.

O dataset CMAPSS (Commercial Modular Aero-Propulsion System Simulation), desenvolvido pela NASA, é um benchmark clássico da literatura de PHM (Prognostics and Health Management). O subconjunto FD001 contém 100 motores com leituras de 21 sensores operacionais até o momento de falha.

## Notebooks

| # | Notebook | Conteúdo |
|---|---|---|
| 01 | [Análise Exploratória](01_eda_cmapss_fd001.ipynb) | Limpeza, distribuições, correlação, métricas PHM (monotonicidade, prognostibilidade) |
| 02 | [Análise de Sobrevivência](02_survival_analysis.ipynb) | Kaplan-Meier, Nelson-Aalen, ajuste de distribuições paramétricas (Weibull, Log-Normal, Log-Logística, Exponencial) |
| 03 | [Feature Engineering](03_feature_engineering.ipynb) | RUL cap, normalização, rolling features, preparação dos dados tabulares e sequenciais |
| 04 | [Modelos Tabulares](04_tabular_models.ipynb) | Regressão Linear, Random Forest, XGBoost, Feature Importance, SHAP |
| 05 | [LSTM](05_deep_learning.ipynb) | Rede neural recorrente para predição sequencial de RUL |
| 06 | [Comparação Final](06_comparativo.ipynb) | Confiabilidade estatística vs machine learning, discussão de trade-offs, conclusão geral |

## Principais achados

**Seleção de sensores (notebook 01)**
Apenas 12 dos 21 sensores carregam informação real de degradação, os demais têm variância praticamente zero. Sensores térmicos, especialmente `temp_lpt_outlet` e `static_hpc_outlet`, concentram o sinal mais forte segundo correlação com RUL e métricas PHM.

**Distribuição de falhas (notebook 02)**
Os motores têm um período de vida mínimo garantido de 128 ciclos. A distribuição Log-Normal descreve melhor a vida útil da frota do que o Weibull, o modelo padrão da literatura de confiabilidade, sugerindo um mecanismo de degradação multiplicativo.

**Impacto do preprocessing (notebooks 03 e 04)**
A correção da normalização e da estratégia de predição no conjunto de teste reduziu o RMSE da Regressão Linear de 58 para 19.6 ciclos, sem qualquer mudança no modelo.

**Comparação de modelos (notebooks 04, 05 e 06)**

| Modelo | RMSE | S-score |
|---|---|---|
| Regressão Linear | 19.60 | 906.49 |
| XGBoost | 17.78 | 766.51 |
| LSTM | 13.52 | 332.35 |

O LSTM superou os modelos tabulares ao aprender diretamente a dinâmica temporal dos sensores, sem depender de rolling features pré-calculadas.

**Confiabilidade estatística vs machine learning (notebook 06)**
As duas abordagens não competem entre si. A análise de sobrevivência responde perguntas de planejamento em nível de frota (estoque de peças, orçamento de manutenção). Os modelos preditivos respondem perguntas operacionais sobre um ativo específico (priorizar inspeção, decidir retirada de operação). A escolha entre XGBoost e LSTM, por sua vez, envolve um trade-off entre precisão e interpretabilidade que depende do contexto de risco da aplicação.

## Dataset

O dataset CMAPSS é disponibilizado publicamente pela NASA Prognostics Center of Excellence.

> A. Saxena and K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set", NASA Ames Prognostics Data Repository, NASA Ames Research Center, Moffett Field, CA.

Fonte: [NASA Prognostics Data Repository](https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/)

## Estrutura do repositório

```
├── data/                     # Dataset CMAPSS (train, test, RUL)
│   └── processed/            # Arrays salvos entre notebooks (.npy)
├── midia/                    # Imagens geradas pelos notebooks
├── 01_eda_cmapss_fd001.ipynb
├── 02_survival_analysis.ipynb
├── 03_feature_engineering.ipynb
├── 04_tabular_models.ipynb
├── 05_deep_learning.ipynb
├── 06_comparativo.ipynb
└── README.md
```

## Tecnologias

Python · Pandas · NumPy · Matplotlib · Seaborn · Scikit-learn · SciPy · Lifelines · XGBoost · TensorFlow/Keras · SHAP

## Como reproduzir

```bash
pip install pandas numpy matplotlib seaborn scikit-learn scipy lifelines xgboost tensorflow shap
```

Baixe os arquivos do dataset CMAPSS FD001 e coloque em `data/`. Execute os notebooks na ordem numérica, cada um depende dos arquivos salvos pelo anterior em `data/processed/`.
