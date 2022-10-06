# Automacao
automatizando um grupo de funcionarios para descobrir quem bateu a meta 
import pandas as pd
from twilio.rest import Client

# Your Account SID from twilio.com/console
account_sid = "AC71d2331716090b0d88d39340d02459e8"
# Your Auth Token from twilio.com/console
auth_token  = "f5a1e3c3fdc86cdab58f6cd440f8f2ce"
client = Client(account_sid, auth_token)

# Abrir os 6 arquivos em Excel
lista_meses = ['janeiro', 'fevereiro', 'março', 'abril', 'maio', 'junho']

for mes in lista_meses:
    tabela_vendas = pd.read_excel(f'{mes}.xlsx')
    if (tabela_vendas ['Vendas'] > 55000).any(): 
        vendedor = tabela_vendas.loc[tabela_vendas ['Vendas'] > 55000, 'Vendedor'].values[0]
        vendas = tabela_vendas.loc[tabela_vendas ['Vendas'] > 55000, 'Vendas'].values[0]
        print(f'No mês {mes} alguém bateu a meta. Vendedor:{vendedor}, Vendas{vendas}')
        message = client.messages.create(
           to="+5561994630856", 
           from_="+17069199480",
           body=f'No mês {mes} alguém bateu a meta. Vendedor:{vendedor}, Vendas{vendas}')
        print(message.sid)
