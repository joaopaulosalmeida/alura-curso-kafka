# Curso de Kafka na plataforma Alura

<h3>Rodando Localmente </h3>

1º Baixar os binários do site oficial e opção Binários/Scala: https://kafka.apache.org/downloads 

2º Descompactar local e num diretório sem espaço nos nomes

3º Acessar o diretório via terminal. Obs: Os executaveis do windows estão dentro de `bin\windows`

4º Previsa estar com o Java COnfigurado na máquina

<h4>Executando o Zookeeper</h4>

O Zooper é o "banco de dados" do Kafka, onde serão persistido todas as informações e configuração do Kafka

Para executar, executar o comando abaixo

`bin\windows\zookeeper-server-start.bat config\zookeeper.properties`

O resultado da execução, será exibido em qual porta ele está sendo execidado


```
[2022-05-27 08:54:30,097] INFO clientPortAddress is 0.0.0.0:2181 (org.apache.zookeeper.server.quorum.QuorumPeerConfig)
```

É possível consultar essa informação no arquivo __zookeeper.properties__ localizado na pasta __config__

```
clientPort=2181
```
<h4>Executando o Kafka</h4>

Com o Zookeeper em execução é possível o Kafka. Para isso, acessando o diretório __bin__ executando o comando abaixo:

`bin\windows\kafka-server-start.bat config\server.properties` 

Entre os logs de execução do comando acima, será exibido a propriedade abaixo na sessão 

```
[2022-05-27 08:55:59,933] INFO KafkaConfig values:
```
O valor da porta onde o Kafka está em execução localmente

```
listeners = PLAINTEXT://:9092
```

<h5>Topics</h5>

O Kafka é um broker de mensageria assim com outros no mercado (IBM Message Queue, RabbiqMQ e Microsoft Messege Queue) e para poder armazenar as mensagens enviadas, é necessário que elas caiam em uma Fila de Trabalho. Exemplificando seria uma especie de Caixa Posta, direcionada um destinatário especifico. Assim os sistemas que desejam enviada mensagem a esse destinário (Producers) terão um "endereço" para enviar e o dono (consumers) terão onde buscar essas informações.

<h5>Criando os Topicos</h5>

Para criar um tópico é necessário fazer a chamada com os devidos parametros para o Kafka, aqui está o comando para a criação de um novo tópico.

```
bin\kafka-topics.bat --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic LOJA_NOVO_PEDIDO
```

Por enquanto os parametros abaixo são os mais importantes
__--bootstrap-server__: Servidor e Portal onde o Kafka está em execução
__--topic__ é o Nome do Topico que estamos criando


Para consultar os tópicos do servidor Kafka

```
kafka-topics.bat --list --bootstrap-serer localhost:9092
```

O resultado seria este:

```
C:\_tools\kafka_2.12-3.2.0\bin\windows>kafka-topics.bat --list --bootstrap-server localhost:9092
LOJA_NOVO_PEDIDO
```


<h4>Enviando mensagens (Producer)</h4>

Producer é o responsável por enviar mensagem para nosso __Topic__ que armazenará as mensagens até que um __Consumer__ caputre essas mensagens.

Para criar novas mensagems, é necessário chamar o serviço responsável que é o __kafka-console-producer__ passando as informações de Broker e do Tópico:
```
kafka-console-producer.bat --broker-list localhost:9092 --topic LOJA_NOVO_PEDIDO
```
Quando executado esse comando, será habilitado as entradas das mensagens. Cada linha é correspondente a uma mensagem como o exemplo abaixo:

```
C:\_tools\kafka_2.12-3.2.0\bin\windows>kafka-console-producer.bat --broker-list localhost:9092 --topic LOJA_NOVO_PEDIDO
>pedido0,550
>pedido1,750
>pedido2,800
>
```

<h4>Lendo as mensagens (Consumer)</h4>

Se enviamos a mengem para um destinário __(Topic)__ temos que recupera-las. Para isso existe o __CONSUMER__ ele é responsável por conectar ao servidor e ao tópico e recuperar a mensagens gravadas lá.

O comando abaixo abaixo lê as mensagens enviadas instantaneamente ao Tópico.
```
kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic LOJA_NOVO_PEDIDO
```

Mas caso os tópicos estejam com mensagens antigas, é necessário acrescentar o --from-beginning para esse propósito. Com isso o kafka apresentará as mensagens desde do inicio (caso elas não tenham sido consumidas).

```
C:\_tools\kafka_2.12-3.2.0\bin\windows>kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic LOJA_NOVO_PEDIDO --from-beginning
pedido0,550
pedido1,750
pedido2,800
```







