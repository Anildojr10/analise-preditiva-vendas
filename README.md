# Análise Preditiva de Vendas

Projeto de previsão de séries temporais de vendas com modelos ARIMA/SARIMA. Contém dois notebooks independentes: um para dados **mensais** (sazonalidade anual) e outro para dados **diários** (sazonalidade semanal).

## Notebooks

### `analise_preditiva.ipynb` — Previsão Mensal
Previsão das vendas para os **próximos 6 meses** a partir de uma série histórica mensal de 66 meses.

**Pipeline:**
1. Carregamento e análise exploratória (`vendas.csv`)
2. Decomposição da série (tendência, sazonalidade, resíduo)
3. Teste de estacionaridade — Dickey-Fuller Aumentado (ADF)
4. Diferenciação dupla para atingir estacionaridade (p-value: 0,75 → 0,000007)
5. Comparação de modelos: AR, MA, ARMA, ARIMA
6. Modelo final: **SARIMA(1,1,1)×(0,1,1,12)** — modelo airline
7. Diagnóstico com teste Ljung-Box
8. Forecast com intervalo de confiança 95%

---

### `previsao_vendas_diaria_semanal.ipynb` — Previsão Diária
Previsão das vendas para os **próximos 7 dias** a partir de 370 dias de histórico diário.

**Pipeline:**
1. Carregamento e limpeza de dados (`relatorio_vendas.csv`)
   - Conversão de formato numérico brasileiro (`"1.234,56"` → float)
   - Remoção de linhas de lixo geradas pelo Excel/Sheets (`12/1899`)
   - Interpolação linear para dias sem registro
2. Análise exploratória e decomposição (sazonalidade semanal, período = 7)
3. Teste ADF — série já estacionária (p-value = 0,0004)
4. Seleção automática de ordem pelo AIC entre candidatos
5. Modelo final: **SARIMAX(2,0,1)×(1,1,1,7)**
6. Diagnóstico com teste Ljung-Box
7. Forecast diário com intervalo de confiança 95%

---

## Dados

| Arquivo | Descrição | Frequência | Registros |
|---|---|---|---|
| `vendas.csv` | Série histórica de vendas mensais | Mensal | 66 meses |
| `relatorio_vendas.csv` | Relatório diário com vendas, cancelamentos e ticket médio | Diária | 370 dias |

---

## Dependências

```bash
pip install pandas numpy matplotlib seaborn statsmodels
```

O projeto já inclui um ambiente virtual (`venv/`). Para ativá-lo:

```bash
# Linux / WSL
source venv/bin/activate

# Windows
venv\Scripts\activate
```

Para executar os notebooks:

```bash
jupyter notebook
```
