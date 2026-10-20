# 📈 Cheat Sheet — Gráficos a partir dos Datasets

Referência rápida dos **9 gráficos** gerados a partir dos arquivos em [`dados/`](dados/). Cada gráfico possui **um script independente** em [`scripts/`](scripts/) e salva a imagem em [`graficos/`](graficos/).

> Os arquivos `dados_dispersao.csv` e `dados_dispersao (1).csv` enviados eram idênticos; foi mantida uma única cópia.

---

## 🗂️ Visão geral

| # | Gráfico | Script | Dataset | Colunas usadas | Função principal |
| :-: | :--- | :--- | :--- | :--- | :--- |
| 1 | Linhas | `scripts/grafico_linhas.py` | `dados_linhas.csv` | `data`, `vendas`, `loja` | `sns.lineplot()` |
| 2 | Barras agrupadas | `scripts/grafico_barras.py` | `dados_barras.csv` | `categoria`, `vendas_medias`, `regiao` | `sns.barplot()` |
| 3 | Frequência | `scripts/grafico_frequencia.py` | `dados_frequencia.csv` | `tipo_atendimento` | `sns.countplot()` |
| 4 | Dispersão | `scripts/grafico_dispersao.py` | `dados_dispersao.csv` | `altura_cm`, `peso_kg`, `sexo` | `sns.scatterplot()` |
| 5 | Regressão | `scripts/grafico_regressao.py` | `dados_regressao.csv` | `renda_mensal`, `gasto_mensal` | `sns.regplot()` |
| 6 | Histograma + KDE | `scripts/grafico_distribuicao.py` | `dados_distribuicao.csv` | `idade` | `sns.histplot()` |
| 7 | Boxplot | `scripts/grafico_boxplot.py` | `dados_boxplot.csv` | `departamento`, `salario` | `sns.boxplot()` |
| 8 | Heatmap | `scripts/grafico_heatmap.py` | `dados_heatmap.csv` | `dia_semana`, `horario`, `movimento_clientes` | `sns.heatmap()` |
| 9 | Pizza / Rosca | `scripts/grafico_pizza.py` | `dados_pizza.csv` | `canal_venda`, `faturamento` | `plt.pie()` |

---

## ▶️ Como executar

```bash
pip install -r requirements.txt
python scripts/grafico_linhas.py        # exemplo: gera graficos/grafico_linhas.png
```

Os scripts usam caminhos relativos à raiz do repositório, então podem ser executados de qualquer diretório.

---

## 🔎 Detalhes por gráfico

### 1. Linhas — evolução das vendas por loja
```python
sns.lineplot(data=df, x="data", y="vendas", hue="loja", marker="o", ax=ax)
```
- `parse_dates=["data"]` na leitura garante eixo temporal correto.
- `hue` cria uma linha por loja.
- ℹ️ Cada dia possui registro de apenas uma loja; por isso as linhas têm "saltos" entre dias.

### 2. Barras — vendas médias por categoria e região
```python
sns.barplot(data=df, x="categoria", y="vendas_medias", hue="regiao", errorbar=None, ax=ax)
```
- Há 1 valor por combinação categoria × região, então `errorbar=None`.

### 3. Frequência — chamados por tipo de atendimento
```python
sns.countplot(data=df, x="tipo_atendimento", order=ordem, ax=ax)
```
- `order=value_counts().index` ordena da maior para a menor frequência.
- `ax.bar_label()` exibe a contagem sobre cada barra.

### 4. Dispersão — altura × peso
```python
sns.scatterplot(data=df, x="altura_cm", y="peso_kg", hue="sexo", style="sexo", ax=ax)
```

### 5. Regressão — renda × gasto
```python
sns.regplot(data=df, x="renda_mensal", y="gasto_mensal", line_kws={"color": "red"}, ax=ax)
```
- `np.polyfit` calcula coeficientes; o R² é exibido no gráfico.

### 6. Histograma + KDE — idade dos clientes
```python
sns.histplot(data=df, x="idade", bins=15, kde=True, ax=ax)
```
- Linhas verticais marcam média e mediana.

### 7. Boxplot — salário por departamento
```python
sns.boxplot(data=df, x="departamento", y="salario", hue="departamento", palette="Pastel1", legend=False, ax=ax)
```

### 8. Heatmap — movimento por dia × horário
```python
matriz = df.pivot(index="dia_semana", columns="horario", values="movimento_clientes").reindex(ordem_dias)
sns.heatmap(matriz, annot=True, fmt="d", cmap="YlOrRd", ax=ax)
```
- `pivot` transforma o formato longo em matriz; `reindex` garante a ordem Segunda → Domingo.

### 9. Pizza / Rosca — faturamento por canal
```python
ax.pie(df["faturamento"], labels=df["canal_venda"], autopct="%1.1f%%", wedgeprops={"width": 0.45})
```
- `wedgeprops={"width": ...}` transforma a pizza em rosca.

---

## 🧰 Boas práticas aplicadas

| Prática | Como |
| :--- | :--- |
| Um script por gráfico | Cada arquivo em `scripts/` é autônomo |
| Caminhos portáveis | `Path(__file__).resolve().parent.parent` |
| Imagem de alta qualidade | `fig.savefig(..., dpi=150)` |
| Layout sem cortes | `fig.tight_layout()` |
| Título e rótulos claros | `set_title`, `set_xlabel`, `set_ylabel` |

Veja também: [`matplotlib_e_seaborn.md`](matplotlib_e_seaborn.md) e [`parametros_graficos.md`](parametros_graficos.md).
