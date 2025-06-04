<p align="center">
  <img src="https://www.pedroserer.com.br/logo_zero.png" alt="logo_zero" />
</p>

<br>
<br>

# Zero 🧠🚀

**Zero** é uma arquitetura de software minimalista, modular e extremamente simples. Foi pensada para ser compreendida em minutos e dar agilidade total ao desenvolvimento. O fluxo é direto, controlado, e sem camadas inúteis.  
Sem mágica. Sem gambiarra. Só o essencial, tudo organizado e funcional.

Desenvolvida e testada extensivamente em ambientes web com PHP no back-end, a arquitetura é compatível com qualquer linguagem ou stack que respeite o fluxo de dados.

Antes de vir para o lado negro da força, leia atentamente o manifesto, para compreender melhor a motivação dessa arquitetura.

<br>

# 🌀 Filosofia

A filosofia da arquitetura **Zero** é baseada em três pilares: simplicidade, reusabilidade e performance. Quanto menos dependências externas, melhor.

Aqui, os dados são os protagonistas. Todo o fluxo da aplicação gira em torno de como os dados entram, são processados e saem. Nada além disso.

A lógica é simples: praticamente todo processo empresarial ou problema real pode ser modelado como um CRUD. A computação resolve problemas porque abstrai esses processos. Então, se tudo é um CRUD, **a solução pode (e deve) ser simples**.

O fluxo de dados funciona da seguinte forma: **CRUD → CONTRACTS → WORKERS → VIEWS/API**.

<ul>
  <li><p><b>CRUD: </b> Cada funcionalidade nasce como um CRUD básico. Pode crescer, mas sem virar um monstro. Nada de violar responsabilidade do arquivo.</p></li>
  <li><p><b>CONTRACTS: </b> São as classes que lidam diretamente com o BD. Isoladas, focadas. Um CRUD por contrato. Se precisar, você estende.</p></li>
  <li><p><b>WORKERS: </b> Capturam os dados dos contratos, aplicam regras de negócio e preparam a saída.</p></li>
  <li><p><b>VIEWS/API: </b> Aqui termina o ciclo. É só a apresentação: seja para humanos (HTML) ou máquinas (JSON, XML, etc). Nada de lógica aqui.</p></li>
</ul>

<br>

## 📁 Estrutura de Pastas

| PASTAS     | DESCRIÇÃO                                                                              |
| ---------- | -------------------------------------------------------------------------------------- |
| /assets    | Arquivos de view (JS/CSS/HTML) em apps web e documentos estáticos como imagens, vídeos |
| /contracts | Contratos de CRUD (1 por funcionalidade)                                               |
| /utils     | Classes utilitárias (tratamento de erros, queries, constantes)                         |
| /workers   | Regras de negócio e orquestração de dados (consome contrato)                           |
| /receiver  | Pasta para recebimento de hooks de outros sistemas ou API's                            |
| /api       | Endpoints públicos ou internos da aplicação (REST, JSON, etc).                         |
| /screens   | Para views em apps mobile ou desktop                                                   |
| /utils     | Classes utilitárias (tratamento de erros, queries, constantes)                         |
| /(root)    | Apenas a view principal (mobile e desktop), ou um conjunto de views no caso da web     |

<br>

## 🧩 Componentes da Arquitetura

### 🛠️ Backend (Core da Lógica de Negócio)

- As pastas abaixo são **obrigatórias** e formam o núcleo do backend na arquitetura **Zero**:

  - ### 🔹 Contracts (`/contracts`)

    - Cada funcionalidade do sistema (ex: Usuário, Produto, Pedido) tem **sua contrato CRUD** separada.
    - Esses contratos definem os métodos esperados para qualquer tipo de operação com o banco.
    - São independentes da linguagem. Em TypeScript, Dart, Java... seguem o mesmo princípio.

  - ### 🔹 Utils (`/utils`)

    - Contém utilitários centrais que apoiam toda a camada de backend:
      - `ErrorHandler` → Classe abstrata que padroniza erros e mensagens de exceção.
      - `QueryProvider` → Armazena queries SQL como **constantes** ou **variáveis dinâmicas**, dependendo da linguagem.
      - `LogicHelper` → Funções para regra de negócio, máscaras, segurança, etc. (máx. ~10 funções).

  - ### 🔹 Workers (`/worker`)
    - Cada contrato tem um worker correspondente.
    - Ele **implementa** as chamadas para o banco via classe e alimenta **views ou APIs**.
    - Exemplo: `UserWorker` chama `UserCRUD` e envia dados para o front ou resposta de API.

- As pastas abaixo são opcionais, devem ser incrementadas somente se necessário:

  - ### 🔹 Receiver (`/receiver`)

    - Arquivos de logs de webhook.
    - Arquivo de logs de recebimento de dados de API's.
    - Arquivo de logs de sockets de redes ou qualquer outro tipo de comunicação de recebimento de dados.

  - ### 🔹 API (`/api`)

    - Contém as API's de cada funcionalidade separadas em pastas com o nome da funcionalidade.
    - Dentro das subpastas deverá ter a implemetação do arquivo de rotas e um de endpoint.
    - os arquivos de rotas devem começar com o nome "rotas- {nome_arquivo}" e os arquivos de api, "api- {nome_arquivo}"

  - ### 🔹 Screens (`/screens`)
    - Contém as telas para aplicações **DESKTOP** ou **MOBILE**:
      - Deverá conter apenas as classes responsáveis pela apresentação dos dados para o usuário.
      - Para esse modelo, deverá ter um arquivo chamado "app" na raiz, que chamará a tela principal do sistema.

### 🛠️ miscellaneous

- As pastas abaixo são opcionais e podem ser usadas tanto para o backend como para o frontend:

  - ### 🔹 ASSETS (`/assets`)

    - Contém ativos estáticos do sistema como imagens, gifs, vídeos e etc.
    - Cada arquivo estático deverá estar contido dentro de uma subpasta com o nome a qual eles representam.
    - Arquivos CSS e JS também estarão dentro dessa pasta, é possível organizar o JS em classes de acordo com suas funcionalidades.

  - ### 🔹 ROOT (`/`)
    - Se for um app **MOBILE** ou **DESKTOP**, então o único arquivo na raiz deverá se chamar "app", seguido da extensão da linguagem.
    - Deverá conter apenas arquivos HTML (views) do sistema.
    - O HTML tem que ser o mais puro possível evitando CSS inline, PHP, JAVA, etc.

<br>

> ⚠️ **NÃO É OBRIGATÓRIO USAR TODAS AS PASTAS EM SEU PROJETO, USE SOMENTE SE PRECISAR**

<br>

## 🗃 Zero modular

Zero consegue se adaptar a uma arquitetura modular: basta criar seus módulos ou domínios e aplicar o Zero em cada um deles. Com isso,
além da facilidade e simplicidade que o Zero oferece no desenvolvimento do sistema, a arquitetura modular melhora a escalabilidade, a
manutenibilidade e a reutilização — permitindo, por exemplo, que cada módulo se torne um microserviço, caso necessário. Além disso,
cada desenvolvedor, ou times de squads diferentes, pode trabalhar paralelamente em módulos específicos de seu interesse, sem afetar o
sistema como um todo e sem conflitos entre si.

<br>

```shell
.
(root)
├── usuarios
├──── assets
├──── contracts
├──── worker
├──── utils
├──── api
├── pagamentos
├──── assets
├──── contracts
├──── worker
├──── utils
├──── api
├── relatorios
├──── assets
├──── contracts
├──── worker
├──── utils
├──── api

```

<br>

## 🚀 Escalabilidade

Essa arquitetura suporta sistemas mais complexos, que contenham pedidos, estoque, comissão, notificações, suporte e etc, pois ela trata tudo como um contrato, por exemplo:

- Estoque é um CRUD;
- Comissão é um CRUD;
- Notificações é um CRUD;
- Suporte é também um CRUD.

Basicamente qualquer coisa é um CRUD, sendo que cada funcionalidade vai ter um contrato (CRUD), um com os métodos que asseguram o funcionamento correto do sistema, uma classe de rotas e seu próprio endpoint. Se for necessário atomicidade, então cabe o desenvolvedor escolher qual arquivo será responsável por controlar a atomicidade, por exemplo, imagine o seguinte fluxo: **CRIAR PEDIDOS → DISPARAR FATURAMENTO → ATUALIZAR O ESTOQUE → GERAR COMISSÃO PARA O VENDEDOR → ENVIAR UM EMAIL**.

Se for necessário dar um rollback, é possível fazer de várias maneiras simples, sendo uma, que o sistema só valida tudo no final de todas as etapas, num arquivo que envia o email. Então o sistema cria o pedido e o insere no banco, que não teria problema se o pedido não fosse concluído já basta ter uma flag nesse pedido, sendo bom até para análises de marketing. Se ele conseguir atualizar o estoque, ele dispara o faturamento e se tudo ocorrer bem com o diparo, ele gera a comissão e envia o email.

Isso reforça a escalabilidade da arquitetura, deixando a cabeça do desenvolvedor mais tranquila em relação a escolha da arquitetura, uma coisa a menos para se preocupar.

<br>

## 💡 Por que usar o Zero?

- ✅ Leitura fácil (até seu gato entende 🐱).
- ⚡ Desenvolvimento absurdamente rápido (especialmente com IAs).
- 🔐 Segurança fácil de controlar, porque nada fica oculto.
- 🧠 Arquitetura leve que cabe na cabeça.
- 🧰 Fácil manutenção e extensibilidade.
- 🔄 Baixo uso de memória e CPU.
- 🔧 Serve para qualquer projeto, do mais simples ao corporativo.

<br>

## 👨‍🔧 Exemplo prático

Imagine que você tem um sistema web que processa pagamentos, então a estrutura de arquivos é a seguinte:

```shell
.

├── assets
│   └── _css
│   └── _imagens
│   └── _javascript
├── contracts
│   └── contracts-bd.php
│   └── contracts-usuarios.php
│   └── contracts-pagamentos.php
│   └── contracts-csv.php
├── worker
│   └── worker-usuarios.php
│   └── worker-pagamentos.php
│   └── worker-csv.php
├── utils
│   └── erros.php
│   └── funcoes.php
│   └── queries.php
├── api
│   └── pagamentos
│       └── rotas-pagamentos.php
│       └── api-pagamentos.php
├── login.html
├── dashboard-pagamentos.html

```

### Contracts Pagamentos.php

```php
require_once 'contracts-bd.php';
require_once '../utils/queries.php';

/**
 * @author Pedro Stein Serer
 * (14/05/2025)
 *
 * @version 1.0.1
 * @copyright Empresa Exemplo
 *
 * Arquivo de definição das operações de pagamentos.
 */

interface PagamentosInterface {
    public function criarTransacao (array $dados): array;
    public function atualizarTransacao(int $id, array $dados): array;
    public function obterTransacao(int $id): array;
    public function deletarTransacao(int $id): array;
}

class Pagamentos implements PagamentosInterface {
    private BD $bancoDeDados;

    // Inicia a classe com uma conexão com o banco de dados.
    function __construct()
    {
        $this->bancoDeDados = new BD;
    }

    /**
     *Descrição da função.
     *
     * @param Descrição dos parâmetros
     *
     * @return descrição do retorno
     */

    public function criarTransacao (array $dados): array
    {
        // Lógica para inserir a transação no banco de dados
    }

    /**
     *Descrição da função.
     *
     * @param Descrição dos parâmetros
     *
     * @return descrição do retorno
     */

    public function atualizarTransacao(int $id, array $dados): array
    {
        // Lógica para atualizar transação no banco de dados
    }

    /**
     *Descrição da função.
     *
     * @param Descrição dos parâmetros
     *
     * @return descrição do retorno
     */

    public function obterTransacao(int $id): array
    {
        // Lógica para obter transação do banco de dados
    }

    /**
     *Descrição da função.
     *
     * @param Descrição dos parâmetros
     *
     * @return descrição do retorno
     */

    public function deletarTransacao(int $id): array
    {
        // Lógica para deletar transação no banco de dados
    }
}

```

---

### WorkerPagamentos.php

```php

require_once '../classes/contracts-pagamentos.php';
require_once '../utils/funcoes.php';

/**
 * @author Pedro Stein Serer
 * (14/05/2025)
 *
 * @version 1.0.1
 * @copyright Empresa Exemplo
 *
 * Arquivo que define os métodos para a lógica de pagamentos.
 */

class WorkerPagamentos
{

    private Pagamentos $pagamentos;

    public function __construct ()
    {
        $this->pagamentos = new Pagamentos;
    }

    /**
     *Descrição da função.
     *
     * @param Descrição dos parâmetros
     *
     * @return descrição do retorno
     */

    public function processarPagamento(array $dadosPagamento): array|string
    {
        // Etapa 1: Criar a transação
        $transacao = $this->pagamentos->criarTransacao($dadosPagamento);

        // Etapa 2: Aplicar descontos
        $transacao = $this->aplicarDesconto($transacao);

        // Etapa 3: Calcular impostos
        $transacao = $this->calcularImposto($transacao);

        // Etapa 4: Gerar fatura
        $fatura = $this->gerarFatura($transacao);

        // Etapa 5: Marcar como pago
        $this->marcarComoPago($transacao, $fatura);

        return $fatura; // Retorna a fatura gerada
    }

    /**
     *Descrição da função.
     *
     * @param Descrição dos parâmetros
     *
     * @return descrição do retorno
     */

    private function aplicarDesconto(array $transacao): array
    {
        // Lógica para aplicar desconto
        return $transacao; // Retorna transação com desconto aplicado
    }

    /**
     *Descrição da função.
     *
     * @param Descrição dos parâmetros
     *
     * @return descrição do retorno
     */

    private function calcularImposto(array $transacao): array
    {
        // Lógica para calcular imposto
        return $transacao; // Retorna transação com imposto calculado
    }

    /**
     *Descrição da função.
     *
     * @param Descrição dos parâmetros
     *
     * @return descrição do retorno
     */

    private function gerarFatura(array $transacao): int
    {
        // Lógica para gerar fatura
        return $transacao; // Retorna a fatura gerada
    }

    /**
     *Descrição da função.
     *
     * @param Descrição dos parâmetros
     *
     * @return descrição do retorno
     */

    private function marcarComoPago(array $transacao, int $fatura): void
    {
        // Lógica para marcar transação como paga
    }
}

```

---

### rotas-pagamentos.php

```php
<?php
    require_once 'worker-pagamentos.php';

    /**
     * @author Pedro Stein Serer
     * (14/05/2025)
     *
     * @version 1.0.1
     * @copyright Empresa Exemplo
     *
     * Arquivo com os métodos de processamento HTTP.
     */

    class RotasPagamentos
    {
        private PagamentosWorker $pagamentosWorker;

        public function __construct() {
            $this->pagamentosWorker = new PagamentosWorker;
        }

        /**
         *Descrição da função.
         *
         * @param Descrição dos parâmetros
         *
         * @return descrição do retorno
         */

        public function metodoGet(string &$resposta): void {
            // Lógica de processamento do método GET
        }

        /**
         *Descrição da função.
         *
         * @param Descrição dos parâmetros
         *
         * @return descrição do retorno
         */

        public function metodoPost(string &$resposta): void {
            // Lógica de processamento do método POST
        }

        /**
         *Descrição da função.
         *
         * @param Descrição dos parâmetros
         *
         * @return descrição do retorno
         */

        public function metodoPut(string &$resposta): void {
            // Lógica de processamento do método PUT
        }

        /**
         *Descrição da função.
         *
         * @param Descrição dos parâmetros
         *
         * @return descrição do retorno
         */

        public function metodoDelete(string &$resposta): void {
            // Lógica de processamento do método DELETE
        }
    }

```

---

### api-pagamentos.php

```php
<?php
    require_once 'rotas-pagamentos.php';

    /**
     * @author Pedro Stein Serer
     * (14/05/2025)
     *
     * @version 1.0.1
     * @copyright Empresa Exemplo
     *
     * Arquivo do Endpoint com as operações de pagamentos.
     */

    header('Content-type: application/json');

    error_reporting(0);

    $metodoRequisicao = $_SERVER['REQUEST_METHOD'];
    $RotasPagamentos = new RotasPagamentos;
    $resposta = '';

    switch ($metodoRequisicao) {
        case 'GET':
            // Método GET: Recupera os dados de um ou mais pagamentos.
            $RotasPagamentos->metodoGet($resposta);
            break;

        case 'POST':
            // Método POST: Cria uma nova transação.
            $RotasPagamentos->metodoPost($resposta);
            break;

        case 'PUT':
            // Método PUT: Atualiza os dados de uma fatura.
            $RotasPagamentos->metodoPut($resposta);
            break;

        case 'DELETE':
            // Método DELETE: Deleta uma fatura.
            $RotasPagamentos->metodoDelete($resposta);
            break;

        default:
            $resposta = "Método não aceito pelo servidor";
            break;
    }

    echo json_encode($resposta, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE);

```

## 🔐 Licenciamento e Direitos

**Zero** é um conceito original criado por [Pedro Serer](https://github.com/Pedro-Serer), e é livre para uso pessoal ou comercial.

Porém, a **atribuição é obrigatória**. Se usar, mencione a origem. Se for derivar, não use o nome "Zero" sem permissão.

<br>

## 📢 Quer contribuir ou adaptar?

Sinta-se livre para sugerir melhorias ou adaptar para seu stack preferido. Só não esquece de manter o espírito: **Zero = Zero Enrolação**.

---

> Simples. Direto. Performático. Do jeito que o código deveria ser.
