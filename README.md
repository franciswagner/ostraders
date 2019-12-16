# Open Source Traders - OSTraders
Projeto de recomendação de compra e venda de ações na [B3](http://www.b3.com.br/pt_br/) - **Recomendado para swing traders**

![](./images/openSourceTraders.png?raw=true)

#### Algumas regras de negócio pertinentes:
* Integração via [Apache Kafka](https://kafka.apache.org/), para alimentar as tabelas dos Candles do serviço de cálculo.
* Validação sintática dados no momento da carga.
* Criação do Candlestick Diário no momento da carga de dados do arquivo posicional.
* Criação do Candlestick Semanal após a execução de carga, para que o sistema contenha todas as informações necessárias.
* Cálculo (Diário e Semanal) da [média móvel simples](https://pt.wikipedia.org/wiki/M%C3%A9dia_m%C3%B3vel).
* Cálculo (Diário e Semanal) da [média móvel exponencial](https://pt.wikipedia.org/wiki/M%C3%A9dia_m%C3%B3vel)
* Cálculo (Diário e Semanal) do [MACD](https://pt.wikipedia.org/wiki/MACD)
* Cálculo (Diário e Semanal) do [Linha de Sinal do MACD](https://www.bussoladoinvestidor.com.br/macd-convergencia-divergencia/)
* Cálculo (Diário e Semanal) do [Histograma MACD](https://www.tradergrafico.com.br/www/newsletter/?Data=31/12/2007)


#### Tecnologias

* [Java 1.8](http://www.oracle.com/technetwork/pt/java/javase/downloads/jdk8-downloads-2133151.html)
* [Spring Boot](https://projects.spring.io/spring-boot/) * Tomcat embedded
* [Spring Batch](https://projects.spring.io/spring-batch/)
* [PostgreSQL](https://www.postgresql.org/) * Onde os dados da carga são armazenados e 
utilizado para as tabelas de controle do Spring Batch
* [Maven](https://maven.apache.org/)
* [Kafka](https://kafka.apache.org/)
* [Flyway](https://flywaydb.org/)
* [Docker](https://docs.docker.com/)

Bibliografia:
* [Como se transformar em um operador e investidor de sucesso de Alexander Elder](https://www.amazon.com.br/Como-transformar-operador-investidor-sucesso/dp/8550801097)

#### Projetos envolvidos na solução

* [carga-bmf-arquivo-cotacoes](https://github.com/ostraders/carga-bmf-arquivo-cotacoes)

* [calcula-cotacoes](https://github.com/ostraders/calcula-cotacoes)


#### Passos para a execução do projeto

Criar as bases de dados conforme descrito abaixo nas linhas de datasource.

#### Porta de execução
Portas de execução padrão 8666 e 8667 

* Swagger da aplicação carga. [localhost:8666/swagger-ui.html](localhost:8666/swagger-ui.html)
* Swagger da aplicação cálculo. [localhost:8667/swagger-ui.html](localhost:8667/swagger-ui.html)

#### Datasources

Obs.: É necessário ter uma base postgresql préviamente instalada(Docker ou instalação Localhost)

* Configuração:

    | Name         | JNDI       | Connection URL                                            | Service Name 			| User 			 | Pass 		    |
    | -------      |:----:      |:-------------:                                            |:-------------:		|:---------- |:---------:   |
    | xxx          | xxx        |jdbc:postgresql://0.0.0.0:5432/bmf                         |                   | dbbmf      | dbbmf        |
    | xxx          | xxx        |jdbc:postgresql://0.0.0.0:5432/calcula_bmf                 |                   | dbbmf      | dbbmf        |

#### Diretórios

Criando a estrura de pastas do projeto:

`sudo mkdir -p data/bmfCarga/{entrada,erro,execucao,saida,zip}`

Caso o seu usuário não seja o admin da máquina, siga os passos abaixo:

Como a criação de pastas na raíz do sistema operacional é condicionada aos usuários administradores do sistema é necessário que seja alterada as permissões da pasta para que o usuário logado consiga efetuar as modificações que o sistema demanda.
Atribuindo ao usuário da máquina como owner da pasta:

`sudo chown -R seu_usuario:seu_usuario /data/*`

#### Mensageria (KAFKA)

`sudo docker-compose up -d`

`sudo docker exec cargabmfarquivocotacoes_kafka_1 /opt/kafka_2.11-0.10.1.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic candlestick-diario`

`sudo docker exec cargabmfarquivocotacoes_kafka_1 /opt/kafka_2.11-0.10.1.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic candlestick-semanal`

##### Cotações Históricas

Para baixar os arquivos de cotação click [aqui:](http://www.b3.com.br/pt_br/market-data-e-indices/servicos-de-dados/market-data/historico/mercado-a-vista/series-historicas/)
