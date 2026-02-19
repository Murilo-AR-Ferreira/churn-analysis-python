# churn-analysis-python
Análise de cancelamento de clientes utilizando Python e Pandas.

# pip install pandas, openpyxl, nbformat, ipykernel, plotly 
# Passo 1: Abrir a base de dados(Importar a base de dados) 
import pandas as pd #pd é um apelido pra biblioteca, só pra escrever menos 
tabela = pd.read_csv("cancelamentos.csv")

# Passo 2: Visualizar a base de dados *kaggle (site que disponibiliza base de dados pra treinar) 
# entender as informações disponíveis 
# possíveis problemas/erros na base de dados 
# Informações que não te ajudam, te atrapalham (Ex: CustomerID) tabela = tabela.drop(columns="CustomerID") #.drop remove oque você quer da tabela display(tabela) 

# Passo 3: Corrigir os problemas da base de dados (Tratamento de dados) 
display(tabela.info())
# Valores em formatos errados 
# Informações vazias -> como nesse caso tem somente 5 informações vazias, não ira atrapalhar na análise 
# Como excluir informações vazias; 
tabela = tabela.dropna() #dropNA seria os valores vazios 
display(tabela.info()) # Vai aparecer a tabela antes e depois do tratamento. 

# Passo 4: Análise inicial (entender quantos clientes cancelaram) 

# Quantidade - contar quantos são 0 e quantos são 1 na coluna "cancelou" 
print(tabela["cancelou"].value_counts()) 
# Porcentagem 
print(tabela["cancelou"].value_counts(normalize=True)) 
#display(tabela["cancelou"].value_counts(normalize=True).map(":.1%}".format)) --> Muda as casas decimais da porcentagem 

# Passo 5: Análise detalhada (causa do cancelamento dos clientes, como cada coluna impacta no cancelamento) 
import plotly.express as px #px é um apelido para escrever menos 
# cria o gráfico
grafico = px.histogram(tabela, x="duracao_contrato", color="cancelou", text_auto=True) 
# Exibe o grafico
grafico.show() 

# Resolvendo os problemas de cancelamento 

# Duração de contrato 
condicao = tabela["duracao_contrato"] != "Monthly" 
tabela = tabela[condicao] # Resolvendo a duração de contrato, a taxa de cancelamento cai de 56% para 46% 

# Ligações no CallCenter -> menores ou iguais a 4 
condicao = tabela["ligacoes_callcenter"] <= 4 
tabela = tabela[condicao] 
# Resolvendo o problema do cliente em no máximo 4 ligações, evita o cancelamento, sai de 46% para 26% 
display(tabela["cancelou"].value_counts(normalize=True)) 
# Todas os gráficos possíveis da comparação de cancelamento: 
import plotly.express as px #px é um apelido para escrever menos 
#comparações possíveis 
for coluna in tabela.columns: 
# cria o gráfico 
grafico = px.histogram(tabela, x=coluna, color="cancelou", text_auto=True) 
# Exibe o grafico 
grafico.show()
