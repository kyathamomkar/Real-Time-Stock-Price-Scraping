# importing the libraries
from bs4 import BeautifulSoup
import requests
import parser
import csv
import os

# Step 1: Sending a HTTP request to a URL
url = "https://money.cnn.com/data/hotstocks/"
# Make a GET request to fetch the raw HTML content
html_content = requests.get(url).text

url2 = "https://finance.yahoo.com/quote/"

print("Which Stock are you interested in:")
print("\nMost Actives:")
# Step 2: Parse the html content
soup = BeautifulSoup(html_content, "lxml")
# print(soup.prettify()) # print the parsed data of html
global str
if os.path.exists("stocks.csv"):
  os.remove("stocks.csv")

# Step 3: Analyze the HTML tag, where your content lives
# Create a data dictionary to store the data.
data = {}
#Get the table having the class wikitable
#gdp_table = soup.find("a", attrs={"class": "wsod_symbol"})
#gdp_table_data = gdp_table.  # contains 2 rows
counter=0
counter2=1
category ="Most Actives"
companyname =""
openvalue =""
previousclose=""
volume=""
marketcap =""
ignore=True
with open('stocks.csv', 'w', newline='') as file:
    writer = csv.writer(file)
    writer.writerow(["category", "ticker", "name","open","previousclose","volume","marketcap"])
    for link in soup.find_all("a", attrs={"class": "wsod_symbol"}):
        if counter>=3:
          html_content2 = requests.get(url2+link.text).text
          soup2 = BeautifulSoup(html_content2,"lxml")
          for link2 in soup2.find_all("h1", attrs={"class": "D(ib)"}):
            companyname = link2.text
            s= companyname.split(" - ")
            companyname =s[1]
          for link3 in soup2.find("td", attrs={"data-test":"OPEN-value"}):
            openvalue = link3.text
          for link4 in soup2.find("td", attrs={"data-test":"PREV_CLOSE-value"}):
            previousclose = link4.text
          for link5 in soup2.find("td", attrs={"data-test":"TD_VOLUME-value"}):
            volume = link5.text
            volume= volume.replace(",","")
          for link6 in soup2.find("td", attrs={"data-test":"MARKET_CAP-value"}):
            marketcap = link6.text

            print(link.text+" "+companyname)
            writer.writerow([category, link.text, companyname,openvalue,previousclose,volume,marketcap])
      
          counter2=counter2+1
        counter=counter+1
    
        if counter2==11:
          category = "Gainers"
          print("\n\n"+category+":")
        elif counter2==21:
          category = "Losers"
          print("\n\n"+category+":")
while(1):
  choice = input("\n\nEnter your choice : ")
  print("\nThe data for "+choice+" is the following:")
  with open('stocks.csv', 'r') as file:
      csv_file_reader = csv.DictReader(file)
      for row in csv_file_reader:
       
          if(row['ticker']==choice):
            print(row['ticker']+" "+row['name'])
            print("OPEN: "+row['open'])
            print("PREV CLOSE: "+row['previousclose'])
            print("VOLUME: "+f"{int(row['volume']):,}")
            print("MARKET CAP: "+row['marketcap'])
