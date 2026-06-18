# GOODWE

## 👥 Equipe

| Nome | RM |
|------|----|
| Ana Gabriela     | rm571312 |
| Kaique           | rm570533 |
| Miguel Antunes   | rm573643 |
| Miguel Gonçalves | rm573793 |

## Descrição do Problema e Contexto
### O Problema
Com o crescimento da mobilidade elétrica, a infraestrutura de recarga compartilhada em condomínios e prédios corporativos enfrenta desafios significativos. Atualmente, não existem mecanismos eficazes para estruturar sessões de recarga, calcular o consumo individual de cada usuário e oferecer uma experiência digital clara e intuitiva. Isso gera dificuldades na gestão, rateio de custos e na transparência para os usuários.

### O Contexto
Este projeto é desenvolvido no âmbito da parceria GoodWe + FIAP, com o objetivo de transformar dados brutos de sessões de recarga em informações estruturadas e inteligentes. A plataforma EV ChargeOps visa oferecer uma solução completa para a gestão operacional de estações de recarga, integrando hardware, software e inteligência artificial para otimizar o uso e a cobrança justa dos serviços.

## Frente 1: Análise de Mercado (Opção A)


### Infraestruturas de Recarga Compartilhada

As infraestruturas de recarga compartilhada são ambientes onde um ou mais carregadores de veículos elétricos são utilizados por diversos usuários. Esse modelo é comum em condomínios residenciais, edifícios corporativos, universidades e estacionamentos compartilhados, onde não é viável instalar um carregador exclusivo para cada veículo.

O principal objetivo desse tipo de infraestrutura é otimizar custos, espaço físico e consumo energético, permitindo que vários usuários tenham acesso ao serviço de recarga.

### Principais Desafios Operacionais

Entre os principais desafios enfrentados pelos gestores estão:

- Identificação dos usuários que utilizam os carregadores;
- Controle das sessões de recarga;
- Monitoramento da disponibilidade dos equipamentos;
- Cálculo do consumo individual de energia;
- Rateio justo dos custos entre os usuários;
- Transparência na cobrança e geração de relatórios;
- Manutenção e monitoramento dos equipamentos.

Esses desafios tornam necessária a utilização de plataformas inteligentes capazes de registrar e analisar os dados gerados durante as recargas.

---

### Funcionamento de uma Sessão de Recarga

Uma sessão de recarga inicia quando o usuário conecta o veículo elétrico ao carregador. Após a autenticação do usuário, por aplicativo, cartão RFID ou outro método de identificação, o carregador libera o fornecimento de energia para o veículo.

Durante toda a sessão, o equipamento monitora e registra diversas informações operacionais. Ao final do carregamento ou quando o veículo é desconectado, a sessão é encerrada e os dados são armazenados para consulta e faturamento.

#### Dados Gerados Durante a Sessão

- Identificação do usuário;
- Data e horário de início;
- Data e horário de término;
- Duração da sessão;
- Energia consumida (kWh);
- Potência instantânea e média;
- Status da sessão;
- Eventos e possíveis falhas.

Esses dados podem ser capturados por meio da API do carregador, protocolos de comunicação como RS-485, conexões Wi-Fi ou LAN e armazenados em bancos de dados para posterior análise.

---

### Modelos de Negócio para Recarga Compartilhada

Atualmente existem diferentes modelos de negócio utilizados no Brasil e em outros países para gerenciar a cobrança da recarga de veículos elétricos.

#### Recarga Gratuita

O usuário utiliza o carregador sem qualquer cobrança. Esse modelo é comum em estabelecimentos que oferecem a recarga como benefício ou estratégia de atração de clientes.

#### Cobrança por kWh

O usuário paga apenas pela quantidade de energia efetivamente consumida durante a sessão. É considerado um dos modelos mais justos e transparentes.

#### Cobrança por Tempo

A cobrança é realizada com base no período em que o carregador permaneceu ocupado, independentemente da quantidade de energia consumida.

#### Assinatura Mensal

O usuário paga uma mensalidade fixa que lhe dá direito a utilizar os carregadores dentro de determinadas condições previamente estabelecidas.

#### Rateio Condominial

Os custos totais de energia são divididos entre os usuários com base em critérios definidos pelo condomínio, normalmente considerando o consumo individual registrado por cada sessão.

#### Créditos Pré-Pagos (Gift Card)

O usuário adiciona créditos à sua conta antes de utilizar os carregadores. A cada sessão de recarga, o valor correspondente ao consumo é descontado automaticamente do saldo disponível.

#### Pagamento Mensal (Pós-Pago)

Todas as sessões realizadas durante o mês são registradas pela plataforma e, ao final do período, é gerada uma fatura consolidada contendo o consumo total e o valor a ser pago pelo usuário.

---

### Zaptec: 
 Plataforma que oferece carregadores inteligentes com monitoramento em tempo real e modelo de cobrança por assinatura.
### Wallbox:
 Solução com foco em carregamento residencial e corporativo, com funcionalidades de controle remoto e integração com sistemas de energia renovável.
### Neocharge:
 Plataforma que permite o rateio automático do consumo de energia entre usuários, com interface digital para gestão e pagamentos.
Cada solução apresenta diferentes abordagens para o problema, com vantagens e limitações que foram consideradas para o desenvolvimento do EV ChargeOps.

## Frente 2: Base Regulatória e Técnica - Mapeamento de APIs Complementares (Opção C)

## Resolução Normativa ANEEL nº 1.000/2021

A Resolução Normativa nº 1.000/2021 da Agência Nacional de Energia Elétrica (ANEEL) estabelece as principais regras para instalação e operação de estações de recarga de veículos elétricos no Brasil.

Um dos pontos mais importantes para o projeto Volt Rate está relacionado à possibilidade de exploração comercial da recarga. De acordo com o Art. 554 da resolução, é permitida a recarga de veículos pertencentes a terceiros, inclusive para fins comerciais, com preços livremente negociados entre as partes. Dessa forma, condomínios, empresas e operadores privados podem oferecer serviços de recarga sem necessidade de autorização específica para comercialização da energia.

A norma também determina que a instalação de estações de recarga deve ser comunicada previamente à distribuidora de energia sempre que houver necessidade de nova conexão elétrica, aumento ou redução de carga instalada ou alteração do nível de tensão da unidade consumidora. Essa exigência busca garantir a segurança e a capacidade da rede elétrica local.

Outro aspecto relevante está previsto no Art. 552, que determina que equipamentos de recarga não exclusivos para uso privado devem ser compatíveis com protocolos abertos de domínio público para comunicação, supervisão e controle remotos. Essa exigência favorece a interoperabilidade entre carregadores e plataformas de gerenciamento, permitindo a integração com sistemas externos de monitoramento e cobrança.

Essas determinações regulatórias favorecem o desenvolvimento do Volt Rate, uma vez que a plataforma depende da coleta de dados dos carregadores para realizar monitoramento, cálculo de custos e geração de relatórios de consumo.

## Carregador GoodWe HCA G2

O carregador GoodWe HCA G2 foi selecionado como referência tecnológica por oferecer recursos de conectividade e monitoramento compatíveis com os objetivos do projeto. O equipamento suporta diferentes interfaces de comunicação, permitindo sua integração com sistemas externos e plataformas de gestão.

### RS-485

A interface RS-485 utiliza comunicação serial industrial e suporta o protocolo Modbus. Essa tecnologia permite que sistemas externos realizem a leitura de informações operacionais do carregador, como potência instantânea, estado de funcionamento e energia consumida. Para o Volt Rate, essa interface pode ser utilizada para integração direta com sistemas locais de monitoramento.

### LAN (Ethernet)

A conexão LAN permite que o carregador seja conectado diretamente à rede local da instalação. Por meio dessa interface, os dados podem ser enviados para servidores de monitoramento e plataformas de gerenciamento, oferecendo maior estabilidade de comunicação em comparação às conexões sem fio.

### Wi-Fi

A conectividade Wi-Fi possibilita a comunicação do carregador com a internet sem necessidade de cabeamento de rede. Essa funcionalidade permite o envio de informações para a plataforma SEMS+ e possibilita monitoramento remoto por aplicativos e sistemas em nuvem.

### Bluetooth

O Bluetooth é utilizado principalmente durante o processo de configuração inicial e manutenção do equipamento. Técnicos e administradores podem acessar parâmetros locais do carregador utilizando dispositivos móveis sem necessidade de conexão direta à internet.

### RFID

A tecnologia RFID permite a autenticação de usuários por meio de cartões ou tags de identificação. Essa funcionalidade é especialmente importante em ambientes compartilhados, pois possibilita registrar qual usuário iniciou cada sessão de recarga. No contexto do Volt Rate, o RFID pode ser utilizado para associar automaticamente o consumo energético ao respectivo usuário e realizar o rateio dos custos.

## API GoodWe (SEMS Portal)

O ecossistema GoodWe utiliza a plataforma SEMS (Smart Energy Management System) para monitoramento remoto de equipamentos conectados. A comunicação entre o carregador e a nuvem permite o armazenamento de informações operacionais que podem ser utilizadas por sistemas externos através de integrações autorizadas.

Entre os principais dados disponibilizados pelo sistema estão:

* Status operacional do carregador;
* Estado da sessão de carregamento;
* Potência instantânea de recarga;
* Energia acumulada entregue ao veículo;
* Histórico de consumo energético;
* Horários de início e término das sessões;
* Eventos de falha ou interrupção;
* Informações de autenticação e gerenciamento dos usuários.

Esses dados são fundamentais para o funcionamento do Volt Rate, pois permitem acompanhar o uso da infraestrutura em tempo real, gerar relatórios de consumo, calcular custos individuais e desenvolver análises baseadas em inteligência artificial.

A integração entre a regulamentação da ANEEL, os recursos de comunicação do GoodWe HCA G2 e os dados disponibilizados pela plataforma SEMS cria uma base tecnológica adequada para o desenvolvimento de soluções voltadas à gestão inteligente de carregadores compartilhados.

---

### Open Charge Map API

A Open Charge Map é uma API pública e colaborativa que fornece informações sobre estações de recarga para veículos elétricos em todo o mundo. A plataforma disponibiliza dados como localização geográfica, tipos de conectores, potência dos carregadores e informações operacionais das estações.

**Objetivo no projeto:**
Integrar informações de pontos de recarga próximos ao usuário, ampliando a experiência de navegação e fornecendo dados adicionais sobre a infraestrutura de carregamento disponível.

**Principais funcionalidades:**
- Localização de estações de recarga.
- Consulta de tipos de conectores.
- Informações sobre potência dos carregadores.
- Cobertura internacional.
- Dados atualizados pela comunidade global.

**Benefícios:**
- API gratuita e aberta.
- Fácil integração.
- Grande quantidade de estações cadastradas.

---

### ANEEL Open Data API

A ANEEL Open Data API disponibiliza dados públicos do setor elétrico brasileiro por meio da plataforma de Dados Abertos da Agência Nacional de Energia Elétrica (ANEEL). Os dados incluem informações sobre tarifas, distribuidoras, geração, transmissão e consumo de energia.

**Objetivo no projeto:**
Utilizar informações oficiais do setor elétrico para complementar análises de consumo energético, custos de recarga e indicadores regulatórios.

**Principais funcionalidades:**
- Consulta de tarifas de energia.
- Informações sobre distribuidoras.
- Dados de geração e consumo energético.
- Indicadores regulatórios do setor elétrico.

**Benefícios:**
- Fonte oficial do Governo Federal.
- Dados confiáveis e atualizados.
- Suporte para análises energéticas e financeiras.

---

### Mapbox API

A Mapbox é uma plataforma de geolocalização e mapas que oferece recursos para criação de mapas interativos, navegação, geocodificação e cálculo de rotas. A API permite integrar visualizações geográficas modernas em aplicações web e mobile.

**Objetivo no projeto:**
Exibir estações de recarga em mapas interativos e facilitar a navegação dos usuários até os pontos de carregamento disponíveis.

**Principais funcionalidades:**
- Mapas interativos.
- Geocodificação de endereços.
- Cálculo de rotas.
- Busca por locais e pontos de interesse.
- Visualização geográfica personalizada.

**Benefícios:**
- Interface moderna e intuitiva.
- Alto nível de personalização.
- Suporte para aplicações web e mobile.

---

## Integração das APIs na Arquitetura

| API | Finalidade |
|------|------------|
| Open Charge Map API | Consulta de estações de recarga e informações dos carregadores |
| ANEEL Open Data API | Obtenção de dados energéticos e tarifários oficiais |
| Mapbox API | Visualização geográfica, mapas interativos e rotas |

### Documentação das APIs

- Open Charge Map API:
  https://openchargemap.org/develop/api

- ANEEL Open Data:
  https://dadosabertos.aneel.gov.br/

- Mapbox API:
  https://docs.mapbox.com/api/overview/

  
## Arquitetura da Solução
### Diagrama

```mermaid
flowchart TD

    A[Carregador HCA G2<br>Captura de sessões e consumo]
    B[Conectividade<br>RS-485 / Wi-Fi / RFID]
    C[Backend]
    D[Banco de Dados]
    E[Modelos de IA]
    G[Frontend]

    A --> B
    B --> C

    C --> D
    C --> E

    subgraph F["APIs Externas"]
        F1[Open Charge Map API]
        F2[ANEEL Open Data API]
        F3[Mapbox API]
    end

    C --> F

    D --> G
    E --> G
    F --> G

    E --> E1[Previsão de Consumo]
    E --> E2[Segmentação de Usuários]

    G --> H[Dashboard]
    G --> I[Faturas]
    G --> J[Controle de Sessões]
    G --> K[Relatórios]
```

#### Carregador HCA G2: Captura dados de sessões de recarga e consumo.
#### Conectividade: Comunicação via RS-485, Wi-Fi e RFID para transmissão dos dados ao backend.
#### Backend: Processamento dos dados, aplicação dos modelos de IA para previsão e segmentação, e cálculo do rateio.
#### Frontend: Interface para gestores e usuários, exibindo informações de consumo, faturas e controle de sessões.

## Modelo de Rateio
O modelo de rateio adotado é baseado no consumo real de energia de cada usuário, garantindo justiça e transparência:

#### Consumo em kWh por sessão.
#### Tempo de ocupação da vaga de recarga.
#### Tarifas fixas e variáveis conforme o custo da energia.
Esse modelo permite que cada usuário pague exatamente pelo que consumiu, evitando conflitos e incentivando o uso consciente.

## Modo de pagamento

### Gift Card (Créditos Pré-Pagos)

O usuário adiciona créditos à sua conta antes de utilizar os carregadores. O funcionamento é semelhante ao de um gift card ou carteira digital.

Exemplos de recarga de saldo:

- R$ 50,00
- R$ 100,00
- R$ 200,00

Ao final de cada sessão de recarga, o sistema calcula automaticamente o valor consumido e desconta do saldo disponível do usuário.

#### Vantagens
- Controle total dos gastos;
- Sem cobranças inesperadas;
- Pagamento antecipado;
- Facilidade para visitantes e usuários temporários.

### Pagamento Mensal (Pós-Pago)

Nesta modalidade, todas as sessões realizadas durante o mês são registradas pela plataforma.

Ao final do período, o sistema gera uma fatura contendo:

- Quantidade de sessões realizadas;
- Consumo total em kWh;
- Tempo de utilização dos carregadores;
- Valor total a ser pago.

A cobrança é realizada apenas no fechamento do mês, permitindo que o usuário utilize os carregadores sem a necessidade de manter créditos disponíveis.

#### Vantagens
- Maior comodidade para usuários frequentes;
- Pagamento consolidado em uma única fatura;
- Melhor acompanhamento do consumo mensal;
- Processo semelhante a contas tradicionais de serviços.
  
## Plano para a Sprint 02
