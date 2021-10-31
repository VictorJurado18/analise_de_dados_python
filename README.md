# Análise de Dados com Python

Desafio:
Você trabalha em uma empresa de telecom e tem clientes de vários serviços diferentes, entre os principais: internet e telefone.

O problema é que, analisando o histórico dos clientes dos últimos anos, você percebeu que a empresa está com Churn de mais de 26% dos clientes.

Isso representa uma perda de milhões para a empresa.

O que a empresa precisa fazer para resolver isso?

Base de dados do Kaggle: https://www.kaggle.com/radmirzosimov/telecom-users-dataset

## :memo: Pré-requisitos:
- Instalar biblioteca plotly:  ```pip install plotly```

## :snake: Executando os scripts:

#passo 1: Importar a base de dados para o Python
```python

import pandas as pd
tabela = pd.read_csv("telecom_users.csv")
display(tabela)
```
#passo 2: Visualizar a base de dados
```python

#Entender quais informações você tem disponivel.
#Descobrir os erros da base de dados.

tabela = tabela.drop("Unnamed: 0",axis=1) #eixo:0 linha eixo:1 coluna
display(tabela)
```
#passo 3: tratamento da base de dados.
```python

#Valores que são numeros mas o python entende como texto.
tabela["TotalGasto"] = pd.to_numeric(tabela["TotalGasto"], errors="coerce") #coerce pega os erros da conversão e exclui
#Valores que estão vazios.

#colunas vazias 
tabela = tabela.dropna(how = "all" , axis = 1 ) 
# all quando vc quer excluir colunas COMPLETAMENTE vazias
# any quando vc quer excluir colunas que tem PELO MENOS 1 valor vazio

#linhas vazias
tabela = tabela.dropna(how = "any" , axis = 0 ) 
#Excluir Informações inuteis

print(tabela.info())
```
:clipboard:- Saida esperada : Tratamos os dados da tabela e limpamos os valores nulos.
```
<class 'pandas.core.frame.DataFrame'>
Int64Index: 5974 entries, 0 to 5985
Data columns (total 21 columns):
 #   Column                  Non-Null Count  Dtype  
---  ------                  --------------  -----  
 0   IDCliente               5974 non-null   object 
 1   Genero                  5974 non-null   object 
 2   Aposentado              5974 non-null   int64  
 3   Casado                  5974 non-null   object 
 4   Dependentes             5974 non-null   object 
 5   MesesComoCliente        5974 non-null   int64  
 6   ServicoTelefone         5974 non-null   object 
 7   MultiplasLinhas         5974 non-null   object 
 8   ServicoInternet         5974 non-null   object 
 9   ServicoSegurancaOnline  5974 non-null   object 
 10  ServicoBackupOnline     5974 non-null   object 
 11  ProtecaoEquipamento     5974 non-null   object 
 12  ServicoSuporteTecnico   5974 non-null   object 
 13  ServicoStreamingTV      5974 non-null   object 
 14  ServicoFilmes           5974 non-null   object 
 15  TipoContrato            5974 non-null   object 
 16  FaturaDigital           5974 non-null   object 
 17  FormaPagamento          5974 non-null   object 
 18  ValorMensal             5974 non-null   float64
 19  TotalGasto              5974 non-null   float64
 20  Churn                   5974 non-null   object 
dtypes: float64(2), int64(2), object(17)
memory usage: 1.0+ MB
None

```
#passo 4: Análise exploratória -> Análise geral -> Ver como estão os cancelamentos
```python


display(tabela["Churn"].value_counts())
display(tabela["Churn"].value_counts(normalize=True).map("{:.1%}".format))
```
:clipboard:- Saida esperada : confirmando que a taxa de cancelamento de serviços é de 26%
```
Nao    4387
Sim    1587
Name: Churn, dtype: int64
Nao    73.4%
Sim    26.6%
Name: Churn, dtype: object
```

#passo 5: Olhando as colunas da base de dados -> identificar o motivo dos clientes cancelarem
```python
import plotly.express as px

#para edições nos graficos: https://plotly.com/python/histograms/

#gerar um grafico comparativo com os cancelamentos de cada coluna da tabela.
for coluna in tabela.columns: 
    print(coluna)
    grafico = px.histogram(tabela, x=coluna, color="Churn")
    grafico.show()
```

## :chart_with_upwards_trend: Conclusão com base nos gráficos:
```
- Clientes com familias maiores(casados e com dependentes) tendem a cancelar menos.

- MesesComoCliente baixo tem MUITO mais chance de cancelar.
    - talvez a primeira experiencia do cliente tenha sido ruim.
    - talvez a gente esteja trazendo clientes desqualificados.
    - criar um incentivo ao cliente nos primeiros meses

- Cliente com internet de Fibra tendem a cancelar mais.
    - É possivel que exista algum problema com o serviço de fibra.

- Quanto mais serviços o cliente tem, menor a chance dele cancelar.
    - Criar um programa de incentivo aos outros serviços

- Clientes com contrato mensal cancelam muito mais:
    - Incentivar contrato anual ou 2 anos

- Clientes que pagam com boleto eletronico cancelam mais.
    - Evitar pagamentos com boleto eletronico.
```


Desafio feito durante treinamento da Hashtag Programação...
