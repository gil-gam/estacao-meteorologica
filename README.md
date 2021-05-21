#código em python

import dht
import machine
import time
r = machine.Pin(2, machine.Pin.OUT)
r.value(1)
d = dht.DHT11(machine.Pin(4))

#-------------------------------------------------

def conecta(ssid, senha):
import network
import time
station = network.WLAN(network.STA_IF)
station.active(True)
station.connect(ssid, senha)
for t in range(50):
  if station.isconnected():
    break
  time.sleep(0.1)
return station

#-------------------------------------------------

conecta
import urequests
import time
while True:
  d.measure()
  if d.temperature() > 31 or d.humidity() > 70:
    print("Temp={} Umid={}".format(d.temperature(), d.humidity()))
    print("Conectando...")
    station = conecta("inserir o nome da rede wi-fi aqui", "inserir a senha do wi-fi aqui")
    if not station.isconnected():
      print("Não conectado!")
    else:
      print("Conectado!...")
      print("Acessando o site...")
      resposta = urequests.get("https://api.thingspeak.com/update?api_key=SM88TV7J9RRMYBO2&field1={}&field2={}".format(d.temperature(), d.humidity()))
      print(resposta.text)
      station.disconnect()
      time.sleep(5)
   else:
    r.value(0)
    break
