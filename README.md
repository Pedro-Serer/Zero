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

  - ### 🔹 Contracts (/contracts)
    
    Cada funcionalidade do sistema (ex: Usuário, Produto, Pedido) deve ter seu contrato CRUD separado.
    
    Esses contratos definem claramente os métodos esperados para qualquer operação com o banco de dados, garantindo que todas as implementações sigam um padrão único e previsível.
    
    **Boas práticas para contracts:**
    
    - Defina interfaces claras, com assinaturas e tipos bem especificados para facilitar implementação e manutenção.
    - Inclua regras básicas de validação nos contratos, como tipos e formatos esperados.
    - Documente cada método com descrições, parâmetros e tipos de retorno (ex: PHPDoc, JSDoc).
    - Mantenha os contratos independentes da linguagem ou framework, para garantir portabilidade e reuso.
    - Use os contratos como contrato firme entre a camada de dados e as regras de negócio — isso evita acoplamento e facilita testes.
    
    Essa estrutura ajuda a garantir que toda operação de leitura, escrita, atualização e exclusão no banco siga um padrão único, tornando o sistema mais previsível, legível e fácil de escalar.


  - ### 🔹 Utils (`/utils`)

    - Pasta dedicada à centralização de funções reutilizáveis, helpers, constantes e utilitários que dão suporte transversal a todo o backend.
    - Componentes essenciais a incluir:
      - **Classe de tratamento unificado de erros** (`ErrorHandler`):  
        - Padroniza a captura, registro e retorno de erros, facilitando o debug e a manutenção.  
        - Garante mensagens claras e consistentes para toda a aplicação.
      - **Funções de sanitização e validação genérica:**  
        - Métodos para limpar e validar dados de entrada, protegendo contra ataques comuns (ex: SQL Injection, XSS).  
        - Garantem que os dados estejam corretos e seguros antes de serem processados.
      - **Helpers e constantes:**  
        - Funções auxiliares para formatação, manipulação de dados e regras simples que são utilizadas em diversos pontos do sistema.  
        - Constantes configuráveis que evitam hardcoding e facilitam alterações globais.
    - **Documentação clara e objetiva:**  
      - Cada utilitário deve ser bem documentado para que qualquer desenvolvedor entenda rapidamente sua função e uso.  
      - Isso facilita a manutenção, o onboard de novos devs e reduz a duplicação de código.
    - A pasta `/utils` é o coração do backend reutilizável, promovendo organização, segurança e consistência em todo o projeto.


  - ### 🔹 Workers (/worker)
  
    Cada contrato tem um worker correspondente.
    
    O worker é responsável por **implementar as chamadas ao banco de dados** através dos contratos, além de aplicar as regras de negócio e orquestrar os dados que serão enviados para as views ou APIs.
    
    **Boas práticas para workers:**
    
    - Mantenha a lógica de negócio concentrada nos workers — as views devem ser “burras” e apenas exibir dados.
    - Use os workers para validar, transformar e processar os dados antes de enviá-los para o front-end ou APIs.
    - Implemente o consumo dos contratos CRUD para garantir consistência no acesso aos dados.
    - Separe a lógica de integração com outros sistemas, se necessário, utilizando os workers para orquestrar esses fluxos.
    - Documente claramente cada método para facilitar manutenção e testes.
    
    Exemplo prático: `UserWorker` utiliza o `UserCRUD` (definido no contrato) para buscar usuários, aplicar regras como filtros ou permissões, e finalmente enviar os dados para o front-end ou responder a uma requisição API.
    
    Essa separação torna o sistema mais modular, fácil de entender e de escalar, mantendo a responsabilidade única para cada componente.


- As pastas abaixo são opcionais, devem ser incrementadas somente se necessário:
  
  - ### 🔹 Receiver (`/receiver`)
  
    - Pasta responsável por **centralizar a entrada de dados externos** no sistema, incluindo integrações via:
      - Webhooks
      - APIs de terceiros
      - Sockets de rede
      - Qualquer outro tipo de comunicação externa
    
    - **Função principal:**  
      - **Filtrar, validar e autenticar** tudo que vem de fora **antes** de permitir qualquer ação no core do sistema.
    
    - **Boas práticas:**
      - **Delegar a lógica de negócio para os Workers:**  
        - Os scripts em `/receiver` devem ser simples e focados em segurança e roteamento.  
        - Após validação, encaminham os dados para o worker responsável processar corretamente.
      - **Registrar logs detalhados:**  
        - Toda requisição recebida deve ser registrada com dados como:
          - Payload original
          - Timestamp
          - IP e headers
          - Status do processamento
        - Isso garante rastreabilidade e facilita o diagnóstico em caso de falha ou ataque.
      - **Segurança em primeiro lugar:**  
        - Verificar assinaturas, tokens ou chaves de autenticação antes de qualquer execução.
        - Rejeitar silenciosamente entradas inválidas ou suspeitas.
    
    - O `/receiver` atua como **guarda de fronteira** do sistema: tudo passa por aqui, nada entra sem ser verificado.


  - ### 🔹 API (`/api`)
  
    - Contém as APIs de cada funcionalidade organizadas em subpastas nomeadas conforme a funcionalidade (ex: `pagamentos`, `usuarios`, `relatorios`).
    - Cada subpasta deve conter dois arquivos principais:
      - **Arquivo de rotas:** Responsável por definir as rotas/endpoints da API. Deve começar com o prefixo `rotas-` seguido do nome da funcionalidade.  
        *Exemplo:* `rotas-pagamentos.php`
      - **Arquivo do endpoint:** Implementa a lógica que responde às requisições das rotas definidas. Deve começar com o prefixo `api-` seguido do nome da funcionalidade.  
        *Exemplo:* `api-pagamentos.php`
    - Essa separação deixa claro onde as rotas são definidas e onde a lógica das respostas está implementada, facilitando manutenção e extensão.
    - As APIs são responsáveis por receber requisições, validar dados, chamar os workers para executar regras de negócio e retornar as respostas formatadas (ex: JSON).
    - Esta camada não deve conter regras de negócio complexas, apenas coordenação e comunicação entre o front-end e o backend.


  - ### 🔹 Screens (`/screens`)
  
    - Responsável por conter as telas de aplicações **DESKTOP** ou **MOBILE**.
    - Cada arquivo representa uma tela ou componente de exibição isolado.
    - A raiz da pasta deve conter um arquivo `app` que funciona como ponto de entrada da interface, chamando a tela principal do sistema.
    
    - #### 🧠 Filosofia:
      - **Views são burras** — elas apenas mostram dados e capturam ações do usuário.
      - Nenhuma regra de negócio deve ser implementada aqui.
      
    - #### 🛠️ Boas práticas:
    
      - **Exibição pura:**
        - Os arquivos em `/screens` devem se limitar à camada visual.
        - Estilização, componentes de UI, e navegação entre telas podem ser controlados aqui.
      
      - **API First:**
        - Toda comunicação com o backend deve ser feita via **chamadas assíncronas** para as rotas da API (`/api`).
        - A tela nunca deve acessar diretamente contratos ou workers.
      
      - **Validação leve:**
        - Inputs podem ser validados localmente para melhorar a UX (ex: campos obrigatórios, formatos de e-mail).
        - Mas **toda validação real e regra de segurança** deve ser feita no backend (worker).
      
      - **Organização clara:**
        - Nomes dos arquivos e pastas devem refletir claramente o propósito da tela (ex: `TelaLogin`, `DashboardPagamentos`, `ResumoFatura`).



### 🛠️ miscellaneous

- As pastas abaixo são opcionais e podem ser usadas tanto para o backend como para o frontend:

  - ### 🔹 ASSETS (`/assets`)

    - Contém ativos estáticos do sistema como imagens, gifs, vídeos e etc.
    - Cada arquivo estático deverá estar contido dentro de uma subpasta com o nome a qual eles representam.
    - Arquivos CSS e JS também estarão dentro dessa pasta, é possível organizar o JS em classes de acordo com suas funcionalidades.

    - **Detalhes**:
      - 🔁 As **views** e os **assets** formam juntos a camada de apresentação.  
      - 🔒 Eles **nunca acessam o banco diretamente** — recebem dados prontos da API.  
      - 📐 Simples, previsível e escalável.

  - ### 🔹 ROOT (`/`)
    - Se for um app **MOBILE** ou **DESKTOP**, então o único arquivo na raiz deverá se chamar "app", seguido da extensão da linguagem.
    - Deverá conter apenas arquivos HTML (views) do sistema.
    - O HTML tem que ser o mais puro possível evitando CSS inline, PHP, JAVA, etc.

<br>

> ⚠️ **NÃO É OBRIGATÓRIO USAR TODAS AS PASTAS EM SEU PROJETO, USE SOMENTE SE PRECISAR**

<br>

## 🗃 Zero modular

Outra grande habilidade da arquitetura Zero é ser modular, pois basta criar módulos (ou domínios) e aplicar o Zero dentro de 
cada um deles. Isso é perfeito para sistemas grandes e complexos, porque além de organizar, separar as reponsabilidades, ela
dá bastante flexibilidade e simplicidade de compreensão. Isso melhora a escalabilidade e uma futura manutenção. 

A Zero modular pode ser reutilizada em qualquer outra parte do projeto, permitindo também, que cada módulo se torne um microserviço, 
se necessário.

A Zero modular, permite que desenvolvedores ou times de squads diferentes possam trabalhar paralelamente em
módulos específicos de seu interesse, sem afetar o sistema como um todo e sem conflitos entre si, que é essencial para o desenvolvimento 
moderno e em conjunto de modelos de IA.

```shell
.

(root)
├── Usuarios/
│   └── contracts/                     # Cria o CRUD das operações básicas de usuários
│   └── worker/                        # Aplica as regras de gestão de usuários
│   └── utils/                         # Utilitários exclusivos para a gestão de usuários
│   └── api/                           # Faz chama dos recuros de Pagamentos e Relatórios
│   └── receiver/                      # Recebe as solicitações de Pagamentos e Relatórios
├── Pagamentos/
│   └── contracts/                     # Cria o CRUD das operações básicas de pagamentos
│   └── worker/                        # Implementação da regra de negócios para os pagamentos
│   └── utils/                         # Utilitários exclusivos para o setor de pagamentos
│   └── api/                           # Fornece os recursos de Pagamentos para o Usuário
│   └── receiver/                      # Recebe chamadas de API e WebHook para alimentar o sistema
├── Relatórios/
│   └── contracts/                     # Cria o CRUD das operações básicas de relatórios
│   └── worker/                        # Implementa os contratos para alimentar a API
│   └── utils/                         # Utilitários exclusivos para montar os relatórios
│   └── api/                           # Chamam os recursos para gerar relatórios
├── Frontend/
│   └── assets/                        # Arquivos e estilos para alimentar o front-end
│   ├── usuários.html                  # Página de Usuários
│   ├── pagamentos.html                # Página de Pagamentos
├── app.html                           # Arquivo base do sistema

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
