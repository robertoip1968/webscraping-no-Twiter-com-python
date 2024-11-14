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

## Criação das credenciais de acesso ao Twiter/X
    consumer_key = 'sua chave de acesso do twiter/X'
    consumer_secret = 'sua chave secreta do twiter/X'
    bearer_token = 'seu bearer token do twiter/X'
    access_token = 'seu access token do twiter/X'
    access_token_secret = 'seu access token secreto do twiter/X'

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

## Exportação dos dados para um banco de dados SQLite 
    ### Criação e Conexão com o banco de dados 
      con = sqlite3.connect('BD_scrapping.db')
    ### Criação do cursor (para acessar / inserir dados do bd)
      cur = con.cursor()
    ### Exclusão da tabela 'registros' se ela existir
      cur.execute("DROP TABLE IF EXISTS registros")
    ### Criação da tabela de dados
      cur.execute('CREATE TABLE registros (Texto_Tweet TEXT,Retweet TEXT)')
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

        cur.execute("INSERT INTO registros (Texto_Tweet,Retweet) VALUES (?,?)",(texto,RT))

    con.commit()
    
## Exportação dos dados  para Excel
    resultados.to_excel('resultados_twitter.xlsx', index=False) 

## Apresentação do Total de Retweets (RT = "S") e Total de Tweets Originais (RT = "N")

    # Contar o total de retweets e tweets originais
    total_retweets = resultados[resultados['Retweet'] == 'S'].shape[0]
    total_tweets_originais = resultados[resultados['Retweet'] == 'N'].shape[0]

    # Imprimir os totais
    print(f"Total de Retweets (Retweet = 'S'): {total_retweets}")
    print(f"Total de Tweets Originais (Retweet = 'N'): {total_tweets_originais}")

## Apresentação de quantas vezes aparecem as Palavras e o percentual (Trump, bolsonaro, kamala)

    # Contar a frequência das palavras "Trump", "Bolsonaro" e "Kamala"
    trump_count = resultados['Texto_Tweet'].str.contains('Trump', case=False).sum()
    bolsonaro_count = resultados['Texto_Tweet'].str.contains('Bolsonaro', case=False).sum()
    kamala_count = resultados['Texto_Tweet'].str.contains('Kamala', case=False).sum()

    # Calcular o percentual de cada palavra em relação ao total de tweets
    total_tweets = len(resultados)
    trump_percentage = (trump_count / total_tweets) * 100 if total_tweets > 0 else 0
    bolsonaro_percentage = (bolsonaro_count / total_tweets) * 100 if total_tweets > 0 else 0
    kamala_percentage = (kamala_count / total_tweets) * 100 if total_tweets > 0 else 0

    # Imprimir os resultados
    print(f"Frequência de 'Trump': {trump_count} ({trump_percentage:.2f}%)")
    print(f"Frequência de 'Bolsonaro': {bolsonaro_count} ({bolsonaro_percentage:.2f}%)")
    print(f"Frequência de 'Kamala': {kamala_count} ({kamala_percentage:.2f}%)")

