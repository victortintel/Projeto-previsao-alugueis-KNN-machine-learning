# Previsão de Valores de Aluguel (k-NN Regression)

**Autor:** Victor Tintel  
**Notebook:** `PrevendoValoresAlugueis.ipynb`  
**Dados:** Kaggle — *Apartment Rent Data* (link no notebook)

---

## 🎯 Objetivo
Construir um pipeline completo de **Ciência de Dados para regressão** a fim de **estimar o valor de aluguel** anunciado com base em características do imóvel, localização e atributos do anúncio.

---

## 🧩 Pergunta de negócio
> Dado um anúncio de imóvel para locação, **quanto esse aluguel deveria custar** considerando estrutura do imóvel, comodidades, estado/cidade, tempo do anúncio e demais atributos textuais/categóricos?

---

## 🗂️ Dados
- **Fonte:** Kaggle – *apartment-rent-data* (conforme link original no notebook).  
- **Arquivo de trabalho:** `apartments_for_rent_classified_100K.csv` (separador `;`, encoding `ISO-8859-1`).  
- **Exemplos de colunas** (renomeadas para PT-BR no notebook):  
  `ID_Imovel`, `Categoria`, `Titulo`, `Descricao`, `Comodidades`, `Banheiros`, `Quartos`, `Moeda`, `Taxa`, `Foto_Anuncio`, `Permite_Pets`, `VL_Aluguel`, `VL_Aluguel_Exibido`, `Tipo_Aluguel`, `Tamanho`, `Endereco`, `Cidade`, `Estado`, `Latitude`, `Longitude`, `Imobiliaria`, `Inclusao` (timestamp).  
- **Variável alvo (target):** `VL_Aluguel`.

---

## 🔬 EDA (Análise Exploratória)
- Distribuições de variáveis numéricas (ex.: `Tamanho`, `VL_Aluguel`) com histogramas e boxplots.
- Cardinalidade e perfil de categóricas (ex.: `Estado`, `Imobiliaria`, `Permite_Pets`).
- Contagens por `Estado` e por `Imobiliaria` (ranking).
- Exemplos de cruzamentos úteis:  
  - Média de `VL_Aluguel` por Estado  
  - Combinação de amenidades (`Piscina` & `Academia`)

---

## 🏗️ Engenharia de Atributos
- **Comodidades → variáveis binárias:** mapeadas do inglês para PT-BR e extraídas de `Comodidades`, gerando colunas como `Academia`, `Piscina`, `Estacionamento`, etc.
- **Datas:** a partir de `Inclusao`, extração de `Data_Inclusao`, `Hora`, `Ano`.
- **Tempo de anúncio:** criação de `Dias_Anuncio` e `Anos_Anuncio` usando `datetime.now()`.
- **Normalização de categorias:** `Permite_Pets` consolidado em `Sim/Não`.
- **Limpeza/Drop de colunas auxiliares** após derivação (ex.: `Comodidades`, `Inclusao`, `Data_Inclusao`, etc.).
- **Exportação de subset tratado:** `dados_tratados.csv` contendo colunas como:  
  `Banheiros`, `Quartos`, `Permite_Pets`, `Tamanho`, `Academia`, `Piscina`, `Estacionamento`, `Estado`, `Imobiliaria`, `Dias_Anuncio`, `VL_Aluguel`, `Latitude`, `Longitude`.

---

## 🧱 Preparação dos Dados
- **One-Hot Encoding** das variáveis categóricas nominais (ex.: `Estado`, `Imobiliaria`, …).
- **Split Treino/Teste:** 70% / 30% com `random_state=42`.
- **Padronização (StandardScaler)** das features numéricas antes do k-NN.

---

## 🤖 Modelagem
- **Algoritmo:** `KNeighborsRegressor` (k-NN para regressão).
- **Tuning:** varredura de `n_neighbors` (1 → 20) com comparação de **MAPE** em treino e teste.  
  Inclui gráfico no notebook para visualizar o viés/variância ao variar `k`.
- **Métricas reportadas:**
  - **MAE** (Mean Absolute Error)
  - **MAPE** (Mean Absolute Percentage Error) — métrica principal para interpretar erro relativo
  - **RMSE** (Root Mean Squared Error)
  - **LSE** (Least Squares Error) — análise auxiliar

> Observação: o notebook mostra o fluxo de seleção do `k` com base no menor **MAPE** em teste (com estabilidade entre treino/teste).  
> O valor de `k` usado como baseline no código é `n_neighbors=10`, e há uma célula de loop para comparar vários `k` e imprimir o erro.

---

## ✅ Resultados (o que o notebook entrega)
- Pipeline completo **reprodutível**: leitura → EDA → engenharia → preparação → tuning → avaliação.
- Visualizações para **entender dados e hiperparâmetros** (histogramas, boxplots, barplots, curva do erro vs. `k`).
- Tabelas auxiliares (ex.: `df_avaliacao` com `Valor_Real`, `Valor_Previsto` e colunas de erro).

> **Importante:** como o dataset bruto não está versionado no repositório, os valores exatos das métricas dependem da execução local com o arquivo do Kaggle.  
> O README descreve o processo e as métricas calculadas; rode o notebook para ver os números na sua máquina.
