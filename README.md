# Open Source Traders - OSTraders
Projeto de recomendação de compra e venda de ações na [B3](http://www.b3.com.br/pt_br/) - **Recomendado para swing traders**

![](./images/openSourceTraders.png?raw=true)

#### Algumas regras de negócio/soluções técnicas pertinentes:
* Integração via [Apache Kafka](https://kafka.apache.org/), para alimentar as tabelas dos Candles do serviço de cálculo.
* Validação sintática dados no momento da carga.
* Criação do Candlestick Diário no momento da carga de dados do arquivo posicional.
* Criação do Candlestick Semanal após a execução de carga/candles diários, para que o sistema utilize os candles diários como base.
* Cálculo (Diário e Semanal) da [média móvel simples](https://pt.wikipedia.org/wiki/M%C3%A9dia_m%C3%B3vel).
* Cálculo (Diário e Semanal) da [média móvel exponencial](https://pt.wikipedia.org/wiki/M%C3%A9dia_m%C3%B3vel)
* Cálculo (Diário e Semanal) do [MACD](https://pt.wikipedia.org/wiki/MACD)
* Cálculo (Diário e Semanal) do [Linha de Sinal do MACD](https://www.bussoladoinvestidor.com.br/macd-convergencia-divergencia/)
* Cálculo (Diário e Semanal) do [Histograma MACD](https://www.tradergrafico.com.br/www/newsletter/?Data=31/12/2007)
* Cálculo de recomendação de compra e venda de ações por código de negociação.


#### Tecnologias e Dependências

* [Java 11](https://www.azul.com/downloads/zulu-community/?&architecture=x86-64-bit&package=jdk)
* [Spring Boot 1.5](https://projects.spring.io/spring-boot/) * Tomcat embedded
* [Spring Batch](https://projects.spring.io/spring-batch/)
* [PostgreSQL](https://www.postgresql.org/) * Onde os dados da carga são armazenados e 
utilizado para as tabelas de controle do Spring Batch
* [Maven](https://maven.apache.org/)
* [Kafka](https://kafka.apache.org/)
* [Flyway](https://flywaydb.org/)
* [Docker](https://docs.docker.com/)
* [Linux](https://www.linux.org/) Para melhor funcionamento é recomendado o uso do Linux Debian Based(Não foi testado em outras plataformas)
* [otj-pg-embedded](https://github.com/opentable/otj-pg-embedded) PostgreSQL em memória para testes de repository.

Bibliografia:
* [Como se transformar em um operador e investidor de sucesso de Alexander Elder](https://www.amazon.com.br/Como-transformar-operador-investidor-sucesso/dp/8550801097)

#### Projetos envolvidos na solução

* [carga-bmf-arquivo-cotacoes](https://github.com/ostraders/carga-bmf-arquivo-cotacoes)

* [calcula-cotacoes](https://github.com/ostraders/calcula-cotacoes)

#### Arquivo de layout de cotações

![Layout_BDIN_20110708.pdf](./files/Layout_BDIN_20110708.pdf)


#### Diagrama de Entidade e Relacionamento

Base de dados: **calcula_bmf**

![](./images/diagramaERCalcula.png?raw=true)

Base de dados: **dbbmf**

![](./images/diagramaERCarga.png?raw=true)


#### Passos para a execução do projeto

#### Porta de execução
Portas de execução padrão 8666 e 8667 

* Swagger da aplicação carga. [localhost:8666/swagger-ui.html](localhost:8666/swagger-ui.html)
* Swagger da aplicação cálculo. [localhost:8667/swagger-ui.html](localhost:8667/swagger-ui.html)


##### Obs.: Para facilitar a criação do projeto foi criado o script bash `monta_ambiente.sh` encontrado na raíz desse projeto.

Nele contem os seguintes passos: 

* Criação das bases de dados conforme descrito abaixo nas linhas de datasource.

    #### Datasources
    
    Obs.: Criadas as bases postgresql necessárias para o projeto(Docker), com as seguintes configs:
    
    * Configuração:
    
        | Name         | JNDI       | Connection URL                                            | Service Name 			| User 			 | Pass 		    |
        | -------      |:----:      |:-------------:                                            |:-------------:		|:---------- |:---------:   |
        | xxx          | xxx        |jdbc:postgresql://0.0.0.0:5432/dbbmf                       |                   | dbbmf      | dbbmf        |
        | xxx          | xxx        |jdbc:postgresql://0.0.0.0:5432/calcula_bmf                 |                   | dbbmf      | dbbmf        |
    
    #### Diretórios
    
    Criadas as estruras de pastas do projeto:
    
    `sudo mkdir -p data/bmfCarga/{entrada,erro,execucao,sucesso,zip}`
    
    `sudo mkdir -p data/bmfCarga/{sql}`
    
    #### Mensageria (KAFKA)
    
    `sudo docker-compose up -d`
    
    `sudo docker exec cargabmfarquivocotacoes_kafka_1 /opt/kafka_2.11-0.10.1.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic candlestick-diario`
    
    `sudo docker exec cargabmfarquivocotacoes_kafka_1 /opt/kafka_2.11-0.10.1.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic candlestick-semanal`

#### Quadro Kanban com as requisições de negócio
Para acessar os requisitos de negócio é necessário acessar o quadro kanban no [Trello](https://trello.com/b/BQKBD0mj/projeto-b3-an%C3%A1lise-a%C3%A7%C3%A3o)

Obs.: Quadro privado, é necessário solicitar permissão para acessar o quadro.

#### Cotações Históricas

Para baixar os arquivos de cotação acesse o site da B3: [B3](http://www.b3.com.br/pt_br/market-data-e-indices/servicos-de-dados/market-data/historico/mercado-a-vista/series-historicas/)

#### Empresas listadas na B3(12/2019)
Para visualizar a lista de empresas listadas na B3 acesse: [infomoney](https://www.infomoney.com.br/cotacoes/empresas-b3/)

#### Idealizado por:

Alexandre Tiago Cocati

[Júlio César Cocati](https://www.linkedin.com/in/juliococati/)

[Ricardo Cocati](https://www.linkedin.com/in/ricardococati/)

