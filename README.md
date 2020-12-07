<p align="center">
  <img src="https://pa1.narvii.com/6593/8548eff0eae32040f9fe90e76f336a4bb2c606fb_hq.gif"/>
</p>

## Visão Geral

Pensar em arquitetura de software sempre foi algo complexo. Nos tempos atuais onde temos o boom dos microsserviços, contêineres e aplicativos em contêiner, isso se tornou um desafio ainda maior para quem desenvolve e arquiteta aplicações. Toda a linha temporal desde o surgimento da web e passando por acontecimentos como: protocolo HTTP, códificação (ASCII, ISO, Unicode), RPC, web services, SOAP, REST, desacoplamente e desentralização de arquiteturas e etc, juntamente com a crescende demanda do mercado, mudaram diretamente a forma como nós desenvolvemos e mantemos software.

A escalabilidade de uma aplicação requer a execução de muitas operações para garantir o acesso contínuo dos usuários finais ao produto. A boa notícia é que, quando há problemas nesses ambientes, as atividades operacionais fornecem informações para ajudar os desenvolvedores e a equipe de TI a encontrar a causa **RAIZ** do que está acontecendo e corrigir o problema **ANTES** que os clientes percebam.

## Introdução

Possuir logs é um dos requisitos mais importantes da observação e operação de qualquer sistema distribuído de grande escala. O **Kubernetes** é o rei dessa festa, orquestrando um exército de contêineres **Docker** em ambientes mais distribuídos. 

Como essa arquitetura atende à demanda por **alta disponibilidade**, **confiabilidade**, **segurança** e **escalonamento automático** durante o horário de pico, a escala dessas operações requer diretamente um gerenciamento mais robusto. Esse contexto se torna mais crítico do que nunca para o monitoramento e solução de problemas.

Atualmente no mercado existem vários agregadores de registro e ferramentas de análise para agregar nesse contexto, mas duas dominam quando o assunto é logs, principalmente no **Kubernetes**:

* **Fluentd**
* **Logstash**

Esses dois monstrinhos possuem as mesmas funcionalidades, mas se diferenciam na abordagem da solução do problema, tornando um preferível ao outro dependendo do seu caso de uso.

<p align="center">
  <img src="https://cloudteam.stefanini.io/wp-content/uploads/2020/12/logstash_fluentdcomparison-1024x431-1-300x126.png" />
</p>

## The logging stack components

Os heróis anônimos da análise de logs são os coletores de logs. Eles se responsabilizam pelo trabalho do scrap de métricas, analise de logs e transporte das informações colhidas para um sistema de armazenamento, que pode ser desde um **Elasticsearch**, até um banco de dados como o **Postgres**. No outro lado, dando suporte a esses heróis, temos as ferramentas de visualização como o **Kibana**, que fornecem uma perspectiva mais amigável ao ser humano para a análise visual daquilo que está sendo colhido.

Para garantir que esse processo seja gerenciado corretamente, precisamos de uma stack de componentes. Essa stack é chamada de **Logging Stack** e nada mais é que um conjunto de ferramentas trabalhando para garantir de forma apropriada o gerenciamento dos logs. Nessa stack temos:

* **Exportador de logs**
* **Coletor de logs**
* **Armazenamento de logs**
* **Visualização de logs**

E como o Fluentd e Logstash, que são coletores de log, interagem na **Logging Stack**? É isso que iremos ver fazendo esse estudo comparativo.

## Logstash

O Logstash faz parte da popular stack **ELK**:

* **Elasticsearch**
* **Logstash** 
* **Kibana**

Esse conjunto é utilizado por muitos para aplicar a **Logging Stack** em seu ambiente.

O **Elasticsearch** é o mecanismo de pesquisa distribuído. Os dados brutos fluem para ele de diferentes tipos de fontes, incluindo: logs, métricas de sistema e aplicativos web. A ingestão de dados é o processo pelo qual esses dados brutos são analisados, normalizados e enriquecidos antes de serem indexados. Depois de indexados, os usuários podem executar consultas e usar agregações para recuperar resumos. É ai que com o **Kibana** criamos visualizações poderosas dos dados que temos armazenados no **Elasticsearch**.

Dizemos então que o Logstash é o mecanismo de coleta de dados de código aberto que faz parte da stack **ELK** e que faz um pipelining em tempo real dos logs do seu ambiente, unificando dados de diferentes fontes dinamicamente e normalizando eles em diferentes destinos de sua escolha.

## Fluentd

O Fluentd foi desenvolvido pela Treasure Data e faz parte da fundação **CNCF**. Como o Logstash da pilha **ELK**, ele também é um coletor de dados de código aberto que permite unificar a coleta e o consumo de dados para permitir uma visão melhor dos dados. O Fluentd retira logs de um determinado conjunto de fontes, processa (convertendo em um formato de dados estruturados) e os encaminha para outros serviços como **Elasticsearch**, armazenamento de objetos, etc. O Fluentd também trabalha junto com **ElasticSearch** e **Kibana**. Isso é conhecido como stack **EFK**.

## Comparando Logstash e Fluentd

Vamos agora comparar as duas ferramentas em relação aos recursos e capacidades para o mundo **DevOps**.

#### Plataforma

Ambas as ferramentas são executadas em **Windows** e **Linux**.

#### Roteamento de eventos

O roteamento de eventos é um recurso importante na coleta de logs. Como dito acima, o Logstash e Fluentd são diferentes nessa abordagem, apesar de fazerem a mesma coisa. O Logstash usa uma abordagem da condicional onde podemos definir certos critérios com instruções If..Then..Else - para realizar ações em nossos dados. Com ele há um roteamento de todos os dados em um único fluxo e, em seguida, com as as instruções algorítmicas if-then, enviamos esses dados ao destino correto. No Fluentd isso é diferente... os eventos são roteados baseados em tags, onde cada entrada (fonte) precisa ser taggeada. Ele compara uma tag com diferentes saídas e envia o evento para a saída correspondente.

A abordagem de Fluentd é mais declarativa, enquanto o método de Logstash é mais procedural. Os programadores treinados em programação procedural podem ver a configuração do Logstash como mais fácil para começar. Por outro lado, o roteamento baseado em tag do Fluentd permite que um roteamento complexo seja expresso com mais clareza... 

De acordo com a minha experiência em alguns testes, taggear eventos é muito mais fácil do que usar if-then-else para cada tipo de evento, então Fluentd tem uma vantagem aqui.

#### Plugins

Os plug-ins estendem as funcionalidade de uma ferramenta. Ambas as duas ferramentas são flexíveis e funcionam com centenas de integrações para soluções analíticas e de armazenamento.

O ecossistema do Logstash é centralizado em um único repositório no **GitHub**. O Fluentd tem um repositório oficial, mas a maioria dos plug-ins estão hospedados em outro lugar. 

Dizer que um é melhor que o outro vai depender da preferência do usuário em como ele deseja gerenciar e coletar esses plugins: de um local **centralizado** (Logstash) ou **descentralizado** (Fluentd). Em termos de eficiência, geralmente é preferível um local centralizado.

#### Transporte

As entradas - como arquivos, syslog e armazenamentos de dados - são usadas para inserir dados no Logstash. O Logstash é limitado a uma fila na memória que contém **20 eventos** e, portanto, depende de uma fila externa, como o **Redis**, para persistência durante a reinicialização. Frequentemente o **Redis** é usado em uma instalação do Logstash para fazer esse enfileiramento dos eventos. Esse problema é conhecido e atualmente a comunidade está trabalha ativamente para ser resolvê-lo, onde o objetivo é manter a fila armazenada em disco. O Fluentd, por outro lado, possui um sistema de buffer mais eficiente.

* **Logstash**: precisa ser implantado com Redis para garantir a confiabilidade.
* **Fluentd**: confiabilidade integrada , mas sua configuração é mais complicada.

Isso significa que, com o Logstash você precisa de uma ferramenta adicional na sua implantação e configuração. Essa dependência exige outra dependência e complexidade ao sistema e pode aumentar o risco de falha. Este não é o caso do Fluentd, que é independente na obtenção de seus dados e possui um sistema configurável de armazenamento em memória ou em disco. Fluentd, portanto, é **mais seguro** do que Logstash em relação ao **transporte de dados**.

#### Registro de desempenho e alto volume

Ambos têm um bom desempenho na maioria dos casos de uso e acompanham de forma consistente mais de **10.000 eventos por segundo**. Embora o desempenho realmente dependa do seu caso de uso específico, sabemos que o Logstash consome **mais memória** do que o Fluentd. 

O Fluentd é escrito em **Ruby**. Ele é bem eficiente e com uma ótima escala, onde na maioria das implantações de pequeno a médio porte, é mais rápido e consome recursos relativamente mínimos. Na sua configuração é utilizado **Ruby** e **Ruby gems** para configurar seus mais de **500 plug-ins**. Sendo o **Ruby** uma linguagem **interpretada**, ela usa muitas extensões **C** para analisar arquivos de log e encaminhar dados, visando fornecer o requisito de velocidade nas coletas. No entanto, devido ao volume de logs ingeridos, problemas de desempenho são esperados, pois muitas extensões **C** (código extra próximo ao **Ruby**!) são necessárias. 

Ambas as ferramentas possuem abordagens mais leves que ocupam menos recursos:

* **Logstash**: **Elastic-Beats**
* **Fluentd**: **Fluent-Bit**

O **Fluent-Bit** é recomendado ao usar aplicativos pequenos ou embutidos. Ele é implementado principalmente em **C** e pode fornecer toda a funcionalidade que você precisa, atendendo às expectativas de desempenho. Os **Elastic-Beats** são variantes leves do Logstash, no entanto seu caso de uso vai além de um mero transporte de dados. Ao utilizá-lo, para exigir também a extração e agregação de dados, você precisará do Logstash e do **Elastic-Beats**.

* **Logstash**: um pouco mais de uso de memória. É usado **Elastic-Beats** para máquinas com poucos recursos.
* **Fluentd**: um pouco menos uso de memória. É usado **Fluent-Bit** e **Fluentd-Forwarder** para máquinas com poucos recursos.

#### Análise de log

Os componentes para análise de log são diferentes por ferramenta de log. O Fluentd usa analisadores internos padrão (JSON, regex, csv etc.) e o Logstash usa plug-ins para isso. Isso torna o Fluentd favorável em relação ao Logstash, porque não precisa de plugins extras instalados, tornando a arquitetura menos complexa e menos sujeita a erros.

#### Suporte Docker

O Docker tem um driver de registro integrado para Fluentd, mas não tem um para Logstash. Com o Fluentd, nenhum agente extra é necessário no contêiner para enviar logs. Os logs são enviados diretamente para o serviço Fluentd de **STDOUT** sem a necessidade de um arquivo de log extra. O Logstash requer um plug-in (filebeat) para ler os logs do aplicativo de **STDOUT** antes que eles possam ser enviados para o Logstash.

Portanto, ao usar contêineres Docker, o Fluentd é o candidato preferido.

#### Coleta de dados de métricas de contêiner

Tanto o Fluentd quanto o Logstash usam o exportador **Prometheus** para coletar métricas de contêiner. O Logstash, como parte da pilha **ELK**, também usa o **MetricBeat**.

#### Codificação

O Logstash pode ser codificado com **JRuby** e Fluentd com **CRuby** . Isso significa que o Fluentd tem uma vantagem aqui já nenhum **Java Runtime** é necessário.

## Logstash x Fluentd: qual usar para o Kubernetes?

O **Data logging** pode ser dividido em duas áreas:

* **Registro de eventos**
* **Erros**

Tanto o Fluentd quanto o Logstash podem lidar com os dois tipos de registro e podem ser usados para diferentes casos de uso, e até mesmo coexistir em seus ambientes para registrar **VMs/aplicativos legados**, bem como microsserviços baseados no **Kubernetes**.

Para ambientes **Kubernetes** o Fluentd parece o candidato ideal devido ao seu driver e analisador de registro **Docker** integrado - que não requer a presença de um agente extra. Em comparação com o Logstash, isso torna a arquitetura menos complexa e também torna menos arriscada para erros de registro. O fato do Fluentd, como o **Kubernetes**, ser um projeto **CNCF**, também é um bônus adicional!

## Conclusão

Não existe a ferramenta ideal, cada contexto é um contexto e cada realidade é uma realidade. Para **Kubernetes** o Fluentd me pareceu mais amigável e fácil, porém isso não impede a utilização do Logstash. O melhor caso é aquele onde você testa as duas estratégias e aplica a mais favorável.

No geral é isso galera. Espero que esse artigo possa ser proveitoso de alguma forma para vocês. Até!

<p align="center">
  <img src="https://thumbs.gfycat.com/DangerousSociableGalago-small.gif" />
</p>
