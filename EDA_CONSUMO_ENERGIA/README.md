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
**Período:** ~20 anos de consumo mensal por segmento e região

Análise de longo prazo do consumo de energia elétrica no Brasil. Aplica decomposição STL para separar tendência, sazonalidade e resíduo — usando os resíduos como detector de eventos históricos.

**Principais achados:**
- O Brasil não tem uma sazonalidade única: Sul/Sudeste consomem mais no verão; Norte/Nordeste, no segundo semestre
- A crise de 2008 afetou quase exclusivamente o setor industrial; o residencial mal sentiu
- O embargo ambiental da **Alunorte (2018)** derrubou o consumo do Pará em ~20% por mais de um ano — visível na tendência de toda a Região Norte
- O COVID (2020) aparece sincronizado em todas as regiões, com o setor comercial registrando a maior queda da série histórica

---

### 02 — Carga Horária do SIN (2019–2026)
**Dados:** ONS — Operador Nacional do Sistema Elétrico  
**Período:** Dados horários dos quatro subsistemas (Norte, Nordeste, Sudeste, Sul)

Análise da dinâmica intradiária do Sistema Interligado Nacional. O foco não é *quanto* se consome, mas *quando* — e como esse padrão varia por região, dia da semana, mês e ano.

**Principais achados:**
- O vale ocorre entre 3h–5h (sistema no limite mínimo); o pico entre 18h–19h (indústria + residencial simultâneos)
- A dispersão do consumo é máxima entre 10h–16h — o horário mais difícil de prever
- Dia útil e fim de semana produzem dois sistemas diferentes: o platô industrial desaparece no domingo, mas o pico noturno persiste
- O **Nordeste** mostra a *duck curve* se aprofundando ano a ano (2019→2026): assinatura direta da expansão solar fotovoltaica
- O **apagão de outubro de 2025** (subestação Bateias/PR) e a **Sexta-Feira Santa** aparecem como anomalias claras no z-score do resíduo

---

### 03 — Correlação Temperatura × Consumo 
**Dados:** ONS + INMET (dados meteorológicos horários)

Quantificação da relação entre temperatura e consumo de carga. A análise vai além do óbvio — explorando assimetria da resposta (calor vs. frio), threshold de temperatura, defasagem temporal e variação do coeficiente de correlação por hora do dia e por região.

---

### 04 — Modelo Preditivo XGBoost *
**Dados:** ONS + INMET + variáveis de calendário

Previsão de carga horária com XGBoost, comparando três abordagens: regressão linear (baseline), Seasonal Naive e XGBoost com features de calendário, lags e temperatura. Validação com TimeSeriesSplit para garantir integridade temporal.

---

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

