# 1819V-PS-ei421884219042210
Aplicação para reportar ruído

# Registo de reuniões

[Reunião 1 - 18/3/19](#re1)

[Reunião 2 - 1/4/19](#re2)

[Reunião 3 - 15/4/19](#re3)

[Reunião 4 - 6/5/19](#re4)



## <a name='re1'></a> Reunião 1 - 18/3/19

* Stack tecnológico: Kotlin para aplicação móvel(Android) e servidor backend(Spring Boot). ReactJS para frontend.
* Utilização de PostgreSql como base de dados e do plugin PostGIS para adição de funcionalidades geo-espaciais
* Definição do primeiro passo como aplicação demonstrável: algo que seja capaz de recolher áudio e demonstrar a localização como ponto num mapa
* Ideia de utilizar aplicação móvel como modo referência para distinção de ruido excessivo e de fundo
* Possível integração de autenticação exterior?
  * [nif.pt](https://www.nif.pt/api/)
  * [id.gov.pt](https://id.gov.pt/)
* Atribuição de possível grau de fiabilidade a utilizadores?
* Tokens de autenticação por zona? (dispositivos estáticos) (FUNCIONALIDADE EXTRA, se houver tempo)
* Registar autoridades e utilizadores "normais" de forma semelhante a alunos e docentes no ISEL (a42xxx e d42xxx)
* [Leaflet](https://leafletjs.com/) ou [OpenLayers](https://openlayers.org/) para visualização de dados em mapas
* Incluir desenho das UI no relatório de progresso. [Utilizar aplicação como figma](https://www.figma.com/)
* [Material Design](https://material.io/design/) para design das UI


## <a name='re2'></a> Reunião 2 - 1/4/19

* Iniciar desenvolvimento do servidor/Web API
* Não complicar design de ambas as aplicações móvel e web 
* [Swagger](https://swagger.io/) ou [RAML](https://raml.org/) para automatização da documentação da Web API
* [react-leaflet-heatmap-layer](https://www.npmjs.com/package/react-leaflet-heatmap-layer) para geração de mapas de calor
* Algoritmos para cálculo do grau de importância de cada amostra de ruido (com base na sua idade) e grau de confiança de cada utilizador
* Filtragem de pontos colocados no mapa com base em filtragens temporais
* [Shneiderman's mantra](http://www.codingthearchitecture.com/2015/01/08/shneidermans_mantra.html), [Gestalt theory](https://en.wikipedia.org/wiki/Principles_of_grouping)
* Número de telemóvel para autenticação
* Limitação do número de pedidos realizados por cada utilizador para evitar ataques de denial of service
* Distribuição adequada do tempo, de modo a implementar as funcionalidades básicas da melhor forma possível e de maneira que sobre tempo suficiente para explorar funcionalidades extra


## <a name='re3'></a> Reunião 3 - 15/4/19

* Norma NFC 6902.
* [Sharp file Geodados CM Lisboa](http://geodados.cm-lisboa.pt/datasets/freguesias-2012)
* Algoritmos de bouding box de pontos.
* Obtenção de determinados pontos on demand.
* Obter todas as queixas após determinada data.
* O servidor tem de conseguir filtrar no espaço, tempo ou por caracteristicas relevantes das queixas.
* É necessário fazer uma pesquisa sobre aplicações semelhantes. (Na minha rua LX)


## <a name='re4'></a> Reunião 4 - 6/5/19

* Implementar para a versão beta:
  - WEB: Adicionar uma página com uma tabela com todas as queixas para uma melhor gestão por parte dos administradores.
  - WEB: Adicionar uma página com detalhes da queixa e outra com detalhes do utilizador.
  - WEB: Implementar filtros para mostrar apenas as queixas desejadas.
  - WEB: Testes de stress com muitas queixas para garantir performance.
  - SERVIDOR: Criar as rotas necessárias para suportar as funcionalidades das aplicações.
  - ANDROID: Registo e login.
  - ANDROID: Iniciar a implementação do Room para cache, assim como os WorkManagers para sincronização periodica e manual.
  - Pensar na implementação do deploy do servidor.
  
* Demonstração das três componentes.
* Não utilizar firebase apenas para notificações. (Utilizar outras formas de sincronização).
* Enviar nas queixas um array com as amostras recolhidas para serem criados gráficos na aplicação web.
