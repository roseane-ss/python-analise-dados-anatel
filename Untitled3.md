```python
import pandas as pd
```


```python
# Definindo o caminho do arquivo CSV (ajuste conforme necessário)
arquivo_csv = r'C:\[...]\Anatel_Consumidor_Reclamacoes_CSV.csv'
```


```python
# Carregando os dados
try:
    df = pd.read_csv(arquivo_csv, encoding='utf-8', sep=';', low_memory=False)
except UnicodeDecodeError:
    df = pd.read_csv(arquivo_csv, encoding='latin1', sep=';', low_memory=False)
```


```python
# Exibindo as primeiras linhas
display(df.head())
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>TIPO_ATENDIMENTO</th>
      <th>SERVICO</th>
      <th>ASSUNTO</th>
      <th>UF</th>
      <th>PRESTADORA</th>
      <th>DATA_ABERTURA</th>
      <th>DATA_RESPOSTA</th>
      <th>RESPONDIDA</th>
      <th>REABERTA</th>
      <th>PrazoResposta</th>
      <th>ID_SITUACAO</th>
      <th>SITUACAO</th>
      <th>RESOLVIDA</th>
      <th>NOTA_CONSUMIDOR</th>
      <th>DATA_FINALIZACAO</th>
      <th>DETALHAMENTO</th>
      <th>ENTIDADEANDAMENTO</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Reclamação</td>
      <td>Banda Larga Fixa</td>
      <td>Cobrança</td>
      <td>SP</td>
      <td>CLARO</td>
      <td>2020-04-27</td>
      <td>2020-05-09</td>
      <td>S</td>
      <td>N</td>
      <td>12</td>
      <td>7</td>
      <td>Finalizada com avaliação</td>
      <td>N</td>
      <td>1</td>
      <td>2020-05-13</td>
      <td>A solicitação foi avaliada pelo {responsavel}</td>
      <td>Usuário WEB</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Reclamação</td>
      <td>Celular Pós-Pago</td>
      <td>Cancelamento</td>
      <td>PR</td>
      <td>TIM</td>
      <td>2019-12-17</td>
      <td>2019-12-27</td>
      <td>S</td>
      <td>N</td>
      <td>10</td>
      <td>6</td>
      <td>Finalizada sem avaliação</td>
      <td>Nulo</td>
      <td>Nulo</td>
      <td>2020-01-07</td>
      <td>A solicitação foi finalizada sem avaliação</td>
      <td>ANATEL</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Reclamação</td>
      <td>Celular Pós-Pago</td>
      <td>Dados cadastrais ou número da linha</td>
      <td>SP</td>
      <td>CLARO</td>
      <td>2023-11-27</td>
      <td>2023-11-29</td>
      <td>S</td>
      <td>N</td>
      <td>2</td>
      <td>6</td>
      <td>Finalizada sem avaliação</td>
      <td>Nulo</td>
      <td>Nulo</td>
      <td>2023-12-10</td>
      <td>A solicitação foi finalizada sem avaliação</td>
      <td>ANATEL</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Reclamação</td>
      <td>Telefone Fixo</td>
      <td>Cobrança</td>
      <td>MG</td>
      <td>CLARO</td>
      <td>2023-12-19</td>
      <td>2023-12-26</td>
      <td>S</td>
      <td>N</td>
      <td>7</td>
      <td>6</td>
      <td>Finalizada sem avaliação</td>
      <td>Nulo</td>
      <td>Nulo</td>
      <td>2024-01-06</td>
      <td>A solicitação foi finalizada sem avaliação</td>
      <td>ANATEL</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Reclamação</td>
      <td>Celular Pré-Pago</td>
      <td>Instalação ou Ativação ou Habilitação</td>
      <td>PE</td>
      <td>VIVO</td>
      <td>2025-01-05</td>
      <td>2025-01-07</td>
      <td>S</td>
      <td>N</td>
      <td>2</td>
      <td>7</td>
      <td>Finalizada com avaliação</td>
      <td>S</td>
      <td>5</td>
      <td>2025-01-07</td>
      <td>A solicitação foi avaliada pelo {responsavel}</td>
      <td>Usuário WEB</td>
    </tr>
  </tbody>
</table>
</div>



```python
# Convertendo colunas de datas
data_cols = ['DATA_ABERTURA', 'DATA_RESPOSTA', 'DATA_FINALIZACAO']
for col in data_cols:
    if col in df.columns:
        df[col] = pd.to_datetime(df[col], errors='coerce')
```


```python
# Garantindo que as colunas de datas foram convertidas corretamente
print(df.dtypes)  # Verificando os tipos de dados
```

    TIPO_ATENDIMENTO             object
    SERVICO                      object
    ASSUNTO                      object
    UF                           object
    PRESTADORA                   object
    DATA_ABERTURA        datetime64[ns]
    DATA_RESPOSTA        datetime64[ns]
    RESPONDIDA                   object
    REABERTA                     object
    PrazoResposta                 int64
    ID_SITUACAO                   int64
    SITUACAO                     object
    RESOLVIDA                    object
    NOTA_CONSUMIDOR              object
    DATA_FINALIZACAO     datetime64[ns]
    DETALHAMENTO                 object
    ENTIDADEANDAMENTO            object
    dtype: object
    


```python
# Convertendo a nota do consumidor para numérico
if 'NOTA_CONSUMIDOR' in df.columns:
    df['NOTA_CONSUMIDOR'] = pd.to_numeric(df['NOTA_CONSUMIDOR'], errors='coerce')
```


```python
# Corrigindo valores ausentes na coluna RESOLVIDA antes da conversão
if 'RESOLVIDA' in df.columns:
    df['RESOLVIDA'] = df['RESOLVIDA'].astype(str).str.strip().str.upper()  # Remove espaços extras e força maiúsculas
    df['RESOLVIDA'] = df['RESOLVIDA'].replace({'NULO': pd.NA, 'NULL': pd.NA, '': pd.NA})  # Normaliza valores ausentes
    df['RESOLVIDA'] = df['RESOLVIDA'].map({'S': 1, 'N': 0})  # Converte para 1 (resolvido) e 0 (não resolvido)
    df['RESOLVIDA'] = df['RESOLVIDA'].astype('Int64')  # Garante que os valores sejam inteiros corretos
```


```python
# Verificando os valores únicos da coluna RESOLVIDA
print("Valores únicos na coluna RESOLVIDA após conversão:")
print(df['RESOLVIDA'].value_counts(dropna=False))
```

    Valores únicos na coluna RESOLVIDA após conversão:
    RESOLVIDA
    <NA>    7076099
    1       2054221
    0        713535
    Name: count, dtype: Int64
    


```python
# Convertendo colunas de "S"/"N" para True/False
cols_booleanas = ['RESPONDIDA', 'REABERTA']
for col in cols_booleanas:
    if col in df.columns:
        df[col] = df[col].map({'S': True, 'N': False})
```


```python
# Calculando o Tempo de Resolução
if {'DATA_ABERTURA', 'DATA_RESPOSTA'}.issubset(df.columns):
    df['Tempo_Resolucao'] = (df['DATA_RESPOSTA'] - df['DATA_ABERTURA']).dt.days
    df['Tempo_Resolucao'] = df['Tempo_Resolucao'].fillna(0).astype(int)  # Trata valores NaN

```


```python
# Calculando a Taxa de Resolução considerando apenas valores válidos
if {'RESOLVIDA', 'TIPO_ATENDIMENTO'}.issubset(df.columns):
    total_reclamacoes = df['RESOLVIDA'].count()  # Considerando apenas valores informados (exclui NaN)
    total_resolvidas = df['RESOLVIDA'].sum()
    if total_reclamacoes > 0:
        taxa_resolucao = (total_resolvidas / total_reclamacoes) * 100
    else:
        taxa_resolucao = None  # Se não houver dados, evitar erro de divisão por zero
    print(f'Taxa de Resolução: {taxa_resolucao:.2f}%' if taxa_resolucao is not None else "Taxa de Resolução: Dados insuficientes")
```

    Taxa de Resolução: 74.22%
    


```python
# Salvando os dados tratados
arquivo_saida = 'dados_tratados_anatel.csv'
df.to_csv(arquivo_saida, index=False, encoding='utf-8', sep=';')

```


```python
print(f"Arquivo processado e salvo como {arquivo_saida}")
```

    Arquivo processado e salvo como dados_tratados_anatel.csv
    


```python
import os

arquivo_saida = 'dados_tratados_anatel.csv'
df.to_csv(arquivo_saida, index=False, encoding='utf-8', sep=';')

# Exibindo o caminho absoluto do arquivo salvo
caminho_absoluto = os.path.abspath(arquivo_saida)
print(f"Arquivo processado e salvo em: {caminho_absoluto}")

```

    Arquivo processado e salvo em: C:\[...]dados_tratados_anatel.csv
    


```python
# Identificando registros onde a DATA_ABERTURA é posterior à DATA_RESPOSTA
df_erro = df[df['DATA_ABERTURA'] > df['DATA_RESPOSTA']]

# Exibindo os primeiros registros errados
print("Registros com data invertida:")
print(df_erro[['DATA_ABERTURA', 'DATA_RESPOSTA', 'Tempo_Resolucao']])

```

    Registros com data invertida:
            DATA_ABERTURA DATA_RESPOSTA  Tempo_Resolucao
    128        2023-11-13    1900-01-01           -45241
    2072       2022-08-15    1900-01-01           -44786
    2942       2023-12-14    1900-01-01           -45272
    3065       2023-11-02    1900-01-01           -45230
    3398       2024-06-08    1900-01-01           -45449
    ...               ...           ...              ...
    9841962    2022-10-21    1900-01-01           -44853
    9842460    2023-09-11    1900-01-01           -45178
    9843056    2020-03-31    1900-01-01           -43919
    9843141    2020-03-18    1900-01-01           -43906
    9843574    2020-09-24    1900-01-01           -44096
    
    [21380 rows x 3 columns]
    


```python
# Corrigindo registros onde a DATA_RESPOSTA está errada (1900-01-01)
df.loc[df['DATA_RESPOSTA'] == "1900-01-01", 'DATA_RESPOSTA'] = pd.NaT

# Calculando o Tempo de Resolução corretamente
if {'DATA_ABERTURA', 'DATA_RESPOSTA'}.issubset(df.columns):
    df['Tempo_Resolucao'] = (df['DATA_RESPOSTA'] - df['DATA_ABERTURA']).dt.days
    df['Tempo_Resolucao'] = df['Tempo_Resolucao'].fillna(0).astype(int)

# Removendo valores negativos do Tempo de Resolução
df = df[df['Tempo_Resolucao'] >= 0]

```


```python
arquivo_saida = r'C:\[...]dados_tratados_anatel.csv'
df.to_csv(arquivo_saida, index=False, encoding='utf-8', sep=';')

print(f"Arquivo atualizado em: {arquivo_saida}")

```

    Arquivo atualizado em: C:\[...]\dados_tratados_anatel.csv
    


```python
# Listando os estados válidos do Brasil
ufs_validas = {"AC", "AL", "AM", "AP", "BA", "CE", "DF", "ES", "GO", "MA", 
               "MT", "MS", "MG", "PA", "PB", "PR", "PE", "PI", "RJ", "RN", 
               "RO", "RR", "RS", "SC", "SE", "SP", "TO"}

# Identificando UFs inválidas
ufs_invalidas = df[~df['UF'].isin(ufs_validas)]['UF'].unique()

# Exibindoo valores inválidos encontrados
print("UFs inválidas detectadas:", ufs_invalidas)

```

    UFs inválidas detectadas: [nan]
    


```python
df['UF'] = df['UF'].fillna('Desconhecido')
```


```python
arquivo_saida = r'C:[...]dados_tratados_anatel.csv'
df.to_csv(arquivo_saida, index=False, encoding='utf-8', sep=';')

print(f"Arquivo atualizado em: {arquivo_saida}")
```

    Arquivo atualizado em: C:[...]dados_tratados_anatel.csv
    


```python

```
