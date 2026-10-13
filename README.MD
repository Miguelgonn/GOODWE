# GoodWe + FIAP: Volt Rate (EV ChargeOps) - Sprint 02

## 👥 Equipe

| Nome | RM |
|------|----|
| Ana Gabriela     | rm571312 |
| Kaique           | rm570533 |
| Miguel Antunes   | rm573643 |
| Miguel Gonçalves | rm573793 |



---

## 1. Sobre a solução

Em condomínios, prédios corporativos e campi, os carregadores de veículos elétricos são compartilhados, mas faltam mecanismos para registrar as sessões de cada usuário, calcular o consumo individual e ratear o custo de forma justa.

O **Volt Rate** é o protótipo da plataforma EV ChargeOps, desenvolvido a partir da pesquisa e da arquitetura definidas na Sprint 01. Ele transforma os dados de cada sessão de recarga em cobrança e em informação para o gestor:

```
Sessão de recarga -> Registro e validação (IA) -> Consumo por usuário -> Cobrança (kWh x tarifa) -> Previsão de demanda (IA)
```


### O que foi implementado

| Parte da solução | Descrição |
|---|---|
| Registro de sessões | Cada sessão guarda usuário (RFID), carregador, início, fim e energia entregue (kWh) |
| Consumo individual | Soma dos kWh por usuário e por unidade, no mês |
| Modelo de rateio | Valor = kWh x tarifa, em dois planos: à vista e mensal por unidade |
| IA 1: detecção de sessões suspeitas | Avalia toda sessão no momento do registro e retém da cobrança as que parecem erradas |
| IA 2: previsão de consumo | Regressão linear com ajuste por dia da semana, que gera uma recomendação de expansão |
| Integrações com APIs | GoodWe SEMS, Open Charge Map e ANEEL Dados Abertos |

---

## 2. Estrutura do repositório

```
goodwe/
└── trabalho/
    ├── volt_rate.py          # Aplicação: dados, sessões, cobrança, IA, menu e demonstração
    ├── apis.py               # Integrações com GoodWe SEMS, Open Charge Map e ANEEL
    ├── dados.json            # Base de dados local (criada automaticamente)
    ├── dados/
    │   └── teste.txt         # Saída da demonstração com dados simulados
```

---

### Menu

| Opção | Função |
|---|---|
| 1 | Registrar sessão de recarga |
| 2 | Ver cobrança do mês |
| 3 | IA: prever consumo |
| 4 | IA: sessões suspeitas (revisar) |
| 5 | Importar sessões da GoodWe SEMS |
| 6 | Ver estações de recarga próximas (Open Charge Map) |
| 7 | Consultar tarifa da ANEEL |
| 0 | Sair |

### Variáveis de ambiente (apenas para as APIs externas)

O protótipo funciona sem elas, usando os dados simulados. As credenciais ficam fora do código:

```bash
export VOLT_OCM_KEY="chave-open-charge-map"
export VOLT_SEMS_USER="usuario-sems"
export VOLT_SEMS_PASSWORD="senha-sems"
export VOLT_SEMS_PATH="caminho-do-endpoint-de-sessoes"
export VOLT_ANEEL_RESOURCE="id-do-conjunto-de-dados"   # opcional
```

---

## 3. Funcionamento

### Dados

Os dados ficam em `dados.json`, com três blocos:

| Bloco | Conteúdo |
|---|---|
| `config` | Tarifa (R$/kWh), distribuidora e carregadores com a potência máxima (kW) |
| `usuarios` | Nome, unidade, placa, RFID e plano (`avista` ou `mensal`) |
| `sessoes` | RFID, carregador, início, fim, kWh, status e motivos de suspeita |

Os carregadores simulados são `HCA-G2-01` (7 kW), `HCA-G2-02` (7 kW) e `HCA-G2-03` (11 kW), em referência ao modelo HCA G2 da GoodWe.

O RFID identifica o usuário que iniciou a sessão, como previsto na arquitetura da Sprint 01. Cada sessão tem um status:

| Status | Significado |
|---|---|
| `ok` | Sessão normal; entra na cobrança e na previsão |
| `suspeita` | Retida pela IA; não é cobrada até a revisão do gestor |
| `aprovada` | Suspeita aprovada pelo gestor; passa a ser cobrada |
| `rejeitada` | Suspeita rejeitada pelo gestor; nunca é cobrada |

### Modelo de rateio

```
Valor = Energia consumida (kWh) x Tarifa (R$/kWh)
```

A tarifa padrão é R$ 0,95/kWh e pode ser substituída pela tarifa oficial da distribuidora, consultada na API da ANEEL (opção 7 do menu).

| Plano | Funcionamento |
|---|---|
| À vista | Cada recarga é paga ao terminar. O sistema mostra o número de recargas, os kWh e o valor pago no mês |
| Mensal | O consumo de todos os moradores de uma mesma unidade é somado em uma fatura única no fim do mês, com o detalhamento por morador e veículo |

Casos excepcionais, conforme definido na Sprint 01:

| Caso | Tratamento |
|---|---|
| Sessão interrompida | Cobra-se apenas a energia efetivamente entregue |
| Usuário sem recarga no período | Sem cobrança |
| Dois veículos na mesma unidade | Consumos consolidados na fatura mensal da unidade |
| Sessão repetida na importação | Ignorada, com base no identificador externo |
| Sessão suspeita | Retida da cobrança até a decisão do gestor |

A cobrança implementada é de **visualização**: o sistema calcula e exibe os valores, mas não emite boleto nem se integra a meio de pagamento.

### Módulo de IA

A IA participa do fluxo principal da solução, e não é um recurso separado.

**IA 1: detecção de sessões suspeitas (integrada ao registro e à cobrança)**

Toda sessão passa pela função `avaliar_sessao` dentro de `registrar_sessao`, antes de ser gravada. O resultado define se a sessão será cobrada. As regras são:

| Regra | Critério |
|---|---|
| Usuário não identificado | O RFID não está cadastrado |
| Regra física | A energia informada supera potência do carregador x duração (margem de 10%). Exemplo: 30 kWh em 1 h num carregador de 7 kW |
| Regra estatística | O consumo está mais de 4 desvios-padrão acima da média do histórico (ativa a partir de 20 sessões válidas) |

Sessões que violam alguma regra recebem o status `suspeita` e ficam fora da cobrança. O gestor as revisa na opção 4 do menu e decide se aprova ou rejeita cada uma.

**IA 2: previsão de consumo (integrada ao planejamento da infraestrutura)**

Usa regressão linear por mínimos quadrados sobre o consumo diário em kWh e soma um ajuste médio por dia da semana. Exige ao menos 14 dias de histórico, prevê os próximos 7 dias e compara com a média dos últimos 7 dias. Se a variação prevista passar de +20%, o sistema recomenda avaliar a ampliação do número de carregadores.

Apenas sessões válidas (`ok` ou `aprovada`) entram na previsão, de modo que a IA 1 também protege a qualidade dos dados da IA 2.

###Integrações com APIs

| API | Uso no projeto |
|---|---|
| GoodWe SEMS Portal | Login e importação de sessões de recarga (sem duplicar sessões já registradas) |
| Open Charge Map | Lista de estações próximas, com nome, distância e potência máxima. A busca de exemplo usa a região da FIAP Aclimação |
| ANEEL Dados Abertos | Tarifa residencial (B1, convencional) da distribuidora, somando TUSD e TE e convertendo de R$/MWh para R$/kWh |

O endpoint de sessões do carregador HCA G2 na SEMS depende do contrato com a GoodWe. Por isso o caminho é configurável (`VOLT_SEMS_PATH`) e os nomes dos campos devem ser ajustados na função `normalizar` do `apis.py` conforme a resposta real. Esta integração ainda não foi validada com um carregador real.

---

## 4. Decisões técnicas

| Decisão | Motivo |
|---|---|
| Python com biblioteca padrão apenas | Facilita a execução em qualquer máquina e deixa todo o código legível e explicável pela equipe, sem dependências ocultas |
| Persistência em arquivo JSON | Permite inspecionar e versionar os dados com facilidade durante o desenvolvimento do protótipo |
| Interface de console | Mantém o foco do protótipo na lógica de cobrança e na IA |
| IA baseada em regras físicas, estatística simples e regressão linear | Os resultados são explicáveis ao gestor: cada sessão suspeita é retida com o motivo escrito. Com poucos dados, um modelo simples também é mais confiável que um modelo complexo |
| Sessão suspeita não é cobrada até a revisão | Evita cobrar o usuário por um valor possivelmente errado; a decisão final fica com o gestor |
| Credenciais em variáveis de ambiente | Impede que senhas e chaves de API sejam versionadas no repositório |
| Dados simulados com semente fixa | Torna a demonstração reproduzível |

---

## 5. Desvios em relação à Sprint 01 e justificativas

| Planejado na Sprint 01 | Implementado | Justificativa |
|---|---|---|
| Banco de dados com tabelas de usuários, veículos, sessões e faturas | Arquivo JSON; o veículo (placa) faz parte do cadastro do usuário; a fatura é calculada na hora da consulta | O protótipo tem poucos registros e o foco da sprint foi a lógica de rateio e a IA. A estrutura dos dados foi mantida simples para migrar depois para um banco relacional |
| Dashboard web para usuários e gestores | Interface de console com menu | Priorizamos o núcleo da solução (sessões, cobrança e IA), que é o entregável central da sprint |
| Cadastro de usuários e veículos | Usuários e veículos pré-cadastrados nos dados de exemplo | Os dados simulados bastam para validar o cálculo. Uma tela de cadastro depende da interface web |
| Integração com Google Places | Integração com ANEEL Dados Abertos | A tarifa oficial alimenta diretamente o cálculo da cobrança, o que agrega mais ao fluxo principal do que um segundo serviço de localização. A localização de estações já é atendida pelo Open Charge Map |
| Pré-pago com saldo de créditos | Plano à vista, em que cada recarga é paga ao terminar | O controle de saldo exige meio de pagamento e carteira digital, que ficaram fora do escopo do protótipo |
| Detecção de anomalias com Isolation Forest | Regras físicas e estatísticas (4 desvios-padrão) | Os dados disponíveis são simulados e a explicação do motivo de cada alerta era prioridade. O Isolation Forest fica como evolução quando houver dados reais |
| Mapbox para visualização de mapas | Não implementado | Depende de interface web; a lista de estações aparece no console |

---

## 6. Evidências de funcionamento

A demonstração (`python volt_rate.py demo`) executa o fluxo completo com dados simulados e grava a saída em `evidencias/demonstracao.txt`. Uma cópia está em `dados/teste.txt`.

| Cenário | Resultado |
|---|---|
| Sessão normal: Ana recarrega 11 kWh em 2 h | Status `ok`; valor de R$ 10,45 (11 x 0,95) |
| Kaique: 30 kWh em 1 h num carregador de 7 kW | Status `suspeita`, com o motivo registrado; cobrança retida |
| RFID não cadastrado | Sessão retida como suspeita |
| Sessão de 60 kWh, com média de 16,5 kWh | Sessão retida como suspeita |
| Cobrança à vista | Valor individual por usuário (Ana: 128,6 kWh = R$ 122,16) |
| Cobrança mensal | Fatura por unidade (Apto 303: 316,1 kWh = R$ 300,32), detalhada por morador |
| Previsão de consumo | Previsão de 7 dias, variação de -10% e recomendação de demanda estável |
| Resumo da IA 1 | 4 sessões retidas, somando R$ 120,65 que não foram cobrados |


---

## 7. Próximos passos

- Interface web para usuários e gestores.
- Banco de dados relacional no lugar do JSON.
- Pré-pago com saldo de créditos e integração com meio de pagamento.
- Exibição das estações em mapa (Mapbox).
- Evolução da detecção de anomalias para aprendizado não supervisionado (Isolation Forest).
- Validação da integração com a SEMS em um carregador GoodWe HCA G2 real, como o do Energy Innovation Lab.
