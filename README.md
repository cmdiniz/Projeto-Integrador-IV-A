## Projeto-Integrador-IV-A

### Equipe
* Bruno Lemke dos Santos
* Guilherme de Souza Dias
* Luciano Batista da Cunha
* Marcelo Souza da Rosa
* Ronaldo Silva da Cunha
* Tiago Tessmann Oliveira

### Primeiro Encontro - 15/08/2020 - Prof. Adenauer Yamin

#### Sockets TCP & UDP
[Introdução ao Conceito de Sockets](http://olaria.ucpel.edu.br/materiais/lib/exe/fetch.php?media=introducao-sockets.pdf)

#### Conceitos Relacionados à IoT:
* [Slides](http://olaria.ucpel.edu.br/materiais/lib/exe/fetch.php?media=internet_das_coisas_piv.pdf)
* [IoT Comic Book](https://iotcomicbook.org/)
* [Fog & Cloud](http://olaria.ucpel.edu.br/materiais/lib/exe/fetch.php?media=apresentacao-pi-iv.pdf)

#### Plataformas de Nuvem para IoT
* [Relação de Serviços](http://olaria.ucpel.edu.br/materiais/doku.php?id=plataformas_nuvem_iot)

#### Embarcados para IoT
* [System on a Chip ESP32S](http://olaria.ucpel.edu.br/micropython/doku.php?id=esp32) -- [Oferta Mercado Livre](https://produto.mercadolivre.com.br/MLB-1151473863-esp32-esp32s-placa-modulo-wi-fi-bluetooth-dual-core-_JM?matt_tool=79246729&matt_word=&gclid=CjwKCAjw4MP5BRBtEiwASfwAL0F8mFwv-V3Q_O161Ge-EfIvJKZPkDSirQHND7rrsGmBt5yx62m_8xoC2C4QAvD_BwE&shippingOptionId=undefined)
* [Sensor de Temperatura DS18b20](https://www.maximintegrated.com/en/products/sensors/DS18B20.html) -- [Oferta Mercado Livre](https://produto.mercadolivre.com.br/MLB-1059731944-5x-sensor-de-temperatura-ds18b20-waterproof-arduino-_JM?quantity=1#position=3&type=item&tracking_id=d4f114c0-9fab-4e6d-ae50-cd980b631fb6)

#### Monitorando Informações do Equipamento
[Integrando Bash com Python](http://olaria.ucpel.edu.br/materiais/doku.php?id=integrando-bash-python)
[Exemplo em Bash](http://olaria.ucpel.edu.br/materiais/doku.php?id=script-filtro-informacoes)

#### Conhecendo o Bash de Sistemas Operacionais Posix
* [Sistemas Posix](https://pt.wikipedia.org/wiki/POSIX)
* [Foca Linux](http://www.guiafoca.org/)
* [Ebooks sobre Sistemas Operacionais Posix](https://drive.google.com/drive/folders/0B2INSZz1E5TlVWdkVFM0OUxKXzA)

* Exemplo de uso do Cron: 
  * 0-59/2 * * * * date >> exemplo-pi-iv.txt 
  * A cada 2 minutos o comando é executado

#### Virtualizando
* [Virtual Box](https://www.virtualbox.org/)
* [Linux Mint](https://linuxmint.com/)

#### Transmitindo Informações Sensoriadas do Meio para um Servidor
  * Conceitos
    * [Protocolo MQTT - Material IBM](https://www.ibm.com/developerworks/br/library/iot-mqtt-why-good-for-iot/index.html)
    * [Protocolo MQTT - Material Curto Circuito](https://www.curtocircuito.com.br/blog/introducao-ao-mqtt/)
    * [Slides sobre MQTT - Material UFC](https://pt.slideshare.net/MaurcioMoreiraNeto/protocolo-mqtt-redes-de-computadores)
    * [Comparação MQTT & Outros Protocolos](https://medium.com/internet-das-coisas/iot-05-dando-uma-breve-an%C3%A1lise-no-protocolo-mqtt-e404e977fbb6)
  * Plataformas de Software
    * [Mosquitto da Eclipse Foundation](https://mosquitto.org)
    * [Brokers MQTT gratuitos e pagos para utilizar em projetos da IoT](https://diyprojects.io/8-online-mqtt-brokers-iot-connected-objects-cloud/#.XzfHmEl7nUI)
    * [Explorando o uso de MQTT em Programas Python](https://fazbe.github.io/Usando-o-paho-mqtt-para-Python/)


#### Instalando os Clientes da Plataforma Mosquitto

* sudo apt install mosquitto-clients

* Testes feitos com o Broker: broker.emqx.io

* mosquitto_sub -h broker.emqx.io -t pi4

* mosquitto_pub -h broker.emqx.io -t pi4 -m "Primeira Conexao"


#### Publicando com Script Bash em Broker MQTT
~~~
#!/bin/bash
contador=1
while [ $contador -le 10 ]
do
        mosquitto_pub -h broker.emqx.io -t pi4 -m $contador
        sleep 3
        let contador=contador+1
done
~~~

#### Linguagem Python
* [Aprenda Programar - PythonBrasil](https://wiki.python.org.br/AprendaProgramar)
* [Lendo Ocupação de CPU e Memória em Python](https://www.it-swarm.dev/pt/python/como-obter-cpu-atual-e-ram-uso-em-python/958548632/)
* [Receitas para manipular arquivos de texto em Python](http://devfuria.com.br/python/receitas-para-manipular-arquivos-de-texto/)

#### Comunicando com um Broker MQTT utilizando Python

##### Procedimento de Subscrição
~~~
# Cliente Python para subscrever em um Broker MQTT
#
# Para instalar o paho-mqtt use o comando pip install paho-mqtt ou
# apt get install paho-mqtt
# Faca as instalacoes como root
#
import paho.mqtt.client as mqtt

# Retorno quando um cliente recebe um  CONNACK do Broker, confirmando a subscricao
def on_connect(client, userdata, flags, rc):
    print("Conectado, com o seguinte retorno do Broker: "+str(rc))

    # O subscribe fica no on_connect pois, caso perca a conexao ele a renova
    # Lembrando que quando usado o #, você está falando que tudo que chegar após a barra do topico, sera recebido
    client.subscribe("pi4/#")

# Callback responsavel por receber uma mensagem publicada no topico acima
def on_message(client, userdata, msg):
    print(msg.topic+" "+str(msg.payload))
# adicionar comandos para gravar no banco de dados

client = mqtt.Client()
client.on_connect = on_connect
client.on_message = on_message

# Define um usuário e senha para o Broker, se não tem, não use esta linha
# client.username_pw_set("USUARIO", password="SENHA")

# Conecta no MQTT Broker
client.connect("broker.emqx.io", 1883, 60)

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
# Importa o publish do paho-mqtt
import paho.mqtt.publish as publish

# Publica
publish.single("pi4", "Olá Mundo!", hostname="broker.emqx.io")
~~~
###### Publicação com Laço de Repetição
~~~
import paho.mqtt.publish as publish
import time

# Publica

contador = 0
while (contador < 5):
        publish.single("pi4", contador, hostname="broker.emqx.io")
        contador = contador + 1
        time.sleep(2)
~~~

###### Publicação com Laço de Repetição lendo a Ocupação de CPU

~~~
# Ensures paho is in PYTHONPATH
# Importa o publish do paho-mqtt
import paho.mqtt.publish as publish
import time
import psutil
# Publica

contador = 0
while (contador < 25):
	publish.single("pi4", psutil.cpu_percent(), hostname="broker.emqx.io")
	contador = contador + 1
	time.sleep(3)
~~~

###### Script em Bash para gerar ocupação de CPU
~~~
while true; do

a=7.15/6.74

done

~~~

### Segundo Encontro - 12/09/2020 - Prof. Cláudio Diniz

#### Instalação do MySQL
* [MySQL Downloads](https://mysql.com/downloads)
* [Vídeo de instruções para instalação no Linux (Ubuntu 18.04)](https://www.youtube.com/watch?v=CBK7c1xp-zI)
* [Vídeo de instruções para instalação no Windows 10](https://www.youtube.com/watch?v=fmerTu7dWk8)
* [Vídeo de instruções para instalação no Mac (em inglês)](https://www.youtube.com/watch?v=7S_tz1z_5bA&t=290s)

#### Materiais sobre MySQL
* [Documentação MySQL](https://dev.mysql.com/doc/)
* [Curso completo de MySQL para iniciantes (vídeo em inglês)](https://youtu.be/7S_tz1z_5bA)

#### Modelagem conceitual de banco de dados

* [Modelagem conceitual de banco de dados](https://drive.google.com/file/d/1wJ3lc_D0D7gQDiwK26PSdvKLPAW8f1qZ/view?usp=sharing)
* [Gerando códigos SQL a partir de um Diagrama Entidade-Relacionamento (DER) no MySQL Workbench](https://www.youtube.com/watch?v=UMACnfziDCE)

#### Prática com linguagem SQL (Structured Query Language)

* [Prática com linguagem SQL usando MySQL](https://drive.google.com/file/d/16I9LW3rCATxGYz21vjjMKWQ64t2NKiTr/view?usp=sharing)
* [Material extra sobre SQL](https://drive.google.com/file/d/1XDzaJN88VAHkCYxLOxtHKxb7zhhY7cGq/view?usp=sharing)

#### Acessando o banco de dados com programa em Python

* [Python e MySQL (playlist de vídeos no YouTube)](https://www.youtube.com/playlist?list=PLB5jA40tNf3tRMbTpBA0N7lfDZNLZAa9G)

##### Instalação do conector do MySQL com a linguagem Python

~~~
pip3 install --upgrade pip
pip3 install mysql-connector
~~~

##### Teste de conexão de um banco de dados MySQL com programa em Python

###### Programa Python (teste_conexao.py)

~~~
import mysql.connector

mydb = mysql.connector.connect(
	host = "127.0.0.1",
	user="root",
	passwd="minhasenha"
)

print(mydb) 
~~~

###### Executando o programa Python (teste_conexao.py)

~~~
python3 teste_conexao.py
~~~

###### Saída esperada do programa teste_conexao.py

~~~
<mysql.connector.connection_cext.CMySQLConnection object at 0x...
~~~

##### Mostrando os bancos de dados do MySQL

###### Programa Python (showdatabases.py)

~~~
import mysql.connector

mydb = mysql.connector.connect(
	host = "127.0.0.1",
	user="root",
	passwd="minhasenha"
)

mycursor = mydb.cursor()

mycursor.execute("SHOW DATABASES")

for db in mycursor:
	print(db)
~~~

###### Executando o programa Python (showdatabases.py)

~~~
python3 showdatabases.py
~~~

##### Mostrando as tabelas de um bancos de dados projeto1

###### Programa Python (showtables.py)

~~~
import mysql.connector

mydb = mysql.connector.connect(
	host = "127.0.0.1",
	user="root",
	passwd="minhasenha",
	database="projeto1"
)

mycursor = mydb.cursor()

mycursor.execute("SHOW TABLES")

for tb in mycursor:
	print(tb)
~~~

###### Executando o programa Python (showtables.py)

~~~
python3 showtables.py
~~~

##### Inserindo dados nas tabelas do bancos de dados

###### Programa Python (insert_values.py)

~~~
import mysql.connector

mydb = mysql.connector.connect(
	host = "127.0.0.1",
	user="root",
	passwd="minhasenha",
	database="projeto1"
)

mycursor = mydb.cursor()

sqlFormula = "INSERT INTO departamento (iddepartamento,nome) VALUES (%s, %s)"

departamento1 = (1, "Projetos")

mycursor.execute(sqlFormula, departamento1)

mydb.commit()
~~~

###### Executando o programa Python (insert_values.py)

~~~
python3 insert_values.py
~~~

###### Mostrando o resultado no MySQL Workbench

Você pode executar os seguintes comandos SQL no MySQL Workbench para observar o resultado da escrita dos dados no banco de dados

~~~
USE projeto1;
SELECT * FROM departamento;
~~~

##### Comandos de consulta ao banco de dados

###### Programa Python (select.py)

Buscar todos dados de uma tabela.

~~~
import mysql.connector

mydb = mysql.connector.connect(
	host = "127.0.0.1",
	user="root",
	passwd="minhasenha",
	database="projeto1"
)

mycursor = mydb.cursor()

mycursor.execute("SELECT * FROM departamento")

myresult = mycursor.fetchall()

for row in myresult:
	print(row)
~~~

###### Executando o programa Python (select.py)

~~~
python3 select.py
~~~

###### Programa Python (selectone.py)

Buscar um dado de uma tabela.

~~~
import mysql.connector

mydb = mysql.connector.connect(
	host = "127.0.0.1",
	user="root",
	passwd="minhasenha",
	database="projeto1"
)

mycursor = mydb.cursor()

mycursor.execute("SELECT * FROM departamento")

myresult = mycursor.fetchone()

for row in myresult:
	print(row)
~~~

###### Executando o programa Python (selectone.py)

~~~
python3 selectone.py
~~~

### Projeto Final

* [Visão Geral do Projeto Final](https://docs.google.com/presentation/d/1WLr_SozYkB1iTrH71zghpa4Tljgqp3P9QaTv15OrLP4/)

