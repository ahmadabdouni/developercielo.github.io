---
layout: manual
title: Manual de Integração eCommerce Cielo
description: Manual integração técnica via API
search: true
translated: true
toc_footers: true
categories: manual
sort_order: 2
tags:
  - API Pagamento
language_tabs:
  json: JSON
  shell: cURL
---

# Visão geral - API Cielo eCommerce

O objetivo desta documentação é orientar o desenvolvedor sobre como integrar com a **API Cielo eCommerce da Cielo**, descrevendo as funcionalidades, os métodos a serem utilizados, listando informações a serem enviadas e recebidas, e provendo exemplos.

O mecanismo de integração com o Cielo eCommerce é simples, de modo que apenas conhecimentos intermediários em linguagem de programação para Web, requisições HTTP/HTTPS e manipulação de arquivos JSON, são necessários para implantar a solução Cielo eCommerce com sucesso.

Nesse manual você encontrará a referência sobre todas as operações disponíveis na API REST da API Cielo eCommerce. Estas operações devem ser executadas utilizando sua chave específica (Merchant ID e Merchant Key) nos respectivos endpoints dos ambientes:

|                 | SandBox                                             | Produção                                      |
|:----------------|:---------------------------------------------------:|:---------------------------------------------:|
| **Requisições** | https://apisandbox.cieloecommerce.cielo.com.br      | https://api.cieloecommerce.cielo.com.br/      |
| **Consultas**   | https://apiquerysandbox.cieloecommerce.cielo.com.br | https://apiquery.cieloecommerce.cielo.com.br/ |

Para executar uma operação, combine a URL base do ambiente com a URL da operação desejada e envie utilizando o verbo HTTP conforme descrito na operação.

## Características da solução

A solução **API Cielo eCommerce** da plataforma Cielo eCommerce foi desenvolvida com a tecnologia REST, que é padrão de mercado e independe da tecnologia utilizada por nossos clientes. Dessa forma, é possível integrar-se utilizando as mais variadas linguagens de programação, tais como: 

* ASP
* Net
* Java
* PHP
* Ruby
* Python

> Para Obter exemplos dessas linguagens, veja nosso tutorial de conversão via nosso [**Tutorial Postman**](https://developercielo.github.io/tutorial/postman)

Entre outras características, os atributos que mais se destacam na plataforma Cielo eCommerce:

* **Ausência de aplicativos proprietários**: não é necessário instalar aplicativos no ambiente da loja virtual em nenhuma hipótese.
* **Simplicidade**: o protocolo utilizado é puramente o HTTPS.
* **Facilidade de testes**: a plataforma Cielo oferece um ambiente Sandbox publicamente acessível, que permite ao desenvolvedor a criação de uma conta de testes sem a necessidade de credenciamento, facilitando e agilizando o início da integração.
* **Credenciais**: o tratamento das credenciais do cliente (número de afiliação e chave de acesso) trafega no cabeçalho da requisição HTTP da mensagem.
* **Segurança**: a troca de informações se dá sempre entre o Servidor da Loja e da Cielo, ou seja, sem o browser do comprador.
* **Multiplataforma**: a integração é realizada através de Web Service REST.

## Arquitetura

A integração é realizada através de serviços disponibilizados como Web Services. O modelo empregado é bastante simples: Existem duas URLs (endpoint), uma específica operações que causam efeitos colaterais - como autorização, captura e cancelamento de transações, e uma URL específica para operações que não causam efeitos colaterais, como pesquisa de transações. Essas duas URLs receberão as mensagens HTTP através dos métodos POST, GET ou PUT. Cada tipo de mensagem deve ser enviada para um recurso identificado através do path.

|Método|Descrição|
|---|---|
|**POST**|O método HTTP `POST` é utilizado na criação dos recursos ou no envio de informações que serão processadas. Por exemplo, criação de uma transação.|
|**PUT**|O método HTTP `PUT` é utilizado para atualização de um recurso já existente. Por exemplo, captura ou cancelamento de uma transação previamente autorizada.|
|**GET**|O método HTTP `GET` é utilizado para consultas de recursos já existentes. Por exemplo, consulta de transações.|

|                             | Métodos            | SandBox                                             | Produção                                      |
|-----------------------------|--------------------|-----------------------------------------------------|-----------------------------------------------|
| **Requisição de transação** | **POST** / **PUT** | https://apisandbox.cieloecommerce.cielo.com.br      | https://api.cieloecommerce.cielo.com.br/      |
| **Consultas**               | **GET**            | https://apiquerysandbox.cieloecommerce.cielo.com.br | https://apiquery.cieloecommerce.cielo.com.br/ |

## Glossário 

Para facilitar o entendimento, listamos abaixo um pequeno glossário com os principais termos relacionados ao eCommerce, ao mercado de cartões e adquirencia:

|Termo|Descrição|
|---|---|
|**Autenticação**|processo para assegurar que o comprador é realmente aquele quem diz ser (portador legítimo), geralmente ocorre no banco emissor com uso de um token digital ou cartão com chaves de segurança.|
|**Autorização**|processo para verificar se uma compra pode ou não ser realizada com um cartão. Nesse momento, são feitas diversas verificações com o cartão e com o portador (ex.: adimplência, bloqueios, etc.) É também neste momento que o limite do cartão é sensibilizado com o valor da transação.|
|**Cancelamento**|processo para cancelar uma compra realizada com cartão.|
|**Captura**|processo que confirma uma autorização que foi realizada previamente. Somente após a captura, é que o portador do cartão poderá visualizá-la em seu extrato ou fatura.|
|**Chave de acesso**|é um código de segurança específico de cada loja, gerado pela Cielo, usada para realizar a autenticação e comunicação em todas as mensagens trocadas com a Cielo. Também conhecido como chave de produção e key data.|
|**Comprador**|é o aquele que efetua compra na loja virtual.|
|**Emissor (ou banco emissor)**|É a instituição financeira que emite o cartão de crédito, débito ou voucher.|
|**Estabelecimento comercial ou EC**|Entidade que responde pela loja virtual.|
|**Gateway de pagamentos**|Empresa responsável pelo integração técnica e processamento das transações.|
|**Número de credenciamento**|é um número identificador que o lojista recebe após seu credenciamento junto à Cielo.|
|**Portador**|é a pessoa que tem o porte do cartão no momento da venda.|
|**SecureCode**|programa internacional da Mastercard para possibilitar a autenticação do comprador no momento de uma compra em ambiente eCommerce.|
|**TID (Transaction Identifier)**|código composto por 20 caracteres que identificada unicamente uma transação Cielo eCommerce.|
|**Transação**|é o pedido de compra do portador do cartão na Cielo.|
|**VBV (Verified by Visa)**|Programa internacional da Visa que possibilita a autenticação do comprador no momento de uma compra em ambiente eCommerce.|

## Produtos e Bandeiras suportadas 

A versão atual do Webservice Cielo possui suporte às seguintes bandeiras e produtos:

| Bandeira         | Crédito à vista | Crédito parcelado Loja | Débito | Voucher | Internacional |
|------------------|-----------------|------------------------|--------|---------|---------------|
| Visa             | Sim             | Sim                    | Sim    | *Não*   | Sim           |
| Master Card      | Sim             | Sim                    | Sim    | *Não*   | Sim           |
| American Express | Sim             | Sim                    | *Não*  | *Não*   | Sim           |
| Elo              | Sim             | Sim                    | *Não*  | *Não*   | Sim           |
| Diners Club      | Sim             | Sim                    | *Não*  | *Não*   | Sim           |
| Discover         | Sim             | Não                    | *Não*  | *Não*   | Sim           |
| JCB              | Sim             | Sim                    | *Não*  | *Não*   | Sim           |
| Aura             | Sim             | Sim                    | *Não*  | *Não*   | Sim           |
| Hipercard        | Sim             | Sim                    | *Não*  | *Não*   | *Não*         |
| Hiper            | Sim             | Sim                    | *Não*  | *Não*   | *Não*         |

> Cartões emitidos no exterior não possuem permissão de parcelamento.

# Certificados e segurança

## O que é Certificado SSL?

O Certificado SSL para servidor web oferece autenticidade e integridade dos dados de um web site, proporcionando aos clientes das lojas virtuais a garantia de que estão realmente acessando o site que desejam, e não uma um site fraudador.

Empresas especializadas são responsáveis por fazer a validação do domínio e, dependendo do tipo de certificado, também da entidade detentora do domínio.

### Internet Explorer:

![Certificado EV Internet Explorer]({{ site.baseurl_root }}/images/certificado-ie.jpg)

### Firefox

![Certificado EV Firefox]({{ site.baseurl_root }}/images/certificado-firefox.jpg)

### Google Chrome

![Certificado EV Google Chrome]({{ site.baseurl_root }}/images/certificado-chrome.jpg)

## O que é Certificado EV SSL?

O Certificado EV foi lançado no mercado recentemente e garante um nível de segurança maior para os clientes das lojas virtuais.

Trata-se de um certificado de maior confiança e quando o https for acessado a barra de endereço ficará verde, dando mais confiabilidade aos visitantes do site.

## Como instalar o **Certificado Extended Validation** no servidor da Loja?

Basta instalar os três arquivos a seguir na Trustedstore do servidor. A Cielo não oferece suporte para a instalação do Certificado. Caso não esteja seguro sobre como realizar a instalação do Certificado EV, então você deverá ser contatado o suporte do fornecedor do seu servidor.

* [Certificado Raiz]({{ site.baseurl }}/attachment/Root.crt)
* [Certificado Intermediária]({{ site.baseurl }}/attachment/Intermediate1.crt)
* [Certificado E-Commerce Cielo]({{ site.baseurl }}/attachment/intermediate2.cer)

## Passo a Passo para a Instalação

### Instalação no Servidor da Loja Virtual

O passo a passo para a instalação do Certificado EV deverá ser contatado o suporte do fornecedor do seu servidor.

<aside class="warning"><b>A Cielo não oferece suporte para a instalação do Certificado.</b></aside>

### Acesso do Cliente à Loja Virtual

Normalmente, o browser faz a atualização do Certificado automaticamente, caso não o faça e o cliente entre em contato deverá ser informado os seguintes passos:

**1o Passo:**

Salvar os três arquivos abaixo em uma pasta nova, ou que relembre facilmente, pois será utilizada posteriormente:

* [Certificado Raiz]({{ site.baseurl_root }}/attachment/Root.crt)
* [Certificado Intermediária]({{ site.baseurl_root }}/attachment/intermediate1.crt)
* [Certificado E-Commerce Cielo]({{ site.baseurl_root }}/attachment/intermediate2.cer)

**2o Passo:**

No “Internet Explorer”, clique no menu “Ferramentas” e acesse as “Opções da Internet”:

![Instalar IE]({{ site.baseurl_root }}/images/certificado-instalar-ie-1.jpg)

No “Firefox”, clique no menu “Abrir Menu” e acesse “Avançado” e “Opções”:

![Instalar FF]({{ site.baseurl_root }}/images/certificado-instalar-ff-1.jpg)

No “Chrome”, clique no “Personalizar e Controlar o Google Chrome” e acesse “Configurações” e “Mostrar configurações avançadas... “Alterar Configurações de Proxy e “Conteúdo” e Certificados:

![Instalar GC]({{ site.baseurl_root }}/images/certificado-instalar-gc-1.jpg)

**3o Passo:**

No Internet Explorer, em “Certificados”, clique em “Importar”.

![Instalar IE]({{ site.baseurl_root }}/images/certificado-instalar-ie-2.jpg)

No Firefox clique em “Ver Certificados”, clique em “Importar”

![Instalar FF]({{ site.baseurl_root }}/images/certificado-instalar-ff-2.jpg)

No Chrome clique em “Gerenciar Certificados”, clique em “Importar”

![Instalar GC]({{ site.baseurl_root }}/images/certificado-instalar-gc-2.jpg)

**4o Passo:**

No Internet Explorer e Chrome “Assistente para Importação de Certificados”, clique em “Avançar”.

![Instalar IE e GC]({{ site.baseurl_root }}/images/certificado-instalar-ie-gc-3.jpg)

![Instalar IE e GC]({{ site.baseurl_root }}/images/certificado-instalar-ie-gc-4.jpg)

No Firefox “Aba Servidores ”, clique em “Importar”

![Instalar FF]({{ site.baseurl_root }}/images/certificado-instalar-ff-3.jpg)

**5o Passo:**

No Chrome e Internet Explorer “Assistente para Importação de Certificados”, clique em “Procurar”, procure a pasta onde estão os arquivos e selecione o arquivo “cieloecommerce.cielo.com.br.crt, clique em “Abrir” e em seguida “Avançar”.

![Instalar IE e GC]({{ site.baseurl_root }}/images/certificado-instalar-ie-gc-5.jpg)

![Instalar IE e GC]({{ site.baseurl_root }}/images/certificado-instalar-ie-gc-6.jpg)

**6o Passo:**

Selecionar a opção desejada: adicionar o Certificado em uma pasta padrão ou procurar a pasta de sua escolha.

![Instalar IE e GC]({{ site.baseurl_root }}/images/certificado-instalar-ie-gc-7.jpg)

**7o Passo:**

Clique em “Concluir”.

![Instalar IE e GC]({{ site.baseurl_root }}/images/certificado-instalar-ie-gc-8.jpg)

**8o Passo:**

Clique em “Ok” para concluir a importação.

![Instalar IE e GC]({{ site.baseurl_root }}/images/certificado-instalar-ie-gc-9.jpg)

<aside class="notice">No Firefox não consta a mensagem de Importação com Êxito, apenas conclui a importação.</aside>

O Certificado poderá ser visualizado na aba padrão “Outras Pessoas” ou na escolhida pelo cliente.

![Instalar IE e GC]({{ site.baseurl_root }}/images/certificado-instalar-ie-gc-10.jpg)

**9o Passo:**

Repita o mesmo procedimento para os 3 arquivos enviados.

# Sandbox e Ferramentas

## Sobre o Sandbox

Para facilitar os testes durante a integração, a Cielo oferece um ambiente Sandbox que é composto por duas áreas:

1. Cadastro de conta de testes
2. Endpoints transacionais

|**Requisição**| https://apisandbox.cieloecommerce.cielo.com.br     |
| **Consulta** | https://apiquerysandbox.cieloecommerce.cielo.com.br|

**Vantagens de utilizar o Sandbox**

* Não é necessário uma afiliação para utilizar o Sandbox Cielo.
* Basta acessar o [**Cadastro do Sandbox**](https://cadastrosandbox.cieloecommerce.cielo.com.br/) criar uma conta.
* com o cadastro você receberá um `MerchantId` e um `MerchantKey`,que são as credenciais necessarias para os métodos da API

## Ferramenta para Integração: POSTMAN

O **Postman** é um API Client que facilita aos desenvolvedores criar, compartilhar, testar e documentar APIs. Isso é feito, permitindo aos usuários criar e salvar solicitações HTTPs simples e complexas, bem como ler suas respostas.

A Cielo oferece coleções completas de suas integrações via Postamn, o que facilita o processo de integração com a API Cielo.

Sugerimos que desenvolvedores acessem nosso [**Tutorial**](https://developercielo.github.io/tutorial/postman) sobre a ferramenta para compreender melhor todas as vantagens que ela oferece.

## Cartão de crédito - Sandbox

No sandbox, é necessario utilizar o `Provider` seja utilizado como `SIMULADO`

O Simulado é uma configuração que emula a utilização de pagamentos com Cartão de Crédito. 
Com esse meio de pagamento é possível simular os fluxos de:

* Autorização
* Captura 
* Cancelamento.

Para melhor utilização do Meio de Pagamento Simulado, estamos disponibilizando **cartões de testes** na tabela abaixo.

<aside class="notice">Os <code>status</code> das transações são definidos pelos FINAIS de cada cartão, assim como o <code>ReturnCode</code>.</aside>

| Status da Transação   | Final do Cartão                            | Código de Retorno | Mensagem de Retorno               |
|-----------------------|--------------------------------------------|-------------------|-----------------------------------|
| Autorizado            | 0000.0000.0000.0001<br>0000.0000.0000.0004 | 4/6               | Operação realizada com sucesso    |
| Não Autorizado        | 0000.0000.0000.0002                        | 05                | Não Autorizada                    |
| Não Autorizado        | 0000.0000.0000.0003                        | 57                | Cartão Expirado                   |
| Não Autorizado        | 0000.0000.0000.0005                        | 78                | Cartão Bloqueado                  |
| Não Autorizado        | 0000.0000.0000.0006                        | 99                | Time Out                          |
| Não Autorizado        | 0000.0000.0000.0007                        | 77                | Cartão Cancelado                  |
| Não Autorizado        | 0000.0000.0000.0008                        | 70                | Problemas com o Cartão de Crédito |
| Autorização Aleatória | 0000.0000.0000.0009                        | 99                | Operation Successful / Time Out   |

Exemplo de um Cartão de teste - 4024.0071.5376.3191

As informações de **Cód.Segurança (CVV)** e validade podem ser aleatórias, mantendo o formato - CVV (3 dígitos) Validade (MM/YYYY).

<aside class="notice"><strong>Atenção:</strong> O ambiente de **sandbox** avalia o formato e o final do cartão, caso um cartão real seja enviado, o resultado da operação será idêntico ao descrito na tabela de cartões de teste.</aside>

<aside class="notice"><strong>Tokenização:</strong> transações no ambiente de simulação envolvendo tokenização não funcionaram com base em cartões de teste. Cada cartão salvo na tokenização é tratado como um cartão real, com isso não é usado no processo de simulação.</aside>

<aside class="Warning"><strong>Atenção:</strong> Os Códigos de retorno em Sandbox não são os mesmos disponiveis em produção.</aside>

## Cartão de débito - Sandbox

Cartões de débito não possuem cartões ou dados específicos simulados, como no caso do cartão de crédito. 

O fluxo transacional do cartão de Débito funciona com o Response da transação retornando uma **URL DE AUTENTICAÇÃO** . Na tela de autenticação a opção escolhida define o status da transação.

|Opção|Status|
|---|---|
|Autenticado|Autorizado|
|Não Autenticado|Negado|
|Não usar a URL|Não Finalizado|

<aside class="notice"><strong>Transferência Online:</strong> O mesmo comportamento do Cartão de débito em Sanbox é valido para cartão de débito</aside>

## Outros meios de pagamento - Sandbox

Outros meios de pagamento não possuem cartões ou dados específicos simulados, como no caso do cartão de crédito.
Abaixo especificamos qualquer diferença existente:

|Meio de pagamento|Diferenças|
|---|---|
|Boleto|Não há validação bancaria. O boleto se comporta como um boleto sem registro|
|Cartão de débito|O `provider` utilizado deve ser **SIMULADO** <br><br> A URL de redirecionamento para o ambiente do banco será na verdade uma tela para escolher o estado da autenticação|
|Transferência online|O `provider` utilizado deve ser **SIMULADO** <br><br> A URL de redirecionamento para o ambiente do banco será na verdade uma tela para escolher o estado da autenticação|

# Meios de Pagamento

## Cartão de Crédito

Para que você possa disfrutar de todos os recursos disponíveis em nossa API, é importante que antes você conheça os conceitos envolvidos no processamento de uma transação de cartão de crédito.

|Conceito|Descrição|
|---|---|
|**Autorização**|A autorização (ou pré-autorização) é a principal operação no eCommerce, pois através dela é que uma venda pode ser concretizada. A pré-autorização apenas sensibiliza o limite do cliente, mas ainda não gera cobrança para o consumidor.|
|**Captura**|Ao realizar uma pré-autorização, é necessário a confirmação desta para que a cobrança seja efetivada ao portador do cartão. Através desta operação que se efetiva uma pré-autorização, podendo esta ser executada, em normalmente, em até 5 dias após a data da pré-autorização.|
|**Cancelamento**|O cancelamento é necessário quando, por algum motivo, não se quer mais efetivar uma venda.|
|**Autenticação**|O processo de autenticação possibilita realizar uma venda a qual passará pelo processo de autenticação do banco emissor do cartão, assim trazendo mais segurança para a venda e transferindo para o banco, o risco de fraude.|
|**Cartão protegido**|É uma plataforma que permite o armazenamento seguro de dados sensíveis de cartão de crédito. Estes dados são transformados em um código criptografrado chamado de "token”, que poderá ser armazenado em banco de dados. Com a plataforma, a loja poderá oferecer recursos como "Compra com 1 clique” e "Retentativa de envio de transação”, sempre preservando a integridade e a confidencialidade das informações.|
|**Antifraude**|É uma plataforma de prevenção à fraude que fornece uma análise de risco detalhada das compras on-line. Cada transação é submetida a mais de 260 regras, além das regras específicas de cada segmento, e geram uma recomendação de risco em aproximadamente dois segundos. Este processo é totalmente transparente para o portador do cartão. De acordo com os critérios preestabelecidos, o pedido pode ser automaticamente aceito, recusado ou encaminhado para análise manual.|
|**Recorrente**|A Recorrência Inteligente é um recurso indispensável para estabelicimentos que precisam cobrar regularmente por seus produtos/serviços. É muito utilizado para assinaturas de revistas, mensalidades, licenças de software, entre outros. Os lojistas contarão com recursos diferenciados para modelar sua cobrança de acordo com o seu negócio, pois toda parametrização é configurável, tais como: periodicidade, data de início e fim, quantidade de tentativas, intervalo entre elas, entre outros.|

### Transação Simples

Para criar uma transação que utilizará cartão de crédito, é necessário enviar uma requisição utilizando o método `POST` para o recurso Payment, conforme o exemplo. Esse exemplo contempla o mínimo de campos necessários a serem enviados para a autorização.

<aside class="notice"><strong>Atenção:</strong> Não é possivel realizar uma transação com valor (`Amount`) 0.</aside>

#### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{
   "MerchantOrderId":"2014111703",
   "Customer":{
      "Name":"Comprador crédito simples"
   },
   "Payment":{
     "Type":"CreditCard",
     "Amount":15700,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "CreditCard":{
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"123",
         "Brand":"Visa",
         "CardOnFile":{
            "Usage": "Used",
            "Reason":"Unscheduled"
         }
     },
     "IsCryptoCurrencyNegotiation": true
   }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
   "MerchantOrderId":"2014111703",
   "Customer":{
      "Name":"Comprador crédito simples"
   },
   "Payment":{
     "Type":"CreditCard",
     "Amount":15700,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "CreditCard":{
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"123",
         "Brand":"Visa",
         "CardOnFile":{
            "Usage": "Used",
            "Reason":"Unscheduled"
         }
     },
     "IsCryptoCurrencyNegotiation": true
   }
}
--verbose
```

|Propriedade|Tipo|Tamanho|Obrigatório|Descrição|
|---|---|---|---|---|
|`MerchantId`|Guid|36|Sim|Identificador da loja na Cielo.|
|`MerchantKey`|Texto|40|Sim|Chave Publica para Autenticação Dupla na Cielo.|
|`RequestId`|Guid|36|Não|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.|
|`MerchantOrderId`|Texto|50|Sim|Numero de identificação do Pedido.|
|`Customer.Name`|Texto|255|Não|Nome do Comprador.|
|`Payment.Type`|Texto|100|Sim|Tipo do Meio de Pagamento.|
|`Payment.Amount`|Número|15|Sim|Valor do Pedido (ser enviado em centavos).|
|`Payment.Installments`|Número|2|Sim|Número de Parcelas.|
|`Payment.SoftDescriptor`|Texto|13|Não|Texto impresso na fatura bancaria comprador - Exclusivo para VISA/MASTER - não permite caracteres especiais - Ver Anexo|
|`Payment.IsCryptocurrencyNegotiation`|Booleano|-|Não (default false)|Deve ser enviado com valor “true” caso se trate de uma transação de compra ou venda de Criptomoeda|
|`CreditCard.CardNumber`|Texto|19|Sim|Número do Cartão do Comprador.|
|`CreditCard.Holder`|Texto|25|Não|Nome do Comprador impresso no cartão.|
|`CreditCard.ExpirationDate`|Texto|7|Sim|Data de validade impresso no cartão.|
|`CreditCard.SecurityCode`|Texto|4|Não|Código de segurança impresso no verso do cartão - Ver Anexo.|
|`CreditCard.Brand`|Texto|10|Sim|Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).       |
|`CreditCard.CardOnFile.Usage`|Texto|-|Não|**First** se o cartão foi armazenado e é seu primeiro uso.<br>**Used** se o cartão foi armazenado e ele já foi utilizado anteriormente em outra transação|
|`CreditCard.CardOnFile.Reason`|Texto|-|Condicional|Indica o propósito de armazenamento de cartões, caso o campo "Usage" for "Used".<BR>**Recurring** - Compra recorrente programada (ex. assinaturas)<br>**Unscheduled** - Compra recorrente sem agendamento (ex. aplicativos de serviços)<br>**Installments** - Parcelamento através da recorrência<br>[Veja Mais](https://developercielo.github.io/faq/faq-api-3-0#pagamento-com-credenciais-armazenadas)|

#### Resposta

```json
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador crédito simples"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "455187******0183",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa",
            "CardOnFile":{
               "Usage": "Used",
               "Reason":"Unscheduled"
            }
        },
        "IsCryptoCurrencyNegotiation": true,
        "tryautomaticcancellation":true,
        "ProofOfSale": "674532",
        "Tid": "0305023644309",
        "AuthorizationCode": "123456",
        "PaymentId": "24bc8366-fc31-4d6c-8555-17049a836a07",
        "Type": "CreditCard",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "ReturnCode": "4",
        "ReturnMessage": "Operation Successful",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador crédito simples"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "455187******0183",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa",
            "CardOnFile":{
               "Usage": "Used",
               "Reason":"Unscheduled"
            }
        },
        "IsCryptoCurrencyNegotiation": true,
        "tryautomaticcancellation":true,
        "ProofOfSale": "674532",
        "Tid": "0305023644309",
        "AuthorizationCode": "123456",
        "PaymentId": "24bc8366-fc31-4d6c-8555-17049a836a07",
        "Type": "CreditCard",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "ReturnCode": "4",
        "ReturnMessage": "Operation Successful",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|Texto alfanumérico|
|`Tid`|Id da transação na adquirente.|Texto|20|Texto alfanumérico|
|`AuthorizationCode`|Código de autorização.|Texto|6|Texto alfanumérico|
|`SoftDescriptor`|Texto impresso na fatura bancaria comprador - Exclusivo para VISA/MASTER - não permite caracteres especiais - Ver Anexo|Texto|13|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ECI`|Eletronic Commerce Indicator. Representa o quão segura é uma transação.|Texto|2|Exemplos: 7|
|`Status`|Status da Transação.|Byte|---|2|
|`ReturnCode`|Código de retorno da Adquirência.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da Adquirência.|Texto|512|Texto alfanumérico|
|`tryautomaticcancellation`|Caso ocorra algum erro durante a autorização (status Não Finalizada - "0"), a resposta incluirá o campo “tryautomaticcancellation” como true. Neste caso, a transação será sondada automaticamente, e caso tenha sido autorizada será cancelada automaticamente. Esta funcionalidade deverá estar habilitada para loja. Para habilitar, entre em contato com o nosso suporte técnico. |Booleano|-|true ou false|

### Transação completa

Para criar uma transação que utilizará cartão de crédito, é necessário enviar uma requisição utilizando o método `POST` para o recurso Payment conforme o exemplo. Esse exemplo contempla todos os campos possíveis que podem ser enviados.

#### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014111701",
   "Customer":{  
      "Name":"Comprador crédito completo",
      "Email":"compradorteste@teste.com",
      "Birthdate":"1991-01-02",
      "Address":{  
         "Street":"Rua Teste",
         "Number":"123",
         "Complement":"AP 123",
         "ZipCode":"12345987",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BRA"
      },
        "DeliveryAddress": {
            "Street": "Rua Teste",
            "Number": "123",
            "Complement": "AP 123",
            "ZipCode": "12345987",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        }
   },
   "Payment":{  
     "Currency":"BRL",
     "Country":"BRA",
     "ServiceTaxAmount":0,
     "Installments":1,
     "Interest":"ByMerchant",
     "Capture":true,
     "Authenticate":false,
     "SoftDescriptor":"123456789ABCD",
     "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"123",
         "SaveCard":"false",
         "Brand":"Visa",
         "CardOnFile":{
            "Usage": "Used",
            "Reason":"Unscheduled"
         }
     },
     "IsCryptoCurrencyNegotiation": true,
     "Type":"CreditCard",
     "Amount":15700,
     "AirlineData":{
         "TicketNumber":"AR988983"
     }
   }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2014111701",
   "Customer":{  
      "Name":"Comprador crédito completo",
      "Identity":"11225468954",
      "IdentityType":"CPF",
      "Email":"compradorteste@teste.com",
      "Birthdate":"1991-01-02",
      "Address":{  
         "Street":"Rua Teste",
         "Number":"123",
         "Complement":"AP 123",
         "ZipCode":"12345987",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BRA"
      },
        "DeliveryAddress": {
            "Street": "Rua Teste",
            "Number": "123",
            "Complement": "AP 123",
            "ZipCode": "12345987",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        }
   },
   "Payment":{  
     "ServiceTaxAmount":0,
     "Installments":1,
     "Interest":"ByMerchant",
     "Capture":true,
     "Authenticate":false,
     "SoftDescriptor":"123456789ABCD",
     "CreditCard":{  
         "CardNumber":"4551870000000183",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"123",
         "SaveCard":"false",
         "Brand":"Visa",
         "CardOnFile":{
            "Usage": "Used",
            "Reason":"Unscheduled"
         }
     },
     "IsCryptoCurrencyNegotiation": true,
     "Type":"CreditCard",
     "Amount":15700,
     "AirlineData":{
         "TicketNumber":"AR988983"
     }
   }
}
--verbose
```

|Propriedade|Tipo|Tamanho|Obrigatório|Descrição|
|---|---|---|---|---|
|`MerchantId`|Guid|36|Sim|Identificador da loja na Cielo.|
|`MerchantKey`|Texto|40|Sim|Chave Publica para Autenticação Dupla na Cielo.|
|`RequestId`|Guid|36|Não|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.|
|`MerchantOrderId`|Texto|50|Sim|Numero de identificação do Pedido.|
|`Customer.Name`|Texto|255|Não|Nome do Comprador.|
|`Customer.Status`|Texto|255|Não|Status de cadastro do comprador na loja (NEW / EXISTING)|
|`Customer.Identity`|Texto|14|Não|Número do RG, CPF ou CNPJ do Cliente.|
|`Customer.IdentityType`|Texto|255|Não|Tipo de documento de identificação do comprador (CFP/CNPJ).|
|`Customer.Email`|Texto|255|Não|Email do Comprador.|
|`Customer.Birthdate`|Date|10|Não|Data de nascimento do Comprador.|
|`Customer.Address.Street`|Texto|255|Não|Endereço do Comprador.|
|`Customer.Address.Number`|Texto|15|Não|Número do endereço do Comprador.|
|`Customer.Address.Complement`|Texto|50|Não|Complemento do endereço do Comprador.br|
|`Customer.Address.ZipCode`|Texto|9|Não|CEP do endereço do Comprador.|
|`Customer.Address.City`|Texto|50|Não|Cidade do endereço do Comprador.|
|`Customer.Address.State`|Texto|2|Não|Estado do endereço do Comprador.|
|`Customer.Address.Country`|Texto|35|Não|Pais do endereço do Comprador.|
|`Customer.DeliveryAddress.Street`|Texto|255|Não|Endereço do Comprador.|
|`Customer.Address.Number`|Texto|15|Não|Número do endereço do Comprador.|
|`Customer.DeliveryAddress.Complement`|Texto|50|Não|Complemento do endereço do Comprador.|
|`Customer.DeliveryAddress.ZipCode`|Texto|9|Não|CEP do endereço do Comprador.|
|`Customer.DeliveryAddress.City`|Texto|50|Não|Cidade do endereço do Comprador.|
|`Customer.DeliveryAddress.State`|Texto|2|Não|Estado do endereço do Comprador.|
|`Customer.DeliveryAddress.Country`|Texto|35|Não|Pais do endereço do Comprador.|
|`Payment.Type`|Texto|100|Sim|Tipo do Meio de Pagamento.|
|`Payment.Amount`|Número|15|Sim|Valor do Pedido (ser enviado em centavos).|
|`Payment.Currency`|Texto|3|Não|Moeda na qual o pagamento será feito (BRL).|
|`Payment.Country`|Texto|3|Não|Pais na qual o pagamento será feito.|
|`Payment.Provider`|Texto|15|---|Define comportamento do meio de pagamento (ver Anexo)/NÃO OBRIGATÓRIO PARA CRÉDITO.|
|`Payment.ServiceTaxAmount`|Número|15|Não|[Veja Anexo](https://developercielo.github.io/manual/cielo-ecommerce#service-tax-amount-taxa-de-embarque)|
|`Payment.SoftDescriptor`|Texto|13|Não|Texto impresso na fatura bancaria comprador - Exclusivo para VISA/MASTER - não permite caracteres especiais - Ver Anexo|
|`Payment.Installments`|Número|2|Sim|Número de Parcelas.|
|`Payment.Interest`|Texto|10|Não|Tipo de parcelamento - Loja (ByMerchant) ou Cartão (ByIssuer).|
|`Payment.Capture`|Booleano|---|Não (Default false)|Booleano que identifica que a autorização deve ser com captura automática.|
|`Payment.Authenticate`|Booleano|---|Não (Default false)|Define se o comprador será direcionado ao Banco emissor para autenticação do cartão|
|`Payment.IsCryptocurrencyNegotiation`|Booleano|-|Não (default false)|Deve ser enviado com valor “true” caso se trate de uma transação de compra ou venda de Criptomoeda|
|`Payment.AirlineData.TicketNumber`|alfanumérico|13|Não|Informar o número do principal bilhete aéreo da transação.|
|`CreditCard.CardNumber`|Texto|19|Sim|Número do Cartão do Comprador.|
|`CreditCard.Holder`|Texto|25|Não|Nome do Comprador impresso no cartão.|
|`CreditCard.ExpirationDate`|Texto|7|Sim|Data de validade impresso no cartão.|
|`CreditCard.SecurityCode`|Texto|4|Não|Código de segurança impresso no verso do cartão - Ver Anexo.|
|`CreditCard.SaveCard`|Booleano|---|Não (Default false)|Booleano que identifica se o cartão será salvo para gerar o CardToken.|
|`CreditCard.Brand`|Texto|10|Sim|Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).       |
|`CreditCard.CardOnFile.Usage`|Texto|-|Não|**First** se o cartão foi armazenado e é seu primeiro uso.<br>**Used** se o cartão foi armazenado e ele já foi utilizado anteriormente em outra transação|
|`CreditCard.CardOnFile.Reason`|Texto|-|Condicional|Indica o propósito de armazenamento de cartões, caso o campo "Usage" for "Used".<BR>**Recurring** - Compra recorrente programada (ex. assinaturas)<br>**Unscheduled** - Compra recorrente sem agendamento (ex. aplicativos de serviços)<br>**Installments** - Parcelamento através da recorrência<br>[Veja Mais](https://developercielo.github.io/faq/faq-api-3-0#pagamento-com-credenciais-armazenadas)|

#### Resposta

```json
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador crédito completo",
        "Identity":"11225468954",
        "IdentityType":"CPF",
        "Email": "compradorteste@teste.com",
        "Birthdate": "1991-01-02",
        "Address": {
            "Street": "Rua Teste",
            "Number": "123",
            "Complement": "AP 123",
            "ZipCode": "12345987",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        },
        "DeliveryAddress": {
            "Street": "Rua Teste",
            "Number": "123",
            "Complement": "AP 123",
            "ZipCode": "12345987",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        }
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": true,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "455187******0183",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa",
            "PaymentAccountReference":"92745135160550440006111072222",
         "CardOnFile":{
            "Usage": "Used",
            "Reason":"Unscheduled"
         }
        },
        "IsCryptoCurrencyNegotiation": true,
        "tryautomaticcancellation":true,
        "ProofOfSale": "674532",
        "Tid": "0305020554239",
        "AuthorizationCode": "123456",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId": "24bc8366-fc31-4d6c-8555-17049a836a07",
        "Type": "CreditCard",
        "Amount": 15700,
        "CapturedAmount": 15700,
        "Country": "BRA",
        "AirlineData":{
            "TicketNumber": "AR988983"
        },
        "ExtraDataCollection": [],
        "Status": 2,
        "ReturnCode": "6",
        "ReturnMessage": "Operation Successful",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            }
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador crédito completo",
        "Identity":"11225468954",
        "IdentityType":"CPF",
        "Email": "compradorteste@teste.com",
        "Birthdate": "1991-01-02",
        "Address": {
            "Street": "Rua Teste",
            "Number": "123",
            "Complement": "AP 123",
            "ZipCode": "12345987",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        },
        "DeliveryAddress": {
            "Street": "Rua Teste",
            "Number": "123",
            "Complement": "AP 123",
            "ZipCode": "12345987",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        }
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": true,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "455187******0183",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa",
            "PaymentAccountReference":"92745135160550440006111072222",
         "CardOnFile":{
            "Usage": "Used",
            "Reason":"Unscheduled"
         }
        },
        "IsCryptoCurrencyNegotiation": true,
        "tryautomaticcancellation":true,
        "ProofOfSale": "674532",
        "Tid": "0305020554239",
        "AuthorizationCode": "123456",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId": "24bc8366-fc31-4d6c-8555-17049a836a07",
        "Type": "CreditCard",
        "Amount": 15700,
        "CapturedAmount": 15700,
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 2,
        "ReturnCode": "6",
        "ReturnMessage": "Operation Successful",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            }
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|Texto alfanumérico|
|`Tid`|Id da transação na adquirente.|Texto|20|Texto alfanumérico|
|`AuthorizationCode`|Código de autorização.|Texto|6|Texto alfanumérico|
|`SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais|Texto|13|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ECI`|Eletronic Commerce Indicator. Representa o quão segura é uma transação.|Texto|2|Exemplos: 7|
|`Status`|Status da Transação.|Byte|---|2|
|`ReturnCode`|Código de retorno da Adquirência.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da Adquirência.|Texto|512|Texto alfanumérico|
|`tryautomaticcancellation`|Caso ocorra algum erro durante a autorização (status Não Finalizada - "0"), a resposta incluirá o campo “tryautomaticcancellation” como true. Neste caso, a transação será sondada automaticamente, e caso tenha sido autorizada será cancelada automaticamente. Esta funcionalidade deverá estar habilitada para loja. Para habilitar, entre em contato com o nosso suporte técnico. |Booleano|-|true ou false|
|`Payment.PaymentAccountReference`|O PAR(payment account reference) é o número que associa diferentes tokens a um mesmo cartão. Será retornado pelas bandeiras Master e Visa e repassado para os clientes do e-commerce Cielo. Caso a bandeira não envie a informação o campo não será retornado.|Numérico|29|---|

<aside class="warning">Atenção: Os retornos de autorização estão sujeitos a inserção de novos campos advindos das bandeiras/emissores. Faça sua integração de forma a prever este tipo de comportamento utilizando adequadamente as técnicas de serialização e deserialização de objetos.</aside>

### Transação Autenticada

Para criar uma transação com autenticação que utilizará cartão de crédito, é necessário enviar uma requisição utilizando o método `POST` para o recurso Payment conforme o exemplo.

<aside class="notice"><strong>Autenticação:</strong> Nesta modalidade o portador do cartão é direcionado para o ambiente de autenticação do banco emissor do cartão onde será solicitada a inclusão da senha do cartão.</aside>

#### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{
    "MerchantOrderId":"2014111903",
    "Customer":
    {
        "Name":"Comprador crédito autenticação"
    },
    "Payment":
    {
        "Type":"CreditCard",
        "Amount":15700,
        "Installments":1,
        "Authenticate":true,
        "SoftDescriptor":"123456789ABCD",
        "ReturnUrl":"https://www.cielo.com.br",
        "CreditCard":
        {
            "CardNumber":"1234123412341231",
            "Holder":"Teste Holder",
            "ExpirationDate":"12/2030",
            "SecurityCode":"123",
            "Brand":"Visa",
            "CardOnFile":{
               "Usage": "Used",
               "Reason":"Unscheduled"
            }
        },
        "IsCryptoCurrencyNegotiation": true
    }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2014111903",
   "Customer":{  
      "Name":"Comprador crédito autenticação"
   },
   "Payment":{  
      "Type":"CreditCard",
      "Amount":15700,
      "Installments":1,
      "Authenticate":true,
      "ReturnUrl":"http://www.cielo.com.br",
      "SoftDescriptor":"123456789ABCD",
      "CreditCard":{  
         "CardNumber":"4551870000000183",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"123",
         "Brand":"Visa",
         "CardOnFile":{
            "Usage": "Used",
            "Reason":"Unscheduled"
         }
      },
     "IsCryptoCurrencyNegotiation": true
   }
}
--verbose
```

|Propriedade|Tipo|Tamanho|Obrigatório|Descrição|
|---|---|---|---|---|
|`MerchantId`|Guid|36|Sim|Identificador da loja na Cielo.|
|`MerchantKey`|Texto|40|Sim|Chave Publica para Autenticação Dupla na Cielo.|
|`RequestId`|Guid|36|Não|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.|
|`MerchantOrderId`|Texto|50|Sim|Numero de identificação do Pedido.|
|`Customer.Name`|Texto|255|Não|Nome do Comprador.|
|`Customer.Status`|Texto|255|Não|Status de cadastro do comprador na loja (NEW / EXISTING)|
|`Payment.Type`|Texto|100|Sim|Tipo do Meio de Pagamento.|
|`Payment.Amount`|Número|15|Sim|Valor do Pedido (ser enviado em centavos).|
|`Payment.Provider`|Texto|15|---|Define comportamento do meio de pagamento (ver Anexo)/NÃO OBRIGATÓRIO PARA CRÉDITO.|
|`Payment.Installments`|Número|2|Sim|Número de Parcelas.|
|`Payment.Authenticate`|Booleano|---|Não (Default false)|Define se o comprador será direcionado ao Banco emissor para autenticação do cartão|
|`Payment.IsCryptocurrencyNegotiation`|Booleano|-|Não (default false)|Deve ser enviado com valor “true” caso se trate de uma transação de compra ou venda de Criptomoeda|
|`CreditCard.CardNumber.`|Texto|19|Sim|Número do Cartão do Comprador|
|`CreditCard.Holder`|Texto|25|Não|Nome do Comprador impresso no cartão.|
|`CreditCard.ExpirationDate`|Texto|7|Sim|Data de validade impresso no cartão.|
|`CreditCard.SecurityCode`|Texto|4|Não|Código de segurança impresso no verso do cartão - Ver Anexo.|
|`CreditCard.Brand`|Texto|10|Sim|Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).       |
|`CreditCard.CardOnFile.Usage`|Texto|-|Não|**First** se o cartão foi armazenado e é seu primeiro uso.<br>**Used** se o cartão foi armazenado e ele já foi utilizado anteriormente em outra transação|
|`CreditCard.CardOnFile.Reason`|Texto|-|Condicional|Indica o propósito de armazenamento de cartões, caso o campo "Usage" for "Used".<BR>**Recurring** - Compra recorrente programada (ex. assinaturas)<br>**Unscheduled** - Compra recorrente sem agendamento (ex. aplicativos de serviços)<br>**Installments** - Parcelamento através da recorrência<br>[Veja Mais](https://developercielo.github.io/faq/faq-api-3-0#pagamento-com-credenciais-armazenadas)|

#### Resposta

```json
{
    "MerchantOrderId":"2014111903",
    "Customer":
    {
        "Name":"Comprador crédito autenticação"
    },
    "Payment":
    {
        "ServiceTaxAmount":0,
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":false,
        "Authenticate":true,
        "CreditCard":
        {
            "CardNumber":"123412******1112",
            "Holder":"Teste Holder",
            "ExpirationDate":"12/2030",
            "SaveCard":false,
            "Brand":"Visa",
            "CardOnFile":{
               "Usage": "Used",
               "Reason":"Unscheduled"
            }
        },
        "IsCryptoCurrencyNegotiation": true,
        "AuthenticationUrl":"https://xxxxxxxxxxxx.xxxxx.xxx.xx/xxx/xxxxx.xxxx?id=c5158c1c7b475fdb91a7ad7cc094e7fe",
        "Tid": "1006993069257E521001",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId":"f2dbd5df-c2ee-482f-ab1b-7fee039108c0",
        "Type":"CreditCard",
        "Amount":15700,
        "Currency":"BRL",
        "Country":"BRA",
        "ExtraDataCollection":[],
        "Status":0,
        "ReturnCode": "0",
        "Links":
        [
            {
                "Method":"GET",
                "Rel":"self",
                "Href":"https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{Paymentid}"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId":"2014111903",
    "Customer":
    {
        "Name":"Comprador crédito autenticação"
    },
    "Payment":
    {
        "ServiceTaxAmount":0,
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":false,
        "Authenticate":true,
        "CreditCard":
        {
            "CardNumber":"123412******1112",
            "Holder":"Teste Holder",
            "ExpirationDate":"12/2030",
            "SaveCard":false,
            "Brand":"Visa",
            "CardOnFile":{
               "Usage": "Used",
               "Reason":"Unscheduled"
            }
        },
        "IsCryptoCurrencyNegotiation": true,
        "AuthenticationUrl":"https://xxxxxxxxxxxx.xxxxx.xxx.xx/xxx/xxxxx.xxxx?id=c5158c1c7b475fdb91a7ad7cc094e7fe",
        "Tid": "1006993069257E521001",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId":"f2dbd5df-c2ee-482f-ab1b-7fee039108c0",
        "Type":"CreditCard",
        "Amount":15700,
        "Currency":"BRL",
        "Country":"BRA",
        "ExtraDataCollection":[],
        "Status":0,
        "ReturnCode": "0",
        "Links":
        [
            {
                "Method":"GET",
                "Rel":"self",
                "Href":"https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{Paymentid}"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|Texto alfanumérico|
|`Tid`|Id da transação na adquirente.|Texto|20|Texto alfanumérico|
|`AuthorizationCode`|Código de autorização.|Texto|6|Texto alfanumérico|
|`SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais|Texto|13|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ECI`|Eletronic Commerce Indicator. Representa o quão segura é uma transação.|Texto|2|Exemplos: 7|
|`Status`|Status da Transação.|Byte|---|2|
|`ReturnCode`|Código de retorno da Adquirência.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da Adquirência.|Texto|512|Texto alfanumérico|

## Cartão de Débito

Esse meio de pagamento é liberado automaticamente junto a afiliação de Cielo, podendo ser utilizado com as seguintes bandeiras e bancos:

| MASTERCARD      | VISA            |
|-----------------|-----------------|
| Bradesco        | Bradesco        |
| Banco do Brasil | Banco do Brasil |
| Santander       | Santander       |
| Itaú            | Itaú            |
| CitiBank        | CitiBank        |
| BRB             | N/A             |
| Caixa           | N/A             |
| BancooB         | N/A             |

### Autenticação Débito - 3DS 1.0

Por regra de mercado, todas as transações online realizadas com cartão de débito devem ser autenticadas através de um protocolo chamado 3DS, obrigatoriamente. Atualmente existem 2 versões: 3DS 1.0 e 3DS 2.0.

Na versão 1, o portador será direcionado para o ambiente bancário, onde será desafiado pelo emissor do cartão, digitando a sua senha e concluindo a autenticação. Essa versão não é compatível com dispositivos mobile e ocorrerá desafio em 100% dos casos. Existe a possibilidade de não autenticar transações de débito no e-Commerce, o que é conhecido como “Débito sem Senha”, porém, cabe aos Bancos Emissores do cartão aprovarem tal modelo, pois **não é uma permissão concedida pela Cielo.**

A autenticação é um processo que é mandatório para transações de débito no eCommerce, porém, é possível utilizá-la também para transações de crédito. Fica à critério do lojista autenticar transações de crédito no e-Commerce.

Recentemente, houve o lançamento da nova versão do 3DS 2.0 pelas bandeiras no mercado, permitindo a Cielo e emissores o desenvolvimento dessa solução, que já está disponível para integração. A tendência é que cada vez mais seja utilizada, considerando as suas diversas melhorias e benefícios. Para conhecer o 3DS 2.0, acesse [https://developercielo.github.io/manual/emv3ds](https://developercielo.github.io/manual/emv3ds).

#### MPI – Merchant Plug-in

O Merchant plug-in, conhecido por MPI, é um serviço que permite a realização da chamada de autenticação, integrado e certificado com bandeiras para processamento de autenticação de 3DS. A Cielo permite ao lojista a integração do 3DS 1.0 ou 2.0 através do MPI Interno ou do MPI Externo.

* MPI Interno: serviço já integrado a solução de 3DS Cielo, sem necessidade de integração e/ou contratação adicional. Em caso de utilização de MPI Interno para o 3DS 1.0 siga para a etapa "[Transação Padrão](https://developercielo.github.io/manual/cielo-ecommerce#transa%C3%A7%C3%A3o-padr%C3%A3o)"

* MPI Externo: serviço contratado pelo lojista, sem interferência da Cielo. Muito utilizado quando o lojista já possui um fornecedor de MPI contratado. Em caso de utilização de MPI Externo para o 3DS 1.0, siga a próxima etapa “Autenticação Externa 3DS 1.0”

#### Autenticação Externa – MPI 3DS 1.0

Considerando a escolha por autenticar com 3DS 1.0 utilizando um serviço/fornecedor de MPI contratado (MPI Externo), a Cielo está preparada para receber essas informações na autorização.

##### Criando uma venda com autenticação externa

Para criar uma venda com cartão de crédito ou débito contendo dados de autenticação externa, é necessário enviar uma requisição utilizando o método `POST` para o recurso Payment conforme o exemplo.

###### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{
    "MerchantOrderId":"2014111903",
    "Customer":
    {
        "Name":"Comprador crédito autenticação",
        "Identity":"12345678912",
        "IdentityType":"cpf"
    },
    "Payment":
    {
        "Type":"CreditCard",
        "Amount":15700,
        "Installments":1,
        "Authenticate":true,
        "SoftDescriptor":"123456789ABCD",
        "ReturnUrl":"https://www.cielo.com.br",
        "CreditCard":
        {
            "CardNumber":"1234123412341231",
            "Holder":"Teste Holder",
            "ExpirationDate":"12/2030",
            "SecurityCode":"123",
            "Brand":"Visa"
        },
        "ExternalAuthentication":
        {
            "Cavv":"123456789",
            "Xid":"987654321",
            "Eci":"5"
        }
    }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2014111903",
   "Customer":{  
      "Name":"Comprador crédito autenticação",
      "Identity":"12345678912",
      "IdentityType":"cpf"
   },
   "Payment":{  
      "Type":"CreditCard",
      "Amount":15700,
      "Installments":1,
      "Authenticate":true,
      "ReturnUrl":"http://www.cielo.com.br",
      "SoftDescriptor":"123456789ABCD",
      "CreditCard":{  
         "CardNumber":"4551870000000183",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"123",
         "Brand":"Visa"
      },
      "ExternalAuthentication":{
         "Cavv":"123456789",
         "Xid":"987654321",
         "Eci":"5"
      }
   }
}
--verbose
```

|Propriedade|Tipo|Tamanho|Obrigatório|Descrição|
|---|---|---|---|---|
|`MerchantId`|Guid|36|Sim|Identificador da loja na Cielo.|
|`MerchantKey`|Texto|40|Sim|Chave Publica para Autenticação Dupla na Cielo.|
|`RequestId`|Guid|36|Não|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.|
|`MerchantOrderId`|Texto|50|Sim|Numero de identificação do Pedido.|
|`Customer.Name`|Texto|255|Não|Nome do Comprador.|
|`Customer.Status`|Texto|255|Não|Status de cadastro do comprador na loja (NEW / EXISTING)|
|`Payment.Type`|Texto|100|Sim|Tipo do Meio de Pagamento.|
|`Payment.Amount`|Número|15|Sim|Valor do Pedido (ser enviado em centavos).|
|`Payment.Provider`|Texto|15|---|Define comportamento do meio de pagamento (ver Anexo)/NÃO OBRIGATÓRIO PARA CRÉDITO.|
|`Payment.Installments`|Número|2|Sim|Número de Parcelas.|
|`Payment.Authenticate`|Booleano|---|Não (Default false)|Indica se a transação deve ser autenticada (true) ou não (false). Mesmo para transações autenticadas externamente (fornecedor de autenticação de sua escolha), este campo deve ser enviado com valor “True”, e no nó ExternalAuthentication deve-se enviar os dados retornados pelo mecanismo de autenticação externa escolhido (XID, CAVV e ECI).|
|`Payment.ExternalAuthentication.Cavv`|Texto|28|Sim|O valor Cavv é retornado pelo mecanismo de autenticação.|
|`Payment.ExternalAuthentication.Xid`|Texto|28|Sim|O valor Xid é retornado pelo mecanismo de autenticação.|
|`Payment.ExternalAuthentication.Eci`|Número|1|Sim|O valor Eci é retornado pelo mecanismo de autenticação.|
|`CreditCard.CardNumber.`|Texto|19|Sim|Número do Cartão do Comprador|
|`CreditCard.Holder`|Texto|25|Não|Nome do Comprador impresso no cartão.|
|`CreditCard.ExpirationDate`|Texto|7|Sim|Data de validade impresso no cartão.|
|`CreditCard.SecurityCode`|Texto|4|Não|Código de segurança impresso no verso do cartão - Ver Anexo.|
|`CreditCard.Brand`|Texto|10|Sim|Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).|

###### Resposta

```json
{
    "MerchantOrderId":"2014111903",
    "Customer":
    {
        "Name":"Comprador crédito autenticação",
        "Identity":"12345678912",
        "IdentityType":"cpf"
    },
    "Payment":
    {
        "ServiceTaxAmount":0,
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":false,
        "Authenticate":true,
        "CreditCard":
        {
            "CardNumber":"123412******1112",
            "Holder":"Teste Holder",
            "ExpirationDate":"12/2030",
            "SaveCard":false,
            "Brand":"Visa"
        },
        "AuthenticationUrl":"https://xxxxxxxxxxxx.xxxxx.xxx.xx/xxx/xxxxx.xxxx?id=c5158c1c7b475fdb91a7ad7cc094e7fe",
        "Tid": "1006993069257E521001",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId":"f2dbd5df-c2ee-482f-ab1b-7fee039108c0",
        "Type":"CreditCard",
        "Amount":15700,
        "Currency":"BRL",
        "Country":"BRA",
        "ExtraDataCollection":[],
        "Status":0,
        "ReturnCode":"0",
        "ReturnMessage":"Transacao autorizada"
        "ExternalAuthentication":
        {  
            "Cavv":"123456789",
            "Xid":"987654321",
            "Eci":"5"
        },
        "Links":
        [
            {
                "Method":"GET",
                "Rel":"self",
                "Href":"https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{Paymentid}"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId":"2014111903",
    "Customer":
    {
        "Name":"Comprador crédito autenticação",
        "Identity":"12345678912",
        "IdentityType":"cpf"
    },
    "Payment":
    {
        "ServiceTaxAmount":0,
        "Installments":1,
        "Interest":"ByMerchant",
        "Capture":false,
        "Authenticate":true,
        "CreditCard":
        {
            "CardNumber":"123412******1112",
            "Holder":"Teste Holder",
            "ExpirationDate":"12/2030",
            "SaveCard":false,
            "Brand":"Visa"
        },
        "AuthenticationUrl":"https://xxxxxxxxxxxx.xxxxx.xxx.xx/xxx/xxxxx.xxxx?id=c5158c1c7b475fdb91a7ad7cc094e7fe",
        "Tid": "1006993069257E521001",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId":"f2dbd5df-c2ee-482f-ab1b-7fee039108c0",
        "Type":"CreditCard",
        "Amount":15700,
        "Currency":"BRL",
        "Country":"BRA",
        "ExtraDataCollection":[],
        "Status":0,
        "ReturnCode": "0",
        "ReturnMessage":"Transacao autorizada",
        "ExternalAuthentication":
        {  
            "Cavv":"123456789",
            "Xid":"987654321",
            "Eci":"5"
        },
        "Links":
        [
            {
                "Method":"GET",
                "Rel":"self",
                "Href":"https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{Paymentid}"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|Texto alfanumérico|
|`Tid`|Id da transação na adquirente.|Texto|20|Texto alfanumérico|
|`AuthorizationCode`|Código de autorização.|Texto|6|Texto alfanumérico|
|`SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais|Texto|13|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ECI`|Eletronic Commerce Indicator. Representa o quão segura é uma transação.|Texto|2|Exemplos: 7|
|`Status`|Status da Transação.|Byte|---|2|
|`ReturnCode`|Código de retorno da Adquirência.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da Adquirência.|Texto|512|Texto alfanumérico|

### Transação padrão

Para criar uma venda que utilizará cartão de débito, é necessário fazer um POST para o recurso Payment conforme o exemplo.

> Para realizar uma transação sem autenticação, basta enviar `Authenticate = FALSE`

O exemplo contempla o mínimo de campos necessários a serem enviados para a autorização.

#### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014121201",
   "Customer":{  
      "Name":"Comprador Cartão de débito"
   },
   "Payment":{  
     "Type":"DebitCard",
     "Authenticate": true,
     "Amount":15700,
     "ReturnUrl":"http://www.cielo.com.br",
     "DebitCard":{  
         "CardNumber":"4551870000000183",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"123",
         "Brand":"Visa"
     },
     "IsCryptoCurrencyNegotiation": true
   }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2014121201",
   "Customer":{  
      "Name":"Comprador Cartão de débito"
   },
   "Payment":{  
     "Type":"DebitCard",
     "Authenticate": true,
     "Amount":15700,
     "ReturnUrl":"http://www.cielo.com.br",
     "DebitCard":{  
         "CardNumber":"4551870000000183",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"123",
         "Brand":"Visa"
     },
     "IsCryptoCurrencyNegotiation": true
   }
}
--verbose
```

| Propriedade                | Descrição                                                                                             | Tipo     | Tamanho | Obrigatório        |
|----------------------------|-------------------------------------------------------------------------------------------------------|----------|---------|--------------------|
| `MerchantId`               | Identificador da loja na API Cielo eCommerce.                                                         | Guid     | 36      | Sim                |
| `MerchantKey`              | Chave Publica para Autenticação Dupla na API Cielo eCommerce.                                         | Texto    | 40      | Sim                |
| `RequestId`                | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT | Guid     | 36      | Não                |
| `MerchantOrderId`          | Numero de identificação do Pedido.                                                                    | Texto    | 50      | Sim                |
| `Customer.Name`            | Nome do Comprador.                                                                                    | Texto    | 255     | Não                |
| `Customer.Status`          | Status de cadastro do comprador na loja (NEW / EXISTING) - Utilizado pela análise de fraude           | Texto    | 255     | Não                |
| `Payment.Type`             | Tipo do Meio de Pagamento.                                                                            | Texto    | 100     | Sim                |
| `Payment.Amount`           | Valor do Pedido (ser enviado em centavos).                                                            | Número   | 15      | Sim                |
| `Payment.Authenticate`     | Define se o comprador será direcionado ao Banco emissor para autenticação do cartão                   | Booleano | ---     | Sim (Default TRUE) |
| `Payment.ReturnUrl`        | URI para onde o usuário será redirecionado após o fim do pagamento                                    | Texto    | 1024    | Sim                |
|`Payment.IsCryptocurrencyNegotiation`|Deve ser enviado com valor “true” caso se trate de uma transação de compra ou venda de Criptomoeda|Booleano|-|Não (default false)|
| `DebitCard.CardNumber`     | Número do Cartão do Comprador.                                                                        | Texto    | 19      | Sim                |
| `DebitCard.Holder`         | Nome do Comprador impresso no cartão.                                                                 | Texto    | 25      | Não                |
| `DebitCard.ExpirationDate` | Data de validade impresso no cartão.                                                                  | Texto    | 7       | Sim                |
| `DebitCard.SecurityCode`   | Código de segurança impresso no verso do cartão.                                                      | Texto    | 4       | Não                |
| `DebitCard.Brand`          | Bandeira do cartão.                                                                                   | Texto    | 10      | Sim                |

<aside class="warning">Cartões de Débito, por padrão, devem possuir `Authenticate` como TRUE </aside>

#### Resposta

```json
{
    "MerchantOrderId": "2014121201",
    "Customer": {
        "Name": "Comprador Cartão de débito"
    },
    "Payment": {
        "DebitCard": {
            "CardNumber": "453211******3703",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "IsCryptoCurrencyNegotiation": true,
        "AuthenticationUrl": "https://xxxxxxxxxxxx.xxxxx.xxx.xx/xxx/xxxxx.xxxx?{PaymentId}",
        "Tid": "1006993069207A31A001",
        "PaymentId": "0309f44f-fe5a-4de1-ba39-984f456130bd",
        "Type": "DebitCard",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 0,
        "ReturnCode": "0",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2014121201",
    "Customer": {
        "Name": "Comprador Cartão de débito"
    },
    "Payment": {
        "DebitCard": {
            "CardNumber": "453211******3703",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "IsCryptoCurrencyNegotiation": true,
        "AuthenticationUrl": "https://xxxxxxxxxxxx.xxxxx.xxx.xx/xxx/xxxxx.xxxx?{PaymentId}",
        "Tid": "1006993069207A31A001",
        "PaymentId": "0309f44f-fe5a-4de1-ba39-984f456130bd",
        "Type": "DebitCard",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 0,
        "ReturnCode": "0",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`AuthenticationUrl`|URL para qual o Lojista deve redirecionar o Cliente para o fluxo de Débito.|Texto|56|Url de Autenticação|
|`Tid`|Id da transação na adquirente.|Texto|20|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ReturnUrl`|Url de retorno do lojista. URL para onde o lojista vai ser redirecionado no final do fluxo.|Texto|1024|http://www.urllogista.com.br|
|`Status`|Status da Transação.|Byte|---|0|
|`ReturnCode`|Código de retorno da Adquirência.|Texto|32|Texto alfanumérico|

## Transferência Eletrônica

### Transação Simples

Para criar uma venda de transferência eletronica, é necessário fazer um POST para o recurso Payment conforme o exemplo. Esse exemplo contempla o mínimo de campos necessários a serem enviados para a autorização.

#### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
    "MerchantOrderId":"2017051109",
    "Customer":
    {  
        "Name":"Nome do Comprador",
        "Identity": "12345678909",
        "IdentityType": "CPF",
        "Email": "comprador@cielo.com.br",
        "Address":
        {
             "Street":"Alameda Xingu",
             "Number":"512",
             "Complement":"27 andar",
             "ZipCode":"12345987",
             "City":"São Paulo",
             "State":"SP",
             "Country":"BRA",
             "District":"Alphaville"
         }
  },
    "Payment":
    {  
        "Provider":"Bradesco",
        "Type":"EletronicTransfer",
        "Amount":10000,
        "ReturnUrl":"http://www.cielo.com.br"
    }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
    "MerchantOrderId":"2017051109",
    "Customer":
    {  
        "Name":"Nome do Comprador",
        "Identity": "12345678909",
        "IdentityType": "CPF",
        "Email": "comprador@cielo.com.br",
        "Address":
        {
             "Street":"Alameda Xingu",
             "Number":"512",
             "Complement":"27 andar",
             "ZipCode":"12345987",
             "City":"São Paulo",
             "State":"SP",
             "Country":"BRA",
             "District":"Alphaville"
         }
  },
    "Payment":
    {  
        "Provider":"Bradesco",
        "Type":"EletronicTransfer",
        "Amount":10000,
        "ReturnUrl":"http://www.cielo.com.br"
    }
}
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Numero de identificação do Pedido.|Texto|50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|255|Sim|
|`Customer.Identity`|Número do RG, CPF ou CNPJ do Cliente|Texto|14|Sim|
|`Customer.IdentityType`|Tipo de documento de identificação do comprador (CPF ou CNPJ)|Texto|255|Não|
|`Customer.Status`|Status de cadastro do comprador na loja (NEW / EXISTING) - Utilizado pela análise de fraude|Texto|255|Não|
|`Customer.Email`|Email do comprador|Texto|255|Não|
|`Customer.Address.Street`|Endereço de contato do comprador|Texto|255|Sim|
|`Customer.Address.Number`|Número endereço de contato do comprador|Texto|15|Sim|
|`Customer.Address.Complement`|Complemento do endereço de contato do Comprador|Texto|50|Sim|
|`Customer.Address.ZipCode`|CEP do endereço de contato do comprador|Texto|9|Sim|
|`Customer.Address.City`|Cidade do endereço de contato do comprador|Texto|50|Sim|
|`Customer.Address.State`|Estado do endereço de contato do comprador|Texto|2|Sim|
|`Customer.Address.Country`|Pais do endereço de contato do comprador|Texto|35|Sim|
|`Customer.Address.District`|Bairro do endereço de contato do comprador|Texto|35|Sim|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Payment.Provider`|Define comportamento do meio de pagamento ([Veja Anexo](https://developercielo.github.io/Webservice-3.0/#anexos))/NÃO OBRIGATÓRIO PARA CRÉDITO.|Texto|15|---|

#### Resposta

```json
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador Transferência Eletronica",
    },
    "Payment": {
        "Url": "https://xxx.xxxxxxx.xxx.xx/post/EletronicTransfer/Redirect/{PaymentId}",
        "PaymentId": "765548b6-c4b8-4e2c-b9b9-6458dbd5da0a",
        "Type": "EletronicTransfer",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "Provider": "Bradesco",
        "ExtraDataCollection": [],
        "Status": 0,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador Transferência Eletronica",
    },
    "Payment": {
        "Url": "https://xxx.xxxxxxx.xxx.xx/post/EletronicTransfer/Redirect/{PaymentId}",
        "PaymentId": "765548b6-c4b8-4e2c-b9b9-6458dbd5da0a",
        "Type": "EletronicTransfer",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "Provider": "Bradesco",
        "ExtraDataCollection": [],
        "Status": 0,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`Url`|URL para qual o Lojista deve redirecionar o Cliente para o fluxo de Transferência Eletronica.|Texto|256|Url de Autenticação|
|`Status`|Status da Transação.|Byte|---|0|

## Boleto

### Transação de Boletos

Para criar uma venda cuja a forma de pagamento é boleto, basta fazer um POST conforme o exemplo.

**OBS:** A API suporta boletos registrados e não registrados, sendo o provider o diferenciador entre eles. Sugerimos que valide com seu banco qual o tipo de boleto suportado por sua carteira. A API Aceita apenas boletos **Bradesco** e **Banco do Brasil**

#### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
    "MerchantOrderId":"2014111706",
    "Customer":
    {  
        "Name":"Comprador Teste Boleto",
        "Identity": "1234567890",
        "Address":
        {
          "Street": "Avenida Marechal Câmara",
          "Number": "160",    
          "Complement": "Sala 934",
          "ZipCode" : "22750012",
          "District": "Centro",
          "City": "Rio de Janeiro",
          "State" : "RJ",
          "Country": "BRA"
        }
    },
    "Payment":
    {  
        "Type":"Boleto",
        "Amount":15700,
        "Provider":"INCLUIR PROVIDER",
        "Address": "Rua Teste",
        "BoletoNumber": "123",
        "Assignor": "Empresa Teste",
        "Demonstrative": "Desmonstrative Teste",
        "ExpirationDate": "2020-12-31",
        "Identification": "11884926754",
        "Instructions": "Aceitar somente até a data de vencimento, após essa data juros de 1% dia."
    }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
    "MerchantOrderId":"2014111706",
    "Customer":
    {  
        "Name":"Comprador Teste Boleto",
        "Identity": "1234567890",
        "Address":
        {
          "Street": "Avenida Marechal Câmara",
          "Number": "160",    
          "Complement": "Sala 934",
          "ZipCode" : "22750012",
          "District": "Centro",
          "City": "Rio de Janeiro",
          "State" : "RJ",
          "Country": "BRA"
        }
    },
    "Payment":
    {  
        "Type":"Boleto",
        "Amount":15700,
        "Provider":"INCLUIR PROVIDER",
        "Address": "Rua Teste",
        "BoletoNumber": "123",
        "Assignor": "Empresa Teste",
        "Demonstrative": "Desmonstrative Teste",
        "ExpirationDate": "2020-12-31",
        "Identification": "11884926754",
        "Instructions": "Aceitar somente até a data de vencimento, após essa data juros de 1% dia."
    }
}
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Numero de identificação do Pedido.|Texto|Bradesco: 27<BR>Banco do Brasil: 50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|Bradesco: 34<BR>Banco do Brasil: 60|Não|
|`Customer.Status`|Status de cadastro do comprador na loja(NEW / EXISTING) - Utilizado pela análise de fraude|Texto|255|Não|
|`Customer.Address.ZipCode`|CEP do endereço do Comprador.|Texto|9|Sim|
|`Customer.Address.Country`|Pais do endereço do Comprador.|Texto|35|Sim|
|`Customer.Address.State`|Estado do endereço do Comprador.|Texto|2|Sim|
|`Customer.Address.City`|Cidade do endereço do Comprador.|Texto|Bradesco: 50<BR>Banco do Brasil: 18|Sim|
|`Customer.Address.District`|Bairro do Comprador.|Texto|50|Sim|
|`Customer.Address.Street`|Endereço do Comprador.|Texto|255|Sim|
|`Customer.Address.Number`|Número do endereço do Comprador.|Texto|15|Sim|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Payment.Provider`|Define comportamento do meio de pagamento (ver Anexo)/NÃO OBRIGATÓRIO PARA CRÉDITO.|Texto|15|Sim|
|`Payment.Adress`|Endereço do Cedente.|Texto|255|Não|
|`Payment.BoletoNumber`|Número do Boleto enviado pelo lojista. Usado para contar boletos emitidos ("NossoNumero").|Texto|Bradesco: 11<BR>Banco do Brasil: 9|Não|
|`Payment.Assignor`|Nome do Cedente.|Texto|200|Não|
|`Payment.Demonstrative`|Texto de Demonstrativo.|Texto|255|Não|
|`Payment.ExpirationDate`|Data de expiração do Boleto. Ex. 2020-12-31 |Date|10|Não|
|`Payment.Identification`|Documento de identificação do Cedente.|Texto|14|Não|
|`Payment.Instructions`|Instruções do Boleto.|Texto|255|Não|

#### Resposta

```json
{
    "MerchantOrderId": "2014111706",
    "Customer":
    {
        "Name": "Comprador Boleto Completo",
        "Address":
        {
        "Street": "Av Marechal Camara",
        "Number": "160",
        "ZipCode": "22750012",
        "City": "Rio de Janeiro",
        "State": "RJ",
        "Country": "BRA",
        "District": "Centro"
        }
    },
    "Payment":
    {
        "Instructions": "Aceitar somente até a data de vencimento, após essa data juros de 1% dia.",
        "ExpirationDate": "2015-01-05",
        "Url": "https://apisandbox.cieloecommerce.cielo.com.br/post/pagador/reenvia.asp/a5f3181d-c2e2-4df9-a5b4-d8f6edf6bd51",
        "Number": "123-2",
        "BarCodeNumber": "00096629900000157000494250000000012300656560",
        "DigitableLine": "00090.49420 50000.000013 23006.565602 6 62990000015700",
        "Assignor": "Empresa Teste",
        "Address": "Rua Teste",
        "Identification": "11884926754",
        "PaymentId": "a5f3181d-c2e2-4df9-a5b4-d8f6edf6bd51",
        "Type": "Boleto",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "Provider": "Bradesco",
        "ExtraDataCollection": [],
        "Status": 1,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2014111706",
    "Customer":
    {
        "Name": "Comprador Boleto Completo",
        "Address": {}
    },
    "Payment":
    {
        "Instructions": "Aceitar somente até a data de vencimento, após essa data juros de 1% dia.",
        "ExpirationDate": "2015-01-05",
        "Url": "https://apisandbox.cieloecommerce.cielo.com.br/post/pagador/reenvia.asp/a5f3181d-c2e2-4df9-a5b4-d8f6edf6bd51",
        "Number": "123-2",
        "BarCodeNumber": "00096629900000157000494250000000012300656560",
        "DigitableLine": "00090.49420 50000.000013 23006.565602 6 62990000015700",
        "Assignor": "Empresa Teste",
        "Address": "Rua Teste",
        "Identification": "11884926754",
        "PaymentId": "a5f3181d-c2e2-4df9-a5b4-d8f6edf6bd51",
        "Type": "Boleto",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "Provider": "Bradesco",
        "ExtraDataCollection": [],
        "Status": 1,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`Instructions`|Instruções do Boleto.|Texto|255|Ex: Aceitar somente até a data de vencimento, após essa data juros de 1% dia.|
|`ExpirationDate`|Data de expiração.|Texto|10|2014-12-25|
|`Url`|Url do Boleto gerado.|string|256|Ex:https://.../pagador/reenvia.asp/8464a692-b4bd-41e7-8003-1611a2b8ef2d|
|`Number`|"NossoNumero" gerado.|Texto|50|Ex: 1000000012-8|
|`BarCodeNumber`|Representação numérica do código de barras.|Texto|44|Ex: 00091628800000157000494250100000001200656560|
|`DigitableLine`|Linha digitável.|Texto|256|Ex: 00090.49420 50100.000004 12006.565605 1 62880000015700|
|`Assignor`|Nome do Cedente.|Texto|256|Ex: Loja Teste|
|`Address`|Endereço do Cedente.|Texto|256|Ex: Av. Teste, 160|
|`Identification`|Documento de identificação do Cedente.|Texto|14|CPF ou CNPJ do Cedente sem os caracteres especiais (., /, -)|
|`Status`|Status da Transação.|Byte|---|1|

### Regras Adicionais

Quantidade de caracteres por campo e Provider:

|Propriedade|Observações|Bradesco|Banco do Brasil|
|---|---|---|---|
|`Provider`|N/A|Bradesco2|BancoDoBrasil2|
|`MerchantOrderId`|OBS 1|27|50|
|`Payment.BoletoNumber`|OBS 2|11|9|
|`Customer.Name`|OBS 3|34|60|
|`Customer.Address.Street`|OBS 4|70|OBS 3 / Ver comentário|
|`Customer.Address.Number`|OBS 4|10|OBS 3 / Ver comentário|
|`Customer.Address.Complement`|OBS 4|20|OBS 3 / Ver comentário|
|`Customer.Address.District`|OBS 4|50|OBS 3 / Ver comentário|
|`Customer.Address.City`|N/A|50 - OBS 4|18 - OBS 3|
|`Payment.Instructions`|N/A|255|255|
|`Payment.Demonstrative`|N/A|255|Não é enviado ao banco do Brasil|

> **Comentário Banco Do Brasil**: Os campos `Customer.Address.Street`; `Customer.Address.Number`; `Customer.Address.Complement`; `Customer.Address.District` devem totalizar até 60 caracteres.

|Observações|Bradesco|Banco do Brasil|
|---|---|---|
|**OBS 1:**|Apenas Letras, números e caracteres como "_" e "$"|Não é enviado ao banco|
|**OBS 2:**|O dado é validado pelo banco|Quando enviado acima de 9 posições, a API Cielo trunca automaticamente, considerando os últimos 9 dígitos|
|**OBS 3:**|A API Cielo trunca automaticamente|**Caracteres válidos:** <BR> Letras de A a Z - MAIÚSCULAS <BR> **Caracteres especiais:** hífen (-) e apóstrofo (') <BR><BR> Quando utilizados, não pode conter espaços entre as letras; <BR><BR><BR> **Exemplos corretos**: D'EL-REI, D'ALCORTIVO, SANT'ANA.<BR><BR> **Exemplos incorretos**: D'EL - REI; até um espaço em branco entre palavras|
|**OBS 4:**|O dado é validado pela API Cielo|N/A|

## API QR Code via API E-Commerce

O objetivo desta documentação é orientar o desenvolvedor sobre como integrar com a API E-Commerce da Cielo para gerar o QRCode de Pagamento, descrevendo as funcionalidades, os métodos a serem utilizados, listando informações a serem enviadas e recebidas, e provendo exemplos.

O mecanismo de integração com o Cielo eCommerce é simples, de modo que apenas conhecimentos intermediários em linguagem de programação para Web, requisições HTTP/HTTPS e manipulação de arquivos JSON, são necessários para implantar a solução Cielo eCommerce com sucesso.

Nesse manual você encontrará a referência sobre todas as operações disponíveis na API REST da API Cielo eCommerce. Estas operações devem ser executadas utilizando sua chave específica (Merchant ID e Merchant Key) nos respectivos endpoints dos ambientes:

Ambiente Produção

* **Requisição de transação**: https://api.cieloecommerce.cielo.com.br/
* **Consulta transação**: https://apiquery.cieloecommerce.cielo.com.br/

Ambiente Sandbox

* **Requisição de transação**: https://apisandbox.cieloecommerce.cielo.com.br
* **Consulta transação**: https://apiquerysandbox.cieloecommerce.cielo.com.br

Para executar uma operação, combine a URL base do ambiente com a URL da operação desejada e envie utilizando o verbo HTTP conforme descrito na operação.

### Arquitetura

A integração é realizada através de serviços disponibilizados como Web Services. O modelo empregado é bastante simples: Existem duas URLs (endpoint), uma específica operações que causam efeitos colaterais - como autorização, captura e cancelamento de transações, e uma URL específica para operações que não causam efeitos colaterais, como pesquisa de transações. Essas duas URLs receberão as mensagens HTTP através dos métodos POST, GET ou PUT. Cada tipo de mensagem deve ser enviada para um recurso identificado através do path.

|Método|Descrição|
|---|---|
|**POST**|O método HTTP `POST` é utilizado na criação dos recursos ou no envio de informações que serão processadas. Por exemplo, criação de uma transação.|
|**PUT**|O método HTTP `PUT` é utilizado para atualização de um recurso já existente. Por exemplo, captura ou cancelamento de uma transação previamente autorizada.|
|**GET**|O método HTTP `GET` é utilizado para consultas de recursos já existentes. Por exemplo, consulta de transações.|

### Produtos e Bandeiras suportadas

QR Code de Pagamento possui suporte às seguintes bandeiras e produtos:

| Bandeira         | Crédito à vista | Crédito parcelado Loja | Débito |
|------------------|-----------------|------------------------|--------|
| Visa             | Sim             | Sim                    | *Não*  |
| Master Card      | Sim             | Sim                    | *Não*  |
| American Express | Sim             | Sim                    | *Não*  |
| Elo              | Sim             | Sim                    | *Não*  |
| Diners Club      | Sim             | Sim                    | *Não*  |
| Discover         | Sim             | *Não*                  | *Não*  |
| JCB              | Sim             | Sim                    | *Não*  |
| Aura             | Sim             | Sim                    | *Não*  |
| Hipercard        | Sim             | Sim                    | *Não*  |
| Hiper            | Sim             | Sim                    | *Não*  |

### Sandbox e Ferramentas

#### Sobre o Sandbox

Para facilitar os testes durante a integração, a Cielo oferece um ambiente Sandbox que é composto por duas áreas:

1. Cadastro de conta de testes
2. Endpoints transacionais

|**Requisição**| https://apisandbox.cieloecommerce.cielo.com.br     |
| **Consulta** | https://apiquerysandbox.cieloecommerce.cielo.com.br|

**Vantagens de utilizar o Sandbox**

* Não é necessário uma afiliação para utilizar o Sandbox Cielo.
* Basta acessar o [**Cadastro do Sandbox**](https://cadastrosandbox.cieloecommerce.cielo.com.br/) criar uma conta.
* com o cadastro você receberá um `MerchantId` e um `MerchantKey`,que são as credenciais necessarias para os métodos da API

#### Ferramenta para Integração: POSTMAN

O **Postman** é um API Client que facilita aos desenvolvedores criar, compartilhar, testar e documentar APIs. Isso é feito, permitindo aos usuários criar e salvar solicitações HTTPs simples e complexas, bem como ler suas respostas.

A Cielo oferece coleções completas de suas integrações via Postamn, o que facilita o processo de integração com a API Cielo.

Sugerimos que desenvolvedores acessem nosso [**Tutorial**](https://developercielo.github.io/tutorial/postman) sobre a ferramenta para compreender melhor todas as vantagens que ela oferece.

#### Cartão de crédito - Sandbox

Para testar o cenário de autorização com sucesso via QRCode, utilize o cartão 4551.8700.0000.0183

As informações de **Cód.Segurança (CVV)** e validade podem ser aleatórias, mantendo o formato - CVV (3 dígitos) Validade (MM/YYYY).

### Gerando um QRCode via API

Para criar uma transação que utilizará cartão de crédito, é necessário enviar uma requisição utilizando o método `POST` para o recurso Payment, conforme o exemplo. Esse exemplo contempla o mínimo de campos necessários a serem enviados para a autorização.

<aside class="notice"><strong>Atenção:</strong> Não é possivel realizar uma transação com valor (`Amount`) 0.</aside>

#### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2019010101",
   "Customer":{  
      "Name":"QRCode Test"
   },
   "Payment":{
     "Type":"qrcode",
     "Amount":100,
     "Installments":1,
     "Capture":false
   }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2019010101",
   "Customer":{  
      "Name":"QRCode Test"
   },
   "Payment":{
     "Type":"qrcode",
     "Amount":100,
     "Installments":1,
     "Capture":false
   }
}
--verbose
```

|Propriedade|Tipo|Tamanho|Obrigatório|Descrição|
|---|---|---|---|---|
|`MerchantId`|Guid|36|Sim|Identificador da loja na Cielo.|
|`MerchantKey`|Texto|40|Sim|Chave Publica para Autenticação Dupla na Cielo.|
|`RequestId`|Guid|36|Não|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.|
|`MerchantOrderId`|Texto|50|Sim|Numero de identificação do Pedido.|
|`Customer.Name`|Texto|255|Não|Nome do Comprador.|
|`Payment.Type`|Texto|100|Sim|Tipo do Meio de Pagamento. Enviar **qrcode** para uma transação de QRCode.|
|`Payment.Amount`|Número|15|Sim|Valor do Pedido (ser enviado em centavos).|
|`Payment.Installments`|Número|2|Sim|Número de Parcelas.|
|`Payment.Capture`|Booleano|-|Não|Enviar **true** para uma trasação de captura automática.|

#### Resposta

```json
{
    "MerchantOrderId": "2019010101",
    "Customer": {
        "Name": "QRCode Test"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "Provider": "Cielo",
        "Amount": 100,
        "ReceivedDate": "2019-01-02 10:14:29",
        "Status": 12,
        "IsSplitted": false,
        "QrCode": "iVBORw0KGgoAAAANSUhEUgAAASwAAAEsCAYAAAB5fY51AAAQ1klEQVR42u3de6hlVR(...)",
        "ReturnMessage": "QRCode gerado com sucesso",
        "PaymentId": "5d7e8fd3-70b6-4a88-9660-e064d72fdcdd",
        "Type": "qrcode",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/5d7e8fd3-70b6-4a88-9660-e064d72fdcdd"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2019010101",
    "Customer": {
        "Name": "QRCode Test"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "Provider": "Cielo",
        "Amount": 100,
        "ReceivedDate": "2019-01-02 10:14:29",
        "Status": 12,
        "IsSplitted": false,
        "QrCodeBase64Image": "iVBORw0KGgoAAAANSUhEUgAAASwAAAEsCAYAAAB5fY51AAAQ1klEQVR42u3de6hlVR(...)",
        "ReturnMessage": "QRCode gerado com sucesso",
        "PaymentId": "5d7e8fd3-70b6-4a88-9660-e064d72fdcdd",
        "Type": "qrcode",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/5d7e8fd3-70b6-4a88-9660-e064d72fdcdd"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`QrCodeBase64Image`|QRCode codificado na base 64. Por exemplo, a imagem poderá ser apresentada na página utilizando o código HTML como este:<br> <code>&lt;img src="data:image/png;base64,{código da imagem na base 64}"&gt;</code>|Texto|variável|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido, necessário para futuras operações como Consulta, Captura e Cancelamento.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`Status`|Status da Transação. No caso de uma transação de geração de QRCode de pagamento, o status inicial é 12 (Pending).|Byte|---|2|
|`ReturnCode`|Código de retorno da Adquirência.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da Adquirência.|Texto|512|Texto alfanumérico|

### Consulta - PaymentID

Para consultar uma venda de cartão de crédito, é necessário fazer um GET para o recurso Payment conforme o exemplo.

#### Requisição

<aside class="request"><span class="method get">GET</span> <span class="endpoint">/1/sales/{PaymentId}</span></aside>

```shell
curl
--request GET "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`PaymentId`|Numero de identificação do Pagamento.|Texto|36|Sim|

#### Resposta

```json
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador Teste",
        "Address": {}
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "455187******0183",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "ProofOfSale": "674532",
        "AuthorizationCode": "123456",
        "PaymentId": "24bc8366-fc31-4d6c-8555-17049a836a07",
        "Type": "qrcode",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador Teste",
        "Address": {}
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "455187******0183",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "ProofOfSale": "674532",
        "AuthorizationCode": "123456",
        "PaymentId": "24bc8366-fc31-4d6c-8555-17049a836a07",
        "Type": "CreditCard",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|Texto alfanumérico|
|`AuthorizationCode`|Código de autorização.|Texto|6|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`Status`|Status da Transação.|Byte|---|2|
|`Customer.Name`|Texto|255|Não|Nome do Comprador.|
|`Customer.Status`|Texto|255|Não|Status de cadastro do comprador na loja (NEW / EXISTING)|
|`Payment.Type`|Texto|100|Sim|Tipo do Meio de Pagamento.|
|`Payment.Amount`|Número|15|Sim|Valor do Pedido (ser enviado em centavos).|
|`Payment.Provider`|Texto|15|---|Define comportamento do meio de pagamento (ver Anexo)/NÃO OBRIGATÓRIO PARA CRÉDITO.|
|`Payment.Installments`|Número|2|Sim|Número de Parcelas.|
|`CreditCard.CardNumber`|Texto|19|Sim|Número do Cartão do Comprador.|
|`CreditCard.Holder`|Texto|25|Não|Nome do Comprador impresso no cartão.|
|`CreditCard.ExpirationDate`|Texto|7|Sim|Data de validade impresso no cartão.|
|`CreditCard.SecurityCode`|Texto|4|Não|Código de segurança impresso no verso do cartão - Ver Anexo.|
|`CreditCard.Brand`|Texto|10|Não|Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).|

### Consulta - MerchandOrderID

Não é possível consultar diretamente uma pagamento pelo identificador enviado pela loja (MerchantOrderId), mas é possível obter todos os PaymentIds associados ao identificador.

Para consultar uma venda pelo identificador da loja, é necessário fazer um GET para o recuso sales conforme o exemplo.

#### Requisição

<aside class="request"><span class="method get">GET</span> <span class="endpoint">/1/sales?merchantOrderId={merchantOrderId}</span></aside>

```shell
curls
--request GET " https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales?merchantOrderId={merchantOrderId}"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Campo Identificador do Pedido na Loja.|Texto|36|Sim|

#### Resposta

```json
{
    "Payment": [
        {
            "PaymentId": "5fb4d606-bb63-4423-a683-c966e15399e8",
            "ReceveidDate": "2015-04-06T10:13:39.42"
        },
        {
            "PaymentId": "6c1d45c3-a95f-49c1-a626-1e9373feecc2",
            "ReceveidDate": "2014-12-19T20:23:28.847"
        }
    ]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Payment": [
        {
            "PaymentId": "5fb4d606-bb63-4423-a683-c966e15399e8",
            "ReceveidDate": "2015-04-06T10:13:39.42"
        },
        {
            "PaymentId": "6c1d45c3-a95f-49c1-a626-1e9373feecc2",
            "ReceveidDate": "2014-12-19T20:23:28.847"
        }
    ]
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|

### Captura

A **Captura** é passo exclusivo para transações de Cartões de Crédito.

Ao realizar uma captura, o lojita confirma que o valor autorizado no cartão poderá ser cobrado pela insituição financeira emissora do cartão.

O que a captura gera:

* Ela executa a cobrança do cartão
* Ela inclui o valor da venda na fatura do comprador
* Somente transações capturadas são pagas pela Cielo ao lojista

<aside class="notice"><strong>Atenção:</strong> A captura é um processo com prazo de execução. Verifique em seu cadastro cielo qual o limite habilitado para a sua afiliação. Após esse periodo, não é possivel realiza a Captura da transação</aside>

<aside class="notice"><strong>Atenção:</strong> Captura parcial disponível apenas para transações de crédito</aside>

#### Requisição - Captura Parcial

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/sales/{paymentId}/capture?amount={Valor}</span></aside>

```json

https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{paymentId}/capture?amount={Valor}

```

```shell
curl
--request PUT "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{paymentId}/capture?amount={Valor}"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|Sim|
|`Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Não|
|`ServiceTaxAmount`|[Veja Anexo](https://developercielo.github.io/manual/cielo-ecommerce#service-tax-amount-taxa-de-embarque)|Número|15|Não|

#### Resposta

```json
{
    "Status": 2,
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "6",
    "ProviderReturnMessage": "Operation Successful",
    "ReturnCode": "6",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/8b1d43ee-a918-40d2-ba62-e5665e7ccbd3"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/8b1d43ee-a918-40d2-ba62-e5665e7ccbd3/void"
        }
    ]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Status": 2,
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "6",
    "ProviderReturnMessage": "Operation Successful",
    "ReturnCode": "6",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/8b1d43ee-a918-40d2-ba62-e5665e7ccbd3"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/8b1d43ee-a918-40d2-ba62-e5665e7ccbd3/void"
        }
    ]
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`Status`|Status da Transação.|Byte|---|2|
|`ReasonCode`|Código de retorno da Operação.|Texto|32|Texto alfanumérico|
|`ReasonMessage`|Mensagem de retorno da Operaçãoe.|Texto|512|Texto alfanumérico|
|`ReturnCode`|Código de retorno da adquirente.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da adquirente.|Texto|512|Texto alfanumérico|
|`ProviderReturnCode`|Código de retorno do Provider.|Texto|32|Texto alfanumérico|
|`ProviderReturnMessage`|Mensagem de retorno do Provider.|Texto|512|Texto alfanumérico|

<aside class="notice"><strong>Captura de Taxa de embarque</strong> Para realizar a captura da *taxa de embarque*, basta adicionar o valor do ServiveTaxAmount a ser capturado</aside>

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/sales/{paymentId}/capture?amount={Valor}&serviceTaxAmount=xxx</span></aside>

#### Resposta

```json
{
    "Status": 2,
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "0",
    "ProviderReturnMessage": "Operation Successful",
    "ReturnCode": "0",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8/void"
        }
    ]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Status": 2,
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "0",
    "ProviderReturnMessage": "Operation Successful",
    "ReturnCode": "0",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8/void"
        }
    ]
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`Status`|Status da Transação.|Byte|---|2|
|`ReasonCode`|Código de retorno da Operação.|Texto|32|Texto alfanumérico|
|`ReasonMessage`|Mensagem de retorno da Operaçãoe.|Texto|512|Texto alfanumérico|
|`ReturnCode`|Código de retorno da adquirente.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da adquirente.|Texto|512|Texto alfanumérico|
|`ProviderReturnCode`|Código de retorno do Provider.|Texto|32|Texto alfanumérico|
|`ProviderReturnMessage`|Mensagem de retorno do Provider.|Texto|512|Texto alfanumérico|

### Cancelamento

O **cancelamento** é a operação responsável pela cancelamento total ou parcial de um valor autorizado ou capturado.

Basta realizar um `POST` enviando o valor a ser cancelado.

<aside class="notice"><strong>Atenção:</strong> Cancelamento parcial é disponível apenas para transações *CAPTURADAS*</aside>

<aside class="notice"><strong>Atenção:</strong> O retorno da API soma o total de cancelamentos Parciais, ou seja, se 3 cancelamentos de R$10,00 forem realizados, a API apresentará em seu retorno um total de R$30,00 cancelados</aside>

#### Requisição - cancelamento

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/sales/{PaymentId}/void?amount=XXX </span></aside>

```shell
curl
--request PUT "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void?amount=XXX"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|Sim|
|`Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Não|

#### Resposta

```json
{
    "Status": 2,
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "0",
    "ProviderReturnMessage": "Operation Successful",
    "ReturnCode": "0",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8/void"
        }
    ]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Status": 2,
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "0",
    "ProviderReturnMessage": "Operation Successful",
    "ReturnCode": "0",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8/void"
        }
    ]
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`Status`|Status da Transação.|Byte|---|2|
|`ReasonCode`|Código de retorno da Operação.|Texto|32|Texto alfanumérico|
|`ReasonMessage`|Mensagem de retorno da Operaçãoe.|Texto|512|Texto alfanumérico|
|`ReturnCode`|Código de retorno da adquirente.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da adquirente.|Texto|512|Texto alfanumérico|
|`ProviderReturnCode`|Código de retorno do Provider.|Texto|32|Texto alfanumérico|
|`ProviderReturnMessage`|Mensagem de retorno do Provider.|Texto|512|Texto alfanumérico|

<aside class="notice"><strong>Cancelamento de Taxa de embarque</strong> Para realizar o cancelamento da *taxa de embarque*, basta adicionar o valor do ServiveTaxAmount a ser cancelado</aside>

```
https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{paymentId}/void?amount={Valor}&serviceTaxAmount=xxx
```

## Erros de Integração

Caso ocorram erros de integração em qualquer um dos meios de pagamento, um "response" será retornado contendo um código de erro e uma descrição

### Exemplo

A Data de validade do cartão possui um valor não permitido  como "08/**A**020" e não "08/**2**020", a resposta será:

``` json
[
    {
        "Code": 126,
        "Message": "Credit Card Expiration Date is invalid"
    }
]
```

| Propriedade | Descrição                                                                                                                              |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `Code`      | Código de Erro da API. [Veja a lista de códigos](https://developercielo.github.io/manual/cielo-ecommerce#c%C3%B3digos-de-erros-da-api) |
| `Message`   | Descrição do erro. [Veja a lista de códigos](https://developercielo.github.io/manual/cielo-ecommerce#c%C3%B3digos-de-erros-da-api)     |

# Consulta - Captura - Cancelamento

## Consulta de transações 

### Consulta - PaymentID

Para consultar uma venda de cartão de crédito, é necessário fazer um GET para o recurso Payment conforme o exemplo.

<aside class="notice">São elegíveis para a consulta, apenas transações dentro dos três ultimos meses</aside>

#### Requisição

<aside class="request"><span class="method get">GET</span> <span class="endpoint">/1/sales/{PaymentId}</span></aside>

```shell
curl
--request GET "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`PaymentId`|Numero de identificação do Pagamento.|Texto|36|Sim|

#### Resposta

```json
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador Teste",
        "Address": {}
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "455187******0183",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa",
            "PaymentAccountReference":"92745135160550440006111072222"
        },
        "ProofOfSale": "674532",
        "AuthorizationCode": "123456",
        "Chargebacks": [
            {
                "Amount": 10000,
                "CaseNumber": "123456",
                "Date": "2017-06-04",
                "ReasonCode": "104",
                "ReasonMessage": "Outras Fraudes - Cartao Ausente",
                "Status": "Received",
                "RawData": "Client did not participate and did not authorize transaction"
            }
        ],
        "PaymentId": "24bc8366-fc31-4d6c-8555-17049a836a07",
        "Type": "CreditCard",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador Teste",
        "Address": {}
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "455187******0183",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa",
            "PaymentAccountReference":"92745135160550440006111072222"
        },
        "ProofOfSale": "674532",
        "AuthorizationCode": "123456",
        "Chargebacks": [
            {
                "Amount": 10000,
                "CaseNumber": "123456",
                "Date": "2017-06-04",
                "ReasonCode": "104",
                "ReasonMessage": "Outras Fraudes - Cartao Ausente",
                "Status": "Received",
                "RawData": "Client did not participate and did not authorize transaction"
            }
        ],
        "PaymentId": "24bc8366-fc31-4d6c-8555-17049a836a07",
        "Type": "CreditCard",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|Texto alfanumérico|
|`AuthorizationCode`|Código de autorização.|Texto|6|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`Status`|Status da Transação.|Byte|---|2|
|`Customer.Name`|Texto|255|Não|Nome do Comprador.|
|`Customer.Status`|Texto|255|Não|Status de cadastro do comprador na loja (NEW / EXISTING)|
|`Payment.Type`|Texto|100|Sim|Tipo do Meio de Pagamento.|
|`Payment.Amount`|Número|15|Sim|Valor do Pedido (ser enviado em centavos).|
|`Payment.Provider`|Texto|15|---|Define comportamento do meio de pagamento (ver Anexo)/NÃO OBRIGATÓRIO PARA CRÉDITO.|
|`Payment.Installments`|Número|2|Sim|Número de Parcelas.|
|`Payment.Chargebacks[n].Amount`|Valor do chargeback|Número|15|10000|
|`Payment.Chargebacks[n].CaseNumber`|Número do caso relacionado ao chargeback|Texto|16|Texto alfanumérico|
|`Payment.Chargebacks[n].Date`|Data do chargeback|Date|10|AAAA-MM-DD|
|`Payment.Chargebacks[n].ReasonCode`|Código de motivo do chargeback|Texto|10|Texto alfanumérico|
|`Payment.Chargebacks[n].ReasonMessage`|Mensagem de motivo do chargeback|Texto|512|Texto alfanumérico|
|`Payment.Chargebacks[n].Status`|Status do chargeback <br/> [Lista de Valores - Payment.Chargebacks{n}.Status]({{ site.baseurl_root }}//manual/cielo-ecommerce#lista-de-valores-payment.chargebacks[n].status)|Texto|32|Texto|
|`Payment.Chargebacks[n].RawData`|Titular do cartão ou outra mensagem|Texto|512|Alphanumeric Text|
|`CreditCard.CardNumber`|Texto|19|Sim|Número do Cartão do Comprador.|
|`CreditCard.Holder`|Texto|25|Não|Nome do Comprador impresso no cartão.|
|`CreditCard.ExpirationDate`|Texto|7|Sim|Data de validade impresso no cartão.|
|`CreditCard.SecurityCode`|Texto|4|Não|Código de segurança impresso no verso do cartão - Ver Anexo.|
|`CreditCard.Brand`|Texto|10|Sim|Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).|
|`CreditCard.PaymentAccountReference`|Numérico|29|Não|O PAR(payment account reference) é o número que associa diferentes tokens a um mesmo cartão. Será retornado pelas bandeiras Master e Visa e repassado para os clientes do e-commerce Cielo. Caso a bandeira não envie a informação o campo não será retornado.|

### Consulta - MerchandOrderID

Não é possível consultar diretamente uma pagamento pelo identificador enviado pela loja (MerchantOrderId), mas é possível obter todos os PaymentIds associados ao identificador.

Para consultar uma venda pelo identificador da loja, é necessário fazer um GET para o recuso sales conforme o exemplo.

#### Requisição

<aside class="request"><span class="method get">GET</span> <span class="endpoint">/1/sales?merchantOrderId={merchantOrderId}</span></aside>

```shell
curls
--request GET " https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales?merchantOrderId={merchantOrderId}"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Campo Identificador do Pedido na Loja.|Texto|36|Sim|

#### Resposta

```json
{
    "Payment": [
        {
            "PaymentId": "5fb4d606-bb63-4423-a683-c966e15399e8",
            "ReceveidDate": "2015-04-06T10:13:39.42"
        },
        {
            "PaymentId": "6c1d45c3-a95f-49c1-a626-1e9373feecc2",
            "ReceveidDate": "2014-12-19T20:23:28.847"
        }
    ]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Payment": [
        {
            "PaymentId": "5fb4d606-bb63-4423-a683-c966e15399e8",
            "ReceveidDate": "2015-04-06T10:13:39.42"
        },
        {
            "PaymentId": "6c1d45c3-a95f-49c1-a626-1e9373feecc2",
            "ReceveidDate": "2014-12-19T20:23:28.847"
        }
    ]
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|

### Consulta Recorrência

Para consultar uma Recorrência de cartão de crédito, é necessário fazer um `GET`  conforme o exemplo.

**A Consulta da Recorrência traz dados sobre o agendamento e sobre o processo de transações que se repetem. Elas não retornam dados sobre as transações em si. Para isso, deve ser realizado um `GET` na transação (Disponível em "Consultando vendas)** 

#### Requisição

<aside class="request"><span class="method get">GET</span> <span class="endpoint">/1/RecurrentPayment/{RecurrentPaymentId}</span></aside>

```shell
curl
--request GET "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`RecurrentPaymentId`|Campo Identificador da Recorrência.|Texto|36|Sim|

#### Resposta

```json
{
    "Customer": {
        "Name": "Fulano da Silva"
    },
    "RecurrentPayment": {
        "RecurrentPaymentId": "c30f5c78-fca2-459c-9f3c-9c4b41b09048",
        "NextRecurrency": "2017-06-07",
        "StartDate": "2017-04-07",
        "EndDate": "2017-02-27",
        "Interval": "Bimonthly",
        "Amount": 15000,
        "Country": "BRA",
        "CreateDate": "2017-04-07T00:00:00",
        "Currency": "BRL",
        "CurrentRecurrencyTry": 1,
        "Provider": "Simulado",
        "RecurrencyDay": 7,
        "SuccessfulRecurrences": 0,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/c30f5c78-fca2-459c-9f3c-9c4b41b09048"
            }
        ],
        "RecurrentTransactions": [
            {
                "PaymentId": "f70948a8-f1dd-4b93-a4ad-90428bcbdb84",
                "PaymentNumber": 0,
                "TryNumber": 1
            }
        ],
        "Status": 1,
        "IssuerTransactionId": "009295034362939"
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Customer": {
        "Name": "Fulano da Silva"
    },
    "RecurrentPayment": {
        "RecurrentPaymentId": "c30f5c78-fca2-459c-9f3c-9c4b41b09048",
        "NextRecurrency": "2017-06-07",
        "StartDate": "2017-04-07",
        "EndDate": "2017-02-27",
        "Interval": "Bimonthly",
        "Amount": 15000,
        "Country": "BRA",
        "CreateDate": "2017-04-07T00:00:00",
        "Currency": "BRL",
        "CurrentRecurrencyTry": 1,
        "Provider": "Simulado",
        "RecurrencyDay": 7,
        "SuccessfulRecurrences": 0,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/c30f5c78-fca2-459c-9f3c-9c4b41b09048"
            }
        ],
        "RecurrentTransactions": [
            {
                "PaymentId": "f70948a8-f1dd-4b93-a4ad-90428bcbdb84",
                "PaymentNumber": 0,
                "TryNumber": 1
            }
        ],
        "Status": 1,
        "IssuerTransactionId": "009295034362939"
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`RecurrentPaymentId`|Campo Identificador da próxima recorrência.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`NextRecurrency`|Data da próxima recorrência.|Texto|7|12/2030 (MM/YYYY)|
|`StartDate`|Data do inicio da recorrência.|Texto|7|12/2030 (MM/YYYY)|
|`EndDate`|Data do fim da recorrência.|Texto|7|12/2030 (MM/YYYY)|
|`Interval`|Intervalo entre as recorrência.|Texto|10|/ Monthly / Bimonthly  / Quarterly  / SemiAnnual / Annual |
|`CurrentRecurrencyTry `|Indica o número de tentativa da recorrência atual|Número|1|1|
|`OrderNumber`|Identificado do Pedido na loja|Texto|50|2017051101|
|`Status`|Status do pedido recorrente|Número|1|<br>*1* - Ativo <br>*2* - Finalizado <br>*3*- Desativada pelo Lojista <br> *4* - Desativada por numero de retentativas <BR> *5* - Desativada por cartão de crédito vencido|
|`RecurrencyDay`|O dia da recorrência|Número|2|22|
|`SuccessfulRecurrences`|Quantidade de recorrência realizada com sucesso|Número|2|5|
|`RecurrentTransactions.RecurrentPaymentId`|Id da Recorrência|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`RecurrentTransactions.TransactionId`|Payment ID da transação gerada na recorrência|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`RecurrentTransactions.PaymentNumber`|Número da Recorrência. A primeira é zero|Número|2|3|
|`RecurrentTransactions.TryNumber`|Número da tentativa atual na recorrência específica|Número|2|1|
|`Payment.IssuerTransactionId`|Identificado de autenticação do Emissor para transações de débito recorrentes. Este campo deve ser enviado nas transações subsequentes da primeira transação no modelo de recorrência própria. Já no modelo de recorrência programada, a Cielo será a responsável por enviar o campo nas transações subsequentes.|Texto|15|---|

**Atenção:** O campo `IssuerTransactionId` também pode ser obtido através da consulta da primeira transação da recorrência. Ver detalhes de como fazer uma consulta [**aqui**](https://developercielo.github.io/manual/cielo-ecommerce#consulta-de-transa%C3%A7%C3%B5es).

## Captura

A **Captura** é passo exclusivo para transações de Cartões de Crédito.

Ao realizar uma captura, o lojita confirma que o valor autorizado no cartão poderá ser cobrado pela insituição financeira emissora do cartão.

O que a captura gera:

* Ela executa a cobrança do cartão
* Ela inclui o valor da venda na fatura do comprador
* Somente transações capturadas são pagas pela Cielo ao lojista

<aside class="notice"><strong>Atenção:</strong> A captura é um processo com prazo de execução. Verifique em sem cadastro cielo qual o limite habilitado para a sua afiliação. Após esse periodo, não é possivel realiza a Captura da transação</aside>

### Captura total

Para captura uma venda que utiliza cartão de crédito, é necessário fazer um PUT para o recurso Payment conforme o exemplo.

#### Requisição

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/sales/{PaymentId}/capture</span></aside>

```json

https://api.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture

```

```shell
curl
--request PUT "https://api.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|Sim|
|`Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Não|
|`ServiceTaxAmount`|[Veja Anexo](https://developercielo.github.io/manual/cielo-ecommerce#service-tax-amount-taxa-de-embarque)|Número|15|Não|

#### Resposta

```json
{
    "Status": 2,
    "Tid": "0719094510712",
    "ProofOfSale": "4510712",
    "AuthorizationCode": "693066",
    "ReturnCode": "6",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://api.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://api.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
        }
    ]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Status": 2,
    "Tid": "0719094510712",
    "ProofOfSale": "4510712",
    "ReturnCode": "6",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://api.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://api.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
        }
    ]
}
```

| Propriedade             | Descrição                               | Tipo  | Tamanho | Formato            |
|-------------------------|-----------------------------------------|-------|---------|--------------------|
| `Status`                | Status da Transação.                    | Byte  | ---     | 2                  |
| `ProofOfSale`           | Número da autorização, identico ao NSU. | Texto | 6       | Texto alfanumérico |
| `Tid`                   | Id da transação na adquirente.          | Texto | 20      | Texto alfanumérico |
| `AuthorizationCode`     | Código de autorização.                  | Texto | 6       | Texto alfanumérico |
| `ReturnCode`            | Código de retorno da adquirente.        | Texto | 32      | Texto alfanumérico |
| `ReturnMessage`         | Mensagem de retorno da adquirente.      | Texto | 512     | Texto alfanumérico |

### Captura parcial

A **captura parcial** é o ato de capturar um valor menor que o valor autorizado.Esse modelo de captura pode ocorrer apenas 1 vez por transação. 

**Após a captura, não é possível realizar capturas adicionais no mesmo pedido.**

Basta realizar um `POST` enviando o valor a ser capturado.

<aside class="notice"><strong>Atenção:</strong> Captura parcial disponível apenas para transações de crédito</aside>

#### Requisição - Captura Parcial

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/sales/{paymentId}/capture?amount={Valor}</span></aside>

```json

https://api.cieloecommerce.cielo.com.br/1/sales/{paymentId}/capture?amount={Valor}

```

```shell
curl
--request PUT "https://api.cieloecommerce.cielo.com.br/1/sales/{paymentId}/capture?amount={Valor}"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|Sim|
|`Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Não|
|`ServiceTaxAmount`|[Veja Anexo](https://developercielo.github.io/manual/cielo-ecommerce#service-tax-amount-taxa-de-embarque)|Número|15|Não|

#### Resposta

```json
{
    "Status": 2,
    "Tid": "0719094510712",
    "ProofOfSale": "4510712",
    "AuthorizationCode": "693066",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "6",
    "ProviderReturnMessage": "Operation Successful",
    "ReturnCode": "6",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://api.cieloecommerce.cielo.com.br/1/sales/8b1d43ee-a918-40d2-ba62-e5665e7ccbd3"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://api.cieloecommerce.cielo.com.br/1/sales/8b1d43ee-a918-40d2-ba62-e5665e7ccbd3/void"
        }
    ]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Status": 2,
    "Tid": "0719094510712",
    "ProofOfSale": "4510712",
    "AuthorizationCode": "693066",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "6",
    "ProviderReturnMessage": "Operation Successful",
    "ReturnCode": "6",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://api.cieloecommerce.cielo.com.br/1/sales/8b1d43ee-a918-40d2-ba62-e5665e7ccbd3"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://api.cieloecommerce.cielo.com.br/1/sales/8b1d43ee-a918-40d2-ba62-e5665e7ccbd3/void"
        }
    ]
}
```

| Propriedade             | Descrição                               | Tipo  | Tamanho | Formato            |
|-------------------------|-----------------------------------------|-------|---------|--------------------|
| `Status`                | Status da Transação.                    | Byte  | ---     | 2                  |
| `ProofOfSale`           | Número da autorização, identico ao NSU. | Texto | 6       | Texto alfanumérico |
| `Tid`                   | Id da transação na adquirente.          | Texto | 20      | Texto alfanumérico |
| `AuthorizationCode`     | Código de autorização.                  | Texto | 6       | Texto alfanumérico |
| `ReasonCode`            | Código de retorno da Operação.          | Texto | 32      | Texto alfanumérico |
| `ReasonMessage`         | Mensagem de retorno da Operação.        | Texto | 512     | Texto alfanumérico |
| `ProviderReturnCode`    | Código de retorno do Provider.          | Texto | 32      | Texto alfanumérico |
| `ProviderReturnMessage` | Mensagem de retorno do Provider.        | Texto | 512     | Texto alfanumérico ||
| `ReturnCode`            | Código de retorno da adquirente.        | Texto | 32      | Texto alfanumérico |
| `ReturnMessage`         | Mensagem de retorno da adquirente.      | Texto | 512     | Texto alfanumérico |

<aside class="notice"><strong>Captura de Taxa de embarque</strong> Para realizar a captura da *taxa de embarque*, basta adicionar o valor do ServiveTaxAmount a ser capturado</aside>

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/sales/{paymentId}/capture?amount={Valor}&serviceTaxAmount=xxx</span></aside>

#### Resposta

```json
{
    "Status": 2,
    "Tid": "0719094510712",
    "ProofOfSale": "4510712",
    "AuthorizationCode": "693066",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "0",
    "ProviderReturnMessage": "Operation Successful",
    "ReturnCode": "0",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://api.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://api.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8/void"
        }
    ]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Status": 2,
    "Tid": "0719094510712",
    "ProofOfSale": "4510712",
    "AuthorizationCode": "693066",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "0",
    "ProviderReturnMessage": "Operation Successful",
    "ReturnCode": "0",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://api.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://api.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8/void"
        }
    ]
}
```

| Propriedade             | Descrição                               | Tipo  | Tamanho | Formato            |
|-------------------------|-----------------------------------------|-------|---------|--------------------|
| `Status`                | Status da Transação.                    | Byte  | ---     | 2                  |
| `ProofOfSale`           | Número da autorização, identico ao NSU. | Texto | 6       | Texto alfanumérico |
| `Tid`                   | Id da transação na adquirente.          | Texto | 20      | Texto alfanumérico |
| `AuthorizationCode`     | Código de autorização.                  | Texto | 6       | Texto alfanumérico |
| `ReasonCode`            | Código de retorno da Operação.          | Texto | 32      | Texto alfanumérico |
| `ReasonMessage`         | Mensagem de retorno da Operação.        | Texto | 512     | Texto alfanumérico |
| `ProviderReturnCode`    | Código de retorno do Provider.          | Texto | 32      | Texto alfanumérico |
| `ProviderReturnMessage` | Mensagem de retorno do Provider.        | Texto | 512     | Texto alfanumérico ||
| `ReturnCode`            | Código de retorno da adquirente.        | Texto | 32      | Texto alfanumérico |
| `ReturnMessage`         | Mensagem de retorno da adquirente.      | Texto | 512     | Texto alfanumérico ||

### Captura Via Backoffice

É possivel realizar tanto a captura total quanto a Captura parcial via O Backoffice Cielo.

Acesse nosso [**Tutorial**](https://developercielo.github.io/Tutorial//Backoffice-3.0)  para maiores informações

## Cancelando uma venda

### Cancelando uma venda via API

O processo de cancelamento via API está disponivel apenas para cartão de crédito e débito. 

Cada meio de pagamento sofre impactos diferentes quando uma ordem de cancelamento (VOID) é executada.

|Meio de pagamento|Descrição|Prazo|Participação Cielo|
|---|---|---|---|
|Cartão de crédito|A Cielo devolve o valor via crédito bancario|300 dias pós autorização|Sim|
|Cartão de Débito|Cancelamento apenas na API. O retorno do valor é feito pelo proprio lojista|300 dias pós autorização|Não|

### Cancelamento total

Para cancelar uma venda que utiliza cartão de crédito, é necessário fazer um PUT para o recurso Payment. É possível realizar o cancelamento via PaymentID ou MerchantOrderId (numero do pedido).

<aside class="notice"><strong>Atenção:</strong> O cancelamento por MerchantOrderId afeta sempre a transação mais nova, ou seja, caso haja pedidos com o numero do pedido duplicado, somente o mais atual será cancelado. O pedido anterior não poderá ser cancelado por esse método</aside>

#### Requisição

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/sales/{PaymentId}/void?amount=xxx</span></aside>

ou

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/sales/OrderId/{MerchantOrderId}/void?amount=xxx</span></aside>

```shell
curl
--request PUT "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|Sim|
|`Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Não|

#### Resposta

```json
{
    "Status": 10,
    "Tid": "0719094510712",
    "ProofOfSale": "4510712",
    "AuthorizationCode": "693066",
    "ReturnCode": "9",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
        }
    ]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Status": 10,
    "Tid": "0719094510712",
    "ProofOfSale": "4510712",
    "AuthorizationCode": "693066",
    "ReturnCode": "9",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
        }
    ]
}
```

| Propriedade             | Descrição                               | Tipo  | Tamanho | Formato            |
|-------------------------|-----------------------------------------|-------|---------|--------------------|
| `Status`                | Status da Transação.                    | Byte  | ---     | 2                  |
| `ProofOfSale`           | Número da autorização, identico ao NSU. | Texto | 6       | Texto alfanumérico |
| `Tid`                   | Id da transação na adquirente.          | Texto | 20      | Texto alfanumérico |
| `AuthorizationCode`     | Código de autorização.                  | Texto | 6       | Texto alfanumérico |
| `ReturnCode`            | Código de retorno da adquirente.        | Texto | 32      | Texto alfanumérico |
| `ReturnMessage`         | Mensagem de retorno da adquirente.      | Texto | 512     | Texto alfanumérico |

### Cancelamento parcial

O **cancelamento  parcial** é o ato de cancelar um valor menor que o valor total autorizado/capturado. Esse modelo de cancelamento pode ocorrer inumeras vezes, até que o valor total da transação seja cancelado. 

 Basta realizar um `POST` enviando o valor a ser cancelado.

<aside class="notice"><strong>Atenção:</strong> Cancelamento parcial disponível apenas para transações *CAPTURADAS*</aside>

<aside class="notice"><strong>Atenção:</strong> O retorno da API soma o total de cancelamentos Parciais, ou seja, se 3 cancelamentos de R$10,00 forem realizados, a API apresentará em seu retorno um total de R$30,00 cancelados</aside>

#### Requisição - cancelamento parcial

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/sales/{PaymentId}/void?amount=XXX </span></aside>

```shell
curl
--request PUT "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void?amount=XXX"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|Sim|
|`Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Não|

#### Resposta

```json
{
    "Status": 2,
    "Tid": "0719094510712",
    "ProofOfSale": "4510712",
    "AuthorizationCode": "693066",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "0",
    "ProviderReturnMessage": "Operation Successful",
    "ReturnCode": "0",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8/void"
        }
    ]
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "Status": 2,
    "Tid": "0719094510712",
    "ProofOfSale": "4510712",
    "AuthorizationCode": "693066",
    "ReasonCode": 0,
    "ReasonMessage": "Successful",
    "ProviderReturnCode": "0",
    "ProviderReturnMessage": "Operation Successful",
    "ReturnCode": "0",
    "ReturnMessage": "Operation Successful",
    "Links": [
        {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8"
        },
        {
            "Method": "PUT",
            "Rel": "void",
            "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/4d7be764-0e81-4446-b31e-7eb56bf2c9a8/void"
        }
    ]
}
```

| Propriedade             | Descrição                               | Tipo  | Tamanho | Formato            |
|-------------------------|-----------------------------------------|-------|---------|--------------------|
| `Status`                | Status da Transação.                    | Byte  | ---     | 2                  |
| `ProofOfSale`           | Número da autorização, identico ao NSU. | Texto | 6       | Texto alfanumérico |
| `Tid`                   | Id da transação na adquirente.          | Texto | 20      | Texto alfanumérico |
| `AuthorizationCode`     | Código de autorização.                  | Texto | 6       | Texto alfanumérico |
| `ReturnCode`            | Código de retorno da adquirente.        | Texto | 32      | Texto alfanumérico |
| `ReturnMessage`         | Mensagem de retorno da adquirente.      | Texto | 512     | Texto alfanumérico |
| `ProviderReturnCode`    | Código de retorno do Provider.          | Texto | 32      | Texto alfanumérico |
| `ProviderReturnMessage` | Mensagem de retorno do Provider.        | Texto | 512     | Texto alfanumérico |

<aside class="notice"><strong>Cancelamento de Taxa de embarque</strong> Para realizar o cancelamento da *taxa de embarque*, basta adicionar o valor do ServiveTaxAmount a ser cancelado</aside>

```
https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{paymentId}/void?amount={Valor}&serviceTaxAmount=xxx
```

### Cancelamento via Backoffice

O Cancelamento via Backoffice é a unica opção para realizar o cancelamento de transações de Débito Online.
É possivel realizar tanto o cancelamento total quanto o Cancelamento parcial via O Backoffice Cielo.

Efeitos sobre o meio de pagamento

|Meio de pagamento|Descrição|Prazo|Participação Cielo|
|---|---|---|---|
|Transferência Eletrônica|Cancelamento apenas na API. O retorno do valor é feito pelo proprio lojista|Definido pelo lojista|Não|

Acesse nosso [**Tutorial**](https://developercielo.github.io/Tutorial//Backoffice-3.0)  para maiores informações

## Post de Notificação

### Sobre o POST

A API Cielo e-commerce oferece um sistema de notificação transacional, onde o Lojista fornece um endpoint que receberá uma notificação via `POST`

O Conteudo da notificação será formado por 3 campos:

* `RecurrentPaymentId`- Identificador que representa um conjunto de transações recorrentes 
* `PaymentId`- Identificador que representa a transação
* `ChangeType` - Especifica o tipo de notificação

Com os dados acima, o lojista poderá identificar a transação (via `PaymentId` ou `RecurrentPaymentId`) e a mudança sofrida por ela. Com o `PaymentId` é possivel realizar uma consulta a base transacional da API Cielo E-commerce 

O Post de notificação é enviado com base em uma seleção de eventos  pré-definidos no cadastro da API Cielo E-commerce.
Esses eventos são cadastros pela equipe de suporte Cielo, quando requisitado pelo lojista

### Eventos Notificados

Os eventos passiveis de notificação são:

| Meio de Pagamento            | Evento                                                                   |
|------------------------------|--------------------------------------------------------------------------|
|**Cartão de Crédito**         | Captura                                                                  |
|**Cartão de Crédito**         | Cancelamento                                                             |
|**Cartão de Crédito**         | Sondagem                                                                 |
|**Boleto**                    | Conciliação                                                              |
|**Boleto**                    | Cancelamento Manual                                                      |
|**Transferência eletrônica**  | Confirmadas                                                              |

**Sobre Cartão de débito:** Não notificamos transações de Cartão de débito. Sugerimos que seja criada uma URL de RETORNO, onde o comprador será enviado se a transação for finalizada no ambiente do banco. Quando essa URL for acionada, nossa sugestão é que um `GET` seja executado, buscando informações do pedido na API Cielo</aside>

<br>

A notificação ocorre tambem ocorre em eventos relacionados a **Recorrência Programada Cielo**

| Eventos da Recorrência                                                   |
|--------------------------------------------------------------------------|
| Desabilitado ao atingir número máximo de tentativas (transações negadas) |
| Reabilitação                                                             |
| Finalizado / Data de finalização atingida                                |
| Desativação                                                              |

### Endpoint de Notificação

Uma `URL Status Pagamento` deve ser cadastrada pelo Suporte Cielo, para que o POST de notificação seja executado. 

Características da `URL Status Pagamento` 

* Deve ser **estática**
* Limite de 255  carácteres.

Características do **Post de notificação** 

* É disparado a cada 30 minutos
* Em caso de falha, 3 novas tentativas são realizadas.Se as 3 tentativas falharem, novos envios não ocorrerão.

É possivel cadastrar uma informação para retorno do header da requisição. Basta entrar em contato com o Suporte Cielo e informar os itens abaixo

* `KEY` - Nome do paramêtro 
* `VALUE` - Valor estático a ser retornado

Você pode cadastrar até 3 tipos de informação de retorno no header

```json
{
   "RecurrentPaymentId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
   "PaymentId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
   "ChangeType": "2"
}
```

```shell
curl
--header "key: value"
{
   "RecurrentPaymentId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
   "PaymentId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
   "ChangeType": "2"
}
```

A loja **deverá** retornar como resposta ao notificação: **HTTP Status Code 200 OK**

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`RecurrentPaymentId`|Identificador que representa o pedido Recorrente (aplicável somente para ChangeType 2 ou 4)|GUID|36|Não|
|`PaymentId`|Identificador que representa a transação|GUID|36|Sim|
|`ChangeType`|Especifica o tipo de notificação. Vide tabela abaixo|Número|1|Sim|

|ChangeType|Descrição|
|---|---|
|1|Mudança de status do pagamento|
|2|Recorrência criada|
|3|Mudança de status do Antifraude|
|4|Mudança de status do pagamento recorrente (Ex. desativação automática)|
|5|cancelamento negado|
|7|Notificação de chargeback <br/> Para mais detalhes [Risk Notification](https://braspag.github.io//manual/risknotification)|

# Análise de Fraude (AF)

A API e-commerce Cielo oferece um serviço de analise de risco de fraudes em transações online. A Cielo se integra a empresas de analise de risco, como CyberSource, que realizam uma validação dos dados transacionais e do historico de compras do portador do cartão.
Essa analise retorna fatores de risco e permite que o lojista tome a decisão se dará continuidade a venda.

<aside class="warning">A análise de fraude oferecida pela Cielo avalia o risco de uma transação, mas não vincula o resultado da analise com a cobertura de ChargeBacks. A Cielo não realiza transações "garantidas"</aside>

Para utilizar o AF, é necessario que o serviço seja ativado junto a Cielo.
Existem 3 tipos de configurações de analise de fraude disponiveis:

|Tipo|Descrição|Fornecedor|
|-|-|-|
|**SuperMID Sem BPO**|Regras de analise definidas pela Cielo <BR> Não é possivel costumizar as regras no fornecedor <BR> Não há analista de risco dedicado |CyberSource|
|**SuperMID Com BPO**|Regras de analise definidas pela Cielo <BR> Não é possivel costumizar as regras no fornecedor <BR> Analista de risco dedicado contratado pelo lojista junto ao fornecedor|CyberSource|
|**Hierarquia ou Enterprise**|O Lojista tem um contrato fechado diretamente com o fornecedor da AF, com regras especificas para analise. Cielo deve configurar as credenciais fornecidas pelo Fornecedor da AF no API Cielo E-commerce|CyberSource|

> A Analise de Fraude está disponivel apenas para transações de cartão de crédito.

## Integração

Para criar uma venda com cartão de crédito e analise de fraude, é necessário enviar uma requisição utilizando o método `POST` para o recurso Payment conforme o exemplo.

### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"201411173454307",
   "Customer":{  
      "Name":"Comprador crédito AF",
      "Identity":"12345678909",
      "IdentityType":"CPF",
      "Email":"compradorteste@live.com",
      "Birthdate":"1991-01-02",
      "Mobile":"5521995760078",
      "Phone":"552125553669",
      "Address":{  
         "Street":"Rua Júpter",
         "Number":"174",
         "Complement":"AP 201",
         "ZipCode":"21241140",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BR",
         "District":"Alphaville"
      },
      "DeliveryAddress":{  
         "Street":"Rua Júpter",
         "Number":"174",
         "Complement":"AP 201",
         "ZipCode":"21241140",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BR",
         "District":"Alphaville"
      },
      "BillingAddress":{
         "Street":"Rua Júpter",
         "Number":"174",
         "Complement":"AP 201",
         "ZipCode":"21241140",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BR",
         "District":"Alphaville"
      }
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":100,
     "Currency":"BRL",
     "Country":"BRA",
     "ServiceTaxAmount":0,
     "Installments":1,
     "Interest":"ByMerchant",
     "Capture":false,
     "Authenticate":false,
     "SoftDescriptor":"Mensagem",
     "CreditCard":{  
         "CardNumber":"4024007197692931",
         "Holder":"Teste accept",
         "ExpirationDate":"12/2030",
         "SecurityCode":"023",
         "Brand":"Visa",
         "SaveCard":"false"
     },
     "FraudAnalysis":{
       "Provider":"cybersource",
       "Sequence":"AuthorizeFirst",
       "SequenceCriteria":"OnSuccess",
       "CaptureOnLowRisk":false,
       "VoidOnHighRisk":false,
       "TotalOrderAmount":10000,
       "FingerPrintId":"074c1ee676ed4998ab66491013c565e2",
       "Browser":{
         "CookiesAccepted":false,
         "Email":"compradorteste@live.com",
         "HostName":"Teste",
         "IpAddress":"200.190.150.350",
         "Type":"Chrome"
        },
       "Cart":{
         "IsGift":false,
         "ReturnsAccepted":true,
         "Items":[{
           "GiftCategory":"Undefined",
           "HostHedge":"Off",
           "NonSensicalHedge":"Off",
           "ObscenitiesHedge":"Off",
           "PhoneHedge":"Off",
           "Name":"ItemTeste",
           "Quantity":1,
           "Sku":"201411170235134521346",
           "UnitPrice":123,
           "Risk":"High",
           "TimeHedge":"Normal",
           "Type":"AdultContent",
           "VelocityHedge":"High",
           "Passenger":{
             "Email":"compradorteste@live.com",
             "Identity":"1234567890",
             "Name":"Comprador accept",
             "Rating":"Adult",
             "Phone":"999994444",
             "Status":"Accepted"
            }
           }]
       },
       "MerchantDefinedFields":[{
            "Id":95,
            "Value":"Eu defini isso"
        }],
        "Shipping":{
            "Addressee":"Sr Comprador Teste",
            "Method":"LowCost",
            "Phone":"21114740"
        },
        "Travel":{
            "DepartureTime":"2010-01-02",
            "JourneyType":"Ida",
            "Route":"MAO-RJO",
          "Legs":[{
                "Destination":"GYN",
                "Origin":"VCP"
          }]
        }
     }
  }
}
```

| Propriedade                                   | Tipo     | Tamanho | Obrigatório         | Descrição                                                                                                                         |
|-----------------------------------------------|----------|---------|---------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| `MerchantId`                                  | Guid     | 36      | Sim                 | Identificador da loja na Cielo.                                                                                                   |
| `MerchantKey`                                 | Texto    | 40      | Sim                 | Chave Publica para Autenticação Dupla na Cielo.                                                                                   |
| `RequestId`                                   | Guid     | 36      | Não                 | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.                            |
| `MerchantOrderId`                             | Texto    | 50      | Sim                 | Numero de identificação do Pedido.                                                                                                |
| `Customer.Name`                               | Texto    | 255     | Sim                 | Nome do Comprador.                                                                                                                |
| `Customer.Identity`                           | Texto    | 14      | Sim                 | Número do RG, CPF ou CNPJ do Cliente.                                                                                             |
| `Customer.IdentityType`                       | Texto    | 24      | Sim                 | Tipo de documento de identificação do comprador (CPF ou CNPJ).                                                                    |
| `Customer.Status`                             | Texto    | 255     | Não                 | Status de cadastro do comprador na loja (NEW / EXISTING)                                                                          |
| `Customer.Email`                              | Texto    | 255     | Não                 | Email do Comprador.                                                                                                               |
| `Customer.Birthdate`                          | Date     | 10      | Não                 | Data de nascimento do Comprador no formato AAAA-MM-DD.                                                                            |
| `Customer.Address.Street`                     | Texto    | 54      | Não                 | Endereço do Comprador.                                                                                                            |
| `Customer.Address.Number`                     | Texto    | 5       | Não                 | Número do endereço do Comprador.                                                                                                  |
| `Customer.Address.Complement`                 | Texto    | 14      | Não                 | Complemento do endereço do Comprador.                                                                                             |
| `Customer.Address.ZipCode`                    | Texto    | 9       | Não                 | CEP do endereço do Comprador.                                                                                                     |
| `Customer.Address.City`                       | Texto    | 50      | Não                 | Cidade do endereço do Comprador.                                                                                                  |
| `Customer.Address.State`                      | Texto    | 2       | Não                 | Estado do endereço do Comprador.                                                                                                  |
| `Customer.Address.Country`                    | Texto    | 2       | Não                 | Pais do endereço do Comprador.                                                                                                    |
| `Customer.Address.District`                   | Texto    | 45      | Não                 | Bairro do Comprador.                                                                                                              |
| `Customer.DeliveryAddress.Street`             | Texto    | 54      | Não                 | Endereço do Comprador.                                                                                                            |
| `Customer.DeliveryAddress.Number`             | Texto    | 5       | Não                 | Número do endereço do Comprador.                                                                                                  |
| `Customer.DeliveryAddress.Complement`         | Texto    | 14      | Não                 | Complemento do endereço do Comprador.                                                                                             |
| `Customer.DeliveryAddress.ZipCode`            | Texto    | 9       | Não                 | CEP do endereço do Comprador.                                                                                                     |
| `Customer.DeliveryAddress.City`               | Texto    | 50      | Não                 | Cidade do endereço do Comprador.                                                                                                  |
| `Customer.DeliveryAddress.State`              | Texto    | 2       | Não                 | Estado do endereço do Comprador.                                                                                                  |
| `Customer.DeliveryAddress.Country`            | Texto    | 2       | Não                 | Pais do endereço do Comprador.                                                                                                    |
| `Customer.DeliveryAddress.District`           | Texto    | 45      | Não                 | Bairro do Comprador.                                                                                                              |
| `Customer.BillingAddress.Street`              | Texto    | 54      | Não                 | Endereço de cobrança do Comprador.                                                                                                |
| `Customer.BillingAddress.Number`              | Texto    | 5       | Não                 | Número do endereço de cobrança do Comprador.                                                                                      |
| `Customer.BillingAddress.Complement`          | Texto    | 14      | Não                 | Complemento do endereço de cobrança do Comprador.                                                                                 |
| `Customer.BillingAddress.ZipCode`             | Texto    | 9       | Não                 | CEP do endereço de cobrança do Comprador.                                                                                         |
| `Customer.BillingAddress.City`                | Texto    | 50      | Não                 | Cidade do endereço de cobrança do Comprador.                                                                                      |
| `Customer.BillingAddress.State`               | Texto    | 2       | Não                 | Estado do endereço de cobrança do Comprador.                                                                                      |
| `Customer.BillingAddress.Country`             | Texto    | 2       | Não                 | Pais do endereço de cobrança do Comprador.                                                                                        |
| `Customer.BillingAddress.District`            | Texto    | 45      | Não                 | Bairro do Comprador.                                                                                                              |
| `Payment.Provider`                            | Texto    | 15      | ---                 | Define comportamento do meio de pagamento (ver Anexo)/NÃO OBRIGATÓRIO PARA CRÉDITO.                                               |
| `Payment.Type`                                | Texto    | 100     | Sim                 | Tipo do Meio de Pagamento.                                                                                                        |
| `Payment.Amount`                              | Número   | 15      | Sim                 | Valor do Pedido (ser enviado em centavos).                                                                                        |
| `Payment.Currency`                            | Texto    | 3       | Não                 | Moeda na qual o pagamento será feito (BRL).                |
| `Payment.Country`                             | Texto    | 3       | Não                 | Pais na qual o pagamento será feito.                                                                                              |
| `Payment.ServiceTaxAmount`                    | Número   | 15      | Sim                 | Montante do valor da autorização que deve ser destinado à taxa de serviço. Veja Anexo                                             |
| `Payment.Installments`                        | Número   | 2       | Sim                 | Número de Parcelas.                                                                                                               |
| `Payment.Interest`                            | Texto    | 10      | Não                 | Tipo de parcelamento - Loja (ByMerchant) ou Cartão (ByIssuer).                                                                    |
| `Payment.Capture`                             | Booleano | ---     | Não                 | Booleano que identifica que a autorização deve ser com captura automática. (Default false)                                        |
| `Payment.Authenticate`                        | Booleano | ---     | Não                 | Define se o comprador será direcionado ao Banco emissor para autenticação do cartão (Default false)                               |
| `Payment.SoftDescriptor`                      | Texto    | 13      | Não                 | Texto que será impresso na fatura do portador                                                                                     |
| `CreditCard.CardNumber`                       | Texto    | 16      | Sim                 | Número do Cartão do Comprador.                                                                                                    |
| `CreditCard.Holder`                           | Texto    | 25      | Sim                 | Nome do Comprador impresso no cartão.                                                                                             |
| `CreditCard.ExpirationDate`                   | Texto    | 7       | Sim                 | Data de validade impresso no cartão.                                                                                              |
| `CreditCard.SecurityCode`                     | Texto    | 4       | Sim                 | Código de segurança impresso no verso do cartão - Ver Anexo.                                                                      |
| `CreditCard.SaveCard`                         | Booleano | ---     | Não                 | Booleano que identifica se o cartão será salvo para gerar o CardToken.  (Default false)                                           |
| `CreditCard.Brand`                            | Texto    | 10      | Sim                 | Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper                               |
| `CreditCard.SaveCard`                         | Booleano | ---     | Não (Default false) | Booleano que identifica se o cartão será salvo para gerar o token (CardToken)                                                     |
| `FraudAnalysis.Provider`                      | Texto    | 14      | Sim                 | Fixo "cybersource"                                                                                                                |
| `FraudAnalysis.Sequence`                      | Texto    | 14      | Sim                 | Tipo de Fluxo para realização da análise de fraude. Primeiro Analise (AnalyseFirst) ou Primeiro Autorização (AuthorizeFirst)      |
| `FraudAnalysis.SequenceCriteria`              | Texto    | 9       | Sim                 | Critério do fluxo. **OnSuccess** - Só realiza a análise se tiver sucesso na transação. **Always** - Sempre realiza a análise      |
| `FraudAnalysis.CaptureOnLowRisk`              | Booleano | ---     | Não                 | Quando true, a autorização deve ser com captura automática quando o risco de fraude for considerado baixo (Accept). Em casos de Reject ou Review, o fluxo permanece o mesmo, ou seja, a captura acontecerá conforme o valor especificado no parâmetro “Capture”. Para a utilização deste parâmetro, a sequência do fluxo de análise de risco deve ser obrigatoriamente “AuthorizeFirst”. Por depender do resultado de análise de risco, este parâmetro só terá efeito quando o serviço de Antifraude for contratado.|
| `FraudAnalysis.VoidOnHighRisk`                | Booleano | ---     | Não                 | Quando true, o estorno deve acontecer automaticamente quando o risco de fraude for considerado alto (Reject). Em casos de Accept ou Review, o fluxo permanece o mesmo, ou seja, o estorno deve ser feito manualmente. Para a utilização deste parâmetro, a sequência do fluxo de análise de risco deve ser obrigatoriamente “AuthorizeFirst”. Por depender do resultado de análise de risco, este parâmetro só terá efeito quando o serviço de Antifraude for contratado.|
| `FraudAnalysis.TotalOrderAmount`              | Número   | 15      | Sim                 | Valor total do pedido.                                                                                                            |
| `FraudAnalysis.FingerPrintId`                 | Texto    | 50      | Sim                 | Identificador utilizado para cruzar informações obtidas pelo Browser do internauta com os dados enviados para análise. Este mesmo valor deve ser passado na variável SESSIONID do script do DeviceFingerPrint.|
| `FraudAnalysis.Browser.CookiesAccepted`       | Booleano | ---     | Sim                 | Booleano para identificar se o browser do cliente aceita cookies.                                                                 |
| `FraudAnalysis.Browser.Email`                 | Texto    | 100     | Não                 | E-mail registrado no browser do comprador.                                                                                        |
| `FraudAnalysis.Browser.HostName`              | Texto    | 60      | Não                 | Nome do host onde o comprador estava antes de entrar no site da loja.                                                             |
| `FraudAnalysis.Browser.IpAddress`             | Texto    | 15      | Sim                 | Endereço IP do comprador. É altamente recomendável o envio deste campo.                                                           |
| `FraudAnalysis.Browser.Type`                  | Texto    | 40      | Não                 | Nome do browser utilizado pelo comprador.                                                                                         |
| `FraudAnalysis.Cart.IsGift`                   | Booleano | ---     | Não                 | Booleano que indica se o pedido é para presente ou não.                                                                           |
| `FraudAnalysis.Cart.ReturnsAccepted`          | Booleano | ---     | Não                 | Booleano que define se devoluções são aceitas para o pedido.                                                                      |
| `FraudAnalysis.Cart.Items.GiftCategory`       | Texto    | 9       | Não                 | Campo que avaliará os endereços de cobrança e entrega para diferentes cidades, estados ou países.<br>_“Yes”_(Em caso de divergência entre endereços de cobrança e entrega, marca como risco pequeno)<br>_“No”_(Em caso de divergência entre endereços de cobrança e entrega, marca com risco alto)<br>_“Off”_(Ignora a análise de risco para endereços divergentes)|
| `FraudAnalysis.Cart.Items.HostHedge`          | Texto    | ---     | Não                 | Nível de importância do e-mail e endereços IP dos clientes em risco de pontuação.<br>_“Low”_(Baixa importância do e-mail e endereço IP na análise de risco)<br>_“Normal”_(Média importância do e-mail e endereço IP na análise de risco)<br>_“High”_(Alta importância do e-mail e endereço IP na análise de risco)<br>_“Off”_(E-mail e endereço IP não afetam a análise de risco)|
| `FraudAnalysis.Cart.Items.NonSensicalHedge`   | Texto    | 6       | Não                 | Nível dos testes realizados sobre os dados do comprador com pedidos recebidos sem sentido.<br>_“Low”_(Baixa importância da verificação feita sobre o pedido do comprador, na análise de risco)<br>_“Normal”_(Média importância da verificação feita sobre o pedido do comprador, na análise de risco)<br>_“High”_(Alta importância da verificação feita sobre o pedido do comprador, na análise de risco)<br>_“Off”_(Verificação do pedido do comprador não afeta a análise de risco)|
| `FraudAnalysis.Cart.Items.ObscenitiesHedge`   | Texto    | 6       | Não                 | Nível de obscenidade dos pedidos recebedidos.<br>_“Low”_(Baixa importância da verificação sobre obscenidades do pedido do comprador, na análise de risco)<br>_“Normal”_(Média importância da verificação sobre obscenidades do pedido do comprador, na análise de risco)<br>_“High”_(Alta importância da verificação sobre obscenidades do pedido do comprador, na análise de risco)<br>_“Off”_(Verificação de obscenidade no pedido do comprador não afeta a análise de risco)|
| `FraudAnalysis.Cart.Items.PhoneHedge`         | Texto    | 6       | Não                 | Nível dos testes realizados com os números de telefones.<br>_“Low”_(Baixa importância nos testes realizados com números de telefone)<br>_“Normal”_(Média importância nos testes realizados com números de telefone)<br>_“High”_(Alta importância nos testes realizados com números de telefone)<br>_“Off”_(Testes de números de telefone não afetam a análise de risco)|
| `FraudAnalysis.Cart.Items.Name`               | Texto    | 255     | Sim                 | Nome do Produto.                                                                                                                  |
| `FraudAnalysis.Cart.Items.Quantity`           | Número   | 15      | Sim                 | Quantidade do produto a ser adquirido.                                                                                            |
| `FraudAnalysis.Cart.Items.Sku`                | Texto    | 255     | Sim                 | Código comerciante identificador do produto.                                                                                      |
| `FraudAnalysis.Cart.Items.UnitPrice`          | Número   | 15      | Sim                 | Preço unitário do produto.                                                                                                        |
| `FraudAnalysis.Cart.Items.Risk`               | Texto    | 6       | Não                 | Nível do risco do produto.<br>Low (O produto tem um histórico de poucos chargebacks)<br>Normal (O produto tem um histórico de chargebacks considerado normal)<br>High (O produto tem um histórico de chargebacks acima da média)|
| `FraudAnalysis.Cart.Items.TimeHedge`          | Texto    | -       | Não                 | Nível de importância da hora do dia do pedido do cliente.<br>Low (Baixa importância no horário do dia em que foi feita a compra, para a análise de risco)<br>Normal (Média importância no horário do dia em que foi feita a compra, para a análise de risco)<br>High (Alta importância no horário do dia em que foi feita a compra, para a análise de risco)<br>Off (O horário da compra não afeta a análise de risco)|
| `FraudAnalysis.Cart.Items.Type`               | Texto    | -       | Não                 | Tipo do produto.<br>AdultContent(Conteúdo adulto)<br>Coupon(Cupon de desconto)<br>Default(Opção padrão para análise na CyberSource quando nenhum outro valor é selecionado)<br>EletronicGood(Produto eletrônico)<br>EletronicSoftware(Softwares distribuídos eletronicamente via download)<br>GiftCertificate(Vale presente)<br>HandlingOnly(Taxa de instalação ou manuseio)<br>Service(Serviço)<br>ShippingAndHandling(Frete e taxa de instalação ou manuseio)<br>ShippingOnly(Frete)<br>Subscription(Assinatura)|
| `FraudAnalysis.Cart.Items.VelocityHedge`      | Texto    | 6       | Não                 | Nível de importância de frequência de compra do cliente.<br>Low (Baixa importância no número de compras realizadas pelo cliente nos últimos 15 minutos)<br>Normal (Média importância no número de compras realizadas pelo cliente nos últimos 15 minutos)<br>High (Alta importância no número de compras realizadas pelo cliente nos últimos 15 minutos)<br>Off (A frequência de compras realizadas pelo cliente não afeta a análise de fraude)|
| `FraudAnalysis.Cart.Items.Passenger.Email`    | Texto    | 255     | Não                 | Email do Passageiro.                                                                                                              |
| `FraudAnalysis.Cart.Items.Passenger.Identity` | Texto    | 32      | Não                 | Id do passageiro a quem o bilheite foi emitido.                                                                                   |
| `FraudAnalysis.Cart.Items.Passenger.Name`     | Texto    | 120     | Não                 | Nome do passageiro.                                                                                                               |
| `FraudAnalysis.Cart.Items.Passenger.Rating`   | Texto    | -       | Não                 | Classificação do Passageiro.<br>Valores do Campo:<br>“Adult” (Passageiro adulto)<br>“Child”(Passageiro criança)<br>“Infant”(Passageiro infantil)<br>“Youth”(Passageiro adolescente)<br>“Student”(Passageiro estudante)<br>“SeniorCitizen“(Passageiro idoso)<br>“Military“(Passageiro militar)|
| `FraudAnalysis.Cart.Items.Passenger.Phone`    | Texto    | 15      | Não                 | Número do telefone do passageiro. Para pedidos fora do U.S., a CyberSource recomenda que inclua o código do país. 552133665599 (Ex. Código do Pais 55, Código da Cidade 21, Telefone 33665599)|
| `FraudAnalysis.Cart.Items.Passenger.Status`   | Texto    | 32      | Não                 | Classificação da empresa aérea. Pode-se usar valores como Gold ou Platina.                                                        |
| `FraudAnalysis.MerchantDefinedFields.Id`      | Texto    | ---     | Sim (se aplicável)  | Id das informações adicionais a serem enviadas.                                                                                   |
| `FraudAnalysis.MerchantDefinedFields.Value`   | Texto    | 255     | Sim (se aplicável)  | Valor das informações adicionais a serem enviadas.                                                                                |
| `FraudAnalysis.Shipping.Addressee`            | Texto    | 255     | Não                 | Nome do destinatário da entrega.                                                                                                  |
| `FraudAnalysis.Shipping.Method`               | Texto    | -       | Não                 | Tipo de serviço de entrega do produto.<br>“SameDay” (Serviço de entrega no mesmo dia)<br>“OneDay” (Serviço de entrega noturna ou no dia seguinte)<br>“TwoDay” (Serviço de entrega em dois dias)<br>“ThreeDay” (Serviço de entrega em três dias)<br>“LowCost” (Serviço de entrega de baixo custo)<br>“Pickup” (Produto retirado na loja)<br>“Other” (Outro método de entrega)<br>“None” (Sem serviço de entrega, pois é um serviço ou assinatura)|
| `FraudAnalysis.Shipping.Phone`                | Texto    | 15      | Não                 | Telefone do destinatário da entrega. Ex. 552133665599 (Código do Pais 55, Código da Cidade 21, Telefone 33665599)                 |
| `FraudAnalysis.Travel.JourneyType`            | Texto    | 32      | Sim, caso o nó Travel seja enviado.| Tipo de viagem.                                                                                                    |
| `FraudAnalysis.Travel.DepartureTime`          | DateTime | 23      | Não                 | Data, hora e minuto de partida do vôo. Ex: “2018-01-09 18:00:00”                                                                  |
| `FraudAnalysis.Travel.Passengers.Name`        | Texto    | 120     | Sim, caso o nó Travel seja enviado.| Nome do passageiro.                                                                                                |
| `FraudAnalysis.Travel.Passengers.Identity`    | Texto    | 32      | Sim, caso o nó Travel seja enviado.| Número do RG, CPF ou CNPJ do passageiro.                                                                           |
| `FraudAnalysis.Travel.Passengers.Status`      | Texto    | 32      | Sim, caso o nó Travel seja enviado.| Classificação da companhia aérea. Valores do campo: “Gold”, “Platina”.                                             |
| `FraudAnalysis.Travel.Passengers.Rating`      | Texto    | 15      | Sim, caso o nó Travel seja enviado.| Classificação do Passageiro.<br>Valores do Campo:<br>“Adult” (Passageiro adulto)<br>“Child”(Passageiro criança)<br>“Infant”(Passageiro infantil)<br>“Youth”(Passageiro adolescente)<br>“Student”(Passageiro estudante)<br>“SeniorCitizen“(Passageiro idoso)<br>“Military“(Passageiro militar)|
| `FraudAnalysis.Travel.Passengers.Email`       | Texto    | 255     | Sim, caso o nó Travel seja enviado.| E-mail do passageiro.                                                                                              |
| `FraudAnalysis.Travel.Passengers.Phone`       | Texto    | 15      | Não                 | Número do telefone do passageiro. Para pedidos fora do U.S., a CyberSource recomenda que inclua o código do país. 552133665599 (Ex. Código do Pais 55, Código da Cidade 21, Telefone 33665599)|
| `FraudAnalysis.Travel.Passengers.TravelLegs.Origin`|Texto| 3       | Sim, caso o nó Travel seja enviado.| Código do aeroporto do ponto de origem da viagem.                                                                  |
| `FraudAnalysis.Travel.Passengers.TravelLegs.Destination`|Texto| 3  | Sim, caso o nó Travel seja enviado.| Código do aeroporto do ponto de destino da viagem.                                                                 |

### Resposta

```json
{
    "MerchantOrderId":"201411173454307",
    "Customer": {
        "Name":"Comprador crédito AF",
        "Identity":"12345678909",
        "IdentityType":"CPF",
        "Email":"compradorteste@live.com",
        "Birthdate":"1991-01-02",
        "Phone": "552125553669",
        "Address": {
            "Street":"Rua Júpter",
            "Number":"174",
            "Complement":"AP 201",
            "ZipCode":"21241140",
            "City":"Rio de Janeiro",
            "State":"RJ",
            "Country":"BRA",
            "District":"Alphaville"
        },
        "DeliveryAddress": {
            "Street":"Rua Júpter",
            "Number":"174",
            "Complement":"AP 201",
            "ZipCode":"21241140",
            "City":"Rio de Janeiro",
            "State":"RJ",
            "Country":"BRA",
            "District":"Alphaville"
        },
        "BillingAddress": {
            "Street":"Rua Júpter",
            "Number":"174",
            "Complement":"AP 201",
            "ZipCode":"21241140",
            "City":"Rio de Janeiro",
            "State":"RJ",
            "Country":"BRA",
            "District":"Alphaville"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "402400******2931",
            "Holder": "Teste accept",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "ProofOfSale": "492115",
        "Tid": "10069930692606D31001",
        "AuthorizationCode": "123456",
        "SoftDescriptor":"123456789ABCD",
        "FraudAnalysis": {
        "Provider":"cybersource",
            "Sequence": "AuthorizeFirst",
            "SequenceCriteria": "OnSuccess",
            "FingerPrintId": "074c1ee676ed4998ab66491013c565e2",
            "MerchantDefinedFields": [
                {
                    "Id": 95,
                    "Value": "Eu defini isso"
                }
            ],
            "Cart": {
                "IsGift": false,
                "ReturnsAccepted": true,
                "Items": [
                    {
                        "Type": "AdultContent",
                        "Name": "ItemTeste",
                        "Risk": "High",
                        "Sku": "201411170235134521346",
                        "UnitPrice": 123,
                        "Quantity": 1,
                        "HostHedge": "Off",
                        "NonSensicalHedge": "Off",
                        "ObscenitiesHedge": "Off",
                        "PhoneHedge": "Off",
                        "TimeHedge": "Normal",
                        "VelocityHedge": "High",
                        "GiftCategory": "Undefined",
                        "Passenger": {
                            "Name": "Comprador accept",
                            "Identity": "1234567890",
                            "Status": "Accepted",
                            "Rating": "Adult",
                            "Email": "compradorteste@live.com",
                            "Phone": "999994444"
                        }
                    }
                ]
            },
            "Travel": {
                "Route": "MAO-RJO",
                "DepartureTime": "2010-01-02T00:00:00",
                "JourneyType": "Ida",
                "Legs": [
                    {
                        "Destination": "GYN",
                        "Origin": "VCP"
                    }
                ]
            },
            "Browser": {
                "HostName": "Teste",
                "CookiesAccepted": false,
                "Email": "compradorteste@live.com",
                "Type": "Chrome",
                "IpAddress": "200.190.150.350"
            },
            "Shipping": {
                "Addressee": "Sr Comprador Teste",
                "Phone": "21114740",
                "Method": "LowCost"
            },
            "Id": "0e4d0a3c-e424-4fa5-a573-4eabbd44da42",
            "Status": 1,
            "ReplyData": {
                "AddressInfoCode": "COR-BA^MM-BIN",
                "FactorCode": "B^D^R^Z",
                "Score": 42,
                "BinCountry": "us",
                "CardIssuer": "FIA CARD SERVICES, N.A.",
                "CardScheme": "VisaCredit",
                "HostSeverity": 1,
                "InternetInfoCode": "FREE-EM^RISK-EM",
                "IpRoutingMethod": "Undefined",
                "ScoreModelUsed": "default_lac",
                "CasePriority": 3
            }
        },
        "PaymentId": "04096cfb-3f0a-4ece-946c-3b7dc5d38f19",
        "Type": "CreditCard",
        "Amount": 100,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "ReturnCode": "4",
        "ReturnMessage": "Transação autorizada",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

| Propriedade               | Descrição                                                                                                                                          | Tipo   | Tamanho | Formato                                             |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|--------|---------|-----------------------------------------------------|
| `ProofOfSale`             | Número da autorização, identico ao NSU.                                                                                                            | Texto  | 6       | Texto alfanumérico                                  |
| `Tid`                     | Id da transação na adquirente.                                                                                                                     | Texto  | 20      | Texto alfanumérico                                  |
| `AuthorizationCode`       | Código de autorização.                                                                                                                             | Texto  | 6       | Texto alfanumérico                                  |
| `SoftDescriptor`          | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais                     | Texto  | 13      | Texto alfanumérico                                  |
| `PaymentId`               | Campo Identificador do Pedido.                                                                                                                     | Guid   | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx                |
| `Id`                      | Indentificação da Transação no Antifraud.                                                                                                          | Texto  | 300     | Texto alfanumérico                                  |
| `Status`                  | Status da Transação.                                                                                                                               | Byte   | ---     | 2                                                   |
| `Score`                   | Score total calculado para o pedido. Vai de 0 - 100 -  Quanto mais elevado, maior o risco.                                                         | Número | ---     | Número                                              |
| `BinCountry`              | Sigla do país de origem da compra.                                                                                                                 | Texto  | 2       | us                                                  |
| `CardIssuer`              | Nome do banco ou entidade emissora do cartão.                                                                                                      | Texto  | 128     | Bradesco                                            |
| `ScoreModelUsed`          | Nome do modelo de score utilizado.                                                                                                                 | Texto  | 20      | Ex: default_lac                                     |
| `CasePriority`            | Caso o lojista seja assinante do Enhanced Case Management, ele recebe este valor com o nível de prioridade, sendo 1 o mais alto e 5 o mais baixo.  | Número | ---     | 3                                                   |
| `ReturnCode`              | Código de retorno da Adquirência.                                                                                                                  | Texto  | 32      | Texto alfanumérico                                  |
| `ReturnMessage`           | Mensagem de retorno da Adquirência.                                                                                                                | Texto  | 512     | Texto alfanumérico                                  |
| `HostSeverity`            | Nível de risco do domínio de e-mail do comprador, de 0 a 5, onde 0 é risco indeterminado e 5 representa o risco mais alto.                         | Número | ---     | 5                                                   |
| `CardScheme`              | Tipo da bandeira                                                                                                                                   | Texto  | 20      | Ver Tabela **CardScheme**                           |
| `InternetInfoCode`        | Sequência de códigos que indicam que existe uma excessiva alteração de identidades do comprador. Os códigos são concatenados usando o caractere ^. | Texto  | 255     | Ver tabela **InternetInfoCode**                     |
| `FraudAnalysisReasonCode` | Resultado da análise.                                                                                                                              | Byte   | ---     | Ver tabela **FraudAnalysisReasonCode**              |
| `AddressInfoCode`         | Combinação de códigos que indicam erro no endereço de cobrança e/ou entrega. Os códigos são concatenados usando o caractere ^.                     | Texto  | 255     | Ex: COR-BA^MM-BIN -> Ver tabela **AddressInfoCode** |
| `FactorCode`              | Combinação de códigos que indicam o score do pedido. Os códigos são concatenados usando o caractere ^.                                             | Texto  | 100     | Ex: B^D^R^Z -  Ver tabela **FactorCode**            |
| `IpRoutingMethod`         |Tipo de roteamento de IP utilizado pelo computador.|Texto|---|<BR>Anonymizer<BR>AolBased<BR>CacheProxy<BR>Fixed<BR>InternationalProxy<BR>MobileGateway<BR>Pop<BR>RegionalProxy<BR>Satellite<BR>SuperPop<BR>|

## Tabelas AF

### status do AF

> Os Status abaixo são apresentados no Backoffice da API Cielo E-Commerce, dentro do site Cielo

| Campo           | Descrição                                         | Observações                                                         |
|-----------------|---------------------------------------------------|---------------------------------------------------------------------|
| `Started`       | Transação recebida pela Cielo.                    | Dados da transação foram aceitos e enviados para analise            |
| `Accept`        | Transação aceita após análise de fraude.          | Transação aprovada pela analise de risco                            |
| `Review`        | Transação em revisão após análise de fraude       | Transação encaminhada para analise manual - será analisada por BPO |
| `Reject`        | Transação rejeitada após análise de fraude.       | Transação rejeitada pela analise de risco                           |
| `Unfinished`    | Transação não finalizada por algum erro sistémico | N/A                                                                 |
| `Pendent`       | Transação esperando analise                       | N/A                                                                 |
| `ProviderError` | Transação com erro no provedor de antifraude.     | N/A                                                                 |

### FraudAnalysis.Items

#### GiftCategory

|Campo|Descrição|
|---|---|
|`Yes`|Em caso de divergência entre endereços de cobrança e entrega, marca com risco pequeno.|
|`No`|Em caso de divergência entre endereços de cobrança e entrega, marca com risco alto.|
|`Off`|Ignora a análise de risco para endereços divergentes.|

#### HostHedge

|Campo|Descrição|
|---|---|
|`Low`|Baixa importância do e-mail e endereço IP na análise de risco.|
|`Normal`|Média importância do e-mail e endereço IP na análise de risco.|
|`High`|Alta importância do e-mail e endereço IP na análise de risco.|
|`Off`|E-mail e endereço IP não afetam a análise de risco.|

#### NonSensicalHedge

|Campo|Descrição|
|---|---|
|`Low`|Baixa importância da verificação feita sobre o pedido do comprador, na análise de risco.|
|`Normal`|Média importância da verificação feita sobre o pedido do comprador, na análise de risco.|
|`High`|Alta importância da verificação feita sobre o pedido do comprador, na análise de risco.|
|`Off`|Verificação do pedido do comprador não afeta a análise de risco.|

### FraudAnalysis.Cart

#### ObscenitiesHedge

|Campo|Descrição|
|---|---|
|`Low`|Baixa importância da verificação sobre obscenidades do pedido do comprador, na análise de risco.|
|`Normal`|Média importância da verificação sobre obscenidades do pedido do comprador, na análise de risco.|
|`High`|Alta importância da verificação sobre obscenidades do pedido do comprador, na análise de risco.|
|`Off`|Verificação de obscenidade no pedido do comprador não afeta a análise de risco.|

#### PhoneHedge

|Campo|Descrição|
|---|---|
|`Low`|Baixa importância nos testes realizados com números de telefone.|
|`Normal`|Média importância nos testes realizados com números de telefone.|
|`High`|Alta importância nos testes realizados com números de telefone.|
|`Off`|Testes de números de telefone não afetam a análise de risco.|

#### Risk

|Campo|Descrição|
|---|---|
|`Low`|O produto tem um histórico de poucos chargebacks.|
|`Normal`|O produto tem um histórico de chargebacks considerado normal.|
|`High`|O produto tem um histórico de chargebacks acima da média.|

#### TimeHedge

|Campo|Descrição|
|---|---|
|`Low`|Baixa importância no horário do dia em que foi feita a compra, para a análise de risco.|
|`Normal`|Média importância no horário do dia em que foi feita a compra, para a análise de risco.|
|`High`|Alta importância no horário do dia em que foi feita a compra, para a análise de risco.|
|`Off`|O horário da compra não afeta a análise de risco.|

#### Type

|Campo|Descrição|
|---|---|
|`CN`|Comprador de negócios|
|`CP`|Comprador particular|

#### VelocityHedge

|Campo|Descrição|
|---|---|
|`Low`|Baixa importância no número de compras realizadas pelo cliente nos últimos 15 minutos.|
|`Normal`|Média importância no número de compras realizadas pelo cliente nos últimos 15 minutos.|
|`High`|Alta importância no número de compras realizadas pelo cliente nos últimos 15 minutos.|
|`Off`|A frequência de compras realizadas pelo cliente não afeta a análise de fraude.|

#### Rating

|Campo|Descrição|
|---|---|
|`Adult`|Passageiro adulto.|
|`Child`|Passageiro criança.|
|`Infant`|Passageiro infantil.|
|`Youth`|Passageiro adolescente.|
|`Student`|Passageiro estudante.|
|`SeniorCitizen`|Passageiro idoso.|
|`Military`|Passageiro militar.|

### FraudAnalysis.Shipping

#### Method

|Campo|
|---|
|`None`|
|`SameDay`|
|`OneDay`|
|`TwoDay`|
|`ThreeDay`|
|`LowCost`|
|`Pickup`|
|`Other`|

### FraudAnalysis.ReplyData

#### CardScheme

| Tipo da bandeira       | Descrição             |
|------------------------|-----------------------|
| `MaestroInternational` | Maestro International |
| `MaestroUkDomestic`    | Maestro UK Domestic   |
| `MastercardCredit`     | MasterCard Credit     |
| `MastercardDebit`      | MasterCard Debit      |
| `VisaCredit`           | Visa Credit           |
| `VisaDebit`            | Visa Debit            |
| `VisaElectron`         | Visa Electron         |

#### AddressInfoCode

| Tipo       | Descrição                                                                     |
|------------|-------------------------------------------------------------------------------|
| `COR-BA`   | O endereço de cobrança pode ser normalizado                                   |
| `COR-SA`   | O endereço de entrega pode ser normalizado                                    |
| `INTL-SA`  | O país de entrega é fora dos U.S                                              |
| `MIL-USA`  | Este é um endereço militar nos U.S                                            |
| `MM-A`     | CONFIRME O ENDEREÇO: Os endereços de cobrança e entrega não estão relacionados              |
| `MM-BIN`   | CONFIRME A COMPRA COM O PORTADOR DO CARTÃO: o cartão usado foi emitido em outro país |
| `MM-C`     | CONFIRME O ENDEREÇO: Os endereços de cobrança e entrega não estão relacionados                    |
| `MM-CO`    | CONFIRME O ENDEREÇO: Os endereços de cobrança e entrega não estão relacionados                     |
| `MM-ST`    | CONFIRME O ENDEREÇO: Os endereços de cobrança e entrega não estão relacionados                    |
| `MM-Z`     | CONFIRME O ENDEREÇO: Os endereços de cobrança e entrega não estão relacionados            |
| `UNV-ADDR` | O endereço é inverificável                                                    |

#### InternetInfoCode

|Tipo|Descrição|
|---------|----------------------------------------------------------------------------------------------------|
|`MORPH-B`|CUIDADO: endereço de cobrança foi usado por diversos clientes diferentes|
|`MORPH-C`|CUIDADO: cartão foi usado por diversos clientes diferentes|
|`MORPH-E`|CUIDADO: endereço de email foi usado por diversos clientes diferentes|
|`MORPH-I`|O mesmo endereço IP tem sido utilizado várias vezes com identidades de clientes múltiplos|
|`MORPH-P`|CUIDADO: número de telefone foi usado por diversos clientes diferentes|
|`MORPH-S`|CUIDADO: endereço de envio foi usado por diversos clientes diferentes|

#### FraudAnalysisReasonCode

| Tipo  | Descrição                                                                                                                                                                                              |
|-------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `100` | Operação bem sucedida.                                                                                                                                                                                 |
| `101` | O pedido está faltando um ou mais campos necessários. Possível ação: Veja os campos que estão faltando na lista AntiFraudResponse.MissingFieldCollection. Reenviar o pedido com a informação completa. |
| `102` | Um ou mais campos do pedido contêm dados inválidos. Possível ação: Veja os campos inválidos na lista AntiFraudResponse.InvalidFieldCollection. Reenviar o pedido com as informações corretas.          |
| `150` | Falha no sistema geral. Possível ação: Aguarde alguns minutos e tente reenviar o pedido.                                                                                                               |
| `151` | O pedido foi recebido, mas ocorreu time-out no servidor. Este erro não inclui time-out entre o cliente e o servidor. Possível ação: Aguarde alguns minutos e tente reenviar o pedido.                  |
| `152` | O pedido foi recebido, mas ocorreu time-out. Possível ação: Aguarde alguns minutos e reenviar o pedido.                                                                                                |
| `202` | Prevenção à Fraude recusou o pedido porque o cartão expirou. Você também pode receber este código se a data de validade não coincidir com a data em arquivo do banco emissor. Se o processador de pagamento permite a emissão de créditos para cartões expirados, a CyberSource não limita essa funcionalidade. Possível ação: Solicite um cartão ou outra forma de pagamento.|
| `231` | O número da conta é inválido. Possível ação: Solicite um cartão ou outra forma de pagamento.                                                                                                           |
| `234` | Há um problema com a configuração do comerciante. Possível ação: Não envie o pedido. Entre em contato com o Suporte ao Cliente para corrigir o problema de configuração.                               |
| `400` | A pontuação de fraude ultrapassa o seu limite. Possível ação: Reveja o pedido do cliente.                                                                                                              |
| `480` | O pedido foi marcado para revisão pelo Gerenciador de Decisão.                                                                                                                                         |
| `481` | O pedido foi rejeitado pelo Gerenciador de Decisão                                                                                                                                                     |

#### FactorCode

| Tipo | Descrição                                                                                                                                                                                                            |
|------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `A`  | CONFIRME O ENDEREÇO: o cliente mudou o endereço de entrega duas ou mais vezes nos últimos meses.                                                                                                     |
| `B`  | BIN do cartão ou autorização de risco. Os fatores de risco estão relacionados com BIN de cartão de crédito e/ou verificações de autorização do cartão.                                                               |
| `C`  | CONFIRME A COMPRA COM O PORTADOR DO CARTÃO: o cliente usou mais de seis cartões nos últimos meses. |
| `D`  | Impacto do endereço de e-mail. O cliente usa um provedor de e-mail gratuito ou o endereço de email é arriscado.                                                                                                      |
| `E`  | Lista positiva. O cliente está na sua lista positiva.                                                                                                                                                                |
| `F`  | CUIDADO: alguma das informações a seguir está relacionada a uma transação que foi reportada como fraudulenta (cartão, endereço ou email).                                                                                             |
| `G`  | Localização do computador diferente da localização informada no momento da compra. Atenção!!                                                            |
| `H`  | CONFIRME A COMPRA COM O PORTADOR DO CARTÃO: o cliente trocou de nome duas ou mais vezes nos últimos meses.                                                                                                                       |
| `I`  | CONFIRME O ENDEREÇO: o endereço IP e o domínio de email não correspondem ao endereço de entrega.                                                                                                   |
| `N`  | CONFIRME O DONO DO CARTÃO E O ENDEREÇO: eles contém palavras sem sentido.                                                                                                                |
| `O`  | CONFIRME O DONO DO CARTÃO E O ENDEREÇO: eles contém palavras sem sentido.                                                                                                                                                             |
| `P`  | CONFIRME O DONO DO CARTÃO E O ENDEREÇO: informações cadastrais estão compartilhadas entre diferentes clientes. |
| `Q`  | Inconsistências do telefone. O número de telefone do cliente é suspeito.                                                                                                                                             |
| `R`  | CONFIRME O DONO DO CARTÃO E O ENDEREÇO: há correlações arriscadas no pedido.                                                                                                               |
| `T`  | Cobertura Time. O cliente está a tentar uma compra fora do horário esperado.                                                                                                                                         |
| `U`  | Endereço não verificável. O endereço de cobrança ou de entrega não pode ser verificado.                                                                                                                              |
| `V`  | CONFIRME A COMPRA COM O PORTADOR DO CARTÃO: o cartão foi usado muitas vezes nos últimos 15 minutos.                                                                                                                                           |
| `W`  | CUIDADO: o endereço está relacionado a uma transação que foi reportada como fraudulenta.                                                                                            |
| `Z`  | CONFIRME O ENDEREÇO: Os endereços de cobrança e entrega não estão relacionados.                |

### Outros

| Tipo | Descrição                                                                                                                                                                                                            |
|------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|`INV-EM`    | CONFIRME O EMAIL: O email é inválido.|
|`MM-EMBCO`  | Localização do computador diferente da localização informada no momento da compra. Atenção!!|
|`MM-IPBC`   | CONFIRME O ENDEREÇO: O endereço de cobrança não está relacionado com a localidade do IP.|
|`MM-IPBCO`  | CONFIRME O ENDEREÇO: O endereço de cobrança não está relacionado com a localidade do IP.|
|`MM-IPBST`  | CONFIRME O ENDEREÇO: O endereço de cobrança não está relacionado com a localidade do IP.|
|`MM-IPEM`   | CONFIRME O ENDEREÇO: O endereço de cobrança não está relacionado com a localidade do IP.|
|`RISK-EM`   | CONFIRME A COMPRA E O EMAIL COM O PORTADOR DO CARTÃO: o domínio do email é de alto risco.|
|`UNV-NID`   | CONFIRME A COMPRA COM O PORTADOR DO CARTÃO: o IP usado pertence a um proxy anônimo.|
|`UNV-RISK`  | CONFIRME A COMPRA COM O PORTADOR DO CARTÃO: o IP usado é de alto risco.|
|`UNV-EMBCO` | CONFIRME O ENDEREÇO: O endereço de cobrança não está relacionado com a localidade do IP.|
|`CON-POSNEG`| CUIDADO: alguma das informações a seguir está relacionada a uma transação que foi reportada como fraudulenta (cartão, endereço ou email).|
|`NEG-BA`    | CUIDADO: endereço de cobrança relacionado a transação reportada como fraudulenta.|
|`NEG-BCO`   | CUIDADO: endereço de cobrança relacionado a transação reportada como fraudulenta.|
|`NEG-BIN`   | CUIDADO: BIN do cartão relacionado a transações reportadas como fraudulentas.|
|`NEG-BZC`   | CUIDADO: CEP do endereço de cobrança relacionado a transações reportadas como fraudulentas.|
|`NEG-CC`    | CUIDADO: cartão relacionado a transações reportadas como fraudulentas.|
|`NEG-EM`    | CUIDADO: email relacionado a transações reportadas como fraudulentas.|
|`NEG-EMDOM` | CUIDADO: domínio de email relacionado a transações reportadas como fraudulentas.|
|`NEG-HIST`  | CUIDADO: tranação está relacionada a uma transação que foi reportada como fraudulenta.|
|`NEG-ID`    | CUIDADO: cartão relacionado a transações reportadas como fraudulentas.|
|`NEG-IP`    | CUIDADO: IP relacionado a transações reportadas como fraudulentas.|
|`NEG-IP3`   | CUIDADO: IP relacionado a transações reportadas como fraudulentas.|
|`NEG-PEM`   | CUIDADO: email do passageiro relacionado a transações reportadas como fraudulentas.|
|`NEG-PH`    | CUIDADO: número de telefone relacionado a transações reportadas como fraudulentas.|
|`NEG-PID`   | CUIDADO: ID do passageiro relacionado a transações reportadas como fraudulentas.|
|`NEG-PPH`   | CUIDADO: número de telefone do passageiro relacionado a transações reportadas como fraudulentas.|
|`NEG-SA`    | CUIDADO: endereço de envio relacionado a transação reportada como fraudulenta.|
|`VEL-ADDR`  | CONFIRME A COMPRA E O ENDEREÇO COM O PORTADOR DO CARTÃO: há diversos endereços relacionados ao cartão ou email.|
|`VEL-CC`    | CONFIRME A COMPRA COM O PORTADOR DO CARTÃO: há diversos cartões relacionados ao nome ou email.|
|`VEL-NAME`  | CONFIRME A COMPRA COM O PORTADOR DO CARTÃO: há diversos nomes relacionados ao cartão.|
|`VELS-CC`   | Cartão utilizado diversas vezes recentemente|
|`VELI-CC`   | Cartão utilizado diversas vezes recentemente|
|`VELL-CC`   | Cartão utilizado diversas vezes recentemente|
|`VELV-CC`   | Cartão utilizado diversas vezes recentemente|
|`VELS-EM`   | Email utilizado diversas vezes recentemente|
|`VELI-EM`   | Email utilizado diversas vezes recentemente|
|`VELL-EM`   | Email utilizado diversas vezes recentemente|
|`VELV-EM`   | Email utilizado diversas vezes recentemente|
|`VELS-IP`   | IP utilizado diversas vezes recentemente|
|`VELI-IP`   | IP utilizado diversas vezes recentemente|
|`VELL-IP`   | IP utilizado diversas vezes recentemente|
|`VELV-IP`   | IP utilizado diversas vezes recentemente|
|`VELS-SA`   | Endereço de envio utilizado diversas vezes recentemente|
|`VELI-SA`   | Endereço de envio utilizado diversas vezes recentemente|
|`VELL-SA`   | Endereço de envio utilizado diversas vezes recentemente|
|`VELV-SA`   | Endereço de envio utilizado diversas vezes recentemente|
|`VELS-TIP`  | IP utilizado diversas vezes recentemente|
|`VELI-TIP`  | IP utilizado diversas vezes recentemente|
|`VELL-TIP`  | IP utilizado diversas vezes recentemente|
|`BAD-FP`    | CUIDADO: dispositivo de risco.|
|`INTL-BIN`  | Cartão de crédito emitido fora do Brasil.|
|`MM-TZTLO`  | Fuso horário do dispositivo é incompatível com os fusos horários do país.|
|`MUL-EM`    | O cliente utilizou diversos endereços de email diferentes em várias ordens.|
|`NON-BC`    | Digitação suspeita no nome da cidade.|
|`NON-FN`    | O primeiro e último nome do cliente são identicos.|
|`NON-LN`    | Digitação suspeita no sobrenome do cliente.|
|`RISK-BC`   | Digitação suspeita no nome da cidade.|
|`RISK-BIN`  | CUIDADO: BIN do cartão relacionado a transações reportadas como fraudulentas.|
|`RISK-DEV`  | CUIDADO: dispositivo de risco.|
|`RISK-FN`   | Digitação suspeita no nome do cliente.|
|`RISK-LN`   | Digitação suspeita no sobrenome do cliente.|
|`RISK-PIP`  | IP proxy de risco.|
|`RISK-SD`   | País de envio e de cobrança são diferenres.|
|`RISK-TB`   | Horário do pedido suspeito.|
|`RISK-TIP`  | Endereço de IP de risco.|
|`RISK-TS`   | Horário do pedido suspeito.|

## Device FingerPrint

### O que é o D.FingerPrint

O Device FingerPrint é um javascript que uma vez instalado no checkout da loja captura dados de navegação e informações sobre o equipamento utilizado na compra. Essas informações são utlizadas pela AF para identificar compras fora do padrão do comprador.

### Configurando FingerPrint

Será necessário adicionar duas tags, a _script_ dentro da tag _head_ para uma performance correta e a _noscript_ dentro da tag _body_, para que a coleta dos dados do dispositivo seja realizada mesmo se o Javascript do browser estiver desabilitado.

> **IMPORTANTE:** Se os 2 segmentos de código não forem colocados na página de checkout, os resultados podem não ser precisos.

#### Domain

|Ambiente|Descrição|
|-|-|
|**Testing**| Use **h.online-metrix.net**, que é o DNS do servidor de fingerprint, como apresentado no exemplo de HTML abaixo|
|**Production**| Altere o domínio para uma URL local, e configure seu servidor Web para redirecionar esta URL para **h.online-metrix.net**|

#### Variáveis

| Variável               | Descrição                                                                                       | Supermid                              | Hierarquia                                |
|------------------------|-------------------------------------------------------------------------------------------------|---------------------------------------|-------------------------------------------|
| **ProviderOrgId**      | Sandbox = 1snn5n9w<br>Produção = k8vif92e                                                       | Use o valor padrão `k8vif92e`         | Dado exclusivo fornecido pela CyberSource |
| **ProviderMerchantId** | Identificador da sua loja na Cybersource. Caso não possua, entre em contato com a Cielo         | Use o valor padrão `cielo_webservice` | Dado exclusivo fornecido pela CyberSource |
| **ProviderIdentifier** | Identificador utilizado para cruzar informações obtidas do dispositivo do comprador.Este mesmo identificador deve ser atribuído ao campo `Customer.BrowserFingerprint` que será enviado na requisição da análise.<br>Exemplo: 123456789<br>Obs.:Este identificador poderá ser qualquer valor ou o número do pedido, mas deverá ser único durante 48 horas.| Definido pela loja                    | Definido pela loja                        |

> **Observação:** O resultado da concatenação entre o campo `ProviderMerchantId` e `ProviderIdentifier`, deve ser atribuído ao campo session_id do(s) script(s) que serão incluídos na página de checkout.
> JavaScript

![]({{ site.baseurl_root }}/images/apicieloecommerce/exemploscriptdfp.png)

> **IMPORTANTE:** Certifique-se de copiar todos os dados corretamente e de ter substituído as variáveis corretamente pelos respectivos valores.

#### Configurando seu Servidor Web

Todos os objetos se referem a h.online-metrix.net, que é o DNS do servidor de fingerprint. Quando você estiver pronto para produção, você deve alterar o nome do servidor para uma URL local, e configurar no seu servidor Web um redirecionamento de URL para h.online-metrix.net.

> **IMPORTANTE:** Se você não completar essa seção, você não receberá resultados corretos, e o domínio (URL) do fornecedor de fingerprint ficará visível, sendo mais provável que seu consumidor o bloqueie. 

# Velocity

## O Que É Velocity

O Velocity é um tipo de mecanismo de prevenção à tentativas de fraude, que analisa especificamente o conceito de **“velocidade X dados transacionais”**. Ela analisa a frequência que determinados dados são utilizados e se esse dados estão inscritos em uma lista de comportamentos passiveis de ações de segurança.

Para estabelecimentos comerciais que atuam no mercado de comércio eletrônico e eventualmente recebem transações fraudulentas, o Velocity é um produto que identificará os comportamentos suspeitos de fraude. A ferramenta tem o intuito de auxiliar na análise de fraude por um custo bem menor que uma ferramenta mais tradicional de mercado.

Ela é uma aliada na avaliação de comportamentos suspeitos de compra, pois os cálculos serão baseados em `elementos de rastreabilidade`.

O Velocity oferece 4 tipos de funcionalidades para validar dados transacionais:

| Funcionalidade               | Descrição                                                                                                                                          |
|------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| Regras de segurança velocity | O Lojista defini um grupo de regras de segurança que vão avaliar se determinados dados transacionais se repetem em um intervalo de tempo suspeito  |
| Quarentena                   | Criação de uma lista de dados que serão analisados por um periodo de tempo determinado antes de serem considerados validos ou fraudulentos         |
| BlackList                    | Criação de uma lista de dados que ao serem identificados impedem a transação de ser executada, evitando a criação de uma transação fraudulenta     |
| Whitelist                    | Criação de uma lista de dados que ao serem identificados permitem que a transação de seja executada, mesmo que existam regras de segurança em ação |

A funcionalidade deve ser contratada à parte, e posteriormente habilitada em sua loja pela equipe de Suporte Cielo Ecommerce via o Admin 3.0 

## Integração

O Velocity funciona analisando dados enviados na integração padrão da API Cielo Ecommerce. Não é necessario incluir nenhuma nó adicional a integração da loja para a criação da venda, mas será necessario alterar a forma como os dados são recebidos `Response`.

Quando o Velocity está ativo, a resposta da transação trará um nó específico chamado “Velocity”, com os detalhes da análise.

``` json
{
  "MerchantOrderId": "2017051202",
  "Customer": {
    "Name": "Nome do Comprador",
    "Identity": "12345678909",
    "IdentityType": "CPF",
    "Email": "comprador@cielo.com.br",
    "Address": {
      "Street": "Alameda Xingu",
      "Number": "512",
      "Complement": "27 andar",
      "ZipCode": "12345987",
      "City": "São Paulo",
      "State": "SP",
      "Country": "BRA"
    },
    "DeliveryAddress": {
      "Street": "Alameda Xingu",
      "Number": "512",
      "Complement": "27 andar",
      "ZipCode": "12345987",
      "City": "São Paulo",
      "State": "SP",
      "Country": "BRA"
    }
  },
  "Payment": {
    "ServiceTaxAmount": 0,
    "Installments": 1,
    "Interest": "ByMerchant",
    "Capture": true,
    "Authenticate": false,
    "Recurrent": false,
    "CreditCard": {
      "CardNumber": "455187******0181",
      "Holder": "Nome do Portador",
      "ExpirationDate": "12/2027",
      "SaveCard": false,
      "Brand": "Undefined"
    },
    "VelocityAnalysis": {
      "Id": "2d5e0463-47be-4964-b8ac-622a16a2b6c4",
      "ResultMessage": "Reject",
      "Score": 100,
      "RejectReasons": [
        {
          "RuleId": 49,
          "Message": "Bloqueado pela regra CardNumber. Name: Máximo de 3 Hits de Cartão em 1 dia. HitsQuantity: 3\. HitsTimeRangeInSeconds: 1440\. ExpirationBlockTimeInSeconds: 1440"
        }
      ]
    },
    "PaymentId": "2d5e0463-47be-4964-b8ac-622a16a2b6c4",
    "Type": "CreditCard",
    "Amount": 10000,
    "Currency": "BRL",
    "Country": "BRA",
    "Provider": "Simulado",
    "ReasonCode": 16,
    "ReasonMessage": "AbortedByFraud",
    "Status": 0,
    "ProviderReturnCode": "BP171",
    "ProviderReturnMessage": "Rejected by fraud risk (velocity)",
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquery.cieloecommerce.cielo.com.br/1/sales/2d5e0463-47be-4964-b8ac-622a16a2b6c4"
      }
    ]
  }
}
```

| Propriedade                              | Descrição                         | Tipo   | Tamanho |
|------------------------------------------|-----------------------------------|--------|---------|
| `VelocityAnalysis.Id`                    | Identificador da análise efetuada | GUID   | 36      |
| `VelocityAnalysis.ResultMessage`         | Accept ou Reject                  | Texto  | 25      |
| `VelocityAnalysis.Score`                 | 100                               | Número | 10      |
| `VelocityAnalysis.RejectReasons.RuleId`  | Código da Regra que rejeitou      | Número | 10      |
| `VelocityAnalysis.RejectReasons.Message` | Descrição da Regra que rejeitou   | Texto  | 512     |

# Recorrência

Pagamentos recorrentes são transações de cartão de crédito e débito que devem se repetir após um determinado período de tempo. Para as transações recorrentes com cartão de débito, ocorre autenticação com 3DS 2.0 da primeira transação (risco de chargeback por fraude passa a ser do Banco Emissor), e as transações subsequentes não são submetidas para autenticação (risco de chargeback por fraude permanece com o lojista). A solução de recorrência no débito está disponível apenas para cartões Visa. Em breve estará disponível para cartões Mastercard.

São pagamentos normalmente encontrados em **assinaturas**, onde o comprador deseja ser cobrado automaticamente, mas não quer informar novamente os dados do cartão.

## Tipos de recorrências

A API Cielo Ecommerce funciona com dois tipos de Recorrencia que possuem comportamentos diferentes.

* **Recorrência Própria** - Quando a inteligência da repetição e dados do cartão da recorrência ficam sobre responsabilidade do lojista
* **Recorrência Programada** - Quando a inteligência da repetição e dados do cartão da recorrência ficam sobre responsabilidade da **Cielo**

### Recorrência Própria

Nesse modelo, o lojista é responsavel por criar a inteligência necessaria para:

|Inteligência|Descrição|
|---|---|
|**Salvar os dados da transação**|A loja precisará armazenar a transação e dados do pagamento|
|**Criar repetição transacional**|A loja deverá enviar uma nova transação sempre que necessitar de uma Autorização|
|**Comportamento para transação negada**|Caso uma das transações seja negada, caberá a loja a decisão de "retentar" uma nova autorização|

Em todas as instancias, a recorrencia programada é uma transação padrão para a Cielo, sendo sua unica diferença a necessidade de enviar um a parametro adicional que a define como **Recorrência Própria**

**Paramêtro:** `Payment.Recurrent`= `True`

#### Caso de Uso

Este é um exemplo de como a API Cielo Ecommerce permite a utilização de sistemas externos de recorrência em suas transações

A recorrência própria é uma configuração da API Cielo Ecommerce que permite um lojista utilizar um sistema de recorrência interno especifico as suas necessidades de negócio. 
Nesse modelo, o sistema do lojista é encarregado por definir o período, os dados transacionais, e quando necessário, nos enviar a venda de recorrência.

**Veja um exemplo em uso:**

* Recorrência própria + Cartão Tokenizado

A academia CleverFit possui um sistema de cobrança diferenciado, onde a matricula é cobrada quinzenalmente, mas nunca nos fins de semana.

Por ser um modelo altamente customizado, a CleverFit possui um sistema de recorrência própria, utilizando a API Cielo Ecommerce via dois mecanismos:

1. **Recorrência Própria** - A CleverFit envia os dados da transação como uma venda normal, mas a API identifica que é uma recorrência e aplica regras de autorização diferenciada ao pedido.
1. **Cartão Tokenizado** - A CleverFit mantem os cartões salvos via a tokenização, diminuindo o risco de assegurar os dados  transacionais em seu sistema.

A CleverFit envia a transação quinzenalmente a API Cielo Ecommerce, usando os Tokens salvos na própria API e optando pela Recorrência Própria, que altera a regra de autorização para se adequar a seu modelo de cobrança

### Recorrência Programada

Nesse modelo, a Cielo é responsavel pela inteligência necessaria para executar uma recorrência de maneira automatica.

A **Recorrência Programada** permite que o lojista crie uma transação base, que ao ser enviada para a API Cielo Ecommerce, será salva e executada seguindo as regras definidas pelo lojista. 

Nesse modelo, a API realiza e permite:

|Vantagens|Descrição|
|---|---|
|**Salva dados transacionais**|Salva dados da transação, criando assim um modelo de como serão as proximas Recorrências|
|**Automatiza a recorrência**|Sem atuação da loja, a API cria as transações futuras de acordo com as definições do lojista|
|**Permite atualização de dados**|Caso necessario, a API permite modificações das informações da transação ou do ciclo de recorrência|

A Recorrencia Programada é formada por uma estrutura transacional simples. O Lojista deverá informa na transação os seguintes dados:

``` json
"RecurrentPayment":
{
       "AuthorizeNow":"False",
       "StartDate":"2019-06-01"
       "EndDate":"2019-12-01",
       "Interval":"SemiAnnual"
}
```

Onde podemos definir os dados como:

|Paramêtros|Descrição|
|---|---|
|`AuthorizeNow`|Define que qual o momento que uma recorrencia será criada. Se for enviado como `True`, ela é criada no momento da autorização, se `False`, a recorrencia ficará suspensaaté a data escolhida para ser iniciada.|
|`StartDate`|Define a data que transação da Recorrência Programada será autorizada|
|`EndDate`|Define a data que a Recorrência Programada será encerrada. Se não for enviada, a Recorrência será executada até ser cancelada pelo lojista|
|`Interval`|Intervalo da recorrência.<br /><ul><li>Monthly - Mensal </li><li>Bimonthly - Bimestral </li><li>Quarterly - Trimestral </li><li>SemiAnnual - Semestral </li><li>Annual - Anual </li></ul>|

Quando uma transação é enviada a API Cielo Ecommerce com o nó de Recorrência Programada, o processo de recorrencia passa a ser efetivo quando a transação é considerada **AUTORIZADA**.

Desse ponto em diante, ela passará a ocorre dentro do intervalo definido pelo lojista.

Caracteristicas importantes da **Recorrência Programada**:

|Informação|Descrição|
|---|---|
|**Criação**|A primeira transação é chamada de **"Transação de agendamento"**, todas as transações posteriores serão cópias dessa primeira transação. Ela não precisa ser capturada para que a recorrencia seja criada, basta ser **AUTORIZADA**|
|**Captura**|Transações de Recorrência Programada não precisam ser capturadas. Após a primeira transação, todas as transações de recorrencia são capturadas automaticamente pela API|
|**Identificação**|Transações de Recorrência Programada geram dois tipos de identificação:<br><br>**PAYMENTID**: Identifica 1 transação. É o mesmo identificador das outras transações na API    <br><br>**RECURRENTPAYMENTID**: Identifica Pedido de recorrencia. Um RecurrentPaymentID possui inumeros PaymentID vinculados a ela. Essa é a variavel usada para Cancelar uma Recorrencia Programada|
|**Consultando**|Para consultar, basta usar um dos dois tipos de identificação:<br><br>**PAYMENTID**: Utilizada para consultar UMA TRANSAÇÃO DENTRO DA RECORRÊNCIA    <br><br>**RECURRENTPAYMENTID**: Utilizado para consultar A RECORRÊNCIA.|
|**Cancelamento**|Uma Recorrencia Programada pode ser cancelada de duas maneiras: <br><br>**Lojista**: Solicita o cancelamento da recorrencia. Não cancela transações ja finalizadas antes da ordem de cancelamento da recorrência.  <br><br>**Por cartão invalido**: Caso a API identifique que um cartão salvo está invalido (EX: Expirado) a recorrência será cancelada e não se repetirá, até que o lojista atualize o meio de pagamento. <br><br> **OBS:** Cancelamento de transações dentro da recorrência não encerra o agendamento de transações futuras. Somente o Cancelamento usando o **RecurrentPaymentID** encerra agendamentos futuros.

**Estrutura de um RecurrentPaymentID**

![]({{ site.baseurl_root }}/images/apicieloecommerce/recpaymentid.png)

**Fluxo de uma Recorrência Programada**

![]({{ site.baseurl_root }}/images/apicieloecommerce/fluxosrecprog.png)

#### Caso de uso

Este é um exemplo de como usar as recorrências da API Cielo Ecommerce para elevar suas vendas:

A recorrência é o processo de salvar uma transação e repeti-la em um intervalo de tempo predefinido. Ideal para modelo de assinaturas.

A recorrência programada cielo tem as seguintes características:

* **Intervalos programados:** Mensal, bimestral, trimestral semestral e anual
* **Data de validade:** Permite definir se a recorrência vai se encerrar
* **Retentativa:** se uma transação for negada, vamos retentar a transação até 4x
* **Atualização:** Permite alterar dados da recorrência, como valor, intervalo.

Veja um exemplo em uso:

* **Recorrência Mensal e anual**

A empresa Musicfy oferece um serviço de assinatura de online onde seus clientes pagam para poderem acessar uma biblioteca de músicas e ouvi-las via streaming.

Para captar o máximo de clientes, eles oferecem 2 maneiras de pagamento:

* Mensal por R$19,90 
* Anual (com desconto) de R$180,00 

Como eles executam a cobrança mensal ou anual de seus clientes? 

A MusicFy utiliza a Recorrência programada da API Cielo Ecommerce.

Ao criar uma transação, o Musicfy informa que o pedido em questão deve ser repetir mensalmente ou anualmente e que não há data de termino para a cobrança.

Quais as vantagens de usar a recorrência programada para o MusicFy:

1. **Facilidade:** A cobrança de mensalidade é automática, logo o MusicFy não precisa se preocupar em construir um sistema de cobrança.
    
2. **Usabilidade:** O valor das assinaturas pode ser atualizado sem a necessidade de refazer a transação. Um mês pode ser cancelado ou a recorrência pode ter um delay ( o modelo de 30 dias gratuito) com apenas uma configuração.
    
3. **Segurança:** Não é necessário armazenar dados sensíveis do cartão e do comprador junto a loja.
    
4. **Conversão:** A recorrência programada cielo possui um sistema de retentativa automática. Caso uma das transações seja negada, ela será retentada até 4 vezes, buscando atingir a autorização.

## Criando Recorrências

### Criando uma Recorrência Própria

Para criar uma venda recorrente cuja o processo de recorrência e intervalo serão executados pela propria loja, basta fazer um POST conforme o exemplo.

O paramêtro `Payment.Recurrent`deve ser `true`, caso contrario, a transação será negada.

#### Requisição de crédito

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014113245231706",
   "Customer":{  
      "Name":"Comprador rec propria"
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":1500,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "Recurrent": true,
     "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"262",
         "SaveCard":"false",
         "Brand":"Visa",
         "CardOnFile":{
             "Usage": "First"
         }
     }
   }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
    {
   "MerchantOrderId":"2014113245231706",
   "Customer":{  
      "Name":"Comprador rec propria"
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":1500,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "Recurrent": true,
     "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"262",
         "SaveCard":"false",
         "Brand":"Visa",
         "CardOnFile":{
            "Usage": "First"
         }
     }
   }
}
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|6|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Numero de identificação do Pedido.|Texto|50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|255|Sim|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Payment.Installments`|Número de Parcelas.|Número|2|Sim|
|`Payment.SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - não permite caracteres especiais|Texto|13|Não|
|`Payment.Recurrent`|marcação de uma transação de recorrencia não programada|boolean|5|Não|
|`CreditCard.CardNumber`|Número do Cartão do Comprador.|Texto|19|Sim|
|`CreditCard.Holder`|Nome do Comprador impresso no cartão.|Texto|25|Não|
|`CreditCard.ExpirationDate`|Data de validade impresso no cartão.|Texto|7|Sim|
|`CreditCard.SecurityCode`|Código de segurança impresso no verso do cartão.|Texto|4|Não|
|`CreditCard.Brand`|Bandeira do cartão.|Texto|10|Sim|
|`CreditCard.CardOnFile.Usage`|**First** se o cartão foi armazenado e é seu primeiro uso.<br>**Used** se o cartão foi armazenado e ele já foi utilizado anteriormente em outra transação.|Texto|---|Não|
|`CreditCard.CardOnFile.Reason`|Indica o propósito de armazenamento de cartões, caso o campo `Usage` for `Used`.<br>**Recurring** - Compra recorrente programada (ex. assinaturas).<br>**Unscheduled** - Compra recorrente sem agendamento (ex. aplicativos de serviços).<br>**Installments** - Parcelamento através da recorrência.|Texto|---|Condicional|

#### Resposta de crédito

```json
{
    "MerchantOrderId": "2014113245231706",
    "Customer": {
        "Name": "Comprador rec propria"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "Recurrent": true,
        "CreditCard": {
            "CardNumber": "123412******1231",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "ProofOfSale": "3827556",
        "Tid": "0504043827555",
        "AuthorizationCode": "149867",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId": "737a8d9a-88fe-4f74-931f-acf81149f4a0",
        "Type": "CreditCard",
        "Amount": 1500,
        "Currency": "BRL",
        "Country": "BRA",
        "Provider": "Simulado",
        "ExtraDataCollection": [],
        "Status": 1,
        "ReturnCode": "4",
        "ReturnMessage": "Operation Successful",
        "Link": {
                "Method": "GET",
                "Rel": "recurrentPayment",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}"
            },
            "AuthorizeNow": true
        },
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2014113245231706",
    "Customer": {
        "Name": "Comprador rec propria"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "Recurrent": true,
        "CreditCard": {
            "CardNumber": "123412******1231",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "ProofOfSale": "3827556",
        "Tid": "0504043827555",
        "AuthorizationCode": "149867",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId": "737a8d9a-88fe-4f74-931f-acf81149f4a0",
        "Type": "CreditCard",
        "Amount": 1500,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "ReturnCode": "4",
        "ReturnMessage": "Operation Successful",
        "Link": {
                "Method": "GET",
                "Rel": "recurrentPayment",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}"
            },
            "AuthorizeNow": true
        },
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|6|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Numero de identificação do Pedido.|Texto|50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|255|Não|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Payment.Installments`|Número de Parcelas.|Número|2|Sim|
|`Payment.SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - não permite caracteres especiais|Texto|13|Não|
|`Payment.Recurrent`|marcação de uma transação de recorrencia não programada|boolean|5|Não|
|`CreditCard.CardNumber`|Número do Cartão do Comprador.|Texto|19|Sim|
|`CreditCard.Holder`|Nome do Comprador impresso no cartão.|Texto|25|Não|
|`CreditCard.ExpirationDate`|Data de validade impresso no cartão.|Texto|7|Sim|
|`CreditCard.SecurityCode`|Código de segurança impresso no verso do cartão.|Texto|4|Não|
|`CreditCard.Brand`|Bandeira do cartão.|Texto|10|Sim|

#### Requisição de débito

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014121201",
   "Customer":{  
      "Name":"Comprador rec propria"
   },
   "Payment":{  
     "Type":"DebitCard",
     "Amount":15700,
     "Provider":"Cielo",
     "ReturnUrl":"https://clicktime.symantec.com/3TVsxr2DrNxWzL9C7RZ19v97Vc?u=http%3A%2F%2Fwww.google.com.br%2522",
     "Recurrent": true,
     "DebitCard":{
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"11/2019",
         "SecurityCode":"123",
         "Brand":"Visa",
         "CardOnFile":{
            "Usage": "First"
         }
     },
     "ExternalAuthentication":{
         "Cavv":"A901234A5678A0123A567A90120=",
         "Xid":"A90123A45678A0123A567A90123",
         "Eci":"5",
         "Version":"2"
     }
   }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|6|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Numero de identificação do Pedido.|Texto|50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|255|Sim|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Provider`|Define comportamento do meio de pagamento|Texto|15|---|
|`ReturnUrl`|URI para onde o usuário será redirecionado após o fim do pagamento|Texto|1024|---|
|`Payment.Recurrent`|marcação de uma transação de recorrencia não programada|boolean|5|Não|
|`DebitCard.CardNumber`|Número do Cartão do Comprador.|Texto|19|Sim|
|`DebitCard.Holder`|Nome do Comprador impresso no cartão.|Texto|25|Não|
|`DebitCard.ExpirationDate`|Data de validade impresso no cartão.|Texto|7|Sim|
|`DebitCard.SecurityCode`|Código de segurança impresso no verso do cartão.|Texto|4|Não|
|`DebitCard.Brand`|Bandeira do cartão.|Texto|10|Sim|
|`DebitCard.CardOnFile.Usage`|**First** se o cartão foi armazenado e é seu primeiro uso.<br>**Used** se o cartão foi armazenado e ele já foi utilizado anteriormente em outra transação.|Texto|---|Não|
|`DebitCard.CardOnFile.Reason`|Indica o propósito de armazenamento de cartões, caso o campo `Usage` for `Used`.<br>**Recurring** - Compra recorrente programada (ex. assinaturas).<br>**Unscheduled** - Compra recorrente sem agendamento (ex. aplicativos de serviços).<br>**Installments** - Parcelamento através da recorrência.|Texto|---|Condicional|
|`ExternalAuthentication.Cavv`|O valor Cavv é retornado pelo mecanismo de autenticação.|Texto|28|Sim|
|`ExternalAuthentication.Xid`|O valor Xid é retornado pelo mecanismo de autenticação.|Texto|28|Sim|
|`ExternalAuthentication.Eci`|O valor Eci é retornado pelo mecanismo de autenticação.|Número|1|Sim|
|`ExternalAuthentication.Version`|---|---|---|

#### Resposta de débito

Para transações recorrentes com cartão de débito, após enviado o request de uma transação com solicitação de autenticação 3DS 2.0, a informação de Issuer Transaction ID é retornada na primeira transação autenticada. Essa informação deve ser armazenada e enviada nas transações subsequentes, permitindo ao Banco Emissor vincular as transações subsequentes de débito não autenticadas, com a primeira transação de débito autenticada. Esse modelo está disponível apenas para cartões de débito Visa.

```json
{
   "MerchantOrderId": "2014121201",
   "Customer": {
        "Name": "Comprador rec propria"
    },
   "Payment": {
     "DebitCard": {
         "CardNumber": "409603******0183",
         "Holder": "Teste Holder",
         "ExpirationDate": "11/2019",
         "SaveCard": false,
         "Brand": "Visa",
         "CardOnFile":{
            "Usage": "First",
            "Reason": "Conforme documentação"
         },
         "PaymentAccountReference":"80215935306245595386112369301"
     },
     "Provider": "Cielo",
     "AuthorizationCode": "149867",
     "Eci": "5",
     "Tid": "10069930698EGKISAQCC",
     "ProofOfSale": "165009",
     "Authenticate": true,
     "ExternalAuthentication":{
         "Cavv": "A901234A5678A0123A567A90120=",
         "Xid": "A90123A45678A0123A567A90123",
         "Eci": "5",
         "Version": "2"
     },
     "Recurrent": true,
     "IssuerTransactionId": "009242668734612",
     "Amount": 15700,
     "ReceivedDate": "2019-10-22 16:59:26",
     "CapturedAmount": 15700,
     "CapturedDate": "2019-10-22 16:59:27",
     "ReturnUrl": "https://clicktime.symantec.com/3TVsxr2DrNxWzL9C7RZ19v97Vc?u=http%3A%2F%2Fwww.google.com.br%2522",
     "Status": 2,
     "IsSplitted": false,
     "ReturnMessage": "Transacao capturada com sucesso",
     "ReturnCode": "00",
     "PaymentId": "470e8811-14de-41d1-9a52-3ba9a2bfce37",
     "Type": "DebitCard",
     "Currency": "BRL",
     "Country": "BRA",
     "Links": [
         {
            "Method": "GET",
            "Rel": "self",
            "Href": "https://clicktime.symantec.com/33u6P2R2ydHsCe3omoVXP9r7Vc?u=https%3A%2F%2Fapiquerysandbox.cieloecommerce.cielo.com.br%2F1%2Fsales%2F470e8811-14de-41d1-9a52-3ba9a2bfce37%2522"
         }
     ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|6|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Numero de identificação do Pedido.|Texto|50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|255|Não|
|`DebitCard.CardNumber`|Número do Cartão do Comprador.|Texto|19|Sim|
|`DebitCard.Holder`|Nome do Comprador impresso no cartão.|Texto|25|Não|
|`DebitCard.ExpirationDate`|Data de validade impresso no cartão.|Texto|7|Sim|
|`DebitCard.SecurityCode`|Código de segurança impresso no verso do cartão.|Texto|4|Não|
|`DebitCard.Brand`|Bandeira do cartão.|Texto|10|Sim|
|`DebitCard.CardOnFile.Usage`|**First** se o cartão foi armazenado e é seu primeiro uso.<br>**Used** se o cartão foi armazenado e ele já foi utilizado anteriormente em outra transação.|Texto|---|Não|
|`DebitCard.CardOnFile.Reason`|Indica o propósito de armazenamento de cartões, caso o campo `Usage` for `Used`.<br>**Recurring** - Compra recorrente programada (ex. assinaturas).<br>**Unscheduled** - Compra recorrente sem agendamento (ex. aplicativos de serviços).<br>**Installments** - Parcelamento através da recorrência.|Texto|---|Condicional|
|`DebitCard.PaymentAccountReference`|O PAR(payment account reference) é o número que associa diferentes tokens a um mesmo cartão. Será retornado pelas bandeiras Master e Visa e repassado para os clientes do e-commerce Cielo. Caso a bandeira não envie a informação o campo não será retornado.|Numérico|29|Não|
|`Payment.Provider`|Define comportamento do meio de pagamento (ver Anexo)/NÃO OBRIGATÓRIO PARA CRÉDITO.|Texto|15|---|
|`Payment.AuthorizationCode`|Código de autorização.|Texto|6|---|
|`Payment.Eci`|Eletronic Commerce Indicator. Representa o quão segura é uma transação.|Texto|2|---|
|`Payment.Tid`|Id da transação na adquirente.|Texto|6|---|
|`Payment.ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|---|
|`Payment.Authenticate`|Define se o comprador será direcionado ao Banco emissor para autenticação do cartão|Booleano|---|Não|
|`ExternalAuthentication.Cavv`|O valor Cavv é retornado pelo mecanismo de autenticação.|Texto|28|Sim|
|`ExternalAuthentication.Xid`|O valor Xid é retornado pelo mecanismo de autenticação.|Texto|28|Sim|
|`ExternalAuthentication.Eci`|O valor Eci é retornado pelo mecanismo de autenticação.|Número|1|Sim|
|`ExternalAuthentication.Version`|---|---|---|
|`Payment.Recurrent`|marcação de uma transação de recorrencia não programada|boolean|5|Não|
|`Payment.IssuerTransactionId`|Identificado de autenticação do Emissor para transações de débito recorrentes. Este campo deve ser enviado nas transações subsequentes da primeira transação no modelo de recorrência própria. Já no modelo de recorrência programada, a Cielo será a responsável por enviar o campo nas transações subsequentes.|Texto|15|---|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Payment.ReturnUrl`|URI para onde o usuário será redirecionado após o fim do pagamento|Texto|1024|Sim|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Currency`|Moeda na qual o pagamento será feito (BRL).|Texto|3|Não|
|`Payment.Country`|Pais na qual o pagamento será feito.|Texto|3|Não|

### Criando uma Recorrência Programada

Para criar uma venda recorrente cuja a primeira recorrência é autorizada com a forma de pagamento cartão de crédito, basta fazer um POST conforme o exemplo.

<aside class="notice"><strong>Atenção:</strong> Nessa modalidade de recorrência, a primeira transação deve ser capturada. Todas as transações posteriores serão capturadas automaticamente.</aside>

#### Requisição de crédito

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014113245231706",
   "Customer":{  
      "Name":"Comprador rec programada"
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":1500,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "RecurrentPayment":{
       "AuthorizeNow":"true",
       "EndDate":"2019-12-01",
       "Interval":"SemiAnnual"
     },
     "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"262",
         "SaveCard":"false",
         "Brand":"Visa",
         "CardOnFile":{
           "usage":"first"
         }
     }
   }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
    {
   "MerchantOrderId":"2014113245231706",
   "Customer":{  
      "Name":"Comprador rec programada"
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":1500,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "RecurrentPayment":{
       "AuthorizeNow":"true",
       "EndDate":"2019-12-01",
       "Interval":"SemiAnnual"
     },
     "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"262",
         "SaveCard":"false",
         "Brand":"Visa",
         "CardOnFile":{
           "usage":"first"
         }
     }
   }
}
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|6|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Numero de identificação do Pedido.|Texto|50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|255|Sim|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Payment.Installments`|Número de Parcelas.|Número|2|Sim|
|`Payment.SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - não permite caracteres especiais|Texto|13|Não|
|`Payment.RecurrentPayment.EndDate`|Data para termino da recorrência.|Texto|10|Não|
|`Payment.RecurrentPayment.Interval`|Intervalo da recorrência.<br /><ul><li>Monthly (Default) </li><li>Bimonthly </li><li>Quarterly </li><li>SemiAnnual </li><li>Annual</li></ul>|Texto|10|Não|
|`Payment.RecurrentPayment.AuthorizeNow`|Booleano para saber se a primeira recorrência já vai ser Autorizada ou não.|Booleano|---|Sim|
|`CreditCard.CardNumber`|Número do Cartão do Comprador.|Texto|19|Sim|
|`CreditCard.Holder`|Nome do Comprador impresso no cartão.|Texto|25|Não|
|`CreditCard.ExpirationDate`|Data de validade impresso no cartão.|Texto|7|Sim|
|`CreditCard.SecurityCode`|Código de segurança impresso no verso do cartão.|Texto|4|Não|
|`CreditCard.Brand`|Bandeira do cartão.|Texto|10|Sim|
|`CreditCard.CardOnFile.Usage`|**First** se o cartão foi armazenado e é seu primeiro uso.<br>**Used** se o cartão foi armazenado e ele já foi utilizado anteriormente em outra transação.|Texto|---|Não|
|`CreditCard.CardOnFile.Reason`|Indica o propósito de armazenamento de cartões, caso o campo `Usage` for `Used`.<br>**Recurring** - Compra recorrente programada (ex. assinaturas).<br>**Unscheduled** - Compra recorrente sem agendamento (ex. aplicativos de serviços).<br>**Installments** - Parcelamento através da recorrência.|Texto|---|Condicional|

#### Resposta de crédito

```json
{
    "MerchantOrderId": "2014113245231706",
    "Customer": {
        "Name": "Comprador rec programada"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "123412******1231",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "ProofOfSale": "3827556",
        "Tid": "0504043827555",
        "AuthorizationCode": "149867",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId": "737a8d9a-88fe-4f74-931f-acf81149f4a0",
        "Type": "CreditCard",
        "Amount": 1500,
        "Currency": "BRL",
        "Country": "BRA",
        "Provider": "Simulado",
        "ExtraDataCollection": [],
        "Status": 1,
        "ReturnCode": "4",
        "ReturnMessage": "Operation Successful",
        "RecurrentPayment": {
            "RecurrentPaymentId": "61e5bd30-ec11-44b3-ba0a-56fbbc8274c5",
            "NextRecurrency": "2015-11-04",
            "EndDate": "2019-12-01",
            "Interval": "SemiAnnual",
            "Link": {
                "Method": "GET",
                "Rel": "recurrentPayment",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}"
            },
            "AuthorizeNow": true
        },
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2014113245231706",
    "Customer": {
        "Name": "Comprador rec programada"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "123412******1231",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "ProofOfSale": "3827556",
        "Tid": "0504043827555",
        "AuthorizationCode": "149867",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId": "737a8d9a-88fe-4f74-931f-acf81149f4a0",
        "Type": "CreditCard",
        "Amount": 1500,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "ReturnCode": "4",
        "ReturnMessage": "Operation Successful",
        "RecurrentPayment": {
            "RecurrentPaymentId": "61e5bd30-ec11-44b3-ba0a-56fbbc8274c5",
            "NextRecurrency": "2015-11-04",
            "EndDate": "2019-12-01",
            "Interval": "SemiAnnual",
            "Link": {
                "Method": "GET",
                "Rel": "recurrentPayment",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}"
            },
            "AuthorizeNow": true
        },
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`RecurrentPaymentId`|Campo Identificador da próxima recorrência.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`NextRecurrency`|Data da próxima recorrência.|Texto|7|12/2030 (MM/YYYY)|
|`EndDate`|Data do fim da recorrência.|Texto|7|12/2030 (MM/YYYY)|
|`Interval`|Intervalo entre as recorrência.|Texto|10|/ Monthly / Bimonthly  / Quarterly  / SemiAnnual / Annual |
|`AuthorizeNow`|Booleano para saber se a primeira recorrencia já vai ser Autorizada ou não.|Booleano|---|true ou false|

#### Requisição de débito

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014121201",
   "Customer":{  
      "Name":"Comprador rec programada"
   },
   "Payment":{  
     "Type":"DebitCard",
     "Amount":15700,
     "Provider":"cielo",
     "RecurrentPayment":{
       "AuthorizeNow":"true",
       "EndDate":"2030-02-27",
       "Interval":"bimonthly"
     },
     "ReturnUrl":"https://clicktime.symantec.com/3TVsxr2DrNxWzL9C7RZ19v97Vc?u=http%3A%2F%2Fwww.google.com.br%2522",
     "DebitCard":{  
         "CardNumber":"*****0183",
         "Holder":"Teste Holder",
         "ExpirationDate":"11/2019",
         "SecurityCode":"***",
         "Brand":"Visa",
         "CardOnFile":{
                "usage":"first"
         }
     },
     "ExternalAuthentication":{
         "Cavv":"a901234a5678a0123a567a90120=",
         "Xid":"a90123a45678a0123a567a90123",
         "Eci":"5",
         "Version":"2"
     }
   }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
    {  
   "MerchantOrderId":"2014121201",
   "Customer":{  
      "Name":"Comprador rec programada"
   },
   "Payment":{  
     "Type":"DebitCard",
     "Amount":15700,
     "Provider":"cielo",
     "RecurrentPayment":{
       "AuthorizeNow":"true",
       "EndDate":"2030-02-27",
       "Interval":"bimonthly"
     },
     "ReturnUrl":"https://clicktime.symantec.com/3TVsxr2DrNxWzL9C7RZ19v97Vc?u=http%3A%2F%2Fwww.google.com.br%2522",
     "DebitCard":{  
         "CardNumber":"*****0183",
         "Holder":"Teste Holder",
         "ExpirationDate":"11/2019",
         "SecurityCode":"***",
         "Brand":"Visa",
         "CardOnFile":{
                "usage":"first"
         }
     },
     "ExternalAuthentication":{
         "Cavv":"a901234a5678a0123a567a90120=",
         "Xid":"a90123a45678a0123a567a90123",
         "Eci":"5",
         "Version":"2"
     }
   }
}
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|6|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Numero de identificação do Pedido.|Texto|50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|255|Sim|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Payment.Provider`|Define comportamento do meio de pagamento|Texto|15|---|
|`Payment.RecurrentPayment.EndDate`|Data para termino da recorrência.|Texto|10|Não|
|`Payment.RecurrentPayment.Interval`|Intervalo da recorrência.<br /><ul><li>Monthly (Default) </li><li>Bimonthly </li><li>Quarterly </li><li>SemiAnnual </li><li>Annual</li></ul>|Texto|10|Não|
|`Payment.RecurrentPayment.AuthorizeNow`|Booleano para saber se a primeira recorrência já vai ser Autorizada ou não.|Booleano|---|Sim|
|`Payment.ReturnUrl`|URI para onde o usuário será redirecionado após o fim do pagamento|Texto|1024|Sim|
|`DebitCard.CardNumber`|Número do Cartão do Comprador.|Texto|19|Sim|
|`DebitCard.Holder`|Nome do Comprador impresso no cartão.|Texto|25|Não|
|`DebitCard.ExpirationDate`|Data de validade impresso no cartão.|Texto|7|Sim|
|`DebitCard.SecurityCode`|Código de segurança impresso no verso do cartão.|Texto|4|Não|
|`DebitCard.Brand`|Bandeira do cartão.|Texto|10|Sim|
|`DebitCard.CardOnFile.Usage`|**First** se o cartão foi armazenado e é seu primeiro uso.<br>**Used** se o cartão foi armazenado e ele já foi utilizado anteriormente em outra transação.|Texto|---|Não|
|`DebitCard.CardOnFile.Reason`|Indica o propósito de armazenamento de cartões, caso o campo `Usage` for `Used`.<br>**Recurring** - Compra recorrente programada (ex. assinaturas).<br>**Unscheduled** - Compra recorrente sem agendamento (ex. aplicativos de serviços).<br>**Installments** - Parcelamento através da recorrência.|Texto|---|Condicional|
|`ExternalAuthentication.Cavv`|O valor Cavv é retornado pelo mecanismo de autenticação.|Texto|28|Sim|
|`ExternalAuthentication.Xid`|O valor Xid é retornado pelo mecanismo de autenticação.|Texto|28|Sim|
|`ExternalAuthentication.Eci`|O valor Eci é retornado pelo mecanismo de autenticação.|Número|1|Sim|
|`ExternalAuthentication.Version`|---|---|---|

#### Resposta de débito

Para transações recorrentes com cartão de débito, após enviado o request de uma transação com solicitação de autenticação 3DS 2.0, a informação de Issuer Transaction ID é retornada na primeira transação autenticada. Essa informação será armazenada pela Cielo e enviada nas transações subsequentes, permitindo ao Banco Emissor vincular as transações subsequentes de débito não autenticadas, com a primeira transação de débito autenticada. Esse modelo está disponível apenas para cartões de débito Visa.

```json
{
    "MerchantOrderId": "2014121201",
    "Customer": {
        "Name": "Comprador rec programada"
    },
    "Payment": {
        "DebitCard": {
            "CardNumber": "123412******1231",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa",
            "CardOnFile": {
                "usage":"first"
            }
        },
        "Provider": "cielo",
        "AuthorizationCode": "603094",
        "Eci": "5",
        "Tid": "10069930698dvs6862lc",
        "ProofOfSale": "139053",
        "Authenticate": true,
        "ExternalAuthentication":{
          "Cavv":"a901234a5678a0123a567a90120=",
          "Xid":"a90123a45678a0123a567a90123",
          "Eci":"5",
          "Version":"2"
        },
        "Recurrent": false,
        "IssuerTransactionId":"009277050978376",
        "Amount": 15700,
        "ReceivedDate":"2019-10-04 11:29:49",
        "CapturedAmount":15700,
        "CapturedDate":"2019-10-04 11:30:31",
        "ReturnUrl":"https://clicktime.symantec.com/3TVsxr2DrNxWzL9C7RZ19v97Vc?u=http%3A%2F%2Fwww.google.com.br%2522",
        "RecurrentPayment": {
          "ReasonCode": 0,
          "EndDate": "2030-02-27",
          "Interval": 2,
          "AuthorizeNow": true
        },
        "Status": 2,
        "IsSplitted": false,
        "ReturnMessage": "transacao capturada com sucesso",
        "ReturnCode": "00",
        "PaymentId": "a06f448c-ece6-450b-bbee-46fd9f8172bd",
        "Type": "DebitCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://clicktime.symantec.com/3UMBUMs6zhEfkus8rbrmqgi7Vc?u=https%3A%2F%2Fapiquerysandbox.cieloecommerce.cielo.com.br%2F1%2Fsales%2Fa06f448c-ece6-450b-bbee-46fd9f8172bd%2522"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2014121201",
    "Customer": {
        "Name": "Comprador rec programada"
    },
    "Payment": {
        "DebitCard": {
            "CardNumber": "123412******1231",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa",
            "CardOnFile": {
                "usage":"first"
            }
        },
        "Provider": "cielo",
        "AuthorizationCode": "603094",
        "Eci": "5",
        "Tid": "10069930698dvs6862lc",
        "ProofOfSale": "139053",
        "Authenticate": true,
        "ExternalAuthentication":{
          "Cavv":"a901234a5678a0123a567a90120=",
          "Xid":"a90123a45678a0123a567a90123",
          "Eci":"5",
          "Version":"2"
        },
        "Recurrent": false,
        "IssuerTransactionId":"009277050978376",
        "Amount": 15700,
        "ReceivedDate":"2019-10-04 11:29:49",
        "CapturedAmount":15700,
        "CapturedDate":"2019-10-04 11:30:31",
        "ReturnUrl":"https://clicktime.symantec.com/3TVsxr2DrNxWzL9C7RZ19v97Vc?u=http%3A%2F%2Fwww.google.com.br%2522",
        "RecurrentPayment": {
            "ReasonCode": 0,
            "EndDate": "2030-02-27",
            "Interval": 2,
            "AuthorizeNow": true
        },
        "Status": 2,
        "IsSplitted": false,
        "ReturnMessage": "transacao capturada com sucesso",
        "ReturnCode": "00",
        "PaymentId": "a06f448c-ece6-450b-bbee-46fd9f8172bd",
        "Type": "DebitCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://clicktime.symantec.com/3UMBUMs6zhEfkus8rbrmqgi7Vc?u=https%3A%2F%2Fapiquerysandbox.cieloecommerce.cielo.com.br%2F1%2Fsales%2Fa06f448c-ece6-450b-bbee-46fd9f8172bd%2522"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`DebitCard.CardNumber`|Número do Cartão do Comprador.|Texto|19|Sim|
|`DebitCard.Holder`|Nome do Comprador impresso no cartão.|Texto|25|Não|
|`DebitCard.ExpirationDate`|Data de validade impresso no cartão.|Texto|7|Sim|
|`DebitCard.SecurityCode`|Código de segurança impresso no verso do cartão.|Texto|4|Não|
|`DebitCard.Brand`|Bandeira do cartão.|Texto|10|Sim|
|`DebitCard.CardOnFile.Usage`|**First** se o cartão foi armazenado e é seu primeiro uso.<br>**Used** se o cartão foi armazenado e ele já foi utilizado anteriormente em outra transação.|Texto|---|Não|
|`DebitCard.CardOnFile.Reason`|Indica o propósito de armazenamento de cartões, caso o campo `Usage` for `Used`.<br>**Recurring** - Compra recorrente programada (ex. assinaturas).<br>**Unscheduled** - Compra recorrente sem agendamento (ex. aplicativos de serviços).<br>**Installments** - Parcelamento através da recorrência.|Texto|---|Condicional|
|`Payment.Provider`|Define comportamento do meio de pagamento (ver Anexo)/NÃO OBRIGATÓRIO PARA CRÉDITO.|Texto|15|---|
|`Payment.AuthorizationCode`|Código de autorização.|Texto|6|---|
|`Payment.Eci`|Eletronic Commerce Indicator. Representa o quão segura é uma transação.|Texto|2|---|
|`Payment.Tid`|Id da transação na adquirente.|Texto|6|---|
|`Payment.ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|---|
|`Payment.Authenticate`|Define se o comprador será direcionado ao Banco emissor para autenticação do cartão|Booleano|---|Não|
|`ExternalAuthentication.Cavv`|O valor Cavv é retornado pelo mecanismo de autenticação.|Texto|28|Sim|
|`ExternalAuthentication.Xid`|O valor Xid é retornado pelo mecanismo de autenticação.|Texto|28|Sim|
|`ExternalAuthentication.Eci`|O valor Eci é retornado pelo mecanismo de autenticação.|Número|1|Sim|
|`ExternalAuthentication.Version`|---|---|---|
|`Payment.Recurrent`|marcação de uma transação de recorrencia não programada|boolean|5|Não|
|`Payment.IssuerTransactionId`|Identificado de autenticação do Emissor para transações de débito recorrentes. Este campo deve ser enviado nas transações subsequentes da primeira transação no modelo de recorrência própria. Já no modelo de recorrência programada, a Cielo será a responsável por enviar o campo nas transações subsequentes.|Texto|15|---|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Payment.ReturnUrl`|URI para onde o usuário será redirecionado após o fim do pagamento|Texto|1024|Sim|
|`Payment.RecurrentPayment.EndDate`|Data para termino da recorrência.|Texto|10|Não|
|`Payment.RecurrentPayment.Interval`|Intervalo da recorrência.<br /><ul><li>Monthly (Default) </li><li>Bimonthly </li><li>Quarterly </li><li>SemiAnnual </li><li>Annual</li></ul>|Texto|10|Não|
|`Payment.RecurrentPayment.AuthorizeNow`|Booleano para saber se a primeira recorrência já vai ser Autorizada ou não.|Booleano|---|Sim|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Currency`|Moeda na qual o pagamento será feito (BRL).|Texto|3|Não|
|`Payment.Country`|Pais na qual o pagamento será feito.|Texto|3|Não|

## Agendando uma Recorrência Programada

Para criar uma venda recorrente cuja a primeira recorrência não será autorizada na mesma data com a forma de pagamento cartão de crédito, basta fazer um POST conforme o exemplo.

### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014113245231706",
   "Customer":{  
      "Name":"Comprador rec programada"
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":1500,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "RecurrentPayment":{
       "AuthorizeNow":"false",
       "EndDate":"2019-12-01",
       "Interval":"SemiAnnual",
       "StartDate":"2015-06-01"
     },
     "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"262",
         "SaveCard":"false",
         "Brand":"Visa"
     }
   }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2014113245231706",
   "Customer":{  
      "Name":"Comprador rec programada"
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":1500,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "RecurrentPayment":{
       "AuthorizeNow":"false",
       "EndDate":"2019-12-01",
       "Interval":"SemiAnnual",
       "StartDate":"2015-06-01"
     },
     "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"262",
         "SaveCard":"false",
         "Brand":"Visa"
     }
   }
}
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`RecurrentPaymentId`|Numero de identificação da Recorrência.|Texto|50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|255|Não|
|`Customer.Status`|Status de cadastro do comprador na loja (NEW / EXISTING) - Utilizado pela análise de fraude|Texto|255|Não|
|`Customer.Email`|Email do Comprador.|Texto|255|Não|
|`Customer.Birthdate`|Data de nascimento do Comprador.|Date|10|Não|
|`Customer.Identity`|Número do RG, CPF ou CNPJ do Cliente.|Texto|14|Não|
|`Customer.Address.Street`|Endereço do Comprador.|Texto|255|Não|
|`Customer.Address.Number`|Número do endereço do Comprador.|Texto|15|Não|
|`Customer.Address.Complement`|Complemento do endereço do Comprador.|Texto|50|Não|
|`Customer.Address.ZipCode`|CEP do endereço do Comprador.|Texto|9|Não|
|`Customer.Address.City`|Cidade do endereço do Comprador.|Texto|50|Não|
|`Customer.Address.State`|Estado do endereço do Comprador.|Texto|2|Não|
|`Customer.Address.Country`|Pais do endereço do Comprador.|Texto|35|Não|
|`Customer.Address.District`|Bairro do Comprador.|Texto|50|Não|
|`Customer.DeliveryAddress.Street`|Endereço do Comprador.|Texto|255|Não|
|`Customer.DeliveryAddress.Number`|Número do endereço do Comprador.|Texto|15|Não|
|`Customer.DeliveryAddress.Complement`|Complemento do endereço do Comprador.|Texto|50|Não|
|`Customer.DeliveryAddress.ZipCode`|CEP do endereço do Comprador.|Texto|9|Não|
|`Customer.DeliveryAddress.City`|Cidade do endereço do Comprador.|Texto|50|Não|
|`Customer.DeliveryAddress.State`|Estado do endereço do Comprador.|Texto|2|Não|
|`Customer.DeliveryAddress.Country`|Pais do endereço do Comprador.|Texto|35|Não|
|`Customer.DeliveryAddress.District`|Bairro do Comprador.|Texto|50|Não|

### Resposta

```json
{
    "MerchantOrderId": "2014113245231706",
    "Customer": {
        "Name": "Comprador rec programada"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "123412******1231",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "SoftDescriptor":"123456789ABCD",
        "Type": "CreditCard",
        "Amount": 1500,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 20,
        "RecurrentPayment": {
            "RecurrentPaymentId": "0d2ff85e-594c-47b9-ad27-bb645a103db4",
            "NextRecurrency": "2015-06-01",
            "StartDate": "2015-06-01",
            "EndDate": "2019-12-01",
            "Interval": "SemiAnnual",
            "Link": {
                "Method": "GET",
                "Rel": "recurrentPayment",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{PaymentId}"
            },
            "AuthorizeNow": false
        }
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2014113245231706",
    "Customer": {
        "Name": "Comprador rec programada"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "123412******1231",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "SoftDescriptor":"123456789ABCD",
        "Type": "CreditCard",
        "Amount": 1500,
        "Currency": "BRL",
        "Country": "BRA",
        "Provider": "Simulado",
        "ExtraDataCollection": [],
        "Status": 20,
        "RecurrentPayment": {
            "RecurrentPaymentId": "0d2ff85e-594c-47b9-ad27-bb645a103db4",
            "NextRecurrency": "2015-06-01",
            "StartDate": "2015-06-01",
            "EndDate": "2019-12-01",
            "Interval": "SemiAnnual",
            "Link": {
                "Method": "GET",
                "Rel": "recurrentPayment",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{PaymentId}"
            },
            "AuthorizeNow": false
        }
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`RecurrentPaymentId`|Campo Identificador da próxima recorrência.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`NextRecurrency`|Data da próxima recorrência.|Texto|7|12/2030 (MM/YYYY)|
|`StartDate`|Data do inicio da recorrência.|Texto|7|12/2030 (MM/YYYY)|
|`EndDate`|Data do fim da recorrência.|Texto|7|12/2030 (MM/YYYY)|
|`Interval`|Intervalo entre as recorrência.|Texto|10|/ Monthly / Bimonthly  / Quarterly  / SemiAnnual / Annual |
|`AuthorizeNow`|Booleano para saber se a primeira recorrencia já vai ser Autorizada ou não.|Booleano|---|true ou false|

## Modificando Recorrências

### Modificando dados do comprador

Para alterar os dados do comprador da Recorrência, basta fazer um Put conforme o exemplo.

#### Requisição

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/RecurrentPayment/{RecurrentPaymentId}/Customer</span></aside>

```json
{  
      "Name":"Customer",
      "Email":"customer@teste.com",
      "Birthdate":"1999-12-12",
      "Identity":"22658954236",
      "IdentityType":"CPF",
      "Address":{  
         "Street":"Rua Teste",
         "Number":"174",
         "Complement":"AP 201",
         "ZipCode":"21241140",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BRA"
      },
      "DeliveryAddress":{  
         "Street":"Outra Rua Teste",
         "Number":"123",
         "Complement":"AP 111",
         "ZipCode":"21241111",
         "City":"Qualquer Lugar",
         "State":"QL",
         "Country":"BRA",
        "District":"Teste"
      }
}
```

```shell
curl
--request PUT "https://apisandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}/Customer"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
      "Name":"Customer",
      "Email":"customer@teste.com",
      "Birthdate":"1999-12-12",
      "Identity":"22658954236",
      "IdentityType":"CPF",
      "Address":{  
         "Street":"Rua Teste",
         "Number":"174",
         "Complement":"AP 201",
         "ZipCode":"21241140",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BRA"
      },
      "DeliveryAddress":{  
         "Street":"Outra Rua Teste",
         "Number":"123",
         "Complement":"AP 111",
         "ZipCode":"21241111",
         "City":"Qualquer Lugar",
         "State":"QL",
         "Country":"BRA",
        "District":"Teste"
      }
   }
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`RecurrentPaymentId`|Numero de identificação da Recorrência.|Texto|50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|255|Não|
|`Customer.Status`|Status de cadastro do comprador na loja (NEW / EXISTING) - Utilizado pela análise de fraude|Texto|255|Não|
|`Customer.Email`|Email do Comprador.|Texto|255|Não|
|`Customer.Birthdate`|Data de nascimento do Comprador.|Date|10|Não|
|`Customer.Identity`|Número do RG, CPF ou CNPJ do Cliente.|Texto|14|Não|
|`Customer.IdentityType`|Texto|255|Não|Tipo de documento de identificação do comprador (CFP/CNPJ).|
|`Customer.Address.Street`|Endereço do Comprador.|Texto|255|Não|
|`Customer.Address.Number`|Número do endereço do Comprador.|Texto|15|Não|
|`Customer.Address.Complement`|Complemento do endereço do Comprador.|Texto|50|Não|
|`Customer.Address.ZipCode`|CEP do endereço do Comprador.|Texto|9|Não|
|`Customer.Address.City`|Cidade do endereço do Comprador.|Texto|50|Não|
|`Customer.Address.State`|Estado do endereço do Comprador.|Texto|2|Não|
|`Customer.Address.Country`|Pais do endereço do Comprador.|Texto|35|Não|
|`Customer.Address.District`|Bairro do Comprador.|Texto|50|Não|
|`Customer.DeliveryAddress.Street`|Endereço do Comprador.|Texto|255|Não|
|`Customer.DeliveryAddress.Number`|Número do endereço do Comprador.|Texto|15|Não|
|`Customer.DeliveryAddress.Complement`|Complemento do endereço do Comprador.|Texto|50|Não|
|`Customer.DeliveryAddress.ZipCode`|CEP do endereço do Comprador.|Texto|9|Não|
|`Customer.DeliveryAddress.City`|Cidade do endereço do Comprador.|Texto|50|Não|
|`Customer.DeliveryAddress.State`|Estado do endereço do Comprador.|Texto|2|Não|
|`Customer.DeliveryAddress.Country`|Pais do endereço do Comprador.|Texto|35|Não|
|`Customer.DeliveryAddress.District`|Bairro do Comprador.|Texto|50|Não|

#### Resposta

```shell

HTTP Status 200

```

Veja o Anexo [HTTP Status Code](#http-status-code) para a lista com todos os códigos de status HTTP possivelmente retornados pela API.

### Modificando data final da Recorrência

Para alterar a data final da Recorrência, basta fazer um Put conforme o exemplo.

#### Requisição

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/RecurrentPayment/{RecurrentPaymentId}/EndDate</span></aside>

```json

"2021-01-09"

```

```shell
curl
--request PUT "https://apisandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}/EndDate"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
"2021-01-09"
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`RecurrentPaymentId`|Numero de identificação da Recorrência.|Texto|50|Sim|
|`EndDate`|Data para termino da recorrência.|Texto|10|Sim|

#### Resposta

```shell

HTTP Status 200

```

Veja o Anexo [HTTP Status Code](#http-status-code) para a lista com todos os códigos de status HTTP possivelmente retornados pela API.

### Modificando intevalo da Recorrência

Para alterar o Intervalo da Recorrência, basta fazer um Put conforme o exemplo.

#### Requisição

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/RecurrentPayment/{RecurrentPaymentId}/Interval</span></aside>

```json

6

```

```shell
curl
--request PUT "https://apisandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}/Interval"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
6
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`RecurrentPaymentId`|Numero de identificação da Recorrência.|Texto|50|Sim|
|`Interval`|Intervalo da recorrência.<br>Monthly = 1 <BR>Bimonthly = 2<BR>Quarterly = 3<BR>SemiAnnual = 6<BR>Annual = 12<BR>|Número|2|Sim|

#### Resposta

```shell
HTTP Status 200
```

Veja o Anexo [HTTP Status Code](#http-status-code) para a lista com todos os códigos de status HTTP possivelmente retornados pela API.

### Modificar dia da Recorrência

Para modificar o dia da recorrência, basta fazer um Put conforme o exemplo.

<aside class="notice"><strong>Regra:</strong> Se o novo dia informado for depois do dia atual, iremos atualizar o dia da recorrência com efeito na próxima recorrência Ex.: Hoje é dia 5, e a próxima recorrência é dia 25/05. Quando eu atualizar para o dia 10, a data da próxima recorrência será dia10/05. Se o novo dia informado for antes do dia atual, iremos atualizar o dia da recorrência, porém este só terá efeito depois que a próxima recorrência for executada com sucesso. Ex.: Hoje é dia 5, e a próxima recorrência é dia 25/05. Quando eu atualizar para o dia 3, a data da próxima recorrência permanecerá dia 25/05, e após ela ser executada, a próxima será agendada para o dia 03/06. Se o novo dia informado for antes do dia atual, mas a próxima recorrência for em outro mês, iremos atualizar o dia da recorrência com efeito na próxima recorrência. Ex.: Hoje é dia 5, e a próxima recorrência é dia 25/09. Quando eu atualizar para o dia 3, a data da próxima recorrência será 03/09</aside>

#### Requisição

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/RecurrentPayment/{RecurrentPaymentId}/RecurrencyDay</span></aside>

```json
16
```

```shell
curl
--request PUT "https://apisandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}/RecurrencyDay"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
16
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`RecurrentPaymentId`|Numero de identificação da Recorrência.|Texto|50|Sim|
|`RecurrencyDay`|Dia da Recorrência.|Número|2|Sim|

#### Resposta

```shell
HTTP Status 200
```

Veja o Anexo [HTTP Status Code](#http-status-code) para a lista com todos os códigos de status HTTP possivelmente retornados pela API.

### Modificando o valor da Recorrência

Para modificar o valor da recorrência, basta fazer um Put conforme o exemplo.

#### Requsição

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/RecurrentPayment/{RecurrentPaymentId}/Amount</span></aside>

```json
156
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.com.br/1/RecurrentPayment/{RecurrentPaymentId}/Amount"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
156
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`RecurrentPaymentId`|Numero de identificação da Recorrência.|Texto|50|Sim|
|`Payment.Amount`|Valor do Pedido em centavos: 156 equivale a R$ 1,56|Número|15|Sim|

<aside class="warning">Essa alteração só afeta a data de pagamento da próxima recorrência.</aside>

#### Resposta

```shell
HTTP Status 200
```

Veja o Anexo [HTTP Status Code](#http-status-code) para a lista com todos os códigos de status HTTP possivelmente retornados pela API.

### Modificando data do próximo Pagamento

Para alterar a data do próximo Pagamento, basta fazer um Put conforme o exemplo.

#### Requisição

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/RecurrentPayment/{RecurrentPaymentId}/NextPaymentDate</span></aside>

```json
"2016-06-15"
```

```shell
curl
--request PUT "https://apisandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}/NextPaymentDate"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
"2016-06-15"
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`RecurrentPaymentId`|Numero de identificação da Recorrência.|Texto|50|Sim|
|`NextPaymentDate`|Data de pagamento da próxima recorrência.|Texto|10|Sim|

#### Resposta

```shell
HTTP Status 200
```

Veja o Anexo [HTTP Status Code](#http-status-code) para a lista com todos os códigos de status HTTP possivelmente retornados pela API.

### Modificando dados do Pagamento da Recorrência

Para alterar os dados de pagamento da Recorrência, basta fazer um Put conforme o exemplo.

<aside class="notice"><strong>Atenção:</strong> Essa alteração afeta a todos os dados do nó Payment. Então para manter os dados anteriores você deve informar os campos que não vão sofre alterações com os mesmos valores que já estavam salvos.</aside>

#### Requisição

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/RecurrentPayment/{RecurrentPaymentId}/Payment</span></aside>

```json
{  
   "Type":"CreditCard",
   "Amount":"123",
   "Installments":3,
   "Country":"USA",
   "Currency":"BRL",
   "SoftDescriptor":"123456789ABCD",
   "CreditCard":{  
      "Brand":"Master",
      "Holder":"Teset card",
      "CardNumber":"1234123412341232",
      "ExpirationDate":"12/2030"
   }
}
```

```shell
curl
--request PUT "https://apisandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}/Payment"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "Type":"CreditCard",
   "Amount":"123",
   "Installments":3,
   "Country":"USA",
   "Currency":"BRL",
   "SoftDescriptor":"123456789ABCD",
   "CreditCard":{  
      "Brand":"Master",
      "Holder":"Teset card",
      "CardNumber":"1234123412341232",
      "ExpirationDate":"12/2030"
   }
}
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`RecurrentPaymentId`|Numero de identificação da Recorrência.|Texto|50|Sim|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Payment.Installments`|Número de Parcelas.|Número|2|Sim|
|`Payment.SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - não permite caracteres especiais|Texto|13|Não|
|`CreditCard.CardNumber`|Número do Cartão do Comprador.|Texto|16|Sim|
|`CreditCard.Holder`|Nome do Comprador impresso no cartão.|Texto|25|Não|
|`CreditCard.ExpirationDate`|Data de validade impresso no cartão.|Texto|7|Sim|
|`CreditCard.SecurityCode`|Código de segurança impresso no verso do cartão.|Texto|4|Não|
|`CreditCard.Brand`|Bandeira do cartão.|Texto|10|Sim|

#### Resposta

```shell
HTTP Status 200
```

Veja o Anexo [HTTP Status Code](#http-status-code) para a lista com todos os códigos de status HTTP possivelmente retornados pela API.

### Desabilitando um Pedido Recorrente

Para desabilitar um pedido recorrente, basta fazer um Put conforme o exemplo.

#### Requisição

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/RecurrentPayment/{RecurrentPaymentId}/Deactivate</span></aside>

```shell
curl
--request PUT "https://apisandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}/Deactivate"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`RecurrentPaymentId`|Numero de identificação da Recorrência.|Texto|50|Sim|

#### Resposta

```shell
HTTP Status 200
```

Veja o Anexo [HTTP Status Code](#http-status-code) para a lista com todos os códigos de status HTTP possivelmente retornados pela API.

### Reabilitando um Pedido Recorrente

Para Reabilitar um pedido recorrente, basta fazer um Put conforme o exemplo.

#### Requisição

<aside class="request"><span class="method put">PUT</span> <span class="endpoint">/1/RecurrentPayment/{RecurrentPaymentId}/Reactivate</span></aside>

```shell
curl
--request PUT "https://apisandbox.cieloecommerce.cielo.com.br/1/RecurrentPayment/{RecurrentPaymentId}/Reactivate"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`RecurrentPaymentId`|Numero de identificação da Recorrência.|Texto|50|Sim|

#### Resposta

```shell
HTTP Status 200
```

Veja o Anexo [HTTP Status Code](#http-status-code) para a lista com todos os códigos de status HTTP possivelmente retornados pela API.

## Renova Facil

O uso desta funcionalidade permite a substituição automática de um cartão vencido .
Dessa forma, quando uma transação com marcação de recorrente for submetida para a API e a Cielo identificar que o cartão utilizado foi substituído, sua autorização será negada e serão retornados os dados do novo cartão conforme exemplo.

<aside class="notice"><strong>Atenção:</strong> Necessário solicitar a habilitação desta funcionalidade no cadastro  </aside>

### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014113245231706",
   "Customer":{  
      "Name":"Comprador Renova facil"
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":1500,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "RecurrentPayment":{
       "AuthorizeNow":"true",
       "EndDate":"2019-12-01",
       "Interval":"SemiAnnual"
     },
     "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"262",
         "SaveCard":"false",
         "Brand":"Visa"
     }
   }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
    {
   "MerchantOrderId":"2014113245231706",
   "Customer":{  
      "Name":"Comprador Renova facil"
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":1500,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "RecurrentPayment":{
       "AuthorizeNow":"true",
       "EndDate":"2019-12-01",
       "Interval":"SemiAnnual"
     },
     "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"262",
         "SaveCard":"false",
         "Brand":"Visa"
     }
   }
}
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|6|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Numero de identificação do Pedido.|Texto|50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|255|Não|
|`Customer.Status`|Status de cadastro do comprador na loja (NEW / EXISTING) - Utilizado pela análise de fraude|Texto|255|Não|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Payment.Installments`|Número de Parcelas.|Número|2|Sim|
|`Payment.SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - não permite caracteres especiais|Texto|13|Não|
|`Payment.RecurrentPayment.EndDate`|Data para termino da recorrência.|Texto|10|Não|
|`Payment.RecurrentPayment.Interval`|Intervalo da recorrência.<br /><ul><li>Monthly (Default) </li><li>Bimonthly </li><li>Quarterly </li><li>SemiAnnual </li><li>Annual</li></ul>|Texto|10|Não|
|`Payment.RecurrentPayment.AuthorizeNow`|Booleano para saber se a primeira recorrência já vai ser Autorizada ou não.|Booleano|---|Sim|
|`CreditCard.CardNumber`|Número do Cartão do Comprador.|Texto|19|Sim|
|`CreditCard.Holder`|Nome do Comprador impresso no cartão.|Texto|25|Não|
|`CreditCard.ExpirationDate`|Data de validade impresso no cartão.|Texto|7|Sim|
|`CreditCard.SecurityCode`|Código de segurança impresso no verso do cartão.|Texto|4|Não|
|`CreditCard.Brand`|Bandeira do cartão.|Texto|10|Sim|

### Resposta

```json
{
  "MerchantOrderId": "2014113245231706",
  "Customer": {
    "Name": "Comprador  Renova facil"
  },
  "Payment": {
    "ServiceTaxAmount": 0,
    "Installments": 1,
    "Interest": 0,
    "Capture": false,
    "Authenticate": false,
    "Recurrent": false,
    "CreditCard": {
      "CardNumber": "123412******1231",
      "Holder": "Teste Holder",
      "ExpirationDate": "12/2030",
      "SaveCard": false,
      "Brand": "Visa"
    },
    "Tid": "10447480685P4611AQ9B",
    "ProofOfSale": "087001",
    "SoftDescriptor": "123456789ABCD",
    "Provider": "Cielo",
    "Eci": "0",
    "NewCard": {
       "CardNumber": "40000000000000000",
       "ExpirationDate": "10/2020",
       "SaveCard": false,
        "Brand": "Visa"
    },
    "VelocityAnalysis": {
      "Id": "94f06657-c715-45d2-a563-63f7dbb19e08",
      "ResultMessage": "Accept",
      "Score": 0
    },
    "PaymentId": "94f06657-c715-45d2-a563-63f7dbb19e08",
    "Type": "CreditCard",
    "Amount": 1500,
    "ReceivedDate": "2016-12-26 14:14:21",
    "Currency": "BRL",
    "Country": "BRA",
    "ReturnCode": "KA",
    "ReturnMessage": "Autorizacao negada",
    "Status": 3,
    "RecurrentPayment": {
      "ReasonCode": 7,
      "ReasonMessage": "Denied",
      "EndDate": "2019-12-01",
      "Interval": 6,
      "AuthorizeNow": true
    },
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquery.cieloecommerce.cielo.com.br/1/sales/94f06657-c715-45d2-a563-63f7dbb19e08"
      }
    ]
  }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
  "MerchantOrderId": "2014113245231706",
  "Customer": {
    "Name": "Comprador  Renova facil"
  },
  "Payment": {
    "ServiceTaxAmount": 0,
    "Installments": 1,
    "Interest": 0,
    "Capture": false,
    "Authenticate": false,
    "Recurrent": false,
    "CreditCard": {
      "CardNumber": "123412******1231",
      "Holder": "Teste Holder",
      "ExpirationDate": "12/2030",
      "SaveCard": false,
      "Brand": "Visa"
    },
    "Tid": "10447480685P4611AQ9B",
    "ProofOfSale": "087001",
    "SoftDescriptor": "123456789ABCD",
    "Provider": "Cielo",
    "Eci": "0",
    "NewCard": {
       "CardNumber": "40000000000000000",
       "ExpirationDate": "10/2020",
       "SaveCard": false,
        "Brand": "Visa"
    },
    "VelocityAnalysis": {
      "Id": "94f06657-c715-45d2-a563-63f7dbb19e08",
      "ResultMessage": "Accept",
      "Score": 0
    },
    "PaymentId": "94f06657-c715-45d2-a563-63f7dbb19e08",
    "Type": "CreditCard",
    "Amount": 1500,
    "ReceivedDate": "2016-12-26 14:14:21",
    "Currency": "BRL",
    "Country": "BRA",
    "ReturnCode": "KA",
    "ReturnMessage": "Autorizacao negada",
    "Status": 3,
    "RecurrentPayment": {
      "ReasonCode": 7,
      "ReasonMessage": "Denied",
      "EndDate": "2019-12-01",
      "Interval": 6,
      "AuthorizeNow": true
    },
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquery.cieloecommerce.cielo.com.br/1/sales/94f06657-c715-45d2-a563-63f7dbb19e08"
      }
    ]
  }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`RecurrentPaymentId`|Campo Identificador da próxima recorrência.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`NextRecurrency`|Data da próxima recorrência.|Texto|7|12/2030 (MM/YYYY)|
|`EndDate`|Data do fim da recorrência.|Texto|7|12/2030 (MM/YYYY)|
|`Interval`|Intervalo entre as recorrência.|Texto|10|/ Monthly / Bimonthly  / Quarterly  / SemiAnnual / Annual |
|`AuthorizeNow`|Booleano para saber se a primeira recorrencia já vai ser Autorizada ou não.|Booleano|---|true ou false|

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`NewCard.CardNumber`|Novo número do Cartão do Comprador.|Texto|16|Sim|
|`NewCard.ExpirationDate`|nova data de validade do cartão.|Texto|7|Sim|
|`NewCard.Brand`|Bandeira do cartão.|Texto|10|Sim|
|`NewCard.SaveCard`|Identifica se o cartão gerou Cardtoken durante a transação|Booleano|---|Sim|

### Bandeiras e Emissores Habilitados

Bandeiras e Emissores que já estão com o Renova Facil habilitados:

|Emissores|VISA|MASTER|ELO|
|---|---|---|---|
|`BRADESCO`|Sim|Sim|Sim|
|`BANCO DO BRASIL`|Sim|---|---|
|`SANTADER`|Sim|---|---|
|`CITI`|Sim|---|---|
|`BANCO PAN`|Sim|---|---|

# Tokenização de cartões

## O que é a Tokenização de Cartões:

É uma plataforma que permite o armazenamento seguro de dados sensíveis de cartão de crédito. Estes dados são transformados em um código criptografrado chamado de “token”, que poderá ser armazenado em banco de dados. Com a plataforma, a loja poderá oferecer recursos como "Compra com 1 clique” e "Retentativa de envio de transação”, sempre preservando a integridade e a confidencialidade das informações.

## Criando um Cartão Tokenizado

Para salvar um cartão sem autoriza-lo, basta realizar um posto com os dados do cartão.

### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/card/</span></aside>

```json
{
    "CustomerName": "Comprador Teste Cielo",
    "CardNumber":"4532117080573700",
    "Holder":"Comprador T Cielo",
    "ExpirationDate":"12/2030",
    "Brand":"Visa"
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/card/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "CustomerName": "Comprador Teste Cielo",
    "CardNumber":"4532117080573700",
    "Holder":"Comprador T Cielo",
    "ExpirationDate":"12/2030",
    "Brand":"Visa"
}
--verbose
```

|Propriedade|Tipo|Tamanho|Obrigatório|Descrição|
|---|---|---|---|---|
|`Name`|Texto|255|Sim|Nome do Comprador.|
|`CardNumber`|Texto|16|Sim|Número do Cartão do Comprador.|
|`Holder`|Texto|25|Sim|Nome do Comprador impresso no cartão.|
|`ExpirationDate`|Texto|7|Sim|Data de validade impresso no cartão.|
|`Brand`|Texto|10|Sim|Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover).|

### Resposta

```json
{
  "CardToken": "db62dc71-d07b-4745-9969-42697b988ccb",
  "Links": {
    "Method": "GET",
    "Rel": "self",
    "Href": "https://apiquerydev.cieloecommerce.cielo.com.br/1/card/db62dc71-d07b-4745-9969-42697b988ccb"}
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
  "CardToken": "db62dc71-d07b-4745-9969-42697b988ccb",
  "Links": {
    "Method": "GET",
    "Rel": "self",
    "Href": "https://apiquerydev.cieloecommerce.cielo.com.br/1/card/db62dc71-d07b-4745-9969-42697b988ccb"}
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`Cardtoken`|Token de identificação do Cartão.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|

## Criando um Cartão Tokenizado durante uma autorização

Para salvar um cartão, criando seu token, basta enviar uma requisição padrão de criação de venda, enviado SaveCard como " true". O response retornará o Token do cartão.

### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014111701",
   "Customer":{  
      "Name":"Comprador Teste",
      "Email":"compradorteste@teste.com",
      "Birthdate":"1991-01-02",
      "Address":{  
         "Street":"Rua Teste",
         "Number":"123",
         "Complement":"AP 123",
         "ZipCode":"12345987",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BRA"
      },
        "DeliveryAddress": {
            "Street": "Rua Teste",
            "Number": "123",
            "Complement": "AP 123",
            "ZipCode": "12345987",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        }
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":15700,
     "Currency":"BRL",
     "Country":"BRA",
     "ServiceTaxAmount":0,
     "Installments":1,
     "Interest":"ByMerchant",
     "Capture":true,
     "Authenticate":false,
     "SoftDescriptor":"123456789ABCD",
     "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"123",
         "SaveCard":"true",
         "Brand":"Visa"
     }
   }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2014111701",
   "Customer":{  
      "Name":"Comprador Teste",
      "Identity":"11225468954",
      "IdentityType":"CPF",
      "Email":"compradorteste@teste.com",
      "Birthdate":"1991-01-02",
      "Address":{  
         "Street":"Rua Teste",
         "Number":"123",
         "Complement":"AP 123",
         "ZipCode":"12345987",
         "City":"Rio de Janeiro",
         "State":"RJ",
         "Country":"BRA"
      },
        "DeliveryAddress": {
            "Street": "Rua Teste",
            "Number": "123",
            "Complement": "AP 123",
            "ZipCode": "12345987",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        }
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":15700,
     "ServiceTaxAmount":0,
     "Installments":1,
     "Interest":"ByMerchant",
     "Capture":true,
     "Authenticate":false,
     "SoftDescriptor":"123456789ABCD",
     "CreditCard":{  
         "CardNumber":"4551870000000183",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "SecurityCode":"123",
         "SaveCard":"true",
         "Brand":"Visa"
     }
   }
}
--verbose
```

|Propriedade|Tipo|Tamanho|Obrigatório|Descrição|
|---|---|---|---|---|
|`MerchantId`|Guid|36|Sim|Identificador da loja na Cielo.|
|`MerchantKey`|Texto|40|Sim|Chave Publica para Autenticação Dupla na Cielo.|
|`RequestId`|Guid|36|Não|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.|
|`MerchantOrderId`|Texto|50|Sim|Numero de identificação do Pedido.|
|`Customer.Name`|Texto|255|Não|Nome do Comprador.|
|`Customer.Status`|Texto|255|Não|Status de cadastro do comprador na loja (NEW / EXISTING)|
|`Customer.Identity`|Texto|14|Não|Número do RG, CPF ou CNPJ do Cliente.|
|`Customer.IdentityType`|Texto|255|Não|Tipo de documento de identificação do comprador (CFP/CNPJ).|
|`Customer.Email`|Texto|255|Não|Email do Comprador.|
|`Customer.Birthdate`|Date|10|Não|Data de nascimento do Comprador.|
|`Customer.Address.Street`|Texto|255|Não|Endereço do Comprador.|
|`Customer.Address.Number`|Texto|15|Não|Número do endereço do Comprador.|
|`Customer.Address.Complement`|Texto|50|Não|Complemento do endereço do Comprador.br|
|`Customer.Address.ZipCode`|Texto|9|Não|CEP do endereço do Comprador.|
|`Customer.Address.City`|Texto|50|Não|Cidade do endereço do Comprador.|
|`Customer.Address.State`|Texto|2|Não|Estado do endereço do Comprador.|
|`Customer.Address.Country`|Texto|35|Não|Pais do endereço do Comprador.|
|`Customer.DeliveryAddress.Street`|Texto|255|Não|Endereço do Comprador.|
|`Customer.Address.Number`|Texto|15|Não|Número do endereço do Comprador.|
|`Customer.DeliveryAddress.Complement`|Texto|50|Não|Complemento do endereço do Comprador.|
|`Customer.DeliveryAddress.ZipCode`|Texto|9|Não|CEP do endereço do Comprador.|
|`Customer.DeliveryAddress.City`|Texto|50|Não|Cidade do endereço do Comprador.|
|`Customer.DeliveryAddress.State`|Texto|2|Não|Estado do endereço do Comprador.|
|`Customer.DeliveryAddress.Country`|Texto|35|Não|Pais do endereço do Comprador.|
|`Payment.Type`|Texto|100|Sim|Tipo do Meio de Pagamento.|
|`Payment.Amount`|Número|15|Sim|Valor do Pedido (ser enviado em centavos).|
|`Payment.Currency`|Texto|3|Não|Moeda na qual o pagamento será feito (BRL).|
|`Payment.Country`|Texto|3|Não|Pais na qual o pagamento será feito.|
|`Payment.Provider`|Texto|15|---|Define comportamento do meio de pagamento (ver Anexo)/NÃO OBRIGATÓRIO PARA CRÉDITO.|
|`Payment.Installments`|Número|2|Sim|Número de Parcelas.|
|`Payment.Interest`|Texto|10|Não|Tipo de parcelamento - Loja (ByMerchant) ou Cartão (ByIssuer).|
|`Payment.Capture`|Booleano|---|Não (Default false)|Booleano que identifica que a autorização deve ser com captura automática.|
|`Payment.Authenticate`|Booleano|---|Não (Default false)|Define se o comprador será direcionado ao Banco emissor para autenticação do cartão|
|`Payment.ServiceTaxAmount`|Número|15|Não|[Veja Anexo](https://developercielo.github.io/manual/cielo-ecommerce#service-tax-amount-taxa-de-embarque)|
|`CreditCard.CardNumber`|Texto|19|Sim|Número do Cartão do Comprador.|
|`CreditCard.Holder`|Texto|25|Não|Nome do Comprador impresso no cartão.|
|`CreditCard.ExpirationDate`|Texto|7|Sim|Data de validade impresso no cartão.|
|`CreditCard.SecurityCode`|Texto|4|Não|Código de segurança impresso no verso do cartão - Ver Anexo.|
|`CreditCard.SaveCard`|Booleano|---|Não (Default false)|Booleano que identifica se o cartão será salvo para gerar o CardToken.|
|`CreditCard.Brand`|Texto|10|Sim|Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).|

### Resposta

```json
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador Teste",
        "Identity":"11225468954",
        "IdentityType":"CPF",
        "Email": "compradorteste@teste.com",
        "Birthdate": "1991-01-02",
        "Address": {
            "Street": "Rua Teste",
            "Number": "123",
            "Complement": "AP 123",
            "ZipCode": "12345987",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        },
        "DeliveryAddress": {
            "Street": "Rua Teste",
            "Number": "123",
            "Complement": "AP 123",
            "ZipCode": "12345987",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        }
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": true,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "455187******0183",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": true,
            "CardToken": "d37bf475-307d-47be-b50a-8dcc38c5056c",
            "Brand": "Visa"
        },
        "ProofOfSale": "674532",
        "Tid": "0305020554239",
        "AuthorizationCode": "123456",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId": "24bc8366-fc31-4d6c-8555-17049a836a07",
        "Type": "CreditCard",
        "Amount": 15700,
        "CapturedAmount": 15700,
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 2,
        "ReturnCode": "6",
        "ReturnMessage": "Operation Successful",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            }
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador Teste",
        "Identity":"11225468954",
        "IdentityType":"CPF",
        "Email": "compradorteste@teste.com",
        "Birthdate": "1991-01-02",
        "Address": {
            "Street": "Rua Teste",
            "Number": "123",
            "Complement": "AP 123",
            "ZipCode": "12345987",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        },
        "DeliveryAddress": {
            "Street": "Rua Teste",
            "Number": "123",
            "Complement": "AP 123",
            "ZipCode": "12345987",
            "City": "Rio de Janeiro",
            "State": "RJ",
            "Country": "BRA"
        }
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": true,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "455187******0183",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": true,
            "CardToken": "d37bf475-307d-47be-b50a-8dcc38c5056c"
            "Brand": "Visa"
        },
        "ProofOfSale": "674532",
        "Tid": "0305020554239",
        "AuthorizationCode": "123456",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId": "24bc8366-fc31-4d6c-8555-17049a836a07",
        "Type": "CreditCard",
        "Amount": 15700,
        "CapturedAmount": 15700,
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 2,
        "ReturnCode": "6",
        "ReturnMessage": "Operation Successful",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            }
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|Texto alfanumérico|
|`Tid`|Id da transação na adquirente.|Texto|20|Texto alfanumérico|
|`AuthorizationCode`|Código de autorização.|Texto|6|Texto alfanumérico|
`SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais|Texto|13|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ECI`|Eletronic Commerce Indicator. Representa o quão segura é uma transação.|Texto|2|Exemplos: 7|
|`Status`|Status da Transação.|Byte|---|2|
|`ReturnCode`|Código de retorno da Adquirência.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da Adquirência.|Texto|512|Texto alfanumérico|
|`Cardtoken`|Token de identificação do Cartão.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|

## Criando uma venda com Cartão Tokenizado

Para criar uma venda de cartão de crédito com token do cartão protegido, é necessário fazer um POST para o recurso Payment conforme o exemplo.

### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014111706",
   "Customer":{  
      "Name":"Comprador Teste"
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":100,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "CreditCard":{  
         "CardToken":"6e1bf77a-b28b-4660-b14f-455e2a1c95e9",
         "SecurityCode":"262",
         "Brand":"Visa"
     }
   }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2014111706",
   "Customer":{  
      "Name":"Comprador Teste"
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":100,
     "Installments":1,
     "SoftDescriptor":"123456789ABCD",
     "CreditCard":{  
         "CardToken":"6e1bf77a-b28b-4660-b14f-455e2a1c95e9",
         "SecurityCode":"262",
         "Brand":"Visa"
     }
   }
}
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Numero de identificação do Pedido.|Texto|50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|255|Não|
|`Customer.Status`|Status de cadastro do comprador na loja (NEW / EXISTING) - Utilizado pela análise de fraude|Texto|255|Não|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Payment.Installments`|Número de Parcelas.|Número|2|Sim|
|`Payment.SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - não permite caracteres especiais|Texto|13|Não|
|`Payment.ReturnUrl`|URI para onde o usuário será redirecionado após o fim do pagamento|Texto|1024|Sim quando Authenticate = true|
|`CreditCard.CardToken`|Token de identificação do Cartão.|Guid|36|Sim|
|`CreditCard.SecurityCode`|Código de segurança impresso no verso do cartão.|Texto|4|Não|
|`CreditCard.Brand`|Bandeira do cartão.|Texto|10|Sim|

### Resposta

```json
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador Teste"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "CreditCard": {
            "SaveCard": false,
            "CardToken": "6e1bf77a-b28b-4660-b14f-455e2a1c95e9",
            "Brand": "Visa"
        },
        "ProofOfSale": "5036294",
        "Tid": "0310025036294",
        "AuthorizationCode": "319285",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId": "c3ec8ec4-1ed5-4f8d-afc3-19b18e5962a8",
        "Type": "CreditCard",
        "Amount": 100,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
        {
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador Teste"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "CreditCard": {
            "SaveCard": false,
            "CardToken": "6e1bf77a-b28b-4660-b14f-455e2a1c95e9",
            "Brand": "Visa"
        },
        "ProofOfSale": "5036294",
        "Tid": "0310025036294",
        "AuthorizationCode": "319285",
        "SoftDescriptor":"123456789ABCD",
        "PaymentId": "c3ec8ec4-1ed5-4f8d-afc3-19b18e5962a8",
        "Type": "CreditCard",
        "Amount": 100,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|Texto alfanumérico|
|`Tid`|Id da transação na adquirente.|Texto|20|Texto alfanumérico|
|`AuthorizationCode`|Código de autorização.|Texto|6|Texto alfanumérico|
`SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais|Texto|13|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ECI`|Eletronic Commerce Indicator. Representa o quão segura é uma transação.|Texto|2|Exemplos: 7|
|`Status`|Status da Transação.|Byte|---|2|
|`ReturnCode`|Código de retorno da Adquirência.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da Adquirência.|Texto|512|Texto alfanumérico|

## Criando uma venda com Cartão Tokenizado na 1.5

Para criar uma venda de cartão de crédito com token do webservice 1.5, é necessário fazer um POST para o recurso Payment conforme o exemplo.

Para uso em Sandbox, é possivel simular transações autorizadas ou negadas via tokens de teste:

|Status|Token|
|---|---|
|Autorizado|6fb7a669aca457a9e43009b3d66baef8bdefb49aa85434a5adb906d3f920bfeA|
|Negado|6fb7a669aca457a9e43009b3d66baef8bdefb49aa85434a5adb906d3f920bfeB|

### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014111706",
   "Customer":{  
      "Name":"Comprador token 1.5"     
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":100,
     "Installments":1,
     "CreditCard":{  
        "CardToken":"6fb7a669aca457a9e43009b3d66baef8bdefb49aa85434a5adb906d3f920bfeA",
        "Brand":"Visa"
     }
   }
}
```

```shell
curl
--request POST "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/"
--header "Content-Type: application/json"
--header "MerchantId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--header "MerchantKey: 0123456789012345678901234567890123456789"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{  
   "MerchantOrderId":"2014111706",
   "Customer":{  
      "Name":"Comprador token 1.5"     
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":100,
     "Installments":1,
     "CreditCard":{  
        "CardToken":"6fb7a669aca457a9e43009b3d66baef8bdefb49aa85434a5adb906d3f920bfeA",
        "Brand":"Visa"
     }
   }
}
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|---|---|---|---|---|
|`MerchantId`|Identificador da loja na API Cielo eCommerce.|Guid|36|Sim|
|`MerchantKey`|Chave Publica para Autenticação Dupla na API Cielo eCommerce.|Texto|40|Sim|
|`RequestId`|Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT|Guid|36|Não|
|`MerchantOrderId`|Numero de identificação do Pedido.|Texto|50|Sim|
|`Customer.Name`|Nome do Comprador.|Texto|255|Não|
|`Customer.Status`|Status de cadastro do comprador na loja (NEW / EXISTING) - Utilizado pela análise de fraude|Texto|255|Não|
|`Payment.Type`|Tipo do Meio de Pagamento.|Texto|100|Sim|
|`Payment.Amount`|Valor do Pedido (ser enviado em centavos).|Número|15|Sim|
|`Payment.Installments`|Número de Parcelas.|Número|2|Sim|
|`CreditCard.CardToken`|Token de identificação do Cartão.|Guid|300|Sim|
|`CreditCard.Brand`|Bandeira do cartão.|Texto|10|Sim|

### Resposta

```json
{
  "MerchantOrderId": "2014111706",
  "Customer": {
    "Name": "Comprador token 1.5"
  },
  "Payment": {
    "ServiceTaxAmount": 0,
    "Installments": 1,
    "Interest": 0,
    "Capture": false,
    "Authenticate": false,
    "Recurrent": false,
    "CreditCard": {
      "SaveCard": false,
      "CardToken": "6fb7a669aca457a9e43009b3d66baef8bdefb49aa85434a5adb906d3f920bfeA",
      "Brand": "Visa"
    },
    "Tid": "0307050140148",
    "ProofOfSale": "140148",
    "AuthorizationCode": "045189",
    "Provider": "Simulado",
    "PaymentId": "8c14cdcf-d5a9-46b0-b040-c0d054cd8f76",
    "Type": "CreditCard",
    "Amount": 100,
    "ReceivedDate": "2017-03-07 17:01:40",
    "Currency": "BRL",
    "Country": "BRA",
    "ReturnCode": "4",
    "ReturnMessage": "Operation Successful",
    "Status": 1,
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/8c14cdcf-d5a9-46b0-b040-c0d054cd8f76"
      },
      {
        "Method": "PUT",
        "Rel": "capture",
        "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/8c14cdcf-d5a9-46b0-b040-c0d054cd8f76/capture"
      },
      {
        "Method": "PUT",
        "Rel": "void",
        "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/8c14cdcf-d5a9-46b0-b040-c0d054cd8f76/void"
      }
    ]
  }
}
```

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
  "MerchantOrderId": "2014111706",
  "Customer": {
    "Name": "Comprador token 1.5"
  },
  "Payment": {
    "ServiceTaxAmount": 0,
    "Installments": 1,
    "Interest": 0,
    "Capture": false,
    "Authenticate": false,
    "Recurrent": false,
    "CreditCard": {
      "SaveCard": false,
      "CardToken": "6fb7a669aca457a9e43009b3d66baef8bdefb49aa85434a5adb906d3f920bfeA",
      "Brand": "Visa"
    },
    "Tid": "0307050140148",
    "ProofOfSale": "140148",
    "AuthorizationCode": "045189",
    "Provider": "Simulado",
    "PaymentId": "8c14cdcf-d5a9-46b0-b040-c0d054cd8f76",
    "Type": "CreditCard",
    "Amount": 100,
    "ReceivedDate": "2017-03-07 17:01:40",
    "Currency": "BRL",
    "Country": "BRA",
    "ReturnCode": "4",
    "ReturnMessage": "Operation Successful",
    "Status": 1,
    "Links": [
      {
        "Method": "GET",
        "Rel": "self",
        "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/8c14cdcf-d5a9-46b0-b040-c0d054cd8f76"
      },
      {
        "Method": "PUT",
        "Rel": "capture",
        "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/8c14cdcf-d5a9-46b0-b040-c0d054cd8f76/capture"
      },
      {
        "Method": "PUT",
        "Rel": "void",
        "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/8c14cdcf-d5a9-46b0-b040-c0d054cd8f76/void"
      }
    ]
  }
}

```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|---|---|---|---|---|
|`ProofOfSale`|Número da autorização, identico ao NSU.|Texto|6|Texto alfanumérico|
|`Tid`|Id da transação na adquirente.|Texto|20|Texto alfanumérico|
|`AuthorizationCode`|Código de autorização.|Texto|6|Texto alfanumérico|
`SoftDescriptor`|Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais|Texto|13|Texto alfanumérico|
|`PaymentId`|Campo Identificador do Pedido.|Guid|36|xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`ECI`|Eletronic Commerce Indicator. Representa o quão segura é uma transação.|Texto|2|Exemplos: 7|
|`Status`|Status da Transação.|Byte|---|2|
|`ReturnCode`|Código de retorno da Adquirência.|Texto|32|Texto alfanumérico|
|`ReturnMessage`|Mensagem de retorno da Adquirência.|Texto|512|Texto alfanumérico|

# Consulta Bin

O “**Consulta de Bins**”  é um serviço de **pesquisa de dados do cartão**, de crédito ou débito, que retorna ao lojista da API Cielo e-Commerce informações que permitem validar os dados preenchidos na tela do checkout. O serviço retorna os seguintes dados sobre o cartão:

* **Bandeira do cartão:** Nome da Bandeira
* **Tipo de cartão:** Crédito, Débito ou Múltiplo (Crédito e Débito)
* **Nacionalidade do cartão:** Estrangeiro ou Nacional
* **Cartão Corporativo:** Se o cartão é ou não é corporativo
* **Banco Emissor:** Código e Nome

Essas informações permitem tomar ações no momento do checkout para melhorar a conversão da loja.

<aside class="warning">O Consulta Bin deve ser habilitado pelo Suporte Cielo. Entre em contato com a equipe de Suporte e solicite a habilitação para sua loja.</aside>

## Caso de Uso

Veja um exemplo de uso: **Consulta Bins + recuperação de carrinho**

Um marketplace chamada Submergível possui uma gama de meios de pagamento disponíveis para que suas lojas possam oferecer ao comprador, mas mesmo com toda essa oferta, ela continua com uma taxa de conversão baixa.

Conhecendo a função de consulta Bins da API Cielo Ecommerce, como ela poderia evitar a perda de carrinhos?

Ela pode aplicar a Consulta bins e 3 cenários!

1. Impedir erros com tipo de cartão
2. Oferecer recuperação de carrinhos online
3. Alertar sobre cartões internacionais
4. Impedir erros com tipo de cartão

O Submergível pode usar a consulta bins no carrinho para identificar 2 dos principais erros no preenchimento de formulários de pagamento: 

* **Bandeira errada** e confundir cartão de crédito com débito ○ Bandeiras erradas: Ao preencher o formulário de pagamento, é possível realizar uma consulta e já definir a bandeira correta. Esse é um método muito mais seguro do que sebasear em algoritmos no formulário, pois a base de bins consultada é a da bandeira emissora do cartão.

* **Confusões com cartões:** Ao preencher o formulário de pagamento, é possível realizar uma consulta e avisar ao consumidor se ele está usando um cartão de  débito quando na verdade deveria usar um  de débito

<br>

**Oferecer recuperação de carrinhos online**

* O Submergível pode usar a consulta bins no carrinho para oferecer um novo meio de pagamento caso a transação falhe na primeira tentativa.

* Realizando uma consulta no momento de preenchimento do formulário de pagamento, caso o cartão seja múltiplo (Crédito e Débito), o Submergível pode reter os dados do cartão, e caso a transação de crédito falhe, ele pode oferecer automaticamente ao consumidor uma transação de débito com o mesmo cartão.

<br>

**Alertar sobre cartões internacionais**
O Submergível pode usar a consulta bins no carrinho para alertar compradores internacionais, que caso o cartão não esteja habilitado para transacionar no Brasil, a transação será negada

## Integração

### Request

Basta realizar um `GET` enviado o BIN a nossa URL de consulta:

<aside class="request"><span class="method get">GET</span><span class="endpoint">/1/cardBin/`BIN`</span></aside>

|Campo|Descrição|
|-----|---------|
|`BIN`|6 primeiros dígitos do cartão<br>_Para simular uma consulta em SandBox com retorno `ForeignCard=false`, o terceiro dígito do BIN deve ser 1, e o quinto dígito deve ser diferente de 2 e 3.<br>Exemplos:001040, 501010, 401050_ |

``` json
https://apiquerysandbox.cieloecommerce.cielo.com.br/1/cardBin/420020
```

### Response

``` json
{
    "Status": "00",
    "Provider": "VISA",
    "CardType": "Crédito",
    "ForeignCard": true,
    "CorporateCard": true,
    "Issuer": "Bradesco",
    "IssuerCode": "237"
}
```

| Paramêtro     | Tipo  | Tamanho | Descrição                                                                                                                                                                                  |
|---------------|-------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Status`      | Texto | 2       | Status da requisição de análise de Bins: <br><br> 00 – Analise autorizada <br> 01 – Bandeira não suportada <br> 02 – Cartão não suportado na consulta de bin <br> 73 – Afiliação bloqueada |
| `Provider`    | Texto | 255     | Bandeira do cartão                                                                                                                                                                         |
| `CardType`    | Texto | 20      | Tipo do cartão em uso : <br><br> Crédito <br> Débito <br>Multiplo                                                                                                                          |
| `ForeingCard` | Booleano | -     | Se o cartão é emitido no exterior (False/True)                                                                                                                                             |
| `CorporateCard` | Booleano | -     | Se o cartão é corporativo (False/True)                                                                                                                                             |
| `Issuer` | Texto | 255     | Nome do emissor do cartão                                                                                                                                        |
| `IssuerCode` | Texto | 255     | Código do emissor do cartão                                                                                                                                           |

> **Atenção**: Em SANDBOX os valores retornados são simulações e não validações reais de BINS. Deve ser considerado apenas o retorno do Request e o seu formato. Para identificação real dos BINS, o ambiente de Produção deverá ser utilizado.

# Zero Auth

O **Zero Auth** é uma ferramenta de validação de cartões da API Cielo. A validação permite que o lojista saiba se o cartão é valido ou não antes de enviar a transação para autorização, antecipando o motivo de uma provável não autorização..

O **Zero Auth** pode ser usado de 2 maneiras:

1. **Padrão** - Envio de um cartão padrão, sem tokenização ou analises adicionais
2. **Com cartão Tokenizado** - Envio de um TOKEN 3.0 para analise

É importante destacar que o Zero Auth **não retorna ou analisa** os seguintes itens:

1. Limite de crédito do cartão
2. Informações sobre o portador
3. Não aciona a base bancaria (dispara SMS so portador)

O Zero Auth suporta as seguintes bandeiras:

* **Visa**
* **MasterCard**
* **Elo**

> Caso outras bandeiras sejam enviadas, o erro **57-Bandeira inválida** será exibido.

## Caso de uso

Este é um exemplo de como usar o zero auth para melhorar sua conversão de vendas.

O Zero Auth é uma ferramenta da Cielo que permite verificar se um cartão está valido para realizar uma compra antes que o pedido seja finalizado. Ele faz isso simulando uma autorização, mas sem afetar o limite de crédito ou alertar o portados do cartão sobre o teste.

Ela não informa o limite ou características do cartão ou portador, mas simula uma autorização Cielo, validando dados como:

1. Se  o cartão está valido junto ao banco emissor
2. Se o cartão possui limite disponível
3. Se o cartão funciona no Brasil
4. Se o número do cartão está correto
5. Se o CVV é valido

O Zero Auth também funciona com Cartões tokenizados na Api Cielo Ecommerce 

Veja um exemplo de uso: 

**Zero auth como validador de cartão**

Uma empresa de Streaming chamada FlixNet possui um serviço via assinatura, onde além de realizar uma recorrência, ela possui cartões salvos e recebe novas inscrições diariamente. 
Todas essas etapas exigem que transações sejam realizadas para obter acesso a ferramenta, o que eleva o custo da FlixNet caso as transações não sejam autorizadas. 

Como ela poderia reduzir esse custo? Validando o cartão antes de envia-lo a autorizado.

A FlixNet usa o Zero Auth em 2 momento diferente:

* **Cadastro**: é necessário incluir um cartão para ganhar 30 dias grátis no primeiro mês. 

O problema é que ao se encerrar esse período, se o cartão for invalido, o novo cadastro existe, mas não funciona, pois, o cartão salvo é invalido. A Flix Net resolveu esse problema testando o cartão com o Zero Auth no momento do cadastro, assim, ela já sabe se o cartão está valido e libera a criação da conta. Caso não o cartão não seja aceito, a FlixNet pode sugerir o uso de um outro cartão.

* **Recorrência**: todo mês, antes de realizar a cobrança da Assinatura, a Flixnet testa o cartão com o zero auth, assim sabendo se ele será autorizado ou não.  Isso ajuda o FlixNet a prever quais cartões serão negados, já acionando o assinante para atualização do cadastro antes do dia de pagamento.

## Integração

Para realizar a consulta ao Zero Auth, o lojista deverá enviar uma requisição `POST` para a API Cielo Ecommerce, simulando uma transação. O `POST` deverá ser realizado nas seguintes URL: 

<aside class="request"><span class="method post">POST</span> <span class="endpoint">https://`api`.cieloecommerce.cielo.com.br/1/`zeroauth`</span></aside>

Cada tipo de validação necessita de um contrato tecnico diferente. Eles resultarão em _responses diferenciados_.

### Requisição

#### PADRÃO

``` json
{
    "CardNumber":"1234123412341231",
    "Holder":"Alexsander Rosa",
    "ExpirationDate":"12/2021",
    "SecurityCode":"123",
    "SaveCard":"false",
    "Brand":"Visa"
}
```

Abaixo, a listagem de campos da Requisição:

| Paramêtro        | Descrição                                                                                                             | Tipo    | Tamanho | Obrigatório |
|------------------|-----------------------------------------------------------------------------------------------------------------------|---------|---------|:-----------:|
| `CardType`       | Define o tipo de cartão utilizados:<br><br>*CreditCard*<br>*DebitCard*<br><br>Se não enviado, CreditCard como default | Texto   | 255     | Sim         |
| `CardNumber`     | Número do Cartão do Comprador                                                                                         | Texto   | 16      | Sim         |
| `Holder`         | Nome do Comprador impresso no cartão.                                                                                 | Texto   | 25      | Sim         |
| `ExpirationDate` | Data de e validade impresso no cartão.                                                                                | Texto   | 7       | Sim         |
| `SecurityCode`   | Código de segurança impresso no verso do cartão.                                                                      | Texto   | 4       | Sim         |
| `SaveCard`       | Booleano que identifica se o cartão será salvo para gerar o CardToken.                                                | Boolean | ---     | Sim         |
| `Brand`          | Bandeira do cartão: <br><br>Visa<br>Master<br>| Texto   | 10      | Sim         |
| `CardToken`      | Token do cartão na 3.0                                                                                                | GUID    | 36      | Condicional |

#### COM TOKEN

``` json
{
  "CardToken":"23712c39-bb08-4030-86b3-490a223a8cc9",
  "SaveCard":"false",
  "Brand":"Visa"
}
```

Abaixo, a listagem de campos da Requisição: 

| Paramêtro        | Descrição                                                                                                             | Tipo    | Tamanho | Obrigatório |
|------------------|-----------------------------------------------------------------------------------------------------------------------|---------|---------|:-----------:|
| `Brand`          | Bandeira do cartão: <br><br>Visa<br>Master<br>| Texto   | 10      | não         |
| `CardToken`      | Token do cartão na 3.0                                                                                                | GUID    | 36      | Condicional |

### Resposta

A resposta sempre retorna se o cartão pode ser autorizado no momento. Essa informação apenas significa que o _cartão está valido a transacionar_, mas não necessariamente indica que um determinado valor será autorizado.

Abaixo os campos retornados após a validação:

| Paramêtro       | Descrição                                                                       | Tipo    | Tamanho |
|-----------------|---------------------------------------------------------------------------------|---------|:-------:|
| `Valid`         | Situação do cartão:<br> **True** – Cartão válido<BR>**False** – Cartão Inválido | Boolean | ---     |
| `ReturnCode`    | Código de retorno                                                               | texto   | 2       |
| `ReturnMessage` | Mensagem de retorno                                                             | texto   | 255     |

#### POSITIVA - Cartão Válido

``` json
{
  "Valid": true,
  "ReturnCode": “00”,
  "ReturnMessage", “Transacao autorizada”
}
```

> Consulte <https://developercielo.github.io/Webservice-3.0/#códigos-de-retorno-das-vendas> para visualizar a descrição dos códigos de retorno. 
> O código de retorno **00 representa sucesso no Zero Auth**, os demais códigos são definidos de acordo com a documentação acima.

#### NEGATIVA - Cartão Inválido

``` json
{
       "Valid": false,
       "ReturnCode": "57",
       "ReturnMessage": "Autorizacao negada"
}
```

#### NEGATIVA - Bandeira inválida

``` json
  {    
      "Code": 57,     
      "Message": "Bandeira inválida"   
  }
```

#### NEGATIVA - Restrição Cadastral

``` json
  {    
      "Code": 389,     
      "Message": "Restrição Cadastral"   
  }
```

Caso ocorra algum erro no fluxo, onde não seja possível validar o cartão, o serviço irá retornar erro: 

* 500 – Internal Server Erro
* 400 – Bad Request

# Silent Order Post

Integração que a Cielo oferece aos lojistas, onde os dados de pagamentos são trafegue de forma segura, mantendo o controle total sobre a experiência de Ckeckout.

Esse método possibilita o envio dos dados do pagamento do seu cliente final de forma segura diretamente em nosso sistema. Os campos de pagamento são guardados do lado da Cielo, que conta com a certificação PCI DSS 3.2.

É ideal para lojistas que exigem um alto nível de segurança, sem perder a identidade de sua página. Esse método permite um alto nível de personalização na sua página de checkout.

## Características

* Captura de dados de pagamento diretamente para os sistemas da Cielo por meio dos campos hospedados na sua página através de um script (javascript).
* Compatibilidade com todos os meios de pagamentos disponibilizados ao Gateway (Nacional e Internacional)
* Autenticação do comprador (disponível)
* Redução do escopo de PCI DSS
* Mantenha controle total sobre a experiência de check-out e elementos de gestão da sua marca.

## Fluxo de Autorização

### Fluxo Padrão de Autorização

![Fluxo Padrão](https://desenvolvedores.cielo.com.br/api-portal/sites/default/files/fluxo-padrao-de-autorizacao.jpg)

É preciso que o estabelecimento seja **PCI Compliance** (PCI = Regras de segurança para manipular os dados do cartão)

### Fluxo de Autorização com Silent Order POST

![Fluxo Padrão](https://desenvolvedores.cielo.com.br/api-portal/sites/default/files/fluxo-de-autorizacao-com-sop.jpg)

O servidor **não trafega os dados do cartão** abertamente.

## Fluxo Transacional

![Fluxo Silent Order Post]( https://desenvolvedores.cielo.com.br/api-portal/sites/default/files/images/fluxo-silent-order-post-cielo_new.png)

## Integração

**PASSO 1**

O cliente acaba o checkout, e vai para o processamento do pagamento.

**PASSO 2**

a) O estabelecimento deverá solicitar um ticket (server to server) enviando um POST para a seguinte URL:

**SANDBOX:**
**https://transactionsandbox.pagador.com.br/post/api/public/v1/accesstoken?merchantid={mid_loja}**

**PRODUÇÃO:**
**https://transaction.cieloecommerce.cielo.com.br/post/api/public/v1/accesstoken?merchantid={mid}**

Exemplo: https://transaction.cieloecommerce.cielo.com.br/post/api/public/v1/accesstoken?merchantid=00000000-0000-0000-0000-000000000000

### Requisição

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/v1/accesstoken?merchantid={mid_loja}</span></aside>

```shell
curl
--request POST "https://transaction.cieloecommerce.cielo.com.br/post/api/public/v1/accesstoken?merchantid=00000000-0000-0000-0000-000000000000"
--header "Content-Type: application/json"
--data-binary
--verbose
```

|Propriedade|Descrição|Tipo|Tamanho|Obrigatório|
|-----------|---------|----|-------|-----------|
|`mid_loja`|Identificador da loja na API |Guid |36 |Sim|

### Resposta

```shell
--header "Content-Type: application/json"
--header "RequestId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
--data-binary
{
    "MerchantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "AccessToken": "NjBhMjY1ODktNDk3YS00NGJkLWI5YTQtYmNmNTYxYzhlNjdiLTQwMzgxMjAzMQ==",
    "Issued": "2018-07-23T11:09:32",
    "ExpiresIn": "2018-07-23T11:29:32"
}
```

|Propriedade|Descrição|Tipo|Tamanho|Formato|
|-----------|---------|----|-------|-------|
|`MerchantId`|Identificador da loja na API |Guid |36 |xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx|
|`AccessToken`|Token de Acesso|Texto|--|NjBhMjY1ODktNDk3YS00NGJkLWI5YTQtYmNmNTYxYzhlNjdiLTQwMzgxMjAzMQ==|
|`Issued`|Data e hora da geração |Texto|--|AAAA-MM-DDTHH:MM:SS|
|`ExpiresIn`|Data e hora da expiração |Texto|--|AAAA-MM-DDTHH:MM:SS|

b) Para uso este recurso, por questões de segurança, obrigatoriamente será requerido do lado da Cielo, **no mínimo um IP válido do estabelecimento**. Caso contrário a requisição não será autorizada (**HTTP 401 NotAuthorized**).

**PASSO 3**

a) Como resposta, o estabelecimento receberá um json (HTTP 201 Created) contendo entre outras informações o ticket (AccessToken), como por exemplo:

![Response Ticket]({{ site.baseurl_root }}/images/response-ticket-silent-order-post-cielo.jpg)

Por questões de segurança, este ticket dará permissão para o estabelecimento salvar apenas 1 cartão dentro de um prazo de já estipulado na resposta, através do atributo ExpiresIn (por padrão, 20 minutos). O que acontecer primeiro invalidará esse mesmo ticket para um uso futuro.

**PASSO 4 ao 6**

a) O estabelecimento deverá fazer o download de um script fornecido pela Cielo, e anexá-lo a sua página de checkout. Esse script permitirá à Cielo processar todas as informações de cartão sem intervenção do estabelecimento.
O download poderá ser realizado a partir da seguinte URL: 

**SANDBOX:**
**https://transactionsandbox.pagador.com.br/post/scripts/silentorderpost-1.0.min.js**

**PRODUÇÃO:**
**https://transaction.cieloecommerce.cielo.com.br/post/scripts/silentorderpost-1.0.min.js**

b) O estabelecimento deverá decorar seus inputs do formulário com as seguintes classes:

* Para o portador do cartão de crédito/débito: **bp-sop-cardholdername** 
* Para o número do cartão de crédito/débito: **bp-sop-cardnumber** 
* Para a validade do cartão de crédito/débito: **bp-sop-cardexpirationdate** 
* Para o código de segurança do cartão de crédito/débito: **bp-sop-cardcvvc**

c) O script fornecido pela Cielo fornece três eventos para manipulação e tratamento por parte do estabelecimento. São eles: **onSuccess** (onde será retornado o **“PaymentToken”** após processar os dados do cartão), **onError** (caso haja algum erro no consumo dos serviços da Cielo) e **onInvalid** (onde será retornado o resultado da validação dos inputs).

* Na validação dos inputs, o estabelecimento poderá utilizar toda a camada de validação sobre os dados de cartão realizada pela Cielo e assim simplificar o tratamento no seu formulário de checkout. As mensagens retornadas no resultado da validação são disponibilizadas nas seguintes linguagens: português (default), inglês e espanhol.

* O **PaymentToken** será o token que representará todos os dados de cartão fornecido pelo comprador. O mesmo será utilizado pelo estabelecimento para não haver necessidade de tratar e processar dados de cartão do seu lado.

**Por questões de segurança esse PaymentToken poderá ser usado apenas para 1 autorização na Cielo 3.0. Após o processamento do mesmo, este será invalidado.**

Exemplo de setup a ser realizado pelo estabelecimento na página de checkout:

![Pagina Checkout]({{ site.baseurl_root }}/images/html-silent-order-post.jpg)

# Wallet

## O que são Wallets

São repositorios de cartões e dados de pagamentos para consumidores online. As Carteiras digitais permitem que um consumidor realizar o cadastro de seus dados de pagamento, assim agilizando o processo de compra em lojas habilitadas em suas compras por possuir apenas um cadastro.

> *Para utilizar carteiras na API Cielo eCommerce, o lojista deverá possuir as carteiras integradas em seu checkout*. 

Para maiores informações, sugerimos que entre em contato com o setor tecnico da carteira a qual deseja implementar.

### Wallets Disponiveis

A API Cielo Ecommerce possui integração com:

| Carteira                                                           | 
|--------------------------------------------------------------------|
| [*Apple Pay*](https://www.apple.com/br/apple-pay/)                 |
| [*VisaCheckout*](https://vaidevisa.visa.com.br/site/visa-checkout) | 
| [*MasterPass*](https://masterpass.com/pt-br/)                      | 
| [*Samsung Pay*](https://www.samsung.com.br/samsungpay/)            | 
| [*Google Pay*](https://developercielo.github.io/manual/google-pay) |

<aside class="notice"><strong>Atenção:</strong> Quando o nó “Wallet” for enviado na requisição, o nó “CreditCard” passa a ser opcional.</aside>

<aside class="notice"><strong>Atenção:</strong> Para o cartão de débito, quando for enviado na requisição o nó “Wallet”, será necessário o nó “DebitCard” contendo a “ReturnUrl”.</aside>

<aside class="notice"><strong>Atenção:</strong>  Devido a necessidade de utilização de Chaves efemeras para realizar operações de crédito, a Recorrnência não está disponivel para transações de Wallets </aside>

## Integração base

As wallets na Api Cielo E-commerce possuem duas maneiras de utilização. 

1. **Descriptografia** - Quando o lojista envia os dados da wallet para que a API Cielo e-commerce realize o processamento do cartão
2. **Envio do cartão** - Quando a loja busca o cartão, e o enviar por conta propria a API Cielo e-commerce para processamento

### Componentes

#### Walletkey

O WalletKey é o identificador utilizado pela Cielo para descriptografar payloads retornados pela Wallet. Ele utilizado apenas em integrações no formado `Descriptografia`
Cada Wallet possui um formato de `WalletKeys`. 

| Carteira       | Exemplo        |
|----------------|----------------|
| **VisaCheckout** | `1140814777695873901`   |
| **Apple Pay**    | `9zcCAciwoTS+qBx8jWb++64eHT2QZTWBs6qMVJ0GO+AqpcDVkxGPNpOR/D1bv5AZ62+5lKvucati0+eu7hdilwUYT3n5swkHuIzX2KO80Apx/SkhoVM5dqgyKrak5VD2/drcGh9xqEanWkyd7wl200sYj4QUMbeLhyaY7bCdnnpKDJgpOY6J883fX3TiHoZorb/QlEEOpvYcbcFYs3ELZ7QVtjxyrO2LmPsIkz2BgNm5f+JaJUSAOectahgLZnZR+sRXTDtqLOJQAprs0MNTkPzF95nXGKCCnPV2mfR7z8FHcP7AGqO7aTLBGJLgxFOnRKaFnYlY2E9uTPBbB5JjZywlLIWsPKur5G4m1/E9A6DwjMd0fDYnxjj0bQDfaZpBPeGGPFLu5YYn1IDc`   |
| **Samsung Pay**  | `eyJhbGciOiJSU0ExXzUiLCJraWQiOiIvam1iMU9PL2hHdFRVSWxHNFpxY2VYclVEbmFOUFV1ZUR5M2FWeHBzYXVRPSIsInR5cCI6IkpPU0UiLCJjaGFubmVsU2VjdXJpdHlDb250ZXh0IjoiUlNBX1BLSSIsImVuYyI6IkExMjhHQ00ifQ.cCsGbqgFdzVb1jhXNR--gApzoXH-LldMArSoG59x6i0BbI7jttqxyAdcriSy8q_77VAp3854P9kekjj54RKLrP6APDIr46DI97kjG9E99ONXImnEyamHj95ZH_AW8lvkfa09KAr4537RM8GEXyZoys2vfIW8zqjjicZ8EKIpAixNlmrFJu6-Bo_utsmDN_DuGm69Kk2_nh6txa7ML9PCI59LFfOMniAf7ZwoZUBDCY7Oh8kx3wsZ0kxNBwfyLBCMEYzET0qcIYxePezQpkNcaZ4oogmdNSpYY-KbZGMcWpo1DKhWphDVp0lZcLxA6Q25K78e5AtarR5whN4HUAkurQ.CFjWpHkAVoLCG8q0.NcsTuauebemJXmos_mLMTyLhEHL-p5Wv6J88WkgzyjAt_DW7laiPMYw2sqRXkOiMJLwhifRzbSp8ZgJBM25IX05dKKSS4XfFjJQQjOBHw6PYtEF5pUDMLHML3jcddCrX07abfef_DuP41PqOQYsjwesLZ8XsRj-R0TH4diOZ_GQop8_oawjRIo9eJr9Wbtho0h8kAzHYpfuhamOPT718EaGAY6SSrR7t6nBkzGNkrKAmHkC7aRwe.AbZG53wRqgF0XRG3wUK_UQ`   |

> **Observações:**
> A Wallet MasterPass não possui `WalletKey`.
> O `WalletKey` Apple Pay pode ser obtido dentro do campo `DATA` do payload Apple

#### EphemeralPublicKey

O `EphemeralPublicKey` é a chave utilizado pela Cielo para descriptografar payloads contendo `WalletKeys` enviados pelos lojistas. É utilizado apenas em integrações no formado `Descriptografia`
Cada Wallet possui um formato de `EphemeralPublicKey`.

| Carteira       | Exemplo                                                                                                                          |
|----------------|----------------------------------------------------------------------------------------------------------------------------------|
| *Apple Pay*    | `MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEoedz1NqI6hs9hEO6dBsnn0X0xp5/DKj3gXirjEqxNIJ8JyhGxVB3ITd0E+6uG4W6Evt+kugG8gOhCBrdUU6JwQ==`   |

> *VisaCheckout* / *MasterPass* / *SamsungPay* **não possuem** EphemeralPublicKey

### Descriptografia

#### Requisição

```json
-- Descriptografia
{
  "MerchantOrderId": "2014111708",
  "Customer": {
    "Name": "Exemplo Wallet Padrão",
    "Identity": "11225468954",
    "IdentityType": "CPF"
  },
  "Payment": {
    "Type": "CreditCard",
    "Amount": 100,
    "Installments": 1,
    "Currency": "BRL",
    "Wallet": {
      "Type": "TIPO DE WALLET",
      "WalletKey": "IDENTIFICADOR DA LOJA NA WALLET",
      "AdditionalData": {
        "EphemeralPublicKey": "TOKEN INFORMADO PELA WALLET"
      }
    }
  }
}

```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `CreditCard.Brand`         |Texto   |10       |Sim          |Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).    |
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira:  `VisaCheckout`/ `Masterpass` / `SamsungPay`                            |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |
| `Wallet.AdditionalData.EphemeralPublicKey`| Texto  | 255    | Sim  | Chave retornada pela Wallet para descriptografia. Deve ser enviado em Integrações: `ApplePay`    |
| `Wallet.AdditionalData.capturecode`       | Texto  | 255    | Sim  | Código informado pela `MasterPass` ao lojista                                                    |                                                      

#### Resposta

```json
{
    "MerchantOrderId": "2014111703",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "453211******1521",
            "Holder": "Leonardo Romano",
            "ExpirationDate": "08/2020",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "0319040817883",
        "ProofOfSale": "817883",
        "AuthorizationCode": "027795",
        "Wallet": {
            "Type": "TIPO DE WALLET",
            "WalletKey": "IDENTIFICADOR DA LOJA NA WALLET",
            "Eci": 0
            "AdditionalData": {
                "EphemeralPublicKey": "TOKEN INFORMADO PELA WALLET"
                              },                
                 },
        "SoftDescriptor": "123456789ABCD",
        "Amount": 100,
        "ReceivedDate": "2018-03-19 16:08:16",
        "Status": 1,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "e57b09eb-475b-44b6-ac71-01b9b82f2491",
        "Type": "CreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

| Propriedade         | Descrição                                                                                                                      | Tipo  | Tamanho | Formato                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
| `ProofOfSale`       | Número da autorização, identico ao NSU.                                                                                        | Texto | 6       | Texto alfanumérico                   |
| `Tid`               | Id da transação na adquirente.                                                                                                 | Texto | 20      | Texto alfanumérico                   |
| `AuthorizationCode` | Código de autorização.                                                                                                         | Texto | 6       | Texto alfanumérico                   |
| `SoftDescriptor`    | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais | Texto | 13      | Texto alfanumérico                   |
| `PaymentId`         | Campo Identificador do Pedido.                                                                                                 | Guid  | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx |
| `ECI`               | Eletronic Commerce Indicator. Representa o quão segura é uma transação.                                                        | Texto | 2       | Exemplos: 7                          |
| `Status`            | Status da Transação.                                                                                                           | Byte  | ---     | 2                                    |
| `ReturnCode`        | Código de retorno da Adquirência.                                                                                              | Texto | 32      | Texto alfanumérico                   |
| `ReturnMessage`     | Mensagem de retorno da Adquirência.                                                                                            | Texto | 512     | Texto alfanumérico                   |
| `Type`              | Indica qual o tipo de carteira:  `VisaCheckout`/ `Masterpass` / `ApplePay` / `SamsungPay`                                      | Texto | 255     | Texto alfanumérico                   |
| `Walletkey`         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações                              | Texto | 255     | Ver tabela `WalletKey`               |       
| `AdditionalData.capturecode`        | Código informado pela `MasterPass` ao lojista                                                                  | Texto | 255     | 3                                    | 
| `AdditionalData.EphemeralPublicKey` | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`                         | Texto | 255     | Ver Tabela `EphemeralPublicKey`      | 

### Envio de cartão

#### Requisição

``` json
-- Envio de cartão
{
  "MerchantOrderId": "6242-642-723",
  "Customer": {
    "Name": "Guilherme Gama",
    "Identity": "11225468954",
    "IdentityType": "CPF"
  },
  "Payment": {
    "Type": "CreditCard",
    "Amount": 1100,
    "Provider": "Cielo",
    "Installments": 1,
    "CreditCard": {
      "CardNumber":"4532********6521",
      "Holder":"Guilherme Gama",
          "ExpirationDate":"12/2021",
          "SecurityCode":"123",
          "Brand":"Master"
    },
    "Wallet": {
      "Type": "Tipo de wallet",
      "Eci":"7",
      "Cavv":"AM1mbqehL24XAAa0J04CAoABFA=="
    }
  }
}
```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `CreditCard.Brand`         |Texto   |10       |Sim          |Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).    |
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira: `VisaCheckout`/ `Masterpass` / `ApplePay` / `SamsungPay`                |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |
| `Wallet.ECI`               | Texto  | 3       | Sim         | O ECI (Eletronic Commerce Indicator) representa o quão segura é uma transação. Esse valor deve ser levado em consideração pelo lojista para decidir sobre a captura da transação. |
| `Wallet.CAVV`              | Texto  | 255     | Sim         | Campo de validação retornado pela Wallet e utilizado como base de autorização                           | 

#### Respostas

```json
{
    "MerchantOrderId": "2014111703",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "453211******1521",
            "Holder": "Gama Gama",
            "ExpirationDate": "08/2020",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "0319040817883",
        "ProofOfSale": "817883",
        "AuthorizationCode": "027795",
        "Wallet": {
            "Type": "TIPO DE WALLET",
            "Eci": 0
                 },
        "SoftDescriptor": "123456789ABCD",
        "Amount": 100,
        "ReceivedDate": "2018-03-19 16:08:16",
        "Status": 1,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "e57b09eb-475b-44b6-ac71-01b9b82f2491",
        "Type": "CreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

| Propriedade         | Descrição                                                                                                                      | Tipo  | Tamanho | Formato                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
| `ProofOfSale`       | Número da autorização, identico ao NSU.                                                                                        | Texto | 6       | Texto alfanumérico                   |
| `Tid`               | Id da transação na adquirente.                                                                                                 | Texto | 20      | Texto alfanumérico                   |
| `AuthorizationCode` | Código de autorização.                                                                                                         | Texto | 6       | Texto alfanumérico                   |
| `SoftDescriptor`    | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais | Texto | 13      | Texto alfanumérico                   |
| `PaymentId`         | Campo Identificador do Pedido.                                                                                                 | Guid  | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx |
| `ECI`               | Eletronic Commerce Indicator. Representa o quão segura é uma transação.                                                        | Texto | 2       | Exemplos: 7                          |
| `Status`            | Status da Transação.                                                                                                           | Byte  | ---     | 2                                    |
| `ReturnCode`        | Código de retorno da Adquirência.                                                                                              | Texto | 32      | Texto alfanumérico                   |
| `ReturnMessage`     | Mensagem de retorno da Adquirência.                                                                                            | Texto | 512     | Texto alfanumérico                   |
| `Type`              |  indica qual o tipo de carteira: `VisaCheckout`/ `Masterpass` / `ApplePay` / `SamsungPay`                                      | Texto | 255     | Texto alfanumérico                   |
| `Walletkey`         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações                              | Texto | 255     | Ver tabela `WalletKey`               |       
| `AdditionalData.capturecode`        | Código informado pela `MasterPass` ao lojista                                                                  | Texto | 255     | 3                                    | 

## Apple Pay

### Pré-Requisitos

Para utilização da Apple Pay no formato **Descriptografia** é necessario que a loja ja esteja cadastrada junto a Apple e possua um `MerchantIdentifier`
A integração **Descriptografia** exige que o lojista realize o upload manual de um **certificado CSR no formato PEM** fornecido pela Cielo. Entre em contato com a equipe de atendimento Cielo para obter o Certificado.

#### MerchantIdentifier

Para obter o `MerchantIdentifier` realize os passos abaixo:

1. Log em [**Apple Developer**](https://developer.apple.com/)
2. Selecione [**Certificates, IDs & Profiles**](https://developer.apple.com/library/content/ApplePay_Guide/Configuration.html)
3. Dentro da área "Identifiers" clique em "Merchant IDs"
4. Clique no **+** no canto direito, abaixo de "Registering a Merchant ID"
5. Defina a descrição do MerchantID e o identificador. Exemplo: "merchant.com.CIELO.merchantAccount"
6. Clique em "continuar" e verifique se as informações inseridas estão corretas.
7. Finalize o processo.

> O `MerchantIdentifier` deve ser enviado a Cielo para a criação de um Certificado CSR no formato PEM.

#### Certificado CSR

Após enviar o `MerchantIdentifier` para a equipe de atendimento Cielo, a loja receberá um certificado de extensão `PEM` e deverá seguir os sequintes passos:

1. Log em [**Apple Developer**](https://developer.apple.com/)
2. Selecione [**Certificates, IDs & Profiles**](https://developer.apple.com/library/content/ApplePay_Guide/Configuration.html)
3. Realize o Upload do Certificado 
7. Finalize o processo.

> O Certificado PEM contem o código CSR solicitado pela Apple. <br>
> Formato de um Certificado `.PEM`

-

> -----BEGIN CERTIFICATE REQUEST-----<BR>
> MIHwMIGYAgEAMDgxCzAJBgNVBAYTAkJSMRAwDgYDVQQKDAdicmFzcGFnMRcwFQYD<BR>
> VQQDDA5icmFzcGFnLmNvbS5icjBZMBMGByqGSM49AgEGCCqGSM49AwEHA0IABBIP<BR>
> ULN00aAwYW+sfTettoIl8l9YrDCkF1HEiI9PgwLcM4jCkIAvnrKZ3loLWDi4J8Jh<BR>
> ML01OuTohYS46lqF6p4wCgYIKoZIzj0EAwIDRwAwRAIgWLAPtSWKQ3sJYLc6jmWa<BR>
> RNWCoNR2XBQZKdg5bIGNYpYCIHSLsQVSK8taL7dGirOBxXiOqtUA9hWxn0g1Mf3U<BR>
> VKeU<BR>
> -----END CERTIFICATE REQUEST-----<BR>

### Descriptografia

No modelo apresentado a seguir, demonstramos como utilizar a integração Apple Pay Cielo via o envio do WalletKey + EphemeralPublicKey retornados pela Apple via Payload

#### Requisição

Exemplo de Requisição *Apple Pay*

> É necessário que a loja ja possua cadastro e uma integração Apple Pay, caso contrario não será possivel a integração com a API

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{
  "MerchantOrderId": "6242-642-723",
  "Customer": {
    "Name": "Exemplo Wallet Padrão",
    "Identity": "11225468954",
    "IdentityType": "CPF"
  },
  "Payment": {
    "Type": "CreditCard",
    "Amount": 1100,
    "Provider": "Cielo",
    "Installments": 1,
    "Currency": "BRL",
    "Wallet": {
      "Type": "ApplePay",
      "WalletKey": "9zcCAciwoTS+qBx8jWb++64eHT2QZTWBs6qMVJ0GO+AqpcDVkxGPNpOR/D1bv5AZ62+5lKvucati0+eu7hdilwUYT3n5swkHuIzX2KO80Apx/SkhoVM5dqgyKrak5VD2/drcGh9xqEanWkyd7wl200sYj4QUMbeLhyaY7bCdnnpKDJgpOY6J883fX3TiHoZorb/QlEEOpvYcbcFYs3ELZ7QVtjxyrO2LmPsIkz2BgNm5f+JaJUSAOectahgLZnZR+sRXTDtqLOJQAprs0MNTkPzF95nXGKCCnPV2mfR7z8FHcP7AGqO7aTLBGJLgxFOnRKaFnYlY2E9uTPBbB5JjZywlLIWsPKur5G4m1/E9A6DwjMd0fDYnxjj0bQDfaZpBPeGGPFLu5YYn1IDc",
      "AdditionalData": {
        "EphemeralPublicKey": "MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEoedz1NqI6hs9hEO6dBsnn0X0xp5/DKj3gXirjEqxNIJ8JyhGxVB3ITd0E+6uG4W6Evt+kugG8gOhCBrdUU6JwQ=="
      }
    }
  }
}
```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `CreditCard.Brand`         |Texto   |10       |Sim          |Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).    |
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira: `ApplePay` / `VisaCheckout`/ `Masterpass` |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |
| `Wallet.AdditionalData.EphemeralPublicKey`| Texto  | 255    | Sim  | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`           |
| `Wallet.AdditionalData.capturecode`       | Texto  | 255    | Sim  | Código informado pela `MasterPass` ao lojista                                                    | 

#### Resposta

```json
{
    "MerchantOrderId": "2014111703",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "453211******1521",
            "Holder": "Leonardo Romano",
            "ExpirationDate": "08/2020",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "0319040817883",
        "ProofOfSale": "817883",
        "AuthorizationCode": "027795",
        "Wallet": {
            "Type": "ApplePay",
            "WalletKey": "9zcCAciwoTS+qBx8jWb++64eHT2QZTWBs6qMVJ0GO+AqpcDVkxGPNpOR/D1bv5AZ62+5lKvucati0+eu7hdilwUYT3n5swkHuIzX2KO80Apx/SkhoVM5dqgyKrak5VD2/drcGh9xqEanWkyd7wl200sYj4QUMbeLhyaY7bCdnnpKDJgpOY6J883fX3TiHoZorb/QlEEOpvYcbcFYs3ELZ7QVtjxyrO2LmPsIkz2BgNm5f+JaJUSAOectahgLZnZR+sRXTDtqLOJQAprs0MNTkPzF95nXGKCCnPV2mfR7z8FHcP7AGqO7aTLBGJLgxFOnRKaFnYlY2E9uTPBbB5JjZywlLIWsPKur5G4m1/E9A6DwjMd0fDYnxjj0bQDfaZpBPeGGPFLu5YYn1IDc",
            "Eci": 0
            "AdditionalData": {
                "EphemeralPublicKey": "MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEoedz1NqI6hs9hEO6dBsnn0X0xp5/DKj3gXirjEqxNIJ8JyhGxVB3ITd0E+6uG4W6Evt+kugG8gOhCBrdUU6JwQ=="
                              },                
                 },
        "SoftDescriptor": "123456789ABCD",
        "Amount": 100,
        "ReceivedDate": "2018-03-19 16:08:16",
        "Status": 1,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "e57b09eb-475b-44b6-ac71-01b9b82f2491",
        "Type": "CreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

| Propriedade         | Descrição                                                                                                                      | Tipo  | Tamanho | Formato                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
| `ProofOfSale`       | Número da autorização, identico ao NSU.                                                                                        | Texto | 6       | Texto alfanumérico                   |
| `Tid`               | Id da transação na adquirente.                                                                                                 | Texto | 20      | Texto alfanumérico                   |
| `AuthorizationCode` | Código de autorização.                                                                                                         | Texto | 6       | Texto alfanumérico                   |
| `SoftDescriptor`    | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais | Texto | 13      | Texto alfanumérico                   |
| `PaymentId`         | Campo Identificador do Pedido.                                                                                                 | Guid  | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx |
| `ECI`               | Eletronic Commerce Indicator. Representa o quão segura é uma transação.                                                        | Texto | 2       | Exemplos: 7                          |
| `Status`            | Status da Transação.                                                                                                           | Byte  | ---     | 2                                    |
| `ReturnCode`        | Código de retorno da Adquirência.                                                                                              | Texto | 32      | Texto alfanumérico                   |
| `ReturnMessage`     | Mensagem de retorno da Adquirência.                                                                                            | Texto | 512     | Texto alfanumérico                   |
| `Type`              |  indica qual o tipo de carteira: `ApplePay` / `VisaCheckout`/ `Masterpass`                       | Texto | 255     | Texto alfanumérico                   |
| `Walletkey`         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações                              | Texto | 255     | Ver tabela `WalletKey`               |       
| `AdditionalData.EphemeralPublicKey` | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`                        | Texto | 255     | Ver Tabela `EphemeralPublicKey`      |  
| `AdditionalData.capturecode`        | Código informado pela `MasterPass` ao lojista                                                                  | Texto | 255     | 3                                    | 

### Envio de cartão

No modelo apresentado a seguir, demonstramos como a Apple Pay pode ser utilizada com o envio do cartão aberto, sem a necessidade de WalletKey.

#### Requisição

Nesse modelo, o lojista informa apenas que a transação é de uma Wallet Apple Pay e envia os dados ECI e CAVV fornecidos pela APPLE

* **CAVV** - pode ser extraido do campo `onlinePaymentCryptogram` retornado pela Apple no payload
* **ECI** - pode ser extraido do campo `eciIndicator` retornado pela Apple no payload

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{
  "MerchantOrderId": "6242-642-723",
  "Customer": {
    "Name": "Exemplo Wallet Padrão",
    "Identity": "11225468954",
    "IdentityType": "CPF"
  },
  "Payment": {
    "Type": "CreditCard",
    "Amount": 1100,
    "Provider": "Cielo",
    "Installments": 1,
    "CreditCard": {
      "CardNumber":"4532********6521",
      "Holder":"Exemplo Wallet Padrão",
          "ExpirationDate":"12/2021",
          "SecurityCode":"123",
          "Brand":"Master"
    },
    "Currency": "BRL",
    "Wallet": {
      "Type": "ApplePay",
      "Eci":"7",
      "Cavv":"AM1mbqehL24XAAa0J04CAoABFA=="
    }
  }
}
```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `CreditCard.Brand`         |Texto   |10       |Sim          |Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard /  Hiper).   |
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira: `ApplePay` |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |
| `Wallet.ECI`               | Texto  | 3       | Sim         | O ECI (Eletronic Commerce Indicator) representa o quão segura é uma transação. Esse valor deve ser levado em consideração pelo lojista para decidir sobre a captura da transação. |
| `Wallet.CAVV`              | Texto  | 255     | Sim         | Campo de validação retornado pela Wallet e utilizado como base de autorização                           | 

#### Resposta

```json
{
    "MerchantOrderId": "6242-642-723",
    "Customer": {
        "Name": "Exemplo Wallet Padrão",
        "Identity": "11225468954",
        "IdentityType": "CPF"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "453265******6521",
            "Holder": "Exemplo Wallet Padrão",
            "ExpirationDate": "12/2021",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "10447480687BVV8COCRB",
        "ProofOfSale": "457033",
        "Provider": "Cielo",
        "Eci": "7",
        "Wallet": {
            "Type": "ApplePay",
            "Cavv": "AM1mbqehL24XAAa0J04CAoABFA==",
            "Eci": 7
        },
        "VelocityAnalysis": {
            "Id": "98652f2c-bdfd-47b9-8673-77b80a6fe705",
            "ResultMessage": "Accept",
            "Score": 0
        },
        "Amount": 1100,
        "ReceivedDate": "2018-04-18 16:27:22",
        "Status": 2,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "98652f2c-bdfd-47b9-8673-77b80a6fe705",
        "Type": "CreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

| Propriedade                         | Descrição                                                                                                                                    | Tipo  | Tamanho | Formato                              |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
| `ProofOfSale`                       | Número da autorização, identico ao NSU.                                                                                                      | Texto | 6       | Texto alfanumérico                   |
| `Tid`                               | Id da transação na adquirente.                                                                                                               | Texto | 20      | Texto alfanumérico                   |
| `AuthorizationCode`                 | Código de autorização.                                                                                                                       | Texto | 6       | Texto alfanumérico                   |
| `SoftDescriptor`                    | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais               | Texto | 13      | Texto alfanumérico                   |
| `PaymentId`                         | Campo Identificador do Pedido.                                                                                                               | Guid  | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx |
| `ECI`                               | Eletronic Commerce Indicator. Representa o quão segura é uma transação.                                                                      | Texto | 2       | Exemplos: 7                          |
| `Status`                            | Status da Transação.                                                                                                                         | Byte  | ---     | 2                                    |
| `ReturnCode`                        | Código de retorno da Adquirência.                                                                                                            | Texto | 32      | Texto alfanumérico                   |
| `ReturnMessage`                     | Mensagem de retorno da Adquirência.                                                                                                          | Texto | 512     | Texto alfanumérico                   |
| `Type`                              | indica qual o tipo de carteira: `ApplePay`                                      | Texto | 255     | Texto alfanumérico                   |
| `Walletkey`                         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações                                            | Texto | 255     | Ver tabela `WalletKey`               |
| `AdditionalData.EphemeralPublicKey` | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`                                                      | Texto | 255     | Ver Tabela `EphemeralPublicKey`      |
| `AdditionalData.capturecode`        | Código informado pela `MasterPass` ao lojista                                                                                                | Texto | 255     | 3                                    |
| `ECI`                               | O ECI (Eletronic Commerce Indicator) indica a segurança de uma transação. Deve ser levado em conta pelo lojista para decidir sobre a captura | Texto | 3       | 2                                    |
| `CAVV`                              | Campo de validação retornado pela Wallet e utilizado como base de autorização                                                                | Texto | 255     | --                                   |

## VisaCheckout

### Descriptografia

No modelo apresentado a seguir, demonstramos como utilizar a integração VisaCheckout Cielo via o envio do WalletKey retornados pela Visa via Payload

> O `Walletkey` é o parametro `CallID` retornado pelo VisaCheckout

#### Requisição

Exemplo de Requisição padrão *VisaCheckout*

> É necessário que a loja ja possua cadastro e uma integração VisaCheckout, caso contrario não será possivel a integração com a API

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014111703",
   "Customer":{  
      "Name":"Comprador Teste"
   },
   "Payment":{  
      "Type":"CreditCard",
      "Amount":15700,
      "Installments":1,
      "SoftDescriptor":"123456789ABCD",
      "CreditCard":{  
         "SecurityCode":"123"
      },
      "Wallet":{  
         "Type":"VisaCheckout",
         "WalletKey":"1140814777695873901"
      }
   }
}
```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `CreditCard.Brand`         |Texto   |10       |Sim          |Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).    |
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira: `VisaCheckout`/ `Masterpass` / `SamsungPay` /  `ApplePay` |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |

#### Resposta

```json
{
    "MerchantOrderId": "2014111703",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "453211******1521",
            "Holder": "Gama Gama",
            "ExpirationDate": "08/2020",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "0319040817883",
        "ProofOfSale": "817883",
        "AuthorizationCode": "027795",
        "Wallet": {
            "Type": "VisaCheckout",
            "WalletKey": "1140814777695873901",
            "Eci": 0
            },
        "SoftDescriptor": "123456789ABCD",
        "Amount": 100,
        "ReceivedDate": "2018-03-19 16:08:16",
        "Status": 1,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "e57b09eb-475b-44b6-ac71-01b9b82f2491",
        "Type": "CreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

### Envio de cartão

No modelo apresentado a seguir, demonstramos como a VisaCheckout pode ser utilizada com o envio do cartão aberto, sem a necessidade de WalletKey.

#### Requisição

Nesse modelo, o lojista informa apenas que a transação é da Wallet VisaCheckout e envia os dados ECI e CAVV fornecidos pela Visa

* **ECI** - retornado pela Visa no payload como `DSC_ECI`

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014111703",
   "Customer":{  
      "Name":"Comprador Teste"
   },
   "Payment":{  
      "Type":"CreditCard",
      "Amount":15700,
      "Installments":1,
      "SoftDescriptor":"123456789ABCD",
      "Wallet":{  
         "Type":"VisaCheckout"
         "Eci": 0,
    },
      "CreditCard":{  
         "CardNumber":"1234123412341231",
         "Holder":"Teste Holder",
         "ExpirationDate":"12/2030",
         "Brand":"Visa"
    },
  },
}
```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `CreditCard.Brand`         |Texto   |10       |Sim          |Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).    |
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira: `VisaCheckout`/ `Masterpass` / `SamsungPay` /  `ApplePay` |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |
| `Wallet.ECI`               | Texto  | 3       | Sim         | O ECI (Eletronic Commerce Indicator) representa o quão segura é uma transação. Esse valor deve ser levado em consideração pelo lojista para decidir sobre a captura da transação. |

#### Resposta

```json
{
    "MerchantOrderId": "2014111706",
    "Customer": {
        "Name": "Comprador Visa Checkout"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": "ByMerchant",
        "Capture": false,
        "Authenticate": false,
        "CreditCard": {
            "CardNumber": "455187******0183",
            "Holder": "Teste Holder",
            "ExpirationDate": "12/2030",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "ProofOfSale": "674532",
        "Tid": "0305023644309",
        "AuthorizationCode": "123456",
        "PaymentId": "24bc8366-fc31-4d6c-8555-17049a836a07",
        "Type": "CreditCard",
        "Amount": 15700,
        "Currency": "BRL",
        "Country": "BRA",
        "ExtraDataCollection": [],
        "Status": 1,
        "ReturnCode": "4",
        "ReturnMessage": "Operation Successful",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/{PaymentId}/void"
            }
        ]
    }
}
```

| Propriedade                         | Descrição                                                                                                                                    | Tipo  | Tamanho | Formato                              |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
| `ProofOfSale`                       | Número da autorização, identico ao NSU.                                                                                                      | Texto | 6       | Texto alfanumérico                   |
| `Tid`                               | Id da transação na adquirente.                                                                                                               | Texto | 20      | Texto alfanumérico                   |
| `AuthorizationCode`                 | Código de autorização.                                                                                                                       | Texto | 6       | Texto alfanumérico                   |
| `SoftDescriptor`                    | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais               | Texto | 13      | Texto alfanumérico                   |
| `PaymentId`                         | Campo Identificador do Pedido.                                                                                                               | Guid  | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx |
| `ECI`                               | Eletronic Commerce Indicator. Representa o quão segura é uma transação.                                                                      | Texto | 2       | Exemplos: 7                          |
| `Status`                            | Status da Transação.                                                                                                                         | Byte  | ---     | 2                                    |
| `ReturnCode`                        | Código de retorno da Adquirência.                                                                                                            | Texto | 32      | Texto alfanumérico                   |
| `ReturnMessage`                     | Mensagem de retorno da Adquirência.                                                                                                          | Texto | 512     | Texto alfanumérico                   |
| `Type`                              |indica qual o tipo de carteira: `VisaCheckout`/ `Masterpass` / `SamsungPay` /  `ApplePay`                                    | Texto | 255     | Texto alfanumérico                   |
| `Walletkey`                         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações                                            | Texto | 255     | Ver tabela `WalletKey`               |

## MasterPass

### Envio de Cartão

> A Wallet MasterPass possui integração apenas no formato `Envio de cartão`.

Para utilizar a wallet [**Masterpass**](https://developer.mastercard.com/product/masterpass) é necessario que a loja ja esteja cadastrada junto a Mastercard, e integrada a busca de dados de cartão da plataforma.

#### Requisição

Exemplo de Requisição *Masterpass*

> É necessário que a loja ja possua cadastro e uma integração Masterpass, caso contrario não será possivel a integração com a API

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{  
   "MerchantOrderId":"2014111708",
   "Customer":{  
      "Name":"Comprador MasterPass"     
   },
   "Payment":{  
     "Type":"CreditCard",
     "Amount":15700,
     "Installments":1,
     "CreditCard":{
               "CardNumber": "4532117080573703",
               "Brand": "Visa",
               "SecurityCode":"023"
     },
     "Wallet":{
         "Type":"MasterPass",
         "Eci":"7",
         "Cavv":"AM1mbqehL24XAAa0J04CAoABFA=="
         "AdditionalData":{
               "CaptureCode": "103"
         }
     }
   }
}
```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `CreditCard.Brand`         |Texto   |10       |Sim          |Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).|
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira: `VisaCheckout`/ `Masterpass` / `SamsungPay` /  `ApplePay` |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |
| `Wallet.AdditionalData.capturecode`       | Texto  | 255    | Sim  | Código informado pela `MasterPass` ao lojista                                                    | 

#### Resposta

```json
{
    "MerchantOrderId": "6242-642-723",
    "Customer": {
        "Name": "Exemplo Wallet Padrão",
        "Identity": "11225468954",
        "IdentityType": "CPF"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "453265******6521",
            "Holder": "Exemplo Wallet Padrão",
            "ExpirationDate": "12/2021",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "10447480687BVV8COCRB",
        "ProofOfSale": "457033",
        "Provider": "Cielo",
        "Eci": "7",
        "Wallet": {
            "Type": "Masterpass",
            "Cavv": "AM1mbqehL24XAAa0J04CAoABFA==",
            "Eci": 7
        },
        "VelocityAnalysis": {
            "Id": "98652f2c-bdfd-47b9-8673-77b80a6fe705",
            "ResultMessage": "Accept",
            "Score": 0
        },
        "Amount": 1100,
        "ReceivedDate": "2018-04-18 16:27:22",
        "Status": 2,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "98652f2c-bdfd-47b9-8673-77b80a6fe705",
        "Type": "CreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

| Propriedade                         | Descrição                                                                                                                                    | Tipo  | Tamanho | Formato                              |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
| `ProofOfSale`                       | Número da autorização, identico ao NSU.                                                                                                      | Texto | 6       | Texto alfanumérico                   |
| `Tid`                               | Id da transação na adquirente.                                                                                                               | Texto | 20      | Texto alfanumérico                   |
| `AuthorizationCode`                 | Código de autorização.                                                                                                                       | Texto | 6       | Texto alfanumérico                   |
| `SoftDescriptor`                    | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais               | Texto | 13      | Texto alfanumérico                   |
| `PaymentId`                         | Campo Identificador do Pedido.                                                                                                               | Guid  | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx |
| `ECI`                               | Eletronic Commerce Indicator. Representa o quão segura é uma transação.                                                                      | Texto | 2       | Exemplos: 7                          |
| `Status`                            | Status da Transação.                                                                                                                         | Byte  | ---     | 2                                    |
| `ReturnCode`                        | Código de retorno da Adquirência.                                                                                                            | Texto | 32      | Texto alfanumérico                   |
| `ReturnMessage`                     | Mensagem de retorno da Adquirência.                                                                                                          | Texto | 512     | Texto alfanumérico                   |
| `Type`                              |indica qual o tipo de carteira: `VisaCheckout`/ `Masterpass` / `SamsungPay` /  `ApplePay`                                    | Texto | 255     | Texto alfanumérico                   |
| `Walletkey`                         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações                                            | Texto | 255     | Ver tabela `WalletKey`               |
| `AdditionalData.capturecode`        | Código informado pela `MasterPass` ao lojista                                                                                                | Texto | 255     | 3                                    |
| `ECI`                               | O ECI (Eletronic Commerce Indicator) indica a segurança de uma transação. Deve ser levado em conta pelo lojista para decidir sobre a captura | Texto | 3       | 2                                    |
| `CAVV`                              | Campo de validação retornado pela Wallet e utilizado como base de autorização                                                                | Texto | 255     | --                                   |

## Samsung Pay

### Pré-Requisitos

Para utilização da Samsung no formato **Descriptografia** é necessario que a loja ja esteja cadastrada junto a plataforma da [**Samsung**](https://pay.samsung.com/developers).
A integração **Descriptografia** exige que o lojista realize o upload manual de um **certificado CSR no formato PEM** fornecido pela Cielo. Entre em contato com a equipe de produtos Cielo para obter o Certificado.

#### Certificado CSR

Após se cadastrar junto a SamsungPay, a loja deverá requisitar um certificado de extensão `PEM` a equipe de produtos Cielo. Com o certificado em mãos, deverá seguir os sequintes passos:

1. Log em [**Samsung**](https://pay.samsung.com/developers)
2. Selecione [**My Projects**](https://pay.samsung.com/developers/projects/prdnsvc) para criar serviço
3. Realize o Upload do Certificado Cielo
7. Finalize o processo.

> O Certificado PEM contem o código CSR solicitado pela Samsung. <br>
> Formato de um Certificado `.PEM`

--

> -----BEGIN CERTIFICATE REQUEST-----
> MIICezCCAWMCAQAwODELMAkGA1UEBhMCQlIxEDAOBgNVBAoMB2JyYXNwYWcxFzAV<br>
> BgNVBAMMDmJyYXNwYWcuY29tLmJyMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIB<br>
> CgKCAQEApsk94DAhdgvgUgGT/fufNkofB2AeX/sPXRT0mm92DM8XgbyWw6FgsE2T<br>
> 3SFi5WmYwDo12tVjAydUCRzMc6HDIrLBFJsfHgrZWLlf9QIPLZFW/zA9+qZLP+VW<br>
> nGyGBil+rgNhylXfDGjCvUMJ5bSLbcC2oC1HGO613HsJrbsB96sE107RkFhDEChD<br>
> 9fZi3MoD2lCwVjbAu/zDatoloxy8Bc02HqlK4sVZuPUzFIZ9gg19G/QU6WI2ompv<br>
> akkhc07xS8QIU/XuzMV5KdpCs/mlRH1QQICSHdviu2UKbKlM9KWqpBOeLwGsQ59P<br>
> mQVb5bPgdAix8KvBAWAi8pcdgjWiIwIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQBh<br>
> 4XwAmQabopYgJgb+8UwIV+LbWKszwXVUq9nYfiDN0OT4KguilfNQQMHvULlHVahJ<br>
> ibiRsuFfkmEkvGkrUm1IMCHjwZjDzJmbB/7VwqHzq5HJ9pa9Dt6xRO7psCycSE4N<br>
> m+iQs18muHWkzPFouBw+HDVgD8NJZS0mPKSnOmdWpajUHkpDsXY+XctLI2n6NAc3<br>
> sy65A6ljFGpjdHG+aJc7TjzjSBpNXyY5bys5zGF44wgOKq/md5nMp6IeqYAZ+D1N<br>
> aWvYpwra9kiVLR3742JWgF7P25rCdpwzXO9KiD9T8VxYnEZeFli+LQXb7c6UJjHl<br>
> /mYyDuIyBIna9ij0Ygff<br>
> -----END CERTIFICATE REQUEST-----<br>",

### Descriptografia

No modelo apresentado a seguir, demonstramos como utilizar a integração SamsungPay Cielo via o envio do WalletKey + EphemeralPublicKey retornados pela Samsung via Payload

#### Requisição

Exemplo de Requisição *SamsungPay*

> É necessário que a loja ja possua cadastro e uma integração SamsungPay, caso contrario não será possivel a integração com a API

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{
  "MerchantOrderId":"6242-642-723",
  "Customer":{
     "Name":"Exemplo Wallet Padrão",
     "Identity":"11225468954",
      "IdentityType":"CPF"
  },
  "Payment":{
     "Type":"CreditCard",
     "Amount":1,
     "Provider":"Cielo",
     "Installments":1,
     "Currency":"BRL",
     "Wallet":{
       "Type":"SamsungPay",
       "WalletKey":"eyJhbGciOiJSU0ExXzUiLCJraWQiOiIvam1iMU9PL2hHdFRVSWxHNFpxY2VYclVEbmFOUFV1ZUR5M2FWeHBzYXVRPSIsInR5cCI6IkpPU0UiLCJjaGFubmVsU2VjdXJpdHlDb250ZXh0IjoiUlNBX1BLSSIsImVuYyI6IkExMjhHQ00ifQ.cCsGbqgFdzVb1jhXNR--gApzoXH-LldMArSoG59x6i0BbI7jttqxyAdcriSy8q_77VAp3854P9kekjj54RKLrP6APDIr46DI97kjG9E99ONXImnEyamHj95ZH_AW8lvkfa09KAr4537RM8GEXyZoys2vfIW8zqjjicZ8EKIpAixNlmrFJu6-Bo_utsmDN_DuGm69Kk2_nh6txa7ML9PCI59LFfOMniAf7ZwoZUBDCY7Oh8kx3wsZ0kxNBwfyLBCMEYzET0qcIYxePezQpkNcaZ4oogmdNSpYY-KbZGMcWpo1DKhWphDVp0lZcLxA6Q25K78e5AtarR5whN4HUAkurQ.CFjWpHkAVoLCG8q0.NcsTuauebemJXmos_mLMTyLhEHL-p5Wv6J88WkgzyjAt_DW7laiPMYw2sqRXkOiMJLwhifRzbSp8ZgJBM25IX05dKKSS4XfFjJQQjOBHw6PYtEF5pUDMLHML3jcddCrX07abfef_DuP41PqOQYsjwesLZ8XsRj-R0TH4diOZ_GQop8_oawjRIo9eJr9Wbtho0h8kAzHYpfuhamOPT718EaGAY6SSrR7t6nBkzGNkrKAmHkC7aRwe.AbZG53wRqgF0XRG3wUK_UQ"
    }
  }
}

```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `CreditCard.Brand`         |Texto   |10       |Sim          |Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).|
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira: `ApplePay` / `SamsungPay` / `VisaCheckout`/ `Masterpass` |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |
| `Wallet.AdditionalData.EphemeralPublicKey`| Texto  | 255    | Sim  | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`          |
| `Wallet.AdditionalData.capturecode`       | Texto  | 255    | Sim  | Código informado pela `MasterPass` ao lojista                                                    | 

#### Resposta

```json
{
    "MerchantOrderId": "2014111703",
    "Customer": {
        "Name": "[Guest]"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "453211******1521",
            "Holder": "Leonardo Romano",
            "ExpirationDate": "08/2020",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "0319040817883",
        "ProofOfSale": "817883",
        "AuthorizationCode": "027795",
        "Wallet": {
            "Type": "SamsungPay",
            "WalletKey": "eyJhbGciOiJSU0ExXzUiLCJraWQiOiIvam1iMU9PL2hHdFRVSWxHNFpxY2VYclVEbmFOUFV1ZUR5M2FWeHBzYXVRPSIsInR5cCI6IkpPU0UiLCJjaGFubmVsU2VjdXJpdHlDb250ZXh0IjoiUlNBX1BLSSIsImVuYyI6IkExMjhHQ00ifQ.cCsGbqgFdzVb1jhXNR--gApzoXH-LldMArSoG59x6i0BbI7jttqxyAdcriSy8q_77VAp3854P9kekjj54RKLrP6APDIr46DI97kjG9E99ONXImnEyamHj95ZH_AW8lvkfa09KAr4537RM8GEXyZoys2vfIW8zqjjicZ8EKIpAixNlmrFJu6-Bo_utsmDN_DuGm69Kk2_nh6txa7ML9PCI59LFfOMniAf7ZwoZUBDCY7Oh8kx3wsZ0kxNBwfyLBCMEYzET0qcIYxePezQpkNcaZ4oogmdNSpYY-KbZGMcWpo1DKhWphDVp0lZcLxA6Q25K78e5AtarR5whN4HUAkurQ.CFjWpHkAVoLCG8q0.NcsTuauebemJXmos_mLMTyLhEHL-p5Wv6J88WkgzyjAt_DW7laiPMYw2sqRXkOiMJLwhifRzbSp8ZgJBM25IX05dKKSS4XfFjJQQjOBHw6PYtEF5pUDMLHML3jcddCrX07abfef_DuP41PqOQYsjwesLZ8XsRj-R0TH4diOZ_GQop8_oawjRIo9eJr9Wbtho0h8kAzHYpfuhamOPT718EaGAY6SSrR7t6nBkzGNkrKAmHkC7aRwe.AbZG53wRqgF0XRG3wUK_UQ",
            "Eci": 0
                 },
        "SoftDescriptor": "123456789ABCD",
        "Amount": 100,
        "ReceivedDate": "2018-03-19 16:08:16",
        "Status": 1,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "e57b09eb-475b-44b6-ac71-01b9b82f2491",
        "Type": "CreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

| Propriedade         | Descrição                                                                                                                      | Tipo  | Tamanho | Formato                              |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
| `ProofOfSale`       | Número da autorização, identico ao NSU.                                                                                        | Texto | 6       | Texto alfanumérico                   |
| `Tid`               | Id da transação na adquirente.                                                                                                 | Texto | 20      | Texto alfanumérico                   |
| `AuthorizationCode` | Código de autorização.                                                                                                         | Texto | 6       | Texto alfanumérico                   |
| `SoftDescriptor`    | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais | Texto | 13      | Texto alfanumérico                   |
| `PaymentId`         | Campo Identificador do Pedido.                                                                                                 | Guid  | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx |
| `ECI`               | Eletronic Commerce Indicator. Representa o quão segura é uma transação.                                                        | Texto | 2       | Exemplos: 7                          |
| `Status`            | Status da Transação.                                                                                                           | Byte  | ---     | 2                                    |
| `ReturnCode`        | Código de retorno da Adquirência.                                                                                              | Texto | 32      | Texto alfanumérico                   |
| `ReturnMessage`     | Mensagem de retorno da Adquirência.                                                                                            | Texto | 512     | Texto alfanumérico                   |
| `Type`              |  indica qual o tipo de carteira: `ApplePay` / `SamsungPay` / `VisaCheckout`/ `Masterpass`                       | Texto | 255     | Texto alfanumérico                   |
| `Walletkey`         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações                              | Texto | 255     | Ver tabela `WalletKey`               |       
| `AdditionalData.EphemeralPublicKey` | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`                        | Texto | 255     | Ver Tabela `EphemeralPublicKey`      |  
| `AdditionalData.capturecode`        | Código informado pela `MasterPass` ao lojista                                                                  | Texto | 255     | 3                                    | 

### Envio de cartão

No modelo apresentado a seguir, demonstramos como a SamsungPay pode ser utilizada com o envio do cartão aberto, sem a necessidade de WalletKey.

#### Requisição

Nesse modelo, o lojista informa apenas que a transação é da Wallet SamsungPay e envia os dados ECI e CAVV fornecidos pela Samsung

* **CAVV** - pode ser extraido do campo `Cryptogram` retornado pela Samsung no payload
* **ECI** - retornado pela Samsung Pay no payload campo `eci_indicator` 

<aside class="request"><span class="method post">POST</span> <span class="endpoint">/1/sales/</span></aside>

```json
{
  "MerchantOrderId": "6242-642-723",
  "Customer": {
    "Name": "Exemplo Wallet Padrão",
    "Identity": "11225468954",
    "IdentityType": "CPF"
  },
  "Payment": {
    "Type": "CreditCard",
    "Amount": 1100,
    "Provider": "Cielo",
    "Installments": 1,
    "CreditCard": {
      "CardNumber":"4532********6521",
      "Holder":"Exemplo Wallet Padrão",
          "ExpirationDate":"12/2021",
          "SecurityCode":"123",
          "Brand":"Master"
    },
    "Currency": "BRL",
    "Wallet": {
      "Type": "SamsungPay",
      "Eci":"7",
      "Cavv":"AM1mbqehL24XAAa0J04CAoABFA=="
    }
  }
}
```

| Propriedade                | Tipo   | Tamanho | Obrigatório | Descrição                                                                                               |
|----------------------------|--------|---------|-------------|---------------------------------------------------------------------------------------------------------|
| `MerchantId`               | Guid   | 36      | Sim         | Identificador da loja na Cielo.                                                                         |
| `MerchantKey`              | Texto  | 40      | Sim         | Chave Publica para Autenticação Dupla na Cielo.                                                         |
| `RequestId`                | Guid   | 36      | Não         | Identificador do Request, utilizado quando o lojista usa diferentes servidores para cada GET/POST/PUT.  |
| `MerchantOrderId`          | Texto  | 50      | Sim         | Numero de identificação do Pedido.                                                                      |
| `Customer.Name`            | Texto  | 255     | Não         | Nome do Comprador.                                                                                      |
| `Customer.Status`          | Texto  | 255     | Não         | Status de cadastro do comprador na loja (NEW / EXISTING)                                                |
| `Payment.Type`             | Texto  | 100     | Sim         | Tipo do Meio de Pagamento.                                                                              |
| `Payment.Amount`           | Número | 15      | Sim         | Valor do Pedido (ser enviado em centavos).                                                              |
| `Payment.Installments`     | Número | 2       | Sim         | Número de Parcelas.                                                                                     |
| `CreditCard.CardNumber.`   | Texto  | 19      | Sim         | Número do Cartão do Comprador                                                                           |
| `CreditCard.SecurityCode`  | Texto  | 4       | Não         | Código de segurança impresso no verso do cartão - Ver Anexo.                                            |
| `CreditCard.Brand`         |Texto   |10       |Sim          |Bandeira do cartão (Visa / Master / Amex / Elo / Aura / JCB / Diners / Discover / Hipercard / Hiper).|
| `Wallet.Type`              | Texto  | 255     | Sim         | indica qual o tipo de carteira: `ApplePay` / `SamsungPay` / `VisaCheckout`/ `Masterpass` |
| `Wallet.Walletkey`         | Texto  | 255     | Sim         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações       |
| `Wallet.ECI`               | Texto  | 3       | Sim         | O ECI (Eletronic Commerce Indicator) representa o quão segura é uma transação. Esse valor deve ser levado em consideração pelo lojista para decidir sobre a captura da transação. |
| `Wallet.CAVV`              | Texto  | 255     | Sim         | Campo de validação retornado pela Wallet e utilizado como base de autorização                           | 

#### Resposta

```json
{
    "MerchantOrderId": "6242-642-723",
    "Customer": {
        "Name": "Exemplo Wallet Padrão",
        "Identity": "11225468954",
        "IdentityType": "CPF"
    },
    "Payment": {
        "ServiceTaxAmount": 0,
        "Installments": 1,
        "Interest": 0,
        "Capture": false,
        "Authenticate": false,
        "Recurrent": false,
        "CreditCard": {
            "CardNumber": "453265******6521",
            "Holder": "Exemplo Wallet Padrão",
            "ExpirationDate": "12/2021",
            "SaveCard": false,
            "Brand": "Visa"
        },
        "Tid": "10447480687BVV8COCRB",
        "ProofOfSale": "457033",
        "Provider": "Cielo",
        "Eci": "7",
        "Wallet": {
            "Type": "Samsung",
            "Cavv": "AM1mbqehL24XAAa0J04CAoABFA==",
            "Eci": 7
        },
        "VelocityAnalysis": {
            "Id": "98652f2c-bdfd-47b9-8673-77b80a6fe705",
            "ResultMessage": "Accept",
            "Score": 0
        },
        "Amount": 1100,
        "ReceivedDate": "2018-04-18 16:27:22",
        "Status": 2,
        "IsSplitted": false,
        "ReturnMessage": "Operation Successful",
        "ReturnCode": "4",
        "PaymentId": "98652f2c-bdfd-47b9-8673-77b80a6fe705",
        "Type": "CreditCard",
        "Currency": "BRL",
        "Country": "BRA",
        "Links": [
            {
                "Method": "GET",
                "Rel": "self",
                "Href": "https://apiquerysandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491"
            },
            {
                "Method": "PUT",
                "Rel": "capture",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/capture"
            },
            {
                "Method": "PUT",
                "Rel": "void",
                "Href": "https://apisandbox.cieloecommerce.cielo.com.br/1/sales/e57b09eb-475b-44b6-ac71-01b9b82f2491/void"
            }
        ]
    }
}
```

| Propriedade                         | Descrição                                                                                                                                    | Tipo  | Tamanho | Formato                              |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|-------|---------|--------------------------------------|
| `ProofOfSale`                       | Número da autorização, identico ao NSU.                                                                                                      | Texto | 6       | Texto alfanumérico                   |
| `Tid`                               | Id da transação na adquirente.                                                                                                               | Texto | 20      | Texto alfanumérico                   |
| `AuthorizationCode`                 | Código de autorização.                                                                                                                       | Texto | 6       | Texto alfanumérico                   |
| `SoftDescriptor`                    | Texto que será impresso na fatura bancaria do portador - Disponivel apenas para VISA/MASTER - nao permite caracteres especiais               | Texto | 13      | Texto alfanumérico                   |
| `PaymentId`                         | Campo Identificador do Pedido.                                                                                                               | Guid  | 36      | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx |
| `ECI`                               | Eletronic Commerce Indicator. Representa o quão segura é uma transação.                                                                      | Texto | 2       | Exemplos: 7                          |
| `Status`                            | Status da Transação.                                                                                                                         | Byte  | ---     | 2                                    |
| `ReturnCode`                        | Código de retorno da Adquirência.                                                                                                            | Texto | 32      | Texto alfanumérico                   |
| `ReturnMessage`                     | Mensagem de retorno da Adquirência.                                                                                                          | Texto | 512     | Texto alfanumérico                   |
| `Type`                              | indica qual o tipo de carteira: `ApplePay` / `SamsungPay` / `VisaCheckout`/ `Masterpass`                                      | Texto | 255     | Texto alfanumérico                   |
| `Walletkey`                         | Chave criptografica que identifica lojas nas Wallets - Ver tabela WalletKey para mais informações                                            | Texto | 255     | Ver tabela `WalletKey`               |
| `AdditionalData.EphemeralPublicKey` | Token retornado pela Wallet. Deve ser enviado em Integrações: `ApplePay`                                                      | Texto | 255     | Ver Tabela `EphemeralPublicKey`      |
| `AdditionalData.capturecode`        | Código informado pela `MasterPass` ao lojista                                                                                                | Texto | 255     | 3                                    |
| `ECI`                               | O ECI (Eletronic Commerce Indicator) indica a segurança de uma transação. Deve ser levado em conta pelo lojista para decidir sobre a captura | Texto | 3       | 2                                    |
| `CAVV`                              | Campo de validação retornado pela Wallet e utilizado como base de autorização                                                                | Texto | 255     | --                                   |

# Códigos da API

## Sobre os códigos

A Api Cielo e-commerce possui 4 tipos de códigos retornados que representam diferentes momentos da transação.
Abaixo vamos explica-los na ordem em que podem ocorrer:

|Código|Descrição|
|---|---|
|**HTTP Status Code**|São códigos do padrão HTTP. Eles informam se as informações enviadas a API estão de **fato obtendo sucesso ao atingir nossos ENDPOINTs**. Se valores diferentes de 200 ou 201 estejam aparecendo, há algum empecilho com a comunicação com a API<BR><BR> *Retornado no momento da requisição a API*|
|**Erros da API**|Esses códigos são respostas a **validação do conteúdo dos dados enviados**. Se eles estão sendo exibidos, as chamadas a nossa API foram identificadas e estão sendo validadas. Se esse código for exibido, a requisição contem erros (EX: tamanho/condições/erros de cadastro) que impedem a criação da transação<BR><BR>*Retornado no momento da requisição a API*|
|**Status**|Depois de criada a transação, esses códigos serão retornados, informando como se encontra a transação no momento (EX: `Autorizada` > `Capturada` > `Cancelada`)<BR><BR>*Retornado no campo `Status` *|
|**Retorno das Vendas**|Formado por um **código de Retorno** e uma **mensagem**, esses códigos indicam o **motivo** de um determinado `Status` dentro de uma transação. Eles indicam, por exemplo, se uma transação com `status` negada não foi autorizada devido saldo negativo no banco emissor. <BR><BR>*Retornados nos campos `ReturnCode` e `ReturnMessage`*<BR> *Ocorrem somente em Cartões de crédito e Débito*|

> **OBS**: No  antigo **Webservice 1.5 Cielo**, o `ReturnCode` era considerado como *Status da transação*. Na **API CIELO ECOMMERCE**, o campo `Status` possui códigos próprios, sendo assim, o **campo a ser considerado como base de identificação do status de uma transação**

## HTTP Status Code

|HTTP Status Code|Descrição|
|---|---|
|200|OK (Capture/Void/Get) |
|201|OK (Authorization) |
|400|Bad Request|
|404|Resource Not Found|
|500|Internal Server Error|

## Status transacional

| Código | Status               | Meio de pagamento | Descrição                                                        |
|--------|----------------------|-------------------|------------------------------------------------------------------|
| 0      | **NotFinished**      | ALL               | Aguardando atualização de status                                 |
| 1      | **Authorized**       | ALL               | Pagamento apto a ser capturado ou definido como pago             |
| 2      | **PaymentConfirmed** | ALL               | Pagamento confirmado e finalizado                                |
| 3      | **Denied**           | CC + CD + TF      | Pagamento negado por Autorizador                                 |
| 10     | **Voided**           | ALL               | Pagamento cancelado                                              |
| 11     | **Refunded**         | CC + CD           | Pagamento cancelado após 23:59 do dia de autorização             |
| 12     | **Pending**          | ALL               | Aguardando Status de instituição financeira                      |
| 13     | **Aborted**          | ALL               | Pagamento cancelado por falha no processamento ou por ação do AF |
| 20     | **Scheduled**          | CC              | Recorrência agendada                                             |

-

|Meio de pagamento|Descrição|
|---|---|
|**ALL**|Todos|
|**CC**|Cartão de Crédito|
|**CD**|Cartão de Débito|
|**TF**|Transferencia Eletrônica|
|**BOL**|Boleto|

## Erros de integração

> **Erros da API** - Esses códigos são respostas a **validação do conteúdo dos dados enviados**. <br>
> Se esse código for exibido, a requisição contem erros (EX: tamanho/condições/erros de cadastro) que impedem a criação da transação<BR><BR>*Retornado no momento da requisição a API*

``` json
[
    {
        "Code": 126,
        "Message": "Credit Card Expiration Date is invalid"
    }
]
```

### Códigos de Erros da API

Códigos retornados em caso de erro, identificando o motivo do erro e suas respectivas mensagens.

| Código | Mensagem                                                                                                       | Descrição                                                                                     |
|--------|----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| 0      | Internal error                                                                                                 | Dado enviado excede o tamanho do campo                                                        |
| 100    | RequestId is required                                                                                          | Campo enviado está vazio ou inválido                                                          |
| 101    | MerchantId is required                                                                                         | Campo enviado está vazio ou inválido                                                          |
| 102    | Payment Type is required                                                                                       | Campo enviado está vazio ou inválido                                                          |
| 103    | Payment Type can only contain letters                                                                          | Caracteres especiais não permitidos                                                           |
| 104    | Customer Identity is required                                                                                  | Campo enviado está vazio ou inválido                                                          |
| 105    | Customer Name is required                                                                                      | Campo enviado está vazio ou inválido                                                          |
| 106    | Transaction ID is required                                                                                     | Campo enviado está vazio ou inválido                                                          |
| 107    | OrderId is invalid or does not exists                                                                          | Campo enviado excede o tamanho ou contem caracteres especiais                                 |
| 108    | Amount must be greater or equal to zero                                                                        | Valor da transação deve ser maior que "0"                                                     |
| 109    | Payment Currency is required                                                                                   | Campo enviado está vazio ou inválido                                                          |
| 110    | Invalid Payment Currency                                                                                       | Campo enviado está vazio ou inválido                                                          |
| 111    | Payment Country is required                                                                                    | Campo enviado está vazio ou inválido                                                          |
| 112    | Invalid Payment Country                                                                                        | Campo enviado está vazio ou inválido                                                          |
| 113    | Invalid Payment Code                                                                                           | Campo enviado está vazio ou inválido                                                          |
| 114    | The provided MerchantId is not in correct format                                                               | O MerchantId enviado não é um GUID                                                            |
| 115    | The provided MerchantId was not found                                                                          | O MerchantID não existe ou pertence a outro ambiente (EX: Sandbox)                            |
| 116    | The provided MerchantId is blocked                                                                             | Loja bloqueada, entre em contato com o suporte Cielo                                          |
| 117    | Credit Card Holder is required                                                                                 | Campo enviado está vazio ou inválido                                                          |
| 118    | Credit Card Number is required                                                                                 | Campo enviado está vazio ou inválido                                                          |
| 119    | At least one Payment is required                                                                               | Nó "Payment" não enviado                                                                      |
| 120    | Request IP not allowed. Check your IP White List                                                               | IP bloqueado por questões de segurança                                                        |
| 121    | Customer is required                                                                                           | Nó "Customer" não enviado                                                                     |
| 122    | MerchantOrderId is required                                                                                    | Campo enviado está vazio ou inválido                                                          |
| 123    | Installments must be greater or equal to one                                                                   | Numero de parcelas deve ser superior a 1                                                      |
| 124    | Credit Card is Required                                                                                        | Campo enviado está vazio ou inválido                                                          |
| 125    | Credit Card Expiration Date is required                                                                        | Campo enviado está vazio ou inválido                                                          |
| 126    | Credit Card Expiration Date is invalid                                                                         | Campo enviado está vazio ou inválido                                                          |
| 127    | You must provide CreditCard Number                                                                             | Numero do cartão de crédito é obrigatório                                                     |
| 128    | Card Number length exceeded                                                                                    | Numero do cartão superiro a 16 digitos                                                        |
| 129    | Affiliation not found                                                                                          | Meio de pagamento não vinculado a loja ou Provider inválido                                   |
| 130    | Could not get Credit Card                                                                                      | ---                                                                                           |
| 131    | MerchantKey is required                                                                                        | Campo enviado está vazio ou inválido                                                          |
| 132    | MerchantKey is invalid                                                                                         | O Merchantkey enviado não é um válido                                                         |
| 133    | Provider is not supported for this Payment Type                                                                | Provider enviado não existe                                                                   |
| 134    | FingerPrint length exceeded                                                                                    | Dado enviado excede o tamanho do campo                                                        |
| 135    | MerchantDefinedFieldValue length exceeded                                                                      | Dado enviado excede o tamanho do campo                                                        |
| 136    | ItemDataName length exceeded                                                                                   | Dado enviado excede o tamanho do campo                                                        |
| 137    | ItemDataSKU length exceeded                                                                                    | Dado enviado excede o tamanho do campo                                                        |
| 138    | PassengerDataName length exceeded                                                                              | Dado enviado excede o tamanho do campo                                                        |
| 139    | PassengerDataStatus length exceeded                                                                            | Dado enviado excede o tamanho do campo                                                        |
| 140    | PassengerDataEmail length exceeded                                                                             | Dado enviado excede o tamanho do campo                                                        |
| 141    | PassengerDataPhone length exceeded                                                                             | Dado enviado excede o tamanho do campo                                                        |
| 142    | TravelDataRoute length exceeded                                                                                | Dado enviado excede o tamanho do campo                                                        |
| 143    | TravelDataJourneyType length exceeded                                                                          | Dado enviado excede o tamanho do campo                                                        |
| 144    | TravelLegDataDestination length exceeded                                                                       | Dado enviado excede o tamanho do campo                                                        |
| 145    | TravelLegDataOrigin length exceeded                                                                            | Dado enviado excede o tamanho do campo                                                        |
| 146    | SecurityCode length exceeded                                                                                   | Dado enviado excede o tamanho do campo                                                        |
| 147    | Address Street length exceeded                                                                                 | Dado enviado excede o tamanho do campo                                                        |
| 148    | Address Number length exceeded                                                                                 | Dado enviado excede o tamanho do campo                                                        |
| 149    | Address Complement length exceeded                                                                             | Dado enviado excede o tamanho do campo                                                        |
| 150    | Address ZipCode length exceeded                                                                                | Dado enviado excede o tamanho do campo                                                        |
| 151    | Address City length exceeded                                                                                   | Dado enviado excede o tamanho do campo                                                        |
| 152    | Address State length exceeded                                                                                  | Dado enviado excede o tamanho do campo                                                        |
| 153    | Address Country length exceeded                                                                                | Dado enviado excede o tamanho do campo                                                        |
| 154    | Address District length exceeded                                                                               | Dado enviado excede o tamanho do campo                                                        |
| 155    | Customer Name length exceeded                                                                                  | Dado enviado excede o tamanho do campo                                                        |
| 156    | Customer Identity length exceeded                                                                              | Dado enviado excede o tamanho do campo                                                        |
| 157    | Customer IdentityType length exceeded                                                                          | Dado enviado excede o tamanho do campo                                                        |
| 158    | Customer Email length exceeded                                                                                 | Dado enviado excede o tamanho do campo                                                        |
| 159    | ExtraData Name length exceeded                                                                                 | Dado enviado excede o tamanho do campo                                                        |
| 160    | ExtraData Value length exceeded                                                                                | Dado enviado excede o tamanho do campo                                                        |
| 161    | Boleto Instructions length exceeded                                                                            | Dado enviado excede o tamanho do campo                                                        |
| 162    | Boleto Demostrative length exceeded                                                                            | Dado enviado excede o tamanho do campo                                                        |
| 163    | Return Url is required                                                                                         | URL de retorno não é valida - Não é aceito paginação ou extenções (EX .PHP) na URL de retorno |
| 166    | AuthorizeNow is required                                                                                       | ---                                                                                           |
| 167    | Antifraud not configured                                                                                       | Antifraude não vinculado ao cadastro do lojista                                               |
| 168    | Recurrent Payment not found                                                                                    | Recorrência não encontrada                                                                    |
| 169    | Recurrent Payment is not active                                                                                | Recorrência não está ativa. Execução paralizada                                               |
| 170    | Cartão Protegido not configured                                                                                | Cartão protegido não vinculado ao cadastro do lojista                                         |
| 171    | Affiliation data not sent                                                                                      | Falha no processamento do pedido - Entre em contato com o suporte Cielo                       |
| 172    | Credential Code is required                                                                                    | Falha na validação das credenciadas enviadas                                                  |
| 173    | Payment method is not enabled                                                                                  | Meio de pagamento não vinculado ao cadastro do lojista                                        |
| 174    | Card Number is required                                                                                        | Campo enviado está vazio ou inválido                                                          |
| 175    | EAN is required                                                                                                | Campo enviado está vazio ou inválido                                                          |
| 176    | Payment Currency is not supported                                                                              | Campo enviado está vazio ou inválido                                                          |
| 177    | Card Number is invalid                                                                                         | Campo enviado está vazio ou inválido                                                          |
| 178    | EAN is invalid                                                                                                 | Campo enviado está vazio ou inválido                                                          |
| 179    | The max number of installments allowed for recurring payment is 1                                              | Campo enviado está vazio ou inválido                                                          |
| 180    | The provided Card PaymentToken was not found                                                                   | Token do Cartão protegido não encontrado                                                      |
| 181    | The MerchantIdJustClick is not configured                                                                      | Token do Cartão protegido bloqueado                                                           |
| 182    | Brand is required                                                                                              | Bandeira do cartão não enviado                                                                |
| 183    | Invalid customer bithdate                                                                                      | Data de nascimento inválida ou futura                                                         |
| 184    | Request could not be empty                                                                                     | Falha no formado da requisição. Verifique o código enviado                                    |
| 185    | Brand is not supported by selected provider                                                                    | Bandeira não suportada pela API Cielo                                                         |
| 186    | The selected provider does not support the options provided (Capture, Authenticate, Recurrent or Installments) | Meio de pagamento não suporta o comando enviado                                               |
| 187    | ExtraData Collection contains one or more duplicated names                                                     |                                                                                               |
| 188    | Avs with CPF invalid                                                                                           |                                                                                               |
| 189    | Avs with length of street exceeded                                                                             |                                                                                               |
| 190    | Avs with length of number exceeded                                                                             |                                                                                               |
| 191    | Avs with length of district exceeded                                                                           |                                                                                               |
| 192    | Avs with zip code invalid                                                                                      |                                                                                               |
| 193    | Split Amount must be greater than zero                                                                         |                                                                                               |
| 194    | Split Establishment is Required                                                                                | Campo enviado está vazio ou inválido                                                          |
| 195    | The PlataformId is required                                                                                    | Campo enviado está vazio ou inválido                                                          |
| 196    | DeliveryAddress is required                                                                                    | Campo enviado está vazio ou inválido                                                          |
| 197    | Street is required                                                                                             | Campo enviado está vazio ou inválido                                                          |
| 198    | Number is required                                                                                             | Campo enviado está vazio ou inválido                                                          |
| 199    | ZipCode is required                                                                                            | Campo enviado está vazio ou inválido                                                          |
| 200    | City is required                                                                                               | Campo enviado está vazio ou inválido                                                          |
| 201    | State is required                                                                                              | Campo enviado está vazio ou inválido                                                          |
| 202    | District is required                                                                                           | Campo enviado está vazio ou inválido                                                          |
| 203    | Cart item Name is required                                                                                     | Campo enviado está vazio ou inválido                                                          |
| 204    | Cart item Quantity is required                                                                                 | Campo enviado está vazio ou inválido                                                          |
| 205    | Cart item type is required                                                                                     | Campo enviado está vazio ou inválido                                                          |
| 206    | Cart item name length exceeded                                                                                 |                                                                                               |
| 207    | Cart item description length exceeded                                                                          |                                                                                               |
| 208    | Cart item sku length exceeded                                                                                  |                                                                                               |
| 209    | Shipping addressee sku length exceeded                                                                         |                                                                                               |
| 210    | Shipping data cannot be null                                                                                   |                                                                                               |
| 211    | WalletKey is invalid                                                                                           |                                                                                               |
| 212    | Merchant Wallet Configuration not found                                                                        |                                                                                               |
| 213    | Credit Card Number is invalid                                                                                  |                                                                                               |
| 214    | Credit Card Holder Must Have Only Letters                                                                      |                                                                                               |
| 215    | Agency is required in Boleto Credential                                                                        |                                                                                               |
| 216    | Customer IP address is invalid                                                                                 |                                                                                               |
| 300    | MerchantId was not found                                                                                       |                                                                                               |
| 301    | Request IP is not allowed                                                                                      |                                                                                               |
| 302    | Sent MerchantOrderId is duplicated                                                                             |                                                                                               |
| 303    | Sent OrderId does not exist                                                                                    |                                                                                               |
| 304    | Customer Identity is required                                                                                  | Campo enviado está vazio ou inválido                                                          |
| 306    | Merchant is blocked                                                                                            | Merchant está bloqueado                                                                       |
| 307    | Transaction not found                                                                                          |                                                                                               |
| 308    | Transaction not available to capture                                                                           |                                                                                               |
| 309    | Transaction not available to void                                                                              |                                                                                               |
| 310    | Payment method doest not support this operation                                                                |                                                                                               |
| 311    | Refund is not enabled for this merchant                                                                        |                                                                                               |
| 312    | Transaction not available to refund                                                                            |                                                                                               |
| 313    | Recurrent Payment not found                                                                                    |                                                                                               |
| 314    | Invalid Integration                                                                                            |                                                                                               |
| 315    | Cannot change NextRecurrency with pending payment                                                              |                                                                                               |
| 316    | Cannot set NextRecurrency to past date                                                                         |                                                                                               |
| 317    | Invalid Recurrency Day                                                                                         |                                                                                               |                                                                                              |
| 318    | No transaction found                                                                                           |                                                                                               |
| 319    | Smart recurrency is not enabled                                                                                |                                                                                               |
| 320    | Can not Update Affiliation Because this Recurrency not Affiliation saved                                       |                                                                                               |
| 321    | Can not set EndDate to before next recurrency                                                                  |                                                                                               |
| 322    | Zero Dollar Auth is not enabled                                                                                |                                                                                               |
| 323    | Bin Query is not enabled                                                                                       |                                                                                               |

### Códigos de Retorno

| Código Resposta | Definição                                     | Significado                                                                 | Ação                                                              | Permite Retentativa |
|-----------------|-----------------------------------------------|-----------------------------------------------------------------------------|-------------------------------------------------------------------|---------------------|
| 00              | Transação autorizada com sucesso.             | Transação autorizada com sucesso.                                           | Transação autorizada com sucesso.                                 | Não                 |
| 01              | Transação não autorizada. Transação referida. | Transação não autorizada. Referida (suspeita de fraude) pelo banco emissor. | Transação não autorizada. Entre em contato com seu banco emissor. | Não                 |
| 02              | Transação não autorizada. Transação referida. | Transação não autorizada. Referida (suspeita de fraude) pelo banco emissor. | Transação não autorizada. Entre em contato com seu banco emissor. | Não                 |
|03|Transação não permitida. Erro no cadastramento do código do estabelecimento no arquivo de configuração do TEF|Transação não permitida. Estabelecimento inválido. Entre com contato com a Cielo.|Não foi possível processar a transação. Entre com contato com a Loja Virtual.|Não|
|04|Transação não autorizada. Cartão bloqueado pelo banco emissor.|Transação não autorizada. Cartão bloqueado pelo banco emissor.|Transação não autorizada. Entre em contato com seu banco emissor.|Não|
|05|Transação não autorizada. Cartão inadimplente (Do not honor). |Transação não autorizada. Não foi possível processar a transação. Questão relacionada a segurança, inadimplencia ou limite do portador.|Transação não autorizada. Entre em contato com seu banco emissor.|Apenas 4 vezes em 16 dias.|
|06|Transação não autorizada. Cartão cancelado.|Transação não autorizada. Não foi possível processar a transação. Cartão cancelado permanentemente pelo banco emissor.|Não foi possível processar a transação. Entre em contato com seu banco emissor.|Não|
|07|Transação negada. Reter cartão condição especial|Transação não autorizada por regras do banco emissor.|Transação não autorizada. Entre em contato com seu banco emissor|Não|
|08|Transação não autorizada. Código de segurança inválido.|Transação não autorizada. Código de segurança inválido. Oriente o portador a corrigir os dados e tentar novamente.|Transação não autorizada. Dados incorretos. Reveja os dados e informe novamente.|Não|
|09|Transação cancelada parcialmente com sucesso.                 | Transação cancelada parcialmente com sucesso                                | Transação cancelada parcialmente com sucesso                       | Não                 |
|11|Transação autorizada com sucesso para cartão emitido no exterior|Transação autorizada com sucesso.|Transação autorizada com sucesso.|Não|
|12|Transação inválida, erro no cartão.|Não foi possível processar a transação. Solicite ao portador que verifique os dados do cartão e tente novamente.|Não foi possível processar a transação. reveja os dados informados e tente novamente. Se o erro persistir, entre em contato com seu banco emissor.|Não|
|13|Transação não permitida. Valor da transação Inválido.|Transação não permitida. Valor inválido. Solicite ao portador que reveja os dados e novamente. Se o erro persistir, entre em contato com a Cielo.|Transação não autorizada. Valor inválido. Refazer a transação confirmando os dados informados. Persistindo o erro, entrar em contato com a loja virtual.|Não|
|14|Transação não autorizada. Cartão Inválido|Transação não autorizada. Cartão inválido. Pode ser bloqueio do cartão no banco emissor, dados incorretos ou tentativas de testes de cartão. Use o Algoritmo de Lhum (Mod 10) para evitar transações não autorizadas por esse motivo. Consulte www.cielo.com.br/desenvolvedores para implantar o Algoritmo de Lhum.|Não foi possível processar a transação. reveja os dados informados e tente novamente. Se o erro persistir, entre em contato com seu banco emissor.|Não|
|15|Banco emissor indisponível ou inexistente.|Transação não autorizada. Banco emissor indisponível.|Não foi possível processar a transação. Entre em contato com seu banco emissor.|Não|
|19|Refaça a transação ou tente novamente mais tarde.|Não foi possível processar a transação. Refaça a transação ou tente novamente mais tarde. Se o erro persistir, entre em contato com a Cielo.|Não foi possível processar a transação. Refaça a transação ou tente novamente mais tarde. Se o erro persistir entre em contato com a loja virtual.|Apenas 4 vezes em 16 dias.|
|21|Cancelamento não efetuado. Transação não localizada.|Não foi possível processar o cancelamento. Se o erro persistir, entre em contato com a Cielo.|Não foi possível processar o cancelamento. Tente novamente mais tarde. Persistindo o erro, entrar em contato com a loja virtual.|Não|
|22|Parcelamento inválido. Número de parcelas inválidas.|Não foi possível processar a transação. Número de parcelas inválidas. Se o erro persistir, entre em contato com a Cielo.|Não foi possível processar a transação. Valor inválido. Refazer a transação confirmando os dados informados. Persistindo o erro, entrar em contato com a loja virtual.|Não|
|23|Transação não autorizada. Valor da prestação inválido.|Não foi possível processar a transação. Valor da prestação inválido. Se o erro persistir, entre em contato com a Cielo.|Não foi possível processar a transação. Valor da prestação inválido. Refazer a transação confirmando os dados informados. Persistindo o erro, entrar em contato com a loja virtual.|Não|
|24|Quantidade de parcelas inválido.|Não foi possível processar a transação. Quantidade de parcelas inválido. Se o erro persistir, entre em contato com a Cielo.|Não foi possível processar a transação. Quantidade de parcelas inválido. Refazer a transação confirmando os dados informados. Persistindo o erro, entrar em contato com a loja virtual.|Não|
|25|Pedido de autorização não enviou número do cartão|Não foi possível processar a transação. Solicitação de autorização não enviou o número do cartão. Se o erro persistir, verifique a comunicação entre loja virtual e Cielo.|Não foi possível processar a transação. reveja os dados informados e tente novamente. Persistindo o erro, entrar em contato com a loja virtual.|Apenas 4 vezes em 16 dias.|
|28|Arquivo temporariamente indisponível.|Não foi possível processar a transação. Arquivo temporariamente indisponível. Reveja a comunicação entre Loja Virtual e Cielo. Se o erro persistir, entre em contato com a Cielo.|Não foi possível processar a transação. Entre com contato com a Loja Virtual.|Apenas 4 vezes em 16 dias.|
|30|Transação não autorizada. Decline Message|Não foi possível processar a transação. Solicite ao portador que reveja os dados e tente novamente. Se o erro persistir verifique a comunicação com a Cielo esta sendo feita corretamente|Não foi possível processar a transação. Reveja os dados e tente novamente. Se o erro persistir, entre em contato com a loja|Não|
|39|Transação não autorizada. Erro no banco emissor.|Transação não autorizada. Erro no banco emissor.|Transação não autorizada. Entre em contato com seu banco emissor.|Não|
|41|Transação não autorizada. Cartão bloqueado por perda.|Transação não autorizada. Cartão bloqueado por perda.|Transação não autorizada. Entre em contato com seu banco emissor.|Não|
|43|Transação não autorizada. Cartão bloqueado por roubo.|Transação não autorizada. Cartão bloqueado por roubo.|Transação não autorizada. Entre em contato com seu banco emissor.|Não|
|51|Transação não autorizada. Limite excedido/sem saldo.|Transação não autorizada. Limite excedido/sem saldo.|Transação não autorizada. Entre em contato com seu banco emissor.|A partir do dia seguinte, apenas 4 vezes em 16 dias.|
|52|Cartão com dígito de controle inválido.|Não foi possível processar a transação. Cartão com dígito de controle inválido.|Transação não autorizada. Reveja os dados informados e tente novamente.|Não|
|53|Transação não permitida. Cartão poupança inválido|Transação não permitida. Cartão poupança inválido.|Não foi possível processar a transação. Entre em contato com seu banco emissor.|Não|
|54|Transação não autorizada. Cartão vencido|Transação não autorizada. Cartão vencido.|Transação não autorizada. Refazer a transação confirmando os dados.|Não|
|55|Transação não autorizada. Senha inválida|Transação não autorizada. Senha inválida.|Transação não autorizada. Entre em contato com seu banco emissor.|Não|
|57|Transação não permitida para o cartão|Transação não autorizada. Transação não permitida para o cartão.|Transação não autorizada. Entre em contato com seu banco emissor.|Apenas 4 vezes em 16 dias.|
|58|Transação não permitida. Opção de pagamento inválida.|Transação não permitida. Opção de pagamento inválida. Reveja se a opção de pagamento escolhida está habilitada no cadastro|Transação não autorizada. Entre em contato com sua loja virtual.|Não|
|59|Transação não autorizada. Suspeita de fraude.|Transação não autorizada. Suspeita de fraude.|Transação não autorizada. Entre em contato com seu banco emissor.|Não|
|60|Transação não autorizada.|Transação não autorizada. Tente novamente. Se o erro persistir o portador deve entrar em contato com o banco emissor.|Não foi possível processar a transação. Tente novamente mais tarde. Se o erro persistir, entre em contato com seu banco emissor.|Apenas 4 vezes em 16 dias.|
|61|Banco emissor indisponível.|Transação não autorizada. Banco emissor indisponível.|Transação não autorizada. Tente novamente. Se o erro persistir, entre em contato com seu banco emissor.|Apenas 4 vezes em 16 dias.|
|62|Transação não autorizada. Cartão restrito para uso doméstico|Transação não autorizada. Cartão restrito para uso doméstico.|Transação não autorizada. Entre em contato com seu banco emissor.|A partir do dia seguinte, apenas 4 vezes em 16 dias.|
|63|Transação não autorizada. Violação de segurança|Transação não autorizada. Violação de segurança.|Transação não autorizada. Entre em contato com seu banco emissor.|Não|
|64|Transação não autorizada. Valor abaixo do mínimo exigido pelo banco emissor.|Transação não autorizada. Entre em contato com seu banco emissor.|Transação não autorizada. Valor abaixo do mínimo exigido pelo banco emissor.|Não|
|65|Transação não autorizada. Excedida a quantidade de transações para o cartão.|Transação não autorizada. Excedida a quantidade de transações para o cartão.|Transação não autorizada. Entre em contato com seu banco emissor.|Apenas 4 vezes em 16 dias.|
|67|Transação não autorizada. Cartão bloqueado para compras hoje.|Transação não autorizada. Cartão bloqueado para compras hoje. Bloqueio pode ter ocorrido por excesso de tentativas inválidas. O cartão será desbloqueado automaticamente à meia noite.|Transação não autorizada. Cartão bloqueado temporariamente. Entre em contato com seu banco emissor.|A partir do dia seguinte, apenas 4 vezes em 16 dias.|
|70|Transação não autorizada. Limite excedido/sem saldo.|Transação não autorizada. Limite excedido/sem saldo.|Transação não autorizada. Entre em contato com seu banco emissor.|A partir do dia seguinte, apenas 4 vezes em 16 dias.|
|72|Cancelamento não efetuado. Saldo disponível para cancelamento insuficiente.|Cancelamento não efetuado. Saldo disponível para cancelamento insuficiente. Se o erro persistir, entre em contato com a Cielo.|Cancelamento não efetuado. Tente novamente mais tarde. Se o erro persistir, entre em contato com a loja virtual.|Não|
|74|Transação não autorizada. A senha está vencida.|Transação não autorizada. A senha está vencida.|Transação não autorizada. Entre em contato com seu banco emissor.|Não|
|75|Senha bloqueada. Excedeu tentativas de cartão.|Transação não autorizada.|Sua Transação não pode ser processada. Entre em contato com o Emissor do seu cartão.|Não|
|76|Cancelamento não efetuado. Banco emissor não localizou a transação original|Cancelamento não efetuado. Banco emissor não localizou a transação original|Cancelamento não efetuado. Entre em contato com a loja virtual.|Não|
|77|Cancelamento não efetuado. Não foi localizado a transação original|Cancelamento não efetuado. Não foi localizado a transação original|Cancelamento não efetuado. Entre em contato com a loja virtual.|Não|
|78|Transação não autorizada. Cartão bloqueado primeiro uso.|Transação não autorizada. Cartão bloqueado primeiro uso. Solicite ao portador que desbloqueie o cartão diretamente com seu banco emissor.|Transação não autorizada. Entre em contato com seu banco emissor e solicite o desbloqueio do cartão.|Não|
|80|Transação não autorizada. Divergencia na data de transação/pagamento.|Transação não autorizada. Data da transação ou data do primeiro pagamento inválida.|Transação não autorizada. Refazer a transação confirmando os dados.|Não|
|82|Transação não autorizada. Cartão inválido.|Transação não autorizada. Cartão Inválido. Solicite ao portador que reveja os dados e tente novamente.|Transação não autorizada. Refazer a transação confirmando os dados. Se o erro persistir, entre em contato com seu banco emissor.|Não|
|83|Transação não autorizada. Erro no controle de senhas|Transação não autorizada. Erro no controle de senhas|Transação não autorizada. Refazer a transação confirmando os dados. Se o erro persistir, entre em contato com seu banco emissor.|Não|
|85|Transação não permitida. Falha da operação.|Transação não permitida. Houve um erro no processamento.Solicite ao portador que digite novamente os dados do cartão, se o erro persistir pode haver um problema no terminal do lojista, nesse caso o lojista deve entrar em contato com a Cielo.|Transação não permitida. Informe os dados do cartão novamente. Se o erro persistir, entre em contato com a loja virtual.|Não|
|86|Transação não permitida. Falha da operação.|Transação não permitida. Houve um erro no processamento.Solicite ao portador que digite novamente os dados do cartão, se o erro persistir pode haver um problema no terminal do lojista, nesse caso o lojista deve entrar em contato com a Cielo.|Transação não permitida. Informe os dados do cartão novamente. Se o erro persistir, entre em contato com a loja virtual.|Não|
|88|Falha na criptografia dos dados.|Falha na criptografia dos dados.|Entre em contato com seu banco emissor.|Não|
|89|Erro na transação.|Transação não autorizada. Erro na transação. O portador deve tentar novamente e se o erro persistir, entrar em contato com o banco emissor.|Transação não autorizada. Erro na transação. Tente novamente e se o erro persistir, entre em contato com seu banco emissor.|Apenas 4 vezes em 16 dias.|
|90|Transação não permitida. Falha da operação.|Transação não permitida. Houve um erro no processamento.Solicite ao portador que digite novamente os dados do cartão, se o erro persistir pode haver um problema no terminal do lojista, nesse caso o lojista deve entrar em contato com a Cielo.|Transação não permitida. Informe os dados do cartão novamente. Se o erro persistir, entre em contato com a loja virtual.|Não|
|91|Transação não autorizada. Banco emissor temporariamente indisponível.|Transação não autorizada. Banco emissor temporariamente indisponível.|Transação não autorizada. Banco emissor temporariamente indisponível. Entre em contato com seu banco emissor.|Apenas 4 vezes em 16 dias.|
|92|Transação não autorizada. Tempo de comunicação excedido.|Transação não autorizada. Tempo de comunicação excedido.|Transação não autorizada. Comunicação temporariamente indisponível. Entre em contato com a loja virtual.|Apenas 4 vezes em 16 dias.|
|93|Transação não autorizada. Violação de regra - Possível erro no cadastro.|Transação não autorizada. Violação de regra - Possível erro no cadastro.|Sua transação não pode ser processada. Entre em contato com a loja virtual.|Não|
|94|Transação duplicada.|Transação duplicada enviado para autorização/captura.|O estabelecimento deve revisar as transações enviadas.|Não|
|96|Falha no processamento.|Não foi possível processar a transação. Falha no sistema da Cielo. Se o erro persistir, entre em contato com a Cielo.|Sua Transação não pode ser processada, Tente novamente mais tarde. Se o erro persistir, entre em contato com a loja virtual.|Apenas 4 vezes em 16 dias.|
|97|Valor não permitido para essa transação.|Transação não autorizada. Valor não permitido para essa transação.|Transação não autorizada. Valor não permitido para essa transação.|Não|
|98|Sistema/comunicação indisponível.|Transação não autorizada. Sistema do emissor sem comunicação. Se for geral, verificar SITEF, GATEWAY e/ou Conectividade.|Sua Transação não pode ser processada, Tente novamente mais tarde. Se o erro persistir, entre em contato com a loja virtual.|Apenas 4 vezes em 16 dias.|
|99|Sistema/comunicação indisponível.|Transação não autorizada. Sistema do emissor sem comunicação. Tente mais tarde.  Pode ser erro no SITEF, favor verificar !|Sua Transação não pode ser processada, Tente novamente mais tarde. Se o erro persistir, entre em contato com a loja virtual.|A partir do dia seguinte, apenas 4 vezes em 16 dias.|
|475|Timeout de Cancelamento|A aplicação não respondeu dentro do tempo esperado.|Realizar uma nova tentativa após alguns segundos. Persistindo, entrar em contato com o Suporte.|Não|
|999|Sistema/comunicação indisponível.|Transação não autorizada. Sistema do emissor sem comunicação. Tente mais tarde.  Pode ser erro no SITEF, favor verificar !|Sua Transação não pode ser processada, Tente novamente mais tarde. Se o erro persistir, entre em contato com a loja virtual.|A partir do dia seguinte, apenas 4 vezes em 16 dias.|
|AA|Tempo Excedido|Tempo excedido na comunicação com o banco emissor. Oriente o portador a tentar novamente, se o erro persistir será necessário que o portador contate seu banco emissor.|Tempo excedido na sua comunicação com o banco emissor, tente novamente mais tarde. Se o erro persistir, entre em contato com seu banco.|Apenas 4 vezes em 16 dias.|
|AC|Transação não permitida. Cartão de débito sendo usado com crédito. Use a função débito.|Transação não permitida. Cartão de débito sendo usado com crédito. Solicite ao portador que selecione a opção de pagamento Cartão de Débito.|Transação não autorizada. Tente novamente selecionando a opção de pagamento cartão de débito.|Não|
|AE|Tente Mais Tarde|Tempo excedido na comunicação com o banco emissor. Oriente o portador a tentar novamente, se o erro persistir será necessário que o portador contate seu banco emissor.|Tempo excedido na sua comunicação com o banco emissor, tente novamente mais tarde. Se o erro persistir, entre em contato com seu banco.|Apenas 4 vezes em 16 dias.|
|AF|Transação não permitida. Falha da operação.|Transação não permitida. Houve um erro no processamento.Solicite ao portador que digite novamente os dados do cartão, se o erro persistir pode haver um problema no terminal do lojista, nesse caso o lojista deve entrar em contato com a Cielo.|Transação não permitida. Informe os dados do cartão novamente. Se o erro persistir, entre em contato com a loja virtual.|Não|
|AG|Transação não permitida. Falha da operação.|Transação não permitida. Houve um erro no processamento.Solicite ao portador que digite novamente os dados do cartão, se o erro persistir pode haver um problema no terminal do lojista, nesse caso o lojista deve entrar em contato com a Cielo.|Transação não permitida. Informe os dados do cartão novamente. Se o erro persistir, entre em contato com a loja virtual.|Não|
|AH|Transação não permitida. Cartão de crédito sendo usado com débito. Use a função crédito.|Transação não permitida. Cartão de crédito sendo usado com débito. Solicite ao portador que selecione a opção de pagamento Cartão de Crédito.|Transação não autorizada. Tente novamente selecionando a opção de pagamento cartão de crédito.|Não|
|AI|Transação não autorizada. Autenticação não foi realizada.|Transação não autorizada. Autenticação não foi realizada. O portador não concluiu a autenticação. Solicite ao portador que reveja os dados e tente novamente. Se o erro persistir, entre em contato com a Cielo informando o BIN (6 primeiros dígitos do cartão)|Transação não autorizada. Autenticação não foi realizada com sucesso. Tente novamente e informe corretamente os dados solicitado. Se o erro persistir, entre em contato com o lojista.|Não|
|AJ|Transação não permitida. Transação de crédito ou débito em uma operação que permite apenas Private Label. Tente novamente selecionando a opção Private Label.|Transação não permitida. Transação de crédito ou débito em uma operação que permite apenas Private Label. Solicite ao portador que tente novamente selecionando a opção Private Label. Caso não disponibilize a opção Private Label verifique na Cielo se o seu estabelecimento permite essa operação.|Transação não permitida. Transação de crédito ou débito em uma operação que permite apenas Private Label. Tente novamente e selecione a opção Private Label. Em caso de um novo erro entre em contato com a loja virtual.|Não|
|AV|Transação não autorizada. Dados Inválidos|Falha na validação dos dados da transação. Oriente o portador a rever os dados e tentar novamente.|Falha na validação dos dados. Reveja os dados informados e tente novamente.|Apenas 4 vezes em 16 dias.|
|BD|Transação não permitida. Falha da operação.|Transação não permitida. Houve um erro no processamento.Solicite ao portador que digite novamente os dados do cartão, se o erro persistir pode haver um problema no terminal do lojista, nesse caso o lojista deve entrar em contato com a Cielo.|Transação não permitida. Informe os dados do cartão novamente. Se o erro persistir, entre em contato com a loja virtual.|Não|
|BL|Transação não autorizada. Limite diário excedido.|Transação não autorizada. Limite diário excedido. Solicite ao portador que entre em contato com seu banco emissor.|Transação não autorizada. Limite diário excedido. Entre em contato com seu banco emissor.|A partir do dia seguinte, apenas 4 vezes em 16 dias.|
|BM|Transação não autorizada. Cartão Inválido|Transação não autorizada. Cartão inválido. Pode ser bloqueio do cartão no banco emissor ou dados incorretos. Tente usar o Algoritmo de Lhum (Mod 10) para evitar transações não autorizadas por esse motivo.|Transação não autorizada. Cartão inválido.  Refaça a transação confirmando os dados informados.|Não|
|BN|Transação não autorizada. Cartão ou conta bloqueado.|Transação não autorizada. O cartão ou a conta do portador está bloqueada. Solicite ao portador que entre em contato com  seu banco emissor.|Transação não autorizada. O cartão ou a conta do portador está bloqueada. Entre em contato com  seu banco emissor.|Não|
|BO|Transação não permitida. Falha da operação.|Transação não permitida. Houve um erro no processamento. Solicite ao portador que digite novamente os dados do cartão, se o erro persistir, entre em contato com o banco emissor.|Transação não permitida. Houve um erro no processamento. Digite novamente os dados do cartão, se o erro persistir, entre em contato com o banco emissor.|Apenas 4 vezes em 16 dias.|
|BP|Transação não autorizada. Conta corrente inexistente.|Transação não autorizada. Não possível processar a transação por um erro relacionado ao cartão ou conta do portador. Solicite ao portador que entre em contato com o banco emissor.|Transação não autorizada. Não possível processar a transação por um erro relacionado ao cartão ou conta do portador. Entre em contato com o banco emissor.|Não|
|BP176|Transação não permitida.|Parceiro deve checar se o processo de integração foi concluído com sucesso.|Parceiro deve checar se o processo de integração foi concluído com sucesso.|---|
|BV|Transação não autorizada. Cartão vencido|Transação não autorizada. Cartão vencido.|Transação não autorizada. Refazer a transação confirmando os dados.|Não|
|CF|Transação não autorizada.C79:J79 Falha na validação dos dados.|Transação não autorizada. Falha na validação dos dados. Solicite ao portador que entre em contato com o banco emissor.|Transação não autorizada. Falha na validação dos dados. Entre em contato com o banco emissor.|Não|
|CG|Transação não autorizada. Falha na validação dos dados.|Transação não autorizada. Falha na validação dos dados. Solicite ao portador que entre em contato com o banco emissor.|Transação não autorizada. Falha na validação dos dados. Entre em contato com o banco emissor.|Não|
|DA|Transação não autorizada. Falha na validação dos dados.|Transação não autorizada. Falha na validação dos dados. Solicite ao portador que entre em contato com o banco emissor.|Transação não autorizada. Falha na validação dos dados. Entre em contato com o banco emissor.|Não|
|DF|Transação não permitida. Falha no cartão ou cartão inválido.|Transação não permitida. Falha no cartão ou cartão inválido. Solicite ao portador que digite novamente os dados do cartão, se o erro persistir, entre em contato com o banco|Transação não permitida. Falha no cartão ou cartão inválido. Digite novamente os dados do cartão, se o erro persistir, entre em contato com o banco|Apenas 4 vezes em 16 dias.|
|DM|Transação não autorizada. Limite excedido/sem saldo.|Transação não autorizada. Limite excedido/sem saldo.|Transação não autorizada. Entre em contato com seu banco emissor.|A partir do dia seguinte, apenas 4 vezes em 16 dias.|
|DQ|Transação não autorizada. Falha na validação dos dados.|Transação não autorizada. Falha na validação dos dados. Solicite ao portador que entre em contato com o banco emissor.|Transação não autorizada. Falha na validação dos dados. Entre em contato com o banco emissor.|Não|
|DS|Transação não permitida para o cartão|Transação não autorizada. Transação não permitida para o cartão.|Transação não autorizada. Entre em contato com seu banco emissor.|Apenas 4 vezes em 16 dias.|
|EB|Transação não autorizada. Limite diário excedido.|Transação não autorizada. Limite diário excedido. Solicite ao portador que entre em contato com seu banco emissor.|Transação não autorizada. Limite diário excedido. Entre em contato com seu banco emissor.|A partir do dia seguinte, apenas 4 vezes em 16 dias.|
|EE|Transação não permitida. Valor da parcela inferior ao mínimo permitido.|Transação não permitida. Valor da parcela inferior ao mínimo permitido. Não é permitido parcelas inferiores a R$ 5,00. Necessário rever calculo para parcelas.|Transação não permitida. O valor da parcela está abaixo do mínimo permitido. Entre em contato com a loja virtual.|Não|
|EK|Transação não permitida para o cartão|Transação não autorizada. Transação não permitida para o cartão.|Transação não autorizada. Entre em contato com seu banco emissor.|Apenas 4 vezes em 16 dias.|
|FA|Transação não autorizada.|Transação não autorizada AmEx.|Transação não autorizada. Entre em contato com seu banco emissor.|Não|
|FC|Transação não autorizada. Ligue Emissor|Transação não autorizada. Oriente o portador a entrar em contato com o banco emissor.|Transação não autorizada. Entre em contato com seu banco emissor.|Não|
|FD|Transação negada. Reter cartão condição especial|Transação não autorizada por regras do banco emissor.|Transação não autorizada. Entre em contato com seu banco emissor|Não|
|FE|Transação não autorizada. Divergencia na data de transação/pagamento.|Transação não autorizada. Data da transação ou data do primeiro pagamento inválida.|Transação não autorizada. Refazer a transação confirmando os dados.|Não|
|FF|Cancelamento OK|Transação de cancelamento autorizada com sucesso. ATENÇÂO: Esse retorno é para casos de cancelamentos e não para casos de autorizações.|Transação de cancelamento autorizada com sucesso|Não|
|FG|Transação não autorizada. Ligue AmEx 08007285090.|Transação não autorizada. Oriente o portador a entrar em contato com a Central de Atendimento AmEx.|Transação não autorizada. Entre em contato com a Central de Atendimento AmEx no telefone 08007285090|Não|
|GA|Aguarde Contato|Transação não autorizada. Referida pelo Lynx Online de forma preventiva.|Transação não autorizada. Entre em contato com o lojista.|Não|
|GD|Transação não permitida.|Transação não permitida. Entre em contato com a Cielo.|Transação não permitida. Entre em contato com a Cielo.|---|
|HJ|Transação não permitida. Código da operação inválido.|Transação não permitida. Código da operação Coban inválido.|Transação não permitida. Código da operação Coban inválido. Entre em contato com o lojista.|Não|
|IA|Transação não permitida. Indicador da operação inválido.|Transação não permitida. Indicador da operação Coban inválido.|Transação não permitida. Indicador da operação Coban inválido. Entre em contato com o lojista.|Não|
|JB|Transação não permitida. Valor da operação inválido.|Transação não permitida. Valor da operação Coban inválido.|Transação não permitida. Valor da operação Coban inválido. Entre em contato com o lojista.|Não|
|KA|Transação não permitida. Falha na validação dos dados.|Transação não permitida. Houve uma falha na validação dos dados. Solicite ao portador que reveja os dados e tente novamente. Se o erro persistir verifique a comunicação entre loja virtual e Cielo.|Transação não permitida. Houve uma falha na validação dos dados. reveja os dados informados e tente novamente. Se o erro persistir entre em contato com a Loja Virtual.|Não|
|KB|Transação não permitida. Selecionado a opção incorrente.|Transação não permitida. Selecionado a opção incorreta. Solicite ao portador que reveja os dados e tente novamente. Se o erro persistir deve ser verificado a comunicação entre loja virtual e Cielo.|Transação não permitida. Selecionado a opção incorreta. Tente novamente. Se o erro persistir entre em contato com a Loja Virtual.|Não|
|KE|Transação não autorizada. Falha na validação dos dados.|Transação não autorizada. Falha na validação dos dados. Opção selecionada não está habilitada. Verifique as opções disponíveis para o portador.|Transação não autorizada. Falha na validação dos dados. Opção selecionada não está habilitada. Entre em contato com a loja virtual.|Não|
|N7|Transação não autorizada. Código de segurança inválido.|Transação não autorizada. Código de segurança inválido. Oriente o portador corrigir os dados e tentar novamente.|Transação não autorizada. Reveja os dados e informe novamente.|Não|
|R1|Transação não autorizada. Cartão inadimplente (Do not honor).|Transação não autorizada. Não foi possível processar a transação. Questão relacionada a segurança, inadimplencia ou limite do portador.|Transação não autorizada. Entre em contato com seu banco emissor.|Apenas 4 vezes em 16 dias.|
|U3|Transação não permitida. Falha na validação dos dados.|Transação não permitida. Houve uma falha na validação dos dados. Solicite ao portador que reveja os dados e tente novamente. Se o erro persistir verifique a comunicação entre loja virtual e Cielo.|Transação não permitida. Houve uma falha na validação dos dados. reveja os dados informados e tente novamente. Se o erro persistir entre em contato com a Loja Virtual.|Não|
|GD|Transação não permitida|Transação não permitida|Transação não é possível ser processada no estabelecimento. Entre em contato com a Cielo para obter mais detalhes Transação|Não| 

### Códigos de Motivo de Retorno

|Reason Code|Reason Message|
|---|---|
|0|Successful|
|1|AffiliationNotFound|
|2|IssuficientFunds|
|3|CouldNotGetCreditCard|
|4|ConnectionWithAcquirerFailed|
|5|InvalidTransactionType|
|6|InvalidPaymentPlan|
|7|Denied|
|8|Scheduled|
|9|Waiting|
|10|Authenticated|
|11|NotAuthenticated|
|12|ProblemsWithCreditCard|
|13|CardCanceled|
|14|BlockedCreditCard|
|15|CardExpired|
|16|AbortedByFraud|
|17|CouldNotAntifraud|
|18|TryAgain|
|19|InvalidAmount|
|20|ProblemsWithIssuer|
|21|InvalidCardNumber|
|22|TimeOut|
|23|CartaoProtegidoIsNotEnabled|
|24|PaymentMethodIsNotEnabled|
|98|InvalidRequest|
|99|InternalError|

## Lista de Valores - Payment.Chargebacks[n].Status

|Valor|Descrição|
|:-|:-|
|Received|Chargeback recebido da adquirente|
|AcceptedByMerchant|Chargeback acatado pelo estabelecimento. Neste caso o estabelecimento entende que sofreu de fato um chargeback e não irá realizar a contestação|
|ContestedByMerchant|Chargeback contestado pelo estabelecimento. Neste caso o estabelecimento enviou os documentos necessários para tentar reverter o chargeback|
