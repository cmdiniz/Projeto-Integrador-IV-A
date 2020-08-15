## Projeto-Integrador-IV-A

### Equipe

### Primeiro Encontro - 15/08/2020

#### Introdução à IoT:
* [Slides](http://olaria.ucpel.edu.br/materiais/lib/exe/fetch.php?media=internet_das_coisas_piv.pdf)
* [IoT Comic Book](https://iotcomicbook.org/)
* [Fog & Cloud](http://olaria.ucpel.edu.br/materiais/lib/exe/fetch.php?media=apresentacao-pi-iv.pdf)


#### Transmitindo Informações Sensoriadas do Meio para um Servidor
  * Conceitos
    * [Protocolo MQTT - Material IBM](https://www.ibm.com/developerworks/br/library/iot-mqtt-why-good-for-iot/index.html)
    * [Protocolo MQTT - Material Curto Circuito](https://www.curtocircuito.com.br/blog/introducao-ao-mqtt/)
    * [Slides sobre MQTT - Material UFC](https://pt.slideshare.net/MaurcioMoreiraNeto/protocolo-mqtt-redes-de-computadores)
  * Plataformas de Software
    * [Mosquitto da Eclipse Foundation](https://mosquitto.org)
    * [Brokers MQTT gratuitos e pagos para utilizar em projetos da IoT](https://mntolia.com/10-free-public-private-mqtt-brokers-for-testing-prototyping/)
    * [Explorando o uso de MQTT em Programas Python](https://fazbe.github.io/Usando-o-paho-mqtt-para-Python/)

#### Comunicando com um Broker MQTT utilizando Python

##### Procedimento de Subscrição
~~~
# Cliente Python para subscrever em um Broker MQTT
#
# Para instalar o paho-mqtt use o comando pip install paho-mqtt
import paho.mqtt.client as mqtt

# Retorno quando um cliente recebe um  CONNACK do Broker, confirmando a subscricao
def on_connect(client, userdata, flags, rc):
    print("Conectado, com o seguinte retorno do Broker: "+str(rc))

    # O subscribe fica no on_connect pois, caso perca a conexão ele a renova
    # Lembrando que quando usado o #, você está falando que tudo que chegar após a barra do topico, será recebido
    client.subscribe("PI-3A/#")

# Callback responsavel por receber uma mensagem publicada no tópico acima
def on_message(client, userdata, msg):
    print(msg.topic+" "+str(msg.payload))

client = mqtt.Client()
client.on_connect = on_connect
client.on_message = on_message

# Define um usuário e senha para o Broker, se não tem, não use esta linha
# client.username_pw_set("USUARIO", password="SENHA")

# Conecta no MQTT Broker
client.connect("mqtt.eclipse.org", 1883, 60)

# Blocking call that processes network traffic, dispatches callbacks and
# handles reconnecting.
# Other loop*() functions are available that give a threaded interface and a
# manual interface.
# Inicia o loop
client.loop_forever()
~~~
##### Procedimento de Publicação
~~~
# Ensures paho is in PYTHONPATH
import context
# Importa o publish do paho-mqtt
import paho.mqtt.publish as publish

# Publica
publish.single("PI-3A", "Olá Mundo!", hostname="mqtt.eclipse.org")
~~~

https://medium.com/internet-das-coisas/iot-05-dando-uma-breve-an%C3%A1lise-no-protocolo-mqtt-e404e977fbb6
