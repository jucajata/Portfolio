# import libraries

from bs4 import BeautifulSoup
import requests
import time
import datetime
import smtplib
import pandas as pd
import csv

def check_price():
    # connect to website

    URL = 'https://www.amazon.com/-/es/Camisetas-Witches-Camiseta-divertida-Halloween/dp/B07FKZCMBN/ref=sr_1_6?_encoding=UTF8&c=ts&dchild=1&keywords=new+in+shirts+and+t-shirts+for+women&qid=1630457767&s=apparel&sr=1-6&ts_id=9056922011'

    #headers: http://httpbin.org/get
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.159 Safari/537.36", "Accept-Encoding":"gzip, deflate", "Accept":"text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "DNT":"1","Connection":"close", "Upgrade-Insecure-Requests":"1"}

    page = requests.get(URL, headers=headers)

    soup1 = BeautifulSoup(page.content,"html.parser")
    soup2 = BeautifulSoup(soup1.prettify(),"html.parser")

    # Buscar el id del elemento de la pag web que se quiere obtener
    title = soup2.find(id='productTitle').get_text()

    #Precio se obtiene de esta forma ya que al hacerlo como "title" se obtiene un dato None
    price = soup2.find(id='priceblock_ourprice') or "abc"
    price = str(price.get_text())
    
    title = title.strip()
    
    price = price.split()
    prices = []

    for p in price:
        try:
            a = float(p)
            prices.append(a)
        except:
            continue
    prices

    min_price = prices[0]
    max_price = prices[-1]

    today = datetime.date.today()

    header = ['Date','title','min_price','max_price']
    data = [today,title,min_price,max_price]

    with open('AmazonWebScraperDataset.csv', 'a+', newline='', encoding='utf-8') as f:
        writer = csv.writer(f)
        writer.writerow(data)
    
    return max_price

        
def send_mail():
    server = smtplib.SMTP_SSL('smtp.gmail.com',465)
    server.ehlo()
    #server.starttls()
    server.ehlo()
    server.login('xxxxxxxxxx@gmail.com','******************')
    
    subject = "La camisa que quieres está por debajo de los $20! Es el momento de comprarla!"
    body = "Juan,\n\nEste es el momento que has estado esperando.\n\nAhora es el momento de llevarte la camisa de tus sueños.\n\nNo te lo pierdas!\n\nLink: https://www.amazon.com/-/es/Camisetas-Witches-Camiseta-divertida-Halloween/dp/B07FKZCMBN/ref=sr_1_6?_encoding=UTF8&c=ts&dchild=1&keywords=new+in+shirts+and+t-shirts+for+women&qid=1630457767&s=apparel&sr=1-6&ts_id=9056922011"
   
    msg = f"Subject: {subject}\n\n{body}"

    server.sendmail('xxxxxxxxxx@gmail.com','xxxxxxxxxx@gmail.com',msg.encode("utf8"))
        
# Runs check_price after a set time and inputs data into your CSV
max_price = check_price()

df = pd.read_csv(r'AmazonWebScraperDataset.csv')

    
if float(max_price) < 20:
    send_mail()
    
df
