# webscraping-no-python
## Importação da s bibliotecas Python
    import tweepy as tw
    import pandas as pd
    import time
    import sqlite3

## Criação das credenciais de acesso ao Twiter/X
    consumer_key = '95Y8qgif1AagKGXRTMqqSMA37'
    consumer_secret = 'T8idnCLkOizr7qgQ0MbqVeFJsmnrzsEfzwldMGQJs0SB556n5v'
    bearer_token = 'AAAAAAAAAAAAAAAAAAAAAL%2FDwgEAAAAAdq32EeD1udWP3wpz2h0uSCOHxmI%3D9M5Nu9t4d2GkvjJ7Wrxu8dmFLhwiX0p2SvyqASRBSVdW3auUvH'
    access_token = '1570038060439437312-FprLkQp127CQt27AhmMvfDgnJfjamH'
    access_token_secret = 'fm1LzrTFKDspDX9du6BCYMZjiGsd7F27P3PAX2EJgn8f6'

## Criação do Cliente para acessar o Twiter/X
    cliente = tw.Client(bearer_token,consumer_key, consumer_secret, access_token, access_token_secret)

## Se for exportar os dados para um banco de dados SQLite 
    ### Criação e Conexão com o banco de dados 
      con = sqlite3.connect('BD_scrapping.db')
    ### Criação do cursor (para acessar / inserir dados do bd)
      cur = con.cursor()
    ### Criação da tabela de dados
      cur.execute('CREATE TABLE registros (texto TEXT,RT TEXT)')
    ### Apagar todos os dados da tabela 'registros'
      cur.execute('DELETE FROM registros')
      
