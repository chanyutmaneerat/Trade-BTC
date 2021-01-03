import ccxt
import json
import pandas as pd
import tkinter



# api and secret

apiKey = "API KEY"      
secret = "SECRET KEY"
subaccount = "GLADIATOR(B)"
password = "0"

# exchange detail
exchange = ccxt.ftx({
    'apiKey' : apiKey ,'secret' : secret ,
    'password' : password , 'enableRateLimit': True,
})

# Sub Account Check

if subaccount == "0":
  print("This is Main Account")
else:
  exchange.headers = {
   'ftx-SUBACCOUNT': subaccount,
  }

def getProduct():
    print("PRODUCT =",exchange.symbols)

def getPrice():
    pair = input("Pair:")
    r1 = json.dumps(exchange.fetch_ticker(pair))
    dataPrice = json.loads(r1)
    print(exchange)
    print(pair + '=',dataPrice['last'])

def getBalance():
  dfBalance = pd.DataFrame(exchange.fetch_balance(),columns=['USD','BTC','BTC-PERP','free','used','total'])
  dfBalanceList = dfBalance.values.tolist()
  print(dfBalance) 

def showPending(pair):
    print("Your Pending Order")
    df1 = pd.DataFrame(exchange.fetch_open_orders(pair),
                   columns=['id','datetime','status','symbol','type','side','price','amount','filled','average','remaining'])
    display(df1)

def showMatched(pair):
    print("Your Matched Order")
    df2 = pd.DataFrame(exchange.fetchMyTrades(pair),
                   columns=['id','datetime','status','symbol','type','side','price','amount','filled','average','remaining'])
    display(df2)



def sendOrder():
  pair = "BTC-PERP"                 # PRODUCT ที่เราต้องการเทรด ** # ไม่อยากใส่คู่ที่เทรดบ่อยๆ ใส่คู่ที่ต้องการเทรดแทน input เช่น pair = 'BTC-PERP'
  types = 'limit'                         # ประเภทของคำสั่ง
  side = input("buy or sell:")            # กำหนดฝั่ง BUY/SELL
  usd = float(input("Size-USD:"))         # กรณี Rebalance และต้องกรอกเป็น USD
  price = float(input("Price:"))          # ระดับราคาที่ต้องการ
  size_order = usd/price                  # ใส่ขนาดเป็น BTC, ถ้า Rebalance ให้ใส่เป็น usd/price # แล้วไปกรอกในตัวแปร usd แทน
  reduceOnly = False                      # ปิดโพซิชั่นเท่าจำนวนที่มีเท่านั้น (CREDIT : TY)
  postOnly =  False                       # วางโพซิชั่นเป็น MAKER เท่านั้น
  ioc = False                             # immidate or cancel เช่น ส่งคำสั่งไป Long 1000 market 
                                          # ถ้าไม่ได้ 1000 ก็ไม่เอา เช่นอาจจะเป็น 500 สองตัวก็ไม่เอา
  
  ## Send Order ##
  exchange.create_order(pair, types , side, size_order, price)

  ## Show Order Status##
  print("     ")
  showPending(pair)
  print("     ")
  showMatched(pair)


def cancelOrder():
    id = input("Id Number to Cancel:")
    try:
      exchange.cancel_order(id)
      print('Your Id No. '+ id + ' has been cancelled')
    except:
      print("Please Input Correct Id number")
    
 

def checkOrder():
    pair = "BTC-PERP"   # ไม่อยากใส่คู่ที่เทรดบ่อยๆ ใส่คู่ที่ต้องการเช็คแทน input เช่น pair = 'BTC-PERP'            
    try:
      showPending(pair)
      print("     ")
      showMatched(pair)

    except:
      print("please input correct pair")

getBalance()
