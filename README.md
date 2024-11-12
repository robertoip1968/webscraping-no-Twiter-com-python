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

## Definição dos filtros a serem aplicados
    start = '2024-11-06T08:00:00Z'
    end = '2024-11-07T16:00:00Z'

    resposta = cliente.search_recent_tweets(query='eleição',max_results=100,start_time=start,end_time=end)
    dados = resposta.data
    type(dados)
## Apresentação dos dados coletados em colunas
    if dados:
      tweet_data = []
      for tweet in dados:
        text = tweet.text
        is_retweet = 'Sim' if 'RT @' in text else 'Não'
        tweet_data.append([text, is_retweet])

      ### Criando um DataFrame do Pandas
      df = pd.DataFrame(tweet_data, columns=['Texto do Tweet', 'É Retweet?'])
      print(df)
    else:
      print("Nenhum tweet encontrado.")

## Se for exportar os dados para um banco de dados SQLite 
    ### Criação e Conexão com o banco de dados 
      con = sqlite3.connect('BD_scrapping.db')
    ### Criação do cursor (para acessar / inserir dados do bd)
      cur = con.cursor()
    ### Criação da tabela de dados
      cur.execute('CREATE TABLE registros (texto TEXT,RT TEXT)')
    ### Apagar todos os dados da tabela 'registros'
      cur.execute('DELETE FROM registros')
      con.commit()
## inserção dos dados obtidos na tabela "registros"
    for i in dados:
        texto = i.text
        if (texto[:2] == 'RT'):
            RT = 'S'
        else:
            RT = 'N'

        cur.execute("INSERT INTO registros (texto,RT) VALUES (?,?)",(texto,RT))

    con.commit()
      
