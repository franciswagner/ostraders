# Open Brazil Brokers
Projeto de recomendação de compra e venda de ações na b3

![](./images/openBrazilTrader.png?raw=true)

#### Algumas regras de negócio pertinentes:
* Integração via [Apache Kafka](https://kafka.apache.org/), para alimentar as tabelas de Candles.
* Cálculo da [média móvel simples](https://pt.wikipedia.org/wiki/M%C3%A9dia_m%C3%B3vel).
* Cálculo da [média móvel exponencial](https://pt.wikipedia.org/wiki/M%C3%A9dia_m%C3%B3vel)
* Cálculo do [MACD](https://pt.wikipedia.org/wiki/MACD)


#### Tecnologias

* [Java 1.8](http://www.oracle.com/technetwork/pt/java/javase/downloads/jdk8-downloads-2133151.html)
* [Spring Boot](https://projects.spring.io/spring-boot/) * Tomcat embedded
* [PostgreSQL](https://www.postgresql.org/) * Onde os dados de cálculo são armazenados
* [Maven](https://maven.apache.org/)

#### Passos para a execução do projeto

Criar as bases de dados conforme descrito abaixo nas linhas de datasource.

#### Porta de execução
Porta de execução padrão 8667 

* Swagger da aplicação. [localhost:8667/swagger-ui.html](localhost:8667/swagger-ui.html)

#### Datasources

* Configuração:

    | Name         | JNDI       | Connection URL                                            | Service Name 			| User 			 | Pass 		    |
    | -------      |:----:      |:-------------:                                            |:-------------:		|:---------- |:---------:   |
    | xxx          | xxx        |jdbc:postgresql://0.0.0.0:5432/calcula_bmf                 |                   | dbbmf      | dbbmf        |
    | xxx          | xxx        |jdbc:postgresql://0.0.0.0:5432/bmf                         |                   | dbbmf      | dbbmf        |

#### Diretórios

sudo mkdir -p data/bmfCarga/{entrada,erro,execucao,saida}
