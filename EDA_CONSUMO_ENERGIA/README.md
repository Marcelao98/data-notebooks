# ⚡ Análise do Consumo de Energia Elétrica no Brasil

Uma sequência de análises exploratórias sobre o consumo de energia elétrica no Brasil, construída sobre dados públicos do setor elétrico. Cada notebook responde perguntas diferentes sobre o mesmo fenômeno — partindo da visão macro de duas décadas até a granularidade horária do sistema em tempo quase real.

---

## 📂 Estrutura do Projeto

```
analise-energia/
│
├── 01_consumo_mensal/
│   └── EDA_CONSUMO_MENSAL.ipynb
│
├── 02_carga_horaria/
│   └── EDA_CARGA_HORARIA.ipynb
│
├── 03_temperatura_correlacao/        
│   └── EDA_TEMPERATURA.ipynb
│
└── 04_modelo_xgboost/                
    └── FORECAST_XGBOOST.ipynb
```

---

## 📊 Notebooks

### 01 — Consumo Mensal (1994–2023)

**Dados:** EPE — Empresa de Pesquisa Energética
**Período:** ~20 anos de consumo mensal por segmento e região.

Análise de longo prazo do consumo de energia elétrica no Brasil. Utiliza decomposição STL para separar tendência, sazonalidade e resíduo, empregando os resíduos como detector de eventos históricos que impactaram o sistema elétrico.

**Principais achados:**

* O Brasil não possui uma sazonalidade única: Sul e Sudeste apresentam maior consumo no verão, enquanto Norte e Nordeste concentram maior demanda no segundo semestre.
* A crise financeira de 2008 impactou principalmente o setor industrial; o consumo residencial permaneceu praticamente estável.
* O embargo ambiental da Alunorte (2018) reduziu o consumo do Pará em aproximadamente 20% por mais de um ano, alterando a tendência de toda a Região Norte.
* A pandemia de COVID-19 (2020) aparece de forma sincronizada em todas as regiões, com o setor comercial registrando a maior queda da série histórica.

---

### 02 — Carga Horária do SIN (2019–2026)

**Dados:** ONS — Operador Nacional do Sistema Elétrico
**Período:** Dados horários dos quatro subsistemas (Norte, Nordeste, Sudeste e Sul).

Análise da dinâmica intradiária do Sistema Interligado Nacional. O foco não é apenas quanto se consome, mas quando e como esse comportamento varia entre regiões, dias da semana e estações do ano.

**Principais achados:**

* O vale diário ocorre entre 3h e 5h, enquanto o pico acontece entre 18h e 19h.
* A maior variabilidade do consumo concentra-se entre 10h e 16h, tornando esse intervalo o mais difícil de prever.
* Dias úteis e finais de semana apresentam perfis de carga completamente distintos: o platô industrial praticamente desaparece aos domingos.
* O Nordeste apresenta uma duck curve cada vez mais acentuada entre 2019 e 2026, refletindo a expansão da geração solar fotovoltaica.
* Eventos como o apagão de outubro de 2025 e a Sexta-Feira Santa aparecem claramente como anomalias na série temporal.

---

### 03 — Temperatura × Consumo de Energia

**Dados:** ONS + INMET (dados meteorológicos horários).

Investigação da influência da temperatura sobre a carga elétrica, explorando efeitos de defasagem temporal, diferenças regionais, assimetrias entre calor e frio e variações ao longo do dia.

**Principais achados:**

* A maior correlação entre temperatura e consumo ocorre durante a madrugada, quando menos fatores externos interferem na demanda.
* A temperatura observada de 2 a 3 horas antes explica melhor a carga atual do que a temperatura instantânea, evidenciando a existência de inércia térmica.
* O impacto da temperatura varia entre os subsistemas: no Sul, dias excepcionalmente quentes podem reduzir o consumo ao diminuir a necessidade de aquecimento elétrico durante o inverno.
* Os resultados mostraram que a temperatura deve ser tratada como uma variável dinâmica, incorporando lags e interações com horário e dia da semana para representar adequadamente seu efeito sobre o consumo.

---

### 04 — Previsão de Carga com XGBoost

**Dados:** ONS + INMET + variáveis de calendário.

Construção de um modelo preditivo para estimar a carga horária do Sistema Interligado Nacional, comparando três abordagens: Regressão Linear, Seasonal Naive e XGBoost, utilizando validação temporal com TimeSeriesSplit.

**Principais achados:**

* O XGBoost alcançou **MAPE médio de 2,77%**, reduzindo significativamente o erro em relação aos modelos de referência.
* O Seasonal Naive mostrou que a sazonalidade semanal explica boa parte da demanda, mas apresenta limitações em feriados e eventos climáticos atípicos.
* A Regressão Linear melhorou o desempenho ao incorporar temperatura e calendário, mas não conseguiu capturar relações não lineares entre as variáveis.
* O XGBoost aprendeu essas interações automaticamente, tornando-se o modelo com melhor desempenho em todos os subsistemas.
* A análise de importância das variáveis confirmou os resultados obtidos nas etapas exploratórias: os lags de carga foram as variáveis mais relevantes, enquanto a temperatura defasada em três horas apresentou maior importância do que a temperatura instantânea, validando empiricamente o efeito de inércia térmica observado anteriormente.


## 🛠️ Stack Técnica

| Categoria | Ferramentas |
|-----------|------------|
| Linguagem | Python 3.11 |
| Manipulação | Pandas, NumPy |
| Séries Temporais | Statsmodels (STL, MSTL) |
| Visualização | Plotly, Seaborn, Matplotlib |
| Modelagem | Scikit-learn, XGBoost *(em construção)* |

---

## 📁 Fontes de Dados

- **EPE** — [Consumo de Energia Elétrica](https://www.epe.gov.br/pt/publicacoes-dados-abertos/publicacoes/consumo-de-energia-eletrica)
- **ONS** — [Curva de Carga Horária](https://dados.ons.org.br/dataset/curva-carga-ho)
- **INMET** — [Dados Meteorológicos](https://bdmep.inmet.gov.br/) *(notebook 03)*




---

