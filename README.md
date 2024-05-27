#  Cotações das Ações da B3 (Bovespa) usando Python  #


import pandas as pd

# Define colunas e nomes
colspecs = [(2, 10), (10, 12), (12, 24), (27, 39), (56, 69), (69, 82), (82, 95), (108, 121), (152, 170), (170, 188)]
names = ['Data_Pregao', 'Cod_BDI', 'Sigla_Acao', 'Nome_Acao', 'Prec_Abertura', 'Prec_Max', 'Prec_Min', 'Prec_Fechamento', 'Qtd_Titulos_Negociados', 'Vol_Negocios']

# Lê o arquivo
df = pd.read_fwf('COTAHIST_A2024.TXT', colspecs=colspecs, names=names, skiprows=1)

# Filtra ações com código padrão
df = df[df['Cod_BDI'] == 2].drop(columns=['Cod_BDI'])

# Formata datas e preços
df['Data_Pregao'] = pd.to_datetime(df['Data_Pregao'], format='%Y%m%d')
df[['Prec_Abertura', 'Prec_Max', 'Prec_Min', 'Prec_Fechamento']] = df[['Prec_Abertura', 'Prec_Max', 'Prec_Min', 'Prec_Fechamento']].div(100).astype(float)

# Calcula média
df['Media'] = (df['Prec_Max'] + df['Prec_Min']) / 2

# Exibe o DataFrame
print(df)

# Salva o DataFrame
df.to_csv('cotacoes_b3.csv', index=False)
