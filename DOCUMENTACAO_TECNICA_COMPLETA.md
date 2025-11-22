# 📘 DOCUMENTAÇÃO TÉCNICA COMPLETA - RESTIFY

## Sistema de Serviços Digitais para Restaurantes

---

## 📑 ÍNDICE

1. [Visão Geral do Sistema](#1-visão-geral-do-sistema)
2. [Arquitetura Completa](#2-arquitetura-completa)
3. [Estrutura de Diretórios Detalhada](#3-estrutura-de-diretórios-detalhada)
4. [Sistema de Rotas](#4-sistema-de-rotas)
5. [Controllers - Camada de Controle](#5-controllers---camada-de-controle)
6. [Models - Camada de Dados](#6-models---camada-de-dados)
7. [Views - Camada de Apresentação](#7-views---camada-de-apresentação)
8. [Services - Lógica de Negócio](#8-services---lógica-de-negócio)
9. [Repositories - Persistência](#9-repositories---persistência)
10. [Banco de Dados](#10-banco-de-dados)
11. [Sistema de Autenticação](#11-sistema-de-autenticação)
12. [Sistema de Carrinho](#12-sistema-de-carrinho)
13. [Sistema de Pagamentos](#13-sistema-de-pagamentos)
14. [Frontend e JavaScript](#14-frontend-e-javascript)
15. [Sistema de Internacionalização](#15-sistema-de-internacionalização)
16. [Design System e Paleta de Cores](#16-design-system-e-paleta-de-cores)
17. [Fluxo Completo do Sistema](#17-fluxo-completo-do-sistema)
18. [Conclusão e Próximos Passos](#18-=conclusão-e-próximos-passos)

---

## 1. VISÃO GERAL DO SISTEMA

### 1.1 Descrição
Sistema web desenvolvido em PHP puro com arquitetura MVC para oferecer serviços digitais para restaurantes.

### 1.2 Funcionalidades Principais
```
┌─────────────────────────────────────────────────────────────┐
│                    RESTIFY - FUNCIONALIDADES                │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  👤 AUTENTICAÇÃO                                            │
│     ├─ Login de Restaurantes                               │
│     ├─ Login de Administradores                            │
│     ├─ Registro de Novos Restaurantes                      │
│     └─ Logout com Destruição de Sessão                     │
│                                                             │
│  🛒 CARRINHO DE COMPRAS                                     │
│     ├─ Adicionar Serviços                                  │
│     ├─ Remover Serviços                                    │
│     ├─ Visualizar Total                                    │
│     └─ Checkout                                            │
│                                                             │
│  💳 PAGAMENTOS (Efí Bank)                                   │
│     ├─ PIX (QR Code + Copia e Cola)                        │
│     ├─ Cartão de Crédito                                   │
│     ├─ Boleto Bancário                                     │
│     └─ Carnê (Parcelamento)                                │
│                                                             │
│  📊 DASHBOARDS                                              │
│     ├─ Dashboard Administrativo                            │
│     └─ Dashboard do Restaurante                            │
│                                                             │
│  💬 CHAT EM TEMPO REAL                                      │
│     ├─ Chat Restaurante ↔ Admin                            │
│     └─ Polling a cada 3 segundos                           │
│                                                             │
│  🌍 INTERNACIONALIZAÇÃO                                     │
│     ├─ Português (PT-BR)                                   │
│     ├─ Inglês (EN)                                         │
│     └─ Espanhol (ES)                                       │
│                                                             │
│  🎨 TEMAS                                                   │
│     ├─ Tema Claro                                          │
│     └─ Tema Escuro                                         │
│                                                             │
│  📤 EXPORTAÇÃO                                              │
│     ├─ Exportar Pedidos (CSV)                              │
│     └─ Exportar Restaurantes (CSV)                         │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 1.3 Tecnologias Utilizadas
```
┌──────────────────────────────────────────────────────────┐
│ STACK TECNOLÓGICA                                        │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  Backend:                                                │
│    • PHP 7.4+ (Orientação a Objetos)                    │
│    • SQLite (Banco de Dados)                            │
│    • Composer (Gerenciador de Dependências)             │
│                                                          │
│  Frontend:                                               │
│    • HTML5                                               │
│    • CSS3 (Design Responsivo)                           │
│    • JavaScript Vanilla (ES6+)                          │
│                                                          │
│  Arquitetura:                                            │
│    • MVC (Model-View-Controller)                        │
│    • Repository Pattern                                 │
│    • Factory Pattern                                    │
│    • Observer Pattern                                   │
│    • Strategy Pattern                                   │
│    • Singleton Pattern                                  │
│                                                          │
│  Integrações:                                            │
│    • Efí Bank SDK (Pagamentos)                          │
│                                                          │
│  Servidor:                                               │
│    • Apache/Nginx                                        │
│    • mod_rewrite (URL amigáveis)                        │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

## 2. ARQUITETURA COMPLETA

### 2.1 Diagrama de Arquitetura MVC
```
┌─────────────────────────────────────────────────────────────────────┐
│                         ARQUITETURA MVC                             │
└─────────────────────────────────────────────────────────────────────┘

                            ┌──────────────┐
                            │   USUÁRIO    │
                            │  (Browser)   │
                            └──────┬───────┘
                                   │
                                   │ HTTP Request
                                   ▼
                    ┌──────────────────────────────┐
                    │      public/index.php        │
                    │   (Front Controller)         │
                    │  • Roteamento                │
                    │  • Autoload                  │
                    │  • Inicialização             │
                    └──────────┬───────────────────┘
                               │
                               │ Route Match
                               ▼
                    ┌──────────────────────────────┐
                    │       CONTROLLERS            │
                    │  ┌────────────────────────┐  │
                    │  │  HomeController        │  │
                    │  │  AuthController        │  │
                    │  │  CartController        │  │
                    │  │  PaymentController     │  │
                    │  │  AdminController       │  │
                    │  │  RestaurantController  │  │
                    │  └────────┬───────────────┘  │
                    └───────────┼───────────────────┘
                                │
                ┌───────────────┼───────────────┐
                │               │               │
                ▼               ▼               ▼
        ┌──────────┐    ┌──────────┐   ┌──────────┐
        │ SERVICES │    │  MODELS  │   │  VIEWS   │
        │          │    │          │   │          │
        │ Auth     │◄───┤Restaurant│   │ home.php │
        │ Cart     │    │ Service  │   │ login.php│
        │ Payment  │    │ Order    │   │ cart.php │
        │ Export   │    │ Message  │   │ admin/   │
        │          │    │          │   │ payment/ │
        └────┬─────┘    └──────────┘   └──────────┘
             │
             │ Data Access
             ▼
    ┌─────────────────┐
    │  REPOSITORIES   │
    │                 │
    │ Restaurant      │
    │ Service         │
    │ Order           │
    │ Message         │
    └────────┬────────┘
             │
             │ SQL Queries
             ▼
    ┌─────────────────┐
    │    DATABASE     │
    │   (SQLite)      │
    │                 │
    │ • restaurants   │
    │ • services      │
    │ • orders        │
    │ • order_items   │
    │ • messages      │
    └─────────────────┘
```

### 2.2 Fluxo de Requisição Completo
```
┌────────────────────────────────────────────────────────────────────┐
│                    FLUXO DE REQUISIÇÃO HTTP                        │
└────────────────────────────────────────────────────────────────────┘

1. ENTRADA
   ┌─────────────────────────────────────────────────────────┐
   │ Usuário acessa: http://localhost/RestifyApp/public/cart │
   └─────────────────────────────────────────────────────────┘
                              ↓
2. SERVIDOR WEB (Apache)
   ┌─────────────────────────────────────────────────────────┐
   │ .htaccess redireciona para index.php                    │
   │ RewriteRule ^(.*)$ index.php [QSA,L]                    │
   └─────────────────────────────────────────────────────────┘
                              ↓
3. FRONT CONTROLLER (index.php)
   ┌─────────────────────────────────────────────────────────┐
   │ • Carrega config.php                                    │
   │ • Carrega i18n.php                                      │
   │ • Inicializa sessão                                     │
   │ • Parse da URL: "cart"                                  │
   │ • Busca rota no array $routes                           │
   └─────────────────────────────────────────────────────────┘
                              ↓
4. ROTEAMENTO
   ┌─────────────────────────────────────────────────────────┐
   │ Rota encontrada: ['CartController', 'index']            │
   │ • Instancia CartController                              │
   │ • Chama método index()                                  │
   └─────────────────────────────────────────────────────────┘
                              ↓
5. CONTROLLER (CartController)
   ┌─────────────────────────────────────────────────────────┐
   │ public function index() {                               │
   │   • Instancia CartService                               │
   │   • Busca itens do carrinho                             │
   │   • Calcula total                                       │
   │   • Prepara dados para view                             │
   │   • Inclui view: cart/index.php                         │
   │ }                                                        │
   └─────────────────────────────────────────────────────────┘
                              ↓
6. SERVICE (CartService)
   ┌─────────────────────────────────────────────────────────┐
   │ • getItems() - Retorna $_SESSION['cart']                │
   │ • getTotal() - Calcula soma dos preços                  │
   │ • Usa ServiceRepository para buscar dados               │
   └─────────────────────────────────────────────────────────┘
                              ↓
7. REPOSITORY (ServiceRepository)
   ┌─────────────────────────────────────────────────────────┐
   │ • findById($id) - SELECT * FROM services WHERE id = ?   │
   │ • Retorna objeto Service                                │
   └─────────────────────────────────────────────────────────┘
                              ↓
8. VIEW (cart/index.php)
   ┌─────────────────────────────────────────────────────────┐
   │ • Inclui header.php                                     │
   │ • Renderiza tabela de itens                             │
   │ • Mostra total                                          │
   │ • Botões de ação                                        │
   │ • Inclui footer.php                                     │
   └─────────────────────────────────────────────────────────┘
                              ↓
9. RESPOSTA HTTP
   ┌─────────────────────────────────────────────────────────┐
   │ HTML completo enviado ao navegador                      │
   └─────────────────────────────────────────────────────────┘
```

---

## 3. ESTRUTURA DE DIRETÓRIOS DETALHADA

```
RestifyApp/
│
├── 📁 app/                          # Código-fonte principal da aplicação
│   │
│   ├── 📁 controllers/              # Controllers MVC (Camada de Controle)
│   │   ├── AdminController.php     # Gerenciamento administrativo
│   │   ├── AuthController.php      # Autenticação (login/register/logout)
│   │   ├── CartController.php      # Carrinho de compras
│   │   ├── HomeController.php      # Página inicial
│   │   ├── PaymentController.php   # Processamento de pagamentos
│   │   ├── RestaurantController.php# Dashboard do restaurante
│   │   └── SettingsController.php  # Configurações (idioma/tema/export)
│   │
│   ├── 📁 models/                   # Models (Entidades de Dados)
│   │   ├── Message.php             # Entidade de mensagem do chat
│   │   ├── Order.php               # Entidade de pedido
│   │   ├── Restaurant.php          # Entidade de restaurante
│   │   └── Service.php             # Entidade de serviço
│   │
│   ├── 📁 repositories/             # Repository Pattern (Persistência)
│   │   ├── MessageRepository.php   # Acesso a dados de mensagens
│   │   ├── OrderRepository.php     # Acesso a dados de pedidos
│   │   ├── RestaurantRepository.php# Acesso a dados de restaurantes
│   │   └── ServiceRepository.php   # Acesso a dados de serviços
│   │
│   ├── 📁 services/                 # Services (Lógica de Negócio)
│   │   ├── AuthService.php         # Lógica de autenticação
│   │   ├── CartService.php         # Lógica do carrinho
│   │   ├── EfiPaymentService.php   # Integração Efí Bank
│   │   ├── ExportService.php       # Exportação CSV (Strategy Pattern)
│   │   ├── NotificationService.php # Notificações (Observer Pattern)
│   │   └── PaymentServiceFactory.php# Factory de pagamentos
│   │
│   └── 📁 views/                    # Views (Camada de Apresentação)
│       │
│       ├── 📁 admin/                # Views administrativas
│       │   ├── chat.php            # Chat admin com restaurantes
│       │   ├── dashboard.php       # Dashboard administrativo
│       │   ├── edit-service.php    # Edição de serviços
│       │   ├── orders.php          # Lista de todos os pedidos
│       │   ├── restaurants.php     # Lista de restaurantes
│       │   └── services.php        # Gerenciamento de serviços
│       │
│       ├── 📁 auth/                 # Views de autenticação
│       │   ├── login.php           # Tela de login
│       │   └── register.php        # Tela de cadastro
│       │
│       ├── 📁 cart/                 # Views do carrinho
│       │   ├── checkout.php        # Finalização de compra
│       │   └── index.php           # Visualização do carrinho
│       │
│       ├── 📁 layout/               # Componentes de layout
│       │   ├── footer.php          # Rodapé do site
│       │   ├── header.php          # Cabeçalho com navegação
│       │   └── settings-panel.php  # Painel de configurações
│       │
│       ├── 📁 payment/              # Views de pagamento
│       │   ├── boleto.php          # Geração de boleto
│       │   ├── carne.php           # Geração de carnê
│       │   ├── credit-card.php     # Pagamento com cartão
│       │   ├── pix.php             # Pagamento via PIX
│       │   └── select-method.php   # Seleção de método
│       │
│       ├── 📁 restaurant/           # Views do restaurante
│       │   ├── chat.php            # Chat com admin
│       │   ├── dashboard.php       # Dashboard do restaurante
│       │   └── orders.php          # Pedidos do restaurante
│       │
│       ├── 404.php                 # Página de erro 404
│       └── home.php                # Página inicial
│
├── 📁 config/                       # Configurações do sistema
│   ├── 📁 certificates/            # Certificados Efí Bank
│   │   ├── homologacao-*.p12      # Certificado de homologação
│   │   └── producao-*.p12         # Certificado de produção
│   │
│   ├── autoload.php                # Autoloader de classes
│   ├── config.php                  # Configurações gerais
│   ├── database.php                # Conexão com banco (Singleton)
│   ├── efi_credentials.php         # Credenciais Efí Bank
│   └── i18n.php                    # Sistema de internacionalização
│
├── 📁 database/                     # Banco de dados e scripts SQL
│   ├── clean_database.sql          # Script para limpar dados
│   ├── migrate_payment_method.php  # Migração de métodos de pagamento
│   ├── restify.db                  # Banco SQLite
│   └── schema.sql                  # Schema completo do banco
│
├── 📁 images/                       # Assets de imagem
│   ├── logo.png                    # Logo do sistema (usado no header)
│   └── exemplo.png                 # Imagem de exemplo
│
├── 📁 lang/                         # Arquivos de tradução
│   ├── en.php                      # Traduções em inglês
│   ├── es.php                      # Traduções em espanhol
│   └── pt.php                      # Traduções em português
│
├── 📁 public/                       # Pasta pública (Document Root)
│   │
│   ├── 📁 css/                     # Arquivos de estilo
│   │   └── style.css              # CSS principal do sistema
│   │
│   ├── 📁 js/                      # Scripts JavaScript
│   │   └── app.js                 # JavaScript principal
│   │
│   ├── 📁 webhook/                 # Webhooks de integração
│   │   └── payment.php            # Webhook de pagamento Efí Bank
│   │
│   ├── .htaccess                   # Configuração Apache (URL rewrite)
│   └── index.php                   # Front Controller (ponto de entrada)
│
├── 📁 vendor/                       # Dependências do Composer
│   └── efipay/                     # SDK Efí Bank
│
├── composer.json                    # Dependências do projeto
├── composer.lock                    # Lock de versões
└── README.md                        # Documentação do projeto
```


---

## 4. SISTEMA DE ROTAS

### 4.1 Mapa Completo de Rotas
```
┌────────────────────────────────────────────────────────────────────────┐
│                         MAPA DE ROTAS                                  │
├────────────────────────────────────────────────────────────────────────┤
│                                                                        │
│  PÚBLICAS (Sem autenticação)                                           │
│  ════════════════════════════════════════════════════════════════      │
│                                                                        │
│  GET  /                          → HomeController@index                │
│  GET  /home                      → HomeController@index                │
│                                                                        │
│  GET  /auth/login                → AuthController@login                │
│  POST /auth/login                → AuthController@login                │ 
│  GET  /auth/register             → AuthController@register             │
│  POST /auth/register             → AuthController@register             │
│  GET  /auth/logout               → AuthController@logout               │
│                                                                        │
│  GET  /cart                      → CartController@index                │
│  POST /cart/add                  → CartController@add                  │
│  POST /cart/remove               → CartController@remove               │
│                                                                        │
│  ────────────────────────────────────────────────────────────────      │
│                                                                        │
│  RESTAURANTE (Requer autenticação de restaurante)                      │
│  ════════════════════════════════════════════════════════════════      │
│                                                                        │
│  GET  /restaurant/dashboard      → RestaurantController@dashboard      │
│  GET  /restaurant/orders         → RestaurantController@orders         │
│  GET  /restaurant/chat           → RestaurantController@chat           │
│  GET  /restaurant/messages       → RestaurantController@getMessages    │
│  POST /restaurant/chat           → RestaurantController@chat           │
│                                                                        │
│  GET  /cart/checkout             → CartController@checkout             │
│  POST /cart/checkout             → CartController@checkout             │
│                                                                        │
│  ────────────────────────────────────────────────────────────────      │
│                                                                        │
│  PAGAMENTOS (Requer autenticação de restaurante)                       │
│  ════════════════════════════════════════════════════════════════      │
│                                                                        │
│  GET  /payment/select            → PaymentController@selectMethod      │
│  POST /payment/pix               → PaymentController@processPix        │
│  POST /payment/credit-card       → PaymentController@processCreditCard │
│  POST /payment/boleto            → PaymentController@generateBoleto    │
│  POST /payment/carne             → PaymentController@generateCarne     │
│                                                                        │
│  ────────────────────────────────────────────────────────────────      │
│                                                                        │
│  ADMIN (Requer autenticação de administrador)                          │ 
│  ════════════════════════════════════════════════════════════════      │
│                                                                        │
│  GET  /admin/dashboard           → AdminController@dashboard          │
│  GET  /admin/orders              → AdminController@orders             │
│  POST /admin/orders              → AdminController@orders             │
│  GET  /admin/restaurants         → AdminController@restaurants        │
│  GET  /admin/services            → AdminController@services           │
│  POST /admin/services            → AdminController@services           │
│  GET  /admin/services/edit/{id}  → AdminController@editService        │
│  POST /admin/services/edit/{id}  → AdminController@editService        │
│  GET  /admin/chat                → AdminController@chat               │
│  GET  /admin/chat/{id}           → AdminController@chat               │
│  POST /admin/chat/{id}           → AdminController@chat               │
│  GET  /admin/messages/{id}       → AdminController@getMessages        │
│                                                                        │
│  ────────────────────────────────────────────────────────────────     │
│                                                                        │
│  CONFIGURAÇÕES (Requer autenticação)                                  │
│  ════════════════════════════════════════════════════════════════     │
│                                                                        │
│  POST /settings/language         → SettingsController@updateLanguage  │
│  POST /settings/theme            → SettingsController@updateTheme     │
│  GET  /export/orders             → SettingsController@exportOrders    │
│  GET  /export/restaurants        → SettingsController@exportRestaurants│
│                                                                        │
└────────────────────────────────────────────────────────────────────────┘
```

### 4.2 Sistema de Roteamento (index.php)
```php
// ESTRUTURA DO ROTEAMENTO

┌─────────────────────────────────────────────────────────────┐
│ 1. INICIALIZAÇÃO                                            │
├─────────────────────────────────────────────────────────────┤
│ • require_once '../config/config.php'                       │
│ • require_once '../config/i18n.php'                         │
│ • Inicializa sessão e idioma                                │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│ 2. PARSE DA URL                                             │
├─────────────────────────────────────────────────────────────┤
│ • Captura $_SERVER['REQUEST_URI']                           │
│ • Remove base path                                          │
│ • Remove query string                                       │
│ • Normaliza barras                                          │
│                                                             │
│ Exemplo:                                                    │
│   Input:  /RestifyApp/public/cart?id=1                      │
│   Output: "cart"                                            │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│ 3. ARRAY DE ROTAS                                           │
├─────────────────────────────────────────────────────────────┤
│ $routes = [                                                 │
│     'cart' => ['CartController', 'index'],                  │
│     'auth/login' => ['AuthController', 'login'],            │
│     ...                                                     │
│ ];                                                          │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│ 4. ROTAS DINÂMICAS (Regex)                                  │
├─────────────────────────────────────────────────────────────┤
│ • admin/chat/{id}                                           │
│ • admin/messages/{id}                                       │
│ • admin/services/edit/{id}                                  │
│                                                             │
│ if (preg_match('/^admin\/chat\/(\d+)$/', $path, $matches))  │
│     $controller->chat($matches[1]);                         │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│ 5. INSTANCIAÇÃO E EXECUÇÃO                                  │
├─────────────────────────────────────────────────────────────┤
│ $controller = new $controllerName();                        │
│ $controller->$methodName();                                 │
└─────────────────────────────────────────────────────────────┘
                        ↓
┌─────────────────────────────────────────────────────────────┐
│ 6. TRATAMENTO DE ERROS                                      │
├─────────────────────────────────────────────────────────────┤
│ • Rota não encontrada → 404.php                             │
│ • Erro de execução → HTTP 500                               │
└─────────────────────────────────────────────────────────────┘
```

---

## 5. CONTROLLERS - CAMADA DE CONTROLE

### 5.1 HomeController
```
┌────────────────────────────────────────────────────────────┐
│ HomeController.php                                         │
├────────────────────────────────────────────────────────────┤
│                                                            │
│ RESPONSABILIDADE:                                          │
│   Gerenciar a página inicial do sistema                    │
│                                                            │
│ MÉTODOS:                                                   │
│                                                            │
│ ┌────────────────────────────────────────────────────┐     │
│ │ index()                                            │     │
│ ├────────────────────────────────────────────────────┤     │
│ │ • Instancia ServiceRepository                      │     │ 
│ │ • Busca todos os serviços disponíveis              │     │
│ │ • Passa dados para view home.php                   │     │
│ │ • Renderiza catálogo de serviços                   │     │
│ │                                                    │     │
│ │ FLUXO:                                             │     │
│ │   1. $serviceRepo = new ServiceRepository()        │     │
│ │   2. $services = $serviceRepo->findAll()           │     │
│ │   3. include '../app/views/home.php'               │     │
│ └────────────────────────────────────────────────────┘     │
│                                                            │
│ TRATAMENTO DE ERROS:                                       │
│   • Try-catch para erros de banco                          │
│   • Array vazio se não houver serviços                     │
│   • Log de erros com error_log()                           │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

### 5.2 AuthController
```
┌────────────────────────────────────────────────────────────┐
│ AuthController.php                                         │
├────────────────────────────────────────────────────────────┤
│                                                            │
│ RESPONSABILIDADE:                                          │
│   Gerenciar autenticação de usuários                       │
│                                                            │
│ DEPENDÊNCIAS:                                              │
│   • AuthService (lógica de autenticação)                   │
│                                                            │
│ MÉTODOS:                                                   │
│                                                            │
│ ┌────────────────────────────────────────────────────┐     │
│ │ login()                                            │     │
│ ├────────────────────────────────────────────────────┤     │
│ │ GET:  Exibe formulário de login                    │     │
│ │ POST: Processa credenciais                         │     │
│ │                                                    │     │
│ │ FLUXO POST:                                        │     │
│ │   1. Sanitiza email e senha                        │     │
│ │   2. Valida campos obrigatórios                    │     │
│ │   3. Verifica tipo (admin/restaurant)              │     │ 
│ │   4. Chama AuthService->loginAdmin() ou            │     │
│ │      AuthService->loginRestaurant()                │     │
│ │   5. Redireciona para dashboard apropriado         │     │
│ │                                                    │     │
│ │ VALIDAÇÕES:                                        │     │
│ │   • Email não vazio                                │     │
│ │   • Senha não vazia                                │     │
│ │   • Email válido (FILTER_VALIDATE_EMAIL)           │     │
│ └────────────────────────────────────────────────────┘     │
│                                                            │
│ ┌────────────────────────────────────────────────────┐     │
│ │ register()                                         │     │
│ ├────────────────────────────────────────────────────┤     │
│ │ GET:  Exibe formulário de cadastro                 │     │
│ │ POST: Cria novo restaurante                        │     │
│ │                                                    │     │
│ │ FLUXO POST:                                        │     │
│ │   1. Sanitiza todos os campos                      │     │
│ │   2. Valida campos obrigatórios                    │     │
│ │   3. Valida formato de email                       │     │
│ │   4. Chama AuthService->register()                 │     │
│ │   5. Redireciona para login com sucesso            │     │
│ │                                                    │     │
│ │ CAMPOS:                                            │     │
│ │   • name (nome do restaurante)                     │     │
│ │   • email                                          │     │
│ │   • whatsapp                                       │     │
│ │   • address                                        │     │
│ │   • password (será hasheado)                       │     │
│ └────────────────────────────────────────────────────┘     │
│                                                            │
│ ┌────────────────────────────────────────────────────┐     │
│ │ logout()                                           │     │
│ ├────────────────────────────────────────────────────┤     │
│ │ • Chama AuthService->logout()                      │     │
│ │ • Destrói sessão                                   │     │
│ │ • Redireciona para home                            │     │
│ └────────────────────────────────────────────────────┘     │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

### 5.3 CartController
```
┌────────────────────────────────────────────────────────────┐
│ CartController.php                                         │
├────────────────────────────────────────────────────────────┤
│                                                            │
│ RESPONSABILIDADE:                                          │
│   Gerenciar carrinho de compras                            │
│                                                            │
│ DEPENDÊNCIAS:                                              │
│   • CartService (lógica do carrinho)                       │
│   • ServiceRepository (buscar serviços)                    │
│                                                            │
│ MÉTODOS:                                                   │
│                                                            │
│ ┌────────────────────────────────────────────────────┐     │
│ │ index()                                            │     │
│ ├────────────────────────────────────────────────────┤     │
│ │ • Busca itens do carrinho na sessão                │     │
│ │ • Para cada item, busca dados do serviço           │     │
│ │ • Calcula total                                    │     │
│ │ • Renderiza view cart/index.php                    │     │
│ │                                                    │     │
│ │ DADOS PASSADOS PARA VIEW:                          │     │
│ │   $services = [                                    │     │
│ │     ['service' => Service, 'quantity' => int],     │     │
│ │     ...                                            │     │
│ │   ]                                                │     │
│ │   $total = float                                   │     │
│ └────────────────────────────────────────────────────┘     │
│                                                            │
│ ┌────────────────────────────────────────────────────┐     │
│ │ add()                                              │     │
│ ├────────────────────────────────────────────────────┤    │
│ │ MÉTODO: POST (AJAX)                                │    │
│ │ RETORNO: JSON                                      │    │
│ │                                                    │    │
│ │ FLUXO:                                             │    │
│ │   1. Valida service_id (inteiro positivo)          │    │
│ │   2. Valida quantity (padrão: 1)                   │    │
│ │   3. Chama CartService->addItem()                  │    │
│ │   4. Retorna JSON com sucesso e contador           │    │
│ │                                                    │    │
│ │ RESPOSTA:                                          │    │
│ │   {                                                │    │
│ │     "success": true,                               │    │
│ │     "count": 3                                     │    │
│ │   }                                                │    │
│ └────────────────────────────────────────────────────┘    │
│                                                            │
│ ┌────────────────────────────────────────────────────┐    │
│ │ remove()                                           │    │
│ ├────────────────────────────────────────────────────┤    │
│ │ MÉTODO: POST                                       │    │
│ │                                                    │    │
│ │ • Valida service_id                                │    │
│ │ • Chama CartService->removeItem()                  │    │
│ │ • Redireciona para /cart                           │    │
│ └────────────────────────────────────────────────────┘    │
│                                                            │
│ ┌────────────────────────────────────────────────────┐    │
│ │ checkout()                                         │    │
│ ├────────────────────────────────────────────────────┤    │
│ │ REQUER: Autenticação de restaurante                │    │
│ │                                                    │    │
│ │ GET:  Exibe resumo do pedido                       │    │
│ │ POST: Cria pedido e redireciona para pagamento     │    │
│ │                                                    │    │
│ │ FLUXO POST:                                        │    │
│ │   1. Valida carrinho não vazio                     │    │
│ │   2. Cria objeto Order                             │    │
│ │   3. Adiciona itens ao pedido                      │    │
│ │   4. Salva no banco via OrderRepository            │    │
│ │   5. Limpa carrinho                                │    │
│ │   6. Redireciona para /payment/select              │    │
│ └────────────────────────────────────────────────────┘    │
│                                                            │
└────────────────────────────────────────────────────────────┘
```


---

## 6. MODELS - CAMADA DE DADOS

### 6.1 Restaurant Model
```
┌────────────────────────────────────────────────────────────┐
│ Restaurant.php                                             │
├────────────────────────────────────────────────────────────┤
│                                                            │
│ ENTIDADE: Restaurante                                      │
│                                                            │
│ ATRIBUTOS:                                                 │
│ ┌────────────────────────────────────────────────────┐    │
│ │ • id           : int    (PK, Auto Increment)       │    │
│ │ • name         : string (Nome do restaurante)      │    │
│ │ • email        : string (Email único)              │    │
│ │ • whatsapp     : string (Telefone/WhatsApp)        │    │
│ │ • address      : string (Endereço completo)        │    │
│ │ • password     : string (Hash da senha)            │    │
│ │ • created_at   : datetime (Data de criação)        │    │
│ └────────────────────────────────────────────────────┘    │
│                                                            │
│ CONSTRUTOR:                                                │
│   __construct($data = [])                                  │
│   • Aceita array associativo                               │
│   • Popula propriedades automaticamente                    │
│                                                            │
│ RELACIONAMENTOS:                                           │
│   • 1:N com Order (um restaurante tem vários pedidos)      │
│   • 1:N com Message (um restaurante tem várias mensagens)  │
│                                                            │
│ EXEMPLO DE USO:                                            │
│   $restaurant = new Restaurant([                           │
│       'name' => 'Pizzaria Bella',                          │
│       'email' => 'contato@bella.com',                      │
│       'whatsapp' => '(11) 99999-9999',                     │
│       'address' => 'Rua das Flores, 123',                  │
│       'password' => password_hash('senha', PASSWORD_DEFAULT)│
│   ]);                                                      │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

### 6.2 Service Model
```
┌────────────────────────────────────────────────────────────┐
│ Service.php                                                │
├────────────────────────────────────────────────────────────┤
│                                                            │
│ ENTIDADE: Serviço/Produto                                  │
│                                                            │
│ ATRIBUTOS:                                                 │
│ ┌────────────────────────────────────────────────────┐    │
│ │ • id          : int    (PK, Auto Increment)        │    │
│ │ • name        : string (Nome do serviço)           │    │
│ │ • description : string (Descrição detalhada)       │    │
│ │ • price       : float  (Preço em reais)            │    │
│ │ • type        : string (individual|package)        │    │
│ └────────────────────────────────────────────────────┘    │
│                                                            │
│ TIPOS DE SERVIÇO:                                          │
│   • individual: Serviço único                              │
│   • package: Pacote com múltiplos serviços                 │
│                                                            │
│ SERVIÇOS PADRÃO DO SISTEMA:                                │
│ ┌────────────────────────────────────────────────────┐    │
│ │ 1. Site com Hospedagem      - R$ 299,99 (individual)│   │
│ │ 2. Instagram + 5 Posts      - R$ 199,99 (individual)│   │
│ │ 3. Google Maps + QR Codes   - R$ 149,99 (individual)│   │
│ │ 4. Cardápio Online          - R$ 99,99  (individual)│   │
│ │ 5. Pacote Básico            - R$ 449,99 (package)  │    │
│ │ 6. Pacote Completo          - R$ 649,99 (package)  │    │
│ └────────────────────────────────────────────────────┘    │
│                                                            │
│ RELACIONAMENTOS:                                           │
│   • N:M com Order através de order_items                   │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

### 6.3 Order Model
```
┌────────────────────────────────────────────────────────────┐
│ Order.php                                                  │
├────────────────────────────────────────────────────────────┤
│                                                            │
│ ENTIDADE: Pedido                                           │
│                                                            │
│ ATRIBUTOS PRINCIPAIS:                                      │
│ ┌────────────────────────────────────────────────────┐    │
│ │ • id             : int    (PK, Auto Increment)     │    │
│ │ • restaurant_id  : int    (FK → restaurants)       │    │
│ │ • total_amount   : float  (Valor total)            │    │
│ │ • status         : string (Status do pedido)       │    │
│ │ • payment_method : string (Método de pagamento)    │    │
│ │ • payment_id     : string (ID transação Efí)       │    │
│ │ • payment_status : string (Status do pagamento)    │    │
│ │ • created_at     : datetime (Data de criação)      │    │
│ │ • updated_at     : datetime (Última atualização)   │    │
│ │ • items          : array  (Itens do pedido)        │    │
│ └────────────────────────────────────────────────────┘    │
│                                                            │
│ ATRIBUTOS EXTRAS (JOIN):                                   │
│ ┌────────────────────────────────────────────────────┐    │
│ │ • restaurant_name  : string (Nome do restaurante)  │    │
│ │ • restaurant_email : string (Email do restaurante) │    │
│ └────────────────────────────────────────────────────┘    │
│                                                            │
│ STATUS DO PEDIDO:                                          │
│   • pending    : Aguardando processamento                  │
│   • processing : Em processamento                          │
│   • completed  : Concluído                                 │
│   • cancelled  : Cancelado                                 │
│                                                            │
│ STATUS DO PAGAMENTO:                                       │
│   • pending : Aguardando pagamento                         │
│   • paid    : Pago                                         │
│   • failed  : Falhou                                       │
│                                                            │
│ MÉTODOS DE PAGAMENTO:                                      │
│   • pix         : PIX (QR Code)                            │
│   • credit_card : Cartão de Crédito                        │
│   • boleto      : Boleto Bancário                          │
│   • carne       : Carnê (Parcelamento)                     │
│                                                            │
│ ESTRUTURA DE ITEMS:                                        │
│   $order->items = [                                        │
│       [                                                    │
│           'service_id' => 1,                               │
│           'quantity' => 2,                                 │
│           'price' => 299.99                                │
│       ],                                                   │
│       ...                                                  │
│   ];                                                       │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

### 6.4 Message Model
```
┌────────────────────────────────────────────────────────────┐
│ Message.php                                                │
├────────────────────────────────────────────────────────────┤
│                                                            │
│ ENTIDADE: Mensagem do Chat                                 │
│                                                            │
│ ATRIBUTOS:                                                 │
│ ┌────────────────────────────────────────────────────┐    │
│ │ • id            : int    (PK, Auto Increment)      │    │
│ │ • restaurant_id : int    (FK → restaurants)        │    │
│ │ • sender_type   : string (restaurant|admin)        │    │
│ │ • message       : string (Conteúdo da mensagem)    │    │
│ │ • created_at    : datetime (Data de envio)         │    │
│ └────────────────────────────────────────────────────┘    │
│                                                            │
│ TIPOS DE REMETENTE:                                        │
│   • restaurant : Mensagem enviada pelo restaurante         │
│   • admin      : Mensagem enviada pelo administrador       │
│                                                            │
│ RELACIONAMENTOS:                                           │
│   • N:1 com Restaurant (várias mensagens de um restaurante)│
│                                                            │
└────────────────────────────────────────────────────────────┘
```

### 6.5 Diagrama de Relacionamentos
```
┌─────────────────────────────────────────────────────────────────┐
│                  DIAGRAMA ENTIDADE-RELACIONAMENTO               │
└─────────────────────────────────────────────────────────────────┘

    ┌──────────────────┐
    │   RESTAURANT     │
    ├──────────────────┤
    │ • id (PK)        │
    │ • name           │
    │ • email (UNIQUE) │
    │ • whatsapp       │
    │ • address        │
    │ • password       │
    │ • created_at     │
    └────────┬─────────┘
             │
             │ 1:N
             │
    ┌────────┴─────────┬──────────────────┐
    │                  │                  │
    ▼                  ▼                  ▼
┌─────────┐      ┌─────────┐      ┌──────────┐
│  ORDER  │      │ MESSAGE │      │   ...    │
├─────────┤      ├─────────┤      └──────────┘
│ • id    │      │ • id    │
│ • rest_id│     │ • rest_id│
│ • total │      │ • sender │
│ • status│      │ • message│
└────┬────┘      └─────────┘
     │
     │ 1:N
     │
     ▼
┌──────────────┐         N:M         ┌──────────────┐
│ ORDER_ITEMS  │◄───────────────────►│   SERVICE    │
├──────────────┤                     ├──────────────┤
│ • id         │                     │ • id (PK)    │
│ • order_id   │                     │ • name       │
│ • service_id │                     │ • description│
│ • quantity   │                     │ • price      │
│ • price      │                     │ • type       │
└──────────────┘                     └──────────────┘
```

---

## 7. VIEWS - CAMADA DE APRESENTAÇÃO

### 7.1 Estrutura do Header (layout/header.php)
```
┌─────────────────────────────────────────────────────────────────┐
│                         HEADER LAYOUT                           │
└─────────────────────────────────────────────────────────────────┘

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    ┌─────────────────────────────────────────────────────────┐
    │ META TAGS                                               │
    │ • charset: UTF-8                                        │
    │ • viewport: responsive                                  │
    │ • title: dinâmico via $title                            │
    ├─────────────────────────────────────────────────────────┤
    │ CSS                                                     │
    │ • /css/style.css?v=timestamp (cache busting)            │
    ├─────────────────────────────────────────────────────────┤
    │ JAVASCRIPT GLOBALS                                      │
    │ • window.BASE_URL                                       │
    │ • window.translations (i18n)                            │
    └─────────────────────────────────────────────────────────┘
</head>
<body>

┌───────────────────────────────────────────────────────────────────┐
│                          HEADER BAR                               │
│  ┌─────────────────────────────────────────────────────────────┐ │
│  │ ┌──────────┐                              ┌──────────────┐ │ │
│  │ │  LOGO    │                              │  NAVEGAÇÃO   │ │ │
│  │ │ [IMAGE]  │                              │  • Links     │ │ │
│  │ │logo.png  │                              │  • Carrinho  │ │ │
│  │ └──────────┘                              │  • Controles │ │ │
│  │                                           └──────────────┘ │ │
│  └─────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  LOGO:                                                            │
│    <img src="../images/logo.png" alt="Restify Logo">             │
│    • max-height: 60px                                             │
│    • max-width: 200px                                             │
│    • Responsivo                                                   │
│                                                                   │
│  NAVEGAÇÃO CONDICIONAL:                                           │
│                                                                   │
│  ┌─────────────────────────────────────────────────────────────┐ │
│  │ SE NÃO LOGADO:                                              │ │
│  │   • Login                                                   │ │
│  │   • Registrar                                               │ │
│  │   • Carrinho 🛒                                             │ │
│  └─────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  ┌─────────────────────────────────────────────────────────────┐ │
│  │ SE LOGADO COMO RESTAURANTE:                                 │ │
│  │   • Dashboard                                               │ │
│  │   • Meus Pedidos                                            │ │
│  │   • Chat                                                    │ │
│  │   • Carrinho 🛒 (com contador)                              │ │
│  └─────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  ┌─────────────────────────────────────────────────────────────┐ │
│  │ SE LOGADO COMO ADMIN:                                       │ │
│  │   • Dashboard                                               │ │
│  │   • Pedidos                                                 │ │
│  │   • Restaurantes                                            │ │
│  │   • Serviços                                                │ │
│  │   • Chat                                                    │ │
│  └─────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  CONTROLES DO HEADER (SEMPRE VISÍVEIS):                           │
│  ┌─────────────────────────────────────────────────────────────┐ │
│  │ • Tema (☀️/🌙)     - Toggle claro/escuro                    │ │
│  │ • Idioma (🇧🇷🇺🇸🇪🇸) - Seletor PT/EN/ES                      │ │
│  │ • Export (📊)      - Menu dropdown (se logado)              │ │
│  │ • Logout           - Botão laranja (se logado)              │ │
│  └─────────────────────────────────────────────────────────────┘ │
│                                                                   │
└───────────────────────────────────────────────────────────────────┘

<main class="main">
    <div class="container">
        <!-- Conteúdo da página -->
```

### 7.2 Card de Serviço (Visual)
```
┌─────────────────────────────────────────────────────────────────┐
│                    CARD DE SERVIÇO (PRODUTO)                    │
└─────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│                                                               │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │                      [BADGE]                            │ │
│  │                     PACOTE                              │ │
│  └─────────────────────────────────────────────────────────┘ │
│                                                               │
│                    Site com Hospedagem                        │
│                                                               │
│         Criação de site profissional com                      │
│              hospedagem incluída                              │
│                                                               │
│                      R$ 299,99                                │
│                                                               │
│              ┌─────────────────────┐                          │
│              │  Adicionar ao Carrinho │                       │
│              └─────────────────────┘                          │
│                                                               │
└───────────────────────────────────────────────────────────────┘
  │                                                           │
  └───────────────────────────────────────────────────────────┘
                    Borda Laranja (#fb6f24)

ESPECIFICAÇÕES CSS:
┌─────────────────────────────────────────────────────────────┐
│ • background: #f2f2f2 (tema claro) / #1a1a1a (tema escuro) │
│ • border-radius: 15px                                       │
│ • border-right: 4px solid #fb6f24                           │
│ • border-bottom: 4px solid #fb6f24                          │
│ • box-shadow: 0 4px 8px rgba(0,0,0,0.1)                     │
│ • padding: 1.5rem                                           │
│ • text-align: center                                        │
│                                                             │
│ TÍTULO:                                                     │
│   • font-size: 1.3rem                                       │
│   • font-weight: bold                                       │
│   • color: #000 (claro) / #fff (escuro)                    │
│                                                             │
│ DESCRIÇÃO:                                                  │
│   • font-size: 0.95rem                                      │
│   • color: #666 (claro) / #ccc (escuro)                    │
│                                                             │
│ PREÇO:                                                      │
│   • font-size: 2rem                                         │
│   • font-weight: bold                                       │
│   • color: #548A4C (verde)                                  │
│                                                             │
│ BOTÃO:                                                      │
│   • background: #548A4C                                     │
│   • color: #fff                                             │
│   • border: 1px solid #000                                  │
│   • border-radius: 5px                                      │
│   • padding: 0.6rem 1.5rem                                  │
│                                                             │
│ HOVER:                                                      │
│   • transform: translateY(-5px)                             │
│   • box-shadow: 0 6px 12px rgba(0,0,0,0.15)                │
└─────────────────────────────────────────────────────────────┘
```


### 7.3 Página Home (home.php)
```
┌─────────────────────────────────────────────────────────────────┐
│                         HOME PAGE                               │
└─────────────────────────────────────────────────────────────────┘

<?php include __DIR__ . '/layout/header.php'; ?>

┌───────────────────────────────────────────────────────────────┐
│                      CARD DE BOAS-VINDAS                      │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │ Bem-vindo ao Restify                                    │ │
│  │ Soluções completas para seu restaurante                 │ │
│  └─────────────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│                      NOSSOS SERVIÇOS                          │
└───────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│                      GRID DE SERVIÇOS                         │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                   │
│  │ Serviço 1│  │ Serviço 2│  │ Serviço 3│                   │
│  │ R$ 299,99│  │ R$ 199,99│  │ R$ 149,99│                   │
│  │ [Botão]  │  │ [Botão]  │  │ [Botão]  │                   │
│  └──────────┘  └──────────┘  └──────────┘                   │
│                                                               │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐                   │
│  │ Serviço 4│  │ Pacote 1 │  │ Pacote 2 │                   │
│  │ R$ 99,99 │  │ R$ 449,99│  │ R$ 649,99│                   │
│  │ [Botão]  │  │ [Botão]  │  │ [Botão]  │                   │
│  └──────────┘  └──────────┘  └──────────┘                   │
│                                                               │
│  CSS: display: grid                                           │
│       grid-template-columns: repeat(auto-fit, minmax(300px,1fr))│
│       gap: 1.5rem                                             │
└───────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│                  POR QUE ESCOLHER RESTIFY?                    │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │ Presença │  │Visibilidade│ │ Cardápio │  │ Economia │    │
│  │ Digital  │  │   Local    │ │ Digital  │  │          │    │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘    │
└───────────────────────────────────────────────────────────────┘

<?php include __DIR__ . '/layout/footer.php'; ?>

LÓGICA PHP:
┌─────────────────────────────────────────────────────────────┐
│ foreach ($services as $service):                            │
│     • Renderiza card com dados do serviço                   │
│     • Badge "PACOTE" se type === 'package'                  │
│     • Botão "Adicionar ao Carrinho" com onclick             │
│     • onclick="addToCart(<?= $service->id ?>)"              │
│ endforeach;                                                 │
└─────────────────────────────────────────────────────────────┘
```

### 7.4 Página de Carrinho (cart/index.php)
```
┌─────────────────────────────────────────────────────────────────┐
│                      CARRINHO DE COMPRAS                        │
└─────────────────────────────────────────────────────────────────┘

SE CARRINHO VAZIO:
┌───────────────────────────────────────────────────────────────┐
│  Seu carrinho está vazio                                      │
│  ┌─────────────────────────┐                                  │
│  │ Ver Serviços Disponíveis │                                 │
│  └─────────────────────────┘                                  │
└───────────────────────────────────────────────────────────────┘

SE CARRINHO COM ITENS:
┌───────────────────────────────────────────────────────────────┐
│                         TABELA DE ITENS                       │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │ Serviço │ Preço │ Quantidade │ Subtotal │ Ações        │ │
│  ├─────────────────────────────────────────────────────────┤ │
│  │ Site    │299,99 │     1      │  299,99  │ [Remover]   │ │
│  │ Instagram│199,99│     2      │  399,98  │ [Remover]   │ │
│  ├─────────────────────────────────────────────────────────┤ │
│  │                           TOTAL: R$ 699,97              │ │
│  └─────────────────────────────────────────────────────────┘ │
│                                                               │
│  ┌──────────────────┐  ┌──────────────────┐                  │
│  │ Continuar Comprando│ │ Finalizar Compra │                 │
│  └──────────────────┘  └──────────────────┘                  │
└───────────────────────────────────────────────────────────────┘

LÓGICA:
┌─────────────────────────────────────────────────────────────┐
│ • Verifica se usuário está logado para checkout             │
│ • Se não logado, botão redireciona para /auth/login         │
│ • Se logado, botão redireciona para /cart/checkout          │
│ • Botão remover envia POST para /cart/remove                │
└─────────────────────────────────────────────────────────────┘
```

### 7.5 Dashboard Administrativo (admin/dashboard.php)
```
┌─────────────────────────────────────────────────────────────────┐
│                    DASHBOARD ADMINISTRATIVO                     │
└─────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│                      ESTATÍSTICAS                             │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │  Total   │  │  Pedidos │  │Restaurantes│ │ Receita  │    │
│  │  Pedidos │  │  Hoje    │  │Cadastrados │ │  Total   │    │
│  │   150    │  │    12    │  │     45     │ │R$ 50.000 │    │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘    │
└───────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│                    PEDIDOS RECENTES                           │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │ ID │ Restaurante │ Total │ Status │ Pagamento │ Data  │ │
│  ├─────────────────────────────────────────────────────────┤ │
│  │ 15 │ Pizzaria X  │299,99 │Pending │   PIX     │01/01  │ │
│  │ 14 │ Burger Y    │449,99 │ Paid   │  Cartão   │31/12  │ │
│  └─────────────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│                    AÇÕES RÁPIDAS                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │ Ver Pedidos  │  │Gerenciar     │  │ Exportar     │       │
│  │              │  │Serviços      │  │ Relatórios   │       │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
└───────────────────────────────────────────────────────────────┘

DADOS EXIBIDOS:
┌─────────────────────────────────────────────────────────────┐
│ • Total de pedidos (count)                                  │
│ • Pedidos do dia (WHERE DATE(created_at) = TODAY)           │
│ • Total de restaurantes cadastrados                         │
│ • Receita total (SUM de total_amount WHERE status = paid)   │
│ • Lista dos últimos 10 pedidos                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 8. SERVICES - LÓGICA DE NEGÓCIO

### 8.1 CartService
```
┌─────────────────────────────────────────────────────────────────┐
│ CartService.php                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ RESPONSABILIDADE:                                               │
│   Gerenciar lógica do carrinho de compras na sessão            │
│                                                                 │
│ ARMAZENAMENTO:                                                  │
│   $_SESSION['cart'] = [                                         │
│       service_id => quantity,                                   │
│       1 => 2,  // 2x Serviço ID 1                               │
│       3 => 1   // 1x Serviço ID 3                               │
│   ]                                                             │
│                                                                 │
│ MÉTODOS:                                                        │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ addItem($serviceId, $quantity = 1)                      │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ • Adiciona ou incrementa quantidade                     │    │
│ │ • Se item já existe, soma quantidade                    │    │
│ │ • Se não existe, cria nova entrada                      │    │
│ │                                                         │    │
│ │ EXEMPLO:                                                │    │
│ │   addItem(1, 2);  // Adiciona 2x serviço ID 1          │    │
│ │   addItem(1, 1);  // Agora tem 3x serviço ID 1         │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ removeItem($serviceId)                                  │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ • Remove item completamente do carrinho                 │    │
│ │ • Usa unset() na sessão                                 │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ getItems()                                              │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ • Retorna array de itens                                │    │
│ │ • Retorna [] se carrinho vazio                          │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ getTotal()                                              │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ • Calcula soma total do carrinho                        │    │
│ │ • Para cada item:                                       │    │
│ │   1. Busca serviço no banco                             │    │
│ │   2. Multiplica preço x quantidade                      │    │
│ │   3. Soma ao total                                      │    │
│ │ • Retorna float                                         │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ getItemCount()                                          │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ • Retorna quantidade total de itens                     │    │
│ │ • Usa array_sum() nas quantidades                       │    │
│ │ • Usado no badge do carrinho no header                  │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ clear()                                                 │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ • Limpa carrinho completamente                          │    │
│ │ • Chamado após finalizar pedido                         │    │
│ │ • unset($_SESSION['cart'])                              │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 8.2 AuthService
```
┌─────────────────────────────────────────────────────────────────┐
│ AuthService.php                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ RESPONSABILIDADE:                                               │
│   Gerenciar autenticação e registro de usuários                │
│                                                                 │
│ DEPENDÊNCIAS:                                                   │
│   • RestaurantRepository                                        │
│                                                                 │
│ MÉTODOS:                                                        │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ loginRestaurant($email, $password)                      │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ FLUXO:                                                  │    │
│ │   1. Busca restaurante por email                        │    │
│ │   2. Verifica senha com password_verify()               │    │
│ │   3. Se válido:                                         │    │
│ │      • $_SESSION['restaurant_id'] = id                  │    │
│ │      • $_SESSION['restaurant_name'] = name              │    │
│ │      • return true                                      │    │
│ │   4. Se inválido: return false                          │    │
│ │                                                         │    │
│ │ SEGURANÇA:                                              │    │
│ │   • Usa password_verify() para hash bcrypt              │    │
│ │   • Fallback para senha texto (compatibilidade)         │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ loginAdmin($email, $password)                           │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ FLUXO:                                                  │    │
│ │   1. Busca usuário por email                            │    │
│ │   2. Verifica se id === 999 (admin)                     │    │
│ │   3. Verifica senha                                     │    │
│ │   4. Se válido:                                         │    │
│ │      • $_SESSION['admin'] = true                        │    │
│ │      • $_SESSION['admin_name'] = 'Administrador'        │    │
│ │      • return true                                      │    │
│ │   5. Fallback para credenciais fixas:                   │    │
│ │      • admin@restify.com / admin123                     │    │
│ │                                                         │    │
│ │ CREDENCIAIS PADRÃO:                                     │    │
│ │   Email: admin@restify.com                              │    │
│ │   Senha: admin123                                       │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ register($data)                                         │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ FLUXO:                                                  │    │
│ │   1. Verifica se email já existe                        │    │
│ │   2. Se existe: return erro                             │    │
│ │   3. Hash da senha com password_hash()                  │    │
│ │   4. Cria objeto Restaurant                             │    │
│ │   5. Salva no banco via repository                      │    │
│ │   6. Return sucesso/erro                                │    │
│ │                                                         │    │
│ │ VALIDAÇÕES:                                             │    │
│ │   • Email único (verifica no banco)                     │    │
│ │   • Senha hasheada com PASSWORD_DEFAULT                 │    │
│ │                                                         │    │
│ │ RETORNO:                                                │    │
│ │   ['success' => bool, 'message' => string]              │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ logout()                                                │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ • Destrói sessão completamente                          │    │
│ │ • session_destroy()                                     │    │
│ │ • Remove todas as variáveis de sessão                   │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 8.3 EfiPaymentService (Integração Efí Bank)
```
┌─────────────────────────────────────────────────────────────────┐
│ EfiPaymentService.php                                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ RESPONSABILIDADE:                                               │
│   Integração com API Efí Bank para pagamentos                  │
│                                                                 │
│ MÉTODOS DE PAGAMENTO SUPORTADOS:                                │
│   • PIX (QR Code + Copia e Cola)                                │
│   • Cartão de Crédito                                           │
│   • Boleto Bancário                                             │
│   • Carnê (Parcelamento)                                        │
│                                                                 │
│ CONFIGURAÇÃO:                                                   │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ • Certificado: config/certificates/*.p12               │    │
│ │ • Credenciais: config/efi_credentials.php              │    │
│ │ • Client ID e Client Secret                            │    │
│ │ • Sandbox/Produção                                     │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ FLUXO PIX:                                                      │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ 1. createPixCharge($order)                              │    │
│ │    • Cria cobrança PIX na Efí                           │    │
│ │    • Retorna QR Code (base64)                           │    │
│ │    • Retorna código copia e cola                        │    │
│ │    • Retorna txid (ID da transação)                     │    │
│ │                                                         │    │
│ │ 2. Exibe QR Code para cliente                           │    │
│ │                                                         │    │
│ │ 3. Webhook recebe confirmação                           │    │
│ │    • public/webhook/payment.php                         │    │
│ │    • Atualiza status do pedido                          │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ FLUXO CARTÃO:                                                   │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ 1. processCardPayment($order, $cardData)                │    │
│ │    • Valida dados do cartão                             │    │
│ │    • Envia para Efí                                     │    │
│ │    • Retorna status da transação                        │    │
│ │                                                         │    │
│ │ DADOS DO CARTÃO:                                        │    │
│ │   • Número                                              │    │
│ │   • CVV                                                 │    │
│ │   • Validade                                            │    │
│ │   • Nome do titular                                     │    │
│ │   • CPF                                                 │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ FLUXO BOLETO:                                                   │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ 1. generateBoleto($order)                               │    │
│ │    • Cria boleto na Efí                                 │    │
│ │    • Retorna PDF do boleto                              │    │
│ │    • Retorna código de barras                           │    │
│ │    • Retorna link para pagamento                        │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ FLUXO CARNÊ:                                                    │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ 1. generateCarne($order, $installments)                 │    │
│ │    • Divide valor em parcelas                           │    │
│ │    • Gera múltiplos boletos                             │    │
│ │    • Retorna carnê completo                             │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ WEBHOOK:                                                        │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ URL: https://seusite.com/public/webhook/payment.php    │    │
│ │                                                         │    │
│ │ EVENTOS:                                                │    │
│ │   • pix.received (PIX recebido)                         │    │
│ │   • charge.paid (Cobrança paga)                         │    │
│ │   • charge.failed (Cobrança falhou)                     │    │
│ │                                                         │    │
│ │ AÇÃO:                                                   │    │
│ │   • Atualiza order.payment_status                       │    │
│ │   • Atualiza order.status                               │    │
│ │   • Envia notificação (Observer Pattern)                │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```


---

## 9. REPOSITORIES - PERSISTÊNCIA

### 9.1 Padrão Repository
```
┌─────────────────────────────────────────────────────────────────┐
│                      REPOSITORY PATTERN                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ OBJETIVO:                                                       │
│   Abstrair acesso a dados, separando lógica de negócio         │
│   da persistência                                               │
│                                                                 │
│ BENEFÍCIOS:                                                     │
│   • Facilita testes (mock de dados)                             │
│   • Centraliza queries SQL                                      │
│   • Facilita mudança de banco de dados                          │
│   • Código mais limpo e organizado                              │
│                                                                 │
│ ESTRUTURA:                                                      │
│                                                                 │
│   Controller → Service → Repository → Database                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 9.2 ServiceRepository
```
┌─────────────────────────────────────────────────────────────────┐
│ ServiceRepository.php                                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ RESPONSABILIDADE:                                               │
│   Gerenciar acesso aos dados de serviços                       │
│                                                                 │
│ MÉTODOS:                                                        │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ findAll()                                               │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ SQL: SELECT * FROM services ORDER BY id                 │    │
│ │                                                         │    │
│ │ RETORNO: Array de objetos Service                       │    │
│ │                                                         │    │
│ │ USO: Listar todos os serviços na home                   │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ findById($id)                                           │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ SQL: SELECT * FROM services WHERE id = ?                │    │
│ │                                                         │    │
│ │ RETORNO: Objeto Service ou null                         │    │
│ │                                                         │    │
│ │ USO: Buscar serviço específico para carrinho            │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ create($service)                                        │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ SQL: INSERT INTO services                               │    │
│ │      (name, description, price, type)                   │    │
│ │      VALUES (?, ?, ?, ?)                                │    │
│ │                                                         │    │
│ │ RETORNO: ID do serviço criado                           │    │
│ │                                                         │    │
│ │ USO: Admin criar novo serviço                           │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ update($service)                                        │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ SQL: UPDATE services SET                                │    │
│ │      name = ?, description = ?,                         │    │
│ │      price = ?, type = ?                                │    │
│ │      WHERE id = ?                                       │    │
│ │                                                         │    │
│ │ RETORNO: Boolean (sucesso/falha)                        │    │
│ │                                                         │    │
│ │ USO: Admin editar serviço existente                     │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ delete($id)                                             │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ SQL: DELETE FROM services WHERE id = ?                  │    │
│ │                                                         │    │
│ │ RETORNO: Boolean (sucesso/falha)                        │    │
│ │                                                         │    │
│ │ USO: Admin remover serviço                              │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### 9.3 OrderRepository
```
┌─────────────────────────────────────────────────────────────────┐
│ OrderRepository.php                                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ RESPONSABILIDADE:                                               │
│   Gerenciar pedidos e itens de pedidos                         │
│                                                                 │
│ MÉTODOS:                                                        │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ create($order)                                          │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ TRANSAÇÃO:                                              │    │
│ │   1. BEGIN TRANSACTION                                  │    │
│ │   2. INSERT INTO orders (...)                           │    │
│ │   3. Para cada item:                                    │    │
│ │      INSERT INTO order_items (...)                      │    │
│ │   4. COMMIT                                             │    │
│ │                                                         │    │
│ │ RETORNO: ID do pedido criado                            │    │
│ │                                                         │    │
│ │ ROLLBACK: Se qualquer erro ocorrer                      │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ findById($id)                                           │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ SQL: SELECT o.*, r.name as restaurant_name              │    │
│ │      FROM orders o                                      │    │
│ │      JOIN restaurants r ON o.restaurant_id = r.id       │    │
│ │      WHERE o.id = ?                                     │    │
│ │                                                         │    │
│ │ RETORNO: Objeto Order com dados do restaurante          │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ findByRestaurant($restaurantId)                         │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ SQL: SELECT * FROM orders                               │    │
│ │      WHERE restaurant_id = ?                            │    │
│ │      ORDER BY created_at DESC                           │    │
│ │                                                         │    │
│ │ RETORNO: Array de pedidos do restaurante                │    │
│ │                                                         │    │
│ │ USO: Dashboard do restaurante                           │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ findAll()                                               │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ SQL: SELECT o.*, r.name, r.email                        │    │
│ │      FROM orders o                                      │    │
│ │      JOIN restaurants r ON o.restaurant_id = r.id       │    │
│ │      ORDER BY o.created_at DESC                         │    │
│ │                                                         │    │
│ │ RETORNO: Todos os pedidos com dados do restaurante      │    │
│ │                                                         │    │
│ │ USO: Dashboard administrativo                           │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ updateStatus($orderId, $status)                         │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ SQL: UPDATE orders                                      │    │
│ │      SET status = ?, updated_at = CURRENT_TIMESTAMP     │    │
│ │      WHERE id = ?                                       │    │
│ │                                                         │    │
│ │ STATUS VÁLIDOS:                                         │    │
│ │   • pending, processing, completed, cancelled           │    │
│ │                                                         │    │
│ │ USO: Admin atualizar status do pedido                   │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ updatePaymentStatus($orderId, $status, $paymentId)      │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ SQL: UPDATE orders                                      │    │
│ │      SET payment_status = ?,                            │    │
│ │          payment_id = ?,                                │    │
│ │          updated_at = CURRENT_TIMESTAMP                 │    │
│ │      WHERE id = ?                                       │    │
│ │                                                         │    │
│ │ USO: Webhook de pagamento                               │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ getOrderItems($orderId)                                 │    │
│ ├─────────────────────────────────────────────────────────┤    │
│ │ SQL: SELECT oi.*, s.name, s.description                 │    │
│ │      FROM order_items oi                                │    │
│ │      JOIN services s ON oi.service_id = s.id            │    │
│ │      WHERE oi.order_id = ?                              │    │
│ │                                                         │    │
│ │ RETORNO: Array de itens com dados do serviço            │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 10. BANCO DE DADOS

### 10.1 Diagrama Completo
```
┌─────────────────────────────────────────────────────────────────┐
│                    SCHEMA DO BANCO DE DADOS                     │
│                         (SQLite)                                │
└─────────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│ restaurants                                                  │
├──────────────────────────────────────────────────────────────┤
│ • id           INTEGER PRIMARY KEY AUTOINCREMENT             │
│ • name         TEXT NOT NULL                                 │
│ • email        TEXT UNIQUE NOT NULL                          │
│ • whatsapp     TEXT NOT NULL                                 │
│ • address      TEXT NOT NULL                                 │
│ • password     TEXT NOT NULL                                 │
│ • language     TEXT DEFAULT 'pt'                             │
│ • theme        TEXT DEFAULT 'light'                          │
│ • created_at   DATETIME DEFAULT CURRENT_TIMESTAMP            │
└──────────────────────────────────────────────────────────────┘
                        │
                        │ 1:N
                        ▼
┌──────────────────────────────────────────────────────────────┐
│ orders                                                       │
├──────────────────────────────────────────────────────────────┤
│ • id              INTEGER PRIMARY KEY AUTOINCREMENT          │
│ • restaurant_id   INTEGER NOT NULL → restaurants(id)         │
│ • total_amount    REAL NOT NULL                              │
│ • status          TEXT DEFAULT 'pending'                     │
│ •                 CHECK(status IN ('pending','processing',   │
│ •                                  'completed','cancelled')) │
│ • payment_method  TEXT                                       │
│ • payment_id      TEXT                                       │
│ • payment_status  TEXT DEFAULT 'pending'                     │
│ •                 CHECK(payment_status IN ('pending',        │
│ •                                          'paid','failed')) │
│ • created_at      DATETIME DEFAULT CURRENT_TIMESTAMP         │
│ • updated_at      DATETIME DEFAULT CURRENT_TIMESTAMP         │
└──────────────────────────────────────────────────────────────┘
                        │
                        │ 1:N
                        ▼
┌──────────────────────────────────────────────────────────────┐
│ order_items                                                  │
├──────────────────────────────────────────────────────────────┤
│ • id          INTEGER PRIMARY KEY AUTOINCREMENT              │
│ • order_id    INTEGER NOT NULL → orders(id)                  │
│ • service_id  INTEGER NOT NULL → services(id)                │
│ • quantity    INTEGER DEFAULT 1                              │
│ • price       REAL NOT NULL                                  │
└──────────────────────────────────────────────────────────────┘
                        │
                        │ N:1
                        ▼
┌──────────────────────────────────────────────────────────────┐
│ services                                                     │
├──────────────────────────────────────────────────────────────┤
│ • id          INTEGER PRIMARY KEY AUTOINCREMENT              │
│ • name        TEXT NOT NULL                                  │
│ • description TEXT                                           │
│ • price       REAL NOT NULL                                  │
│ • type        TEXT DEFAULT 'individual'                      │
│ •             CHECK(type IN ('individual','package'))        │
└──────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│ messages                                                     │
├──────────────────────────────────────────────────────────────┤
│ • id            INTEGER PRIMARY KEY AUTOINCREMENT            │
│ • restaurant_id INTEGER NOT NULL → restaurants(id)           │
│ • sender_type   TEXT NOT NULL                                │
│ •               CHECK(sender_type IN ('restaurant','admin')) │
│ • message       TEXT NOT NULL                                │
│ • created_at    DATETIME DEFAULT CURRENT_TIMESTAMP           │
└──────────────────────────────────────────────────────────────┘
```

### 10.2 Dados Iniciais
```
┌─────────────────────────────────────────────────────────────────┐
│                      DADOS SEED (PADRÃO)                        │
└─────────────────────────────────────────────────────────────────┘

ADMIN:
┌─────────────────────────────────────────────────────────────┐
│ ID:       999                                               │
│ Nome:     Admin                                             │
│ Email:    admin@restify.com                                 │
│ Senha:    admin123                                          │
│ WhatsApp: (11) 99999-9999                                   │
└─────────────────────────────────────────────────────────────┘

SERVIÇOS:
┌─────────────────────────────────────────────────────────────┐
│ ID │ Nome                    │ Preço   │ Tipo       │      │
├─────────────────────────────────────────────────────────────┤
│ 1  │ Site com Hospedagem     │ 299,99  │ individual │      │
│ 2  │ Instagram + 5 Posts     │ 199,99  │ individual │      │
│ 3  │ Google Maps + QR Codes  │ 149,99  │ individual │      │
│ 4  │ Cardápio Online         │  99,99  │ individual │      │
│ 5  │ Pacote Básico           │ 449,99  │ package    │      │
│ 6  │ Pacote Completo         │ 649,99  │ package    │      │
└─────────────────────────────────────────────────────────────┘
```

### 10.3 Conexão com Banco (Singleton Pattern)
```
┌─────────────────────────────────────────────────────────────────┐
│ Database.php (config/database.php)                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│ PADRÃO: Singleton                                               │
│                                                                 │
│ OBJETIVO:                                                       │
│   Garantir uma única instância de conexão com o banco          │
│                                                                 │
│ IMPLEMENTAÇÃO:                                                  │
│ ┌─────────────────────────────────────────────────────────┐    │
│ │ class Database {                                        │    │
│ │     private static $instance = null;                    │    │
│ │     private $connection;                                │    │
│ │                                                         │    │
│ │     private function __construct() {                    │    │
│ │         $dbPath = __DIR__.'/../database/restify.db';    │    │
│ │         $this->connection = new PDO(                    │    │
│ │             "sqlite:$dbPath"                            │    │
│ │         );                                              │    │
│ │         $this->connection->setAttribute(                │    │
│ │             PDO::ATTR_ERRMODE,                          │    │
│ │             PDO::ERRMODE_EXCEPTION                      │    │
│ │         );                                              │    │
│ │     }                                                   │    │
│ │                                                         │    │
│ │     public static function getInstance() {              │    │
│ │         if (self::$instance === null) {                 │    │
│ │             self::$instance = new Database();           │    │
│ │         }                                               │    │
│ │         return self::$instance;                         │    │
│ │     }                                                   │    │
│ │                                                         │    │
│ │     public function getConnection() {                   │    │
│ │         return $this->connection;                       │    │
│ │     }                                                   │    │
│ │ }                                                       │    │
│ └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│ USO:                                                            │
│   $db = Database::getInstance()->getConnection();               │
│   $stmt = $db->prepare("SELECT * FROM services");               │
│   $stmt->execute();                                             │
│                                                                 │
│ BENEFÍCIOS:                                                     │
│   • Economia de recursos                                        │
│   • Evita múltiplas conexões                                    │
│   • Controle centralizado                                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 11. SISTEMA DE AUTENTICAÇÃO

### 11.1 Fluxo de Login
```
┌─────────────────────────────────────────────────────────────────┐
│                      FLUXO DE AUTENTICAÇÃO                      │
└─────────────────────────────────────────────────────────────────┘

1. USUÁRIO ACESSA /auth/login
   ┌─────────────────────────────────────────────────────────┐
   │ • Exibe formulário de login                             │
   │ • Campos: email, password, type (admin/restaurant)      │
   └─────────────────────────────────────────────────────────┘
                        ↓
2. USUÁRIO SUBMETE FORMULÁRIO (POST)
   ┌─────────────────────────────────────────────────────────┐
   │ AuthController->login()                                 │
   │   • Sanitiza email (FILTER_SANITIZE_EMAIL)              │
   │   • Valida email (FILTER_VALIDATE_EMAIL)                │
   │   • Verifica campos obrigatórios                        │
   └─────────────────────────────────────────────────────────┘
                        ↓
3. VERIFICA TIPO DE USUÁRIO
   ┌─────────────────────────────────────────────────────────┐
   │ SE type === 'admin':                                    │
   │   → AuthService->loginAdmin()                           │
   │                                                         │
   │ SE type === 'restaurant':                               │
   │   → AuthService->loginRestaurant()                      │
   └─────────────────────────────────────────────────────────┘
                        ↓
4. BUSCA NO BANCO DE DADOS
   ┌─────────────────────────────────────────────────────────┐
   │ RestaurantRepository->findByEmail($email)               │
   │   • SELECT * FROM restaurants WHERE email = ?           │
   └─────────────────────────────────────────────────────────┘
                        ↓
5. VERIFICA SENHA
   ┌─────────────────────────────────────────────────────────┐
   │ password_verify($password, $hashedPassword)             │
   │   • Compara senha informada com hash do banco           │
   │   • Fallback para senha texto (compatibilidade)         │
   └─────────────────────────────────────────────────────────┘
                        ↓
6. CRIA SESSÃO
   ┌─────────────────────────────────────────────────────────┐
   │ SE ADMIN:                                               │
   │   $_SESSION['admin'] = true                             │
   │   $_SESSION['admin_name'] = 'Administrador'             │
   │                                                         │
   │ SE RESTAURANTE:                                         │
   │   $_SESSION['restaurant_id'] = $id                      │
   │   $_SESSION['restaurant_name'] = $name                  │
   └─────────────────────────────────────────────────────────┘
                        ↓
7. REDIRECIONA
   ┌─────────────────────────────────────────────────────────┐
   │ SE ADMIN:                                               │
   │   redirect('/admin/dashboard')                          │
   │                                                         │
   │ SE RESTAURANTE:                                         │
   │   redirect('/restaurant/dashboard')                     │
   └─────────────────────────────────────────────────────────┘
```

### 11.2 Middleware de Autenticação
```
┌─────────────────────────────────────────────────────────────────┐
│                    FUNÇÕES DE VERIFICAÇÃO                       │
│                      (config/config.php)                        │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ isLoggedIn()                                                │
├─────────────────────────────────────────────────────────────┤
│ • Verifica se usuário está autenticado                      │
│ • return isset($_SESSION['restaurant_id']) ||               │
│          isset($_SESSION['admin'])                          │
│                                                             │
│ USO:                                                        │
│   if (!isLoggedIn()) redirect('/auth/login');               │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ isAdmin()                                                   │
├─────────────────────────────────────────────────────────────┤
│ • Verifica se usuário é administrador                       │
│ • return isset($_SESSION['admin']) &&                       │
│          $_SESSION['admin'] === true                        │
│                                                             │
│ USO:                                                        │
│   if (!isAdmin()) redirect('/');                            │
└─────────────────────────────────────────────────────────────┘

APLICAÇÃO NAS VIEWS:
┌─────────────────────────────────────────────────────────────┐
│ <?php if (isLoggedIn()): ?>                                 │
│     <!-- Conteúdo para usuários logados -->                 │
│ <?php else: ?>                                              │
│     <!-- Conteúdo para visitantes -->                       │
│ <?php endif; ?>                                             │
│                                                             │
│ <?php if (isAdmin()): ?>                                    │
│     <!-- Menu administrativo -->                            │
│ <?php endif; ?>                                             │
└─────────────────────────────────────────────────────────────┘
```


---

## 12. SISTEMA DE CARRINHO

### 12.1 Fluxo Completo do Carrinho
```
┌─────────────────────────────────────────────────────────────────┐
│                   FLUXO DO CARRINHO DE COMPRAS                  │
└─────────────────────────────────────────────────────────────────┘

1. ADICIONAR ITEM
   ┌─────────────────────────────────────────────────────────┐
   │ Usuário clica em "Adicionar ao Carrinho"               │
   │   • onclick="addToCart(serviceId)"                      │
   │   • JavaScript envia POST via AJAX                      │
   └─────────────────────────────────────────────────────────┘
                        ↓
   ┌─────────────────────────────────────────────────────────┐
   │ CartController->add()                                   │
   │   • Valida service_id                                   │
   │   • CartService->addItem(serviceId, quantity)           │
   │   • Retorna JSON: {success: true, count: 3}             │
   └─────────────────────────────────────────────────────────┘
                        ↓
   ┌─────────────────────────────────────────────────────────┐
   │ JavaScript atualiza interface                           │
   │   • updateCartCount(count)                              │
   │   • Mostra badge com número de itens                    │
   │   • Exibe alerta de sucesso                             │
   └─────────────────────────────────────────────────────────┘

2. VISUALIZAR CARRINHO
   ┌─────────────────────────────────────────────────────────┐
   │ Usuário acessa /cart                                    │
   │   • CartController->index()                             │
   │   • Busca itens da sessão                               │
   │   • Para cada item, busca dados do serviço              │
   │   • Calcula total                                       │
   │   • Renderiza tabela                                    │
   └─────────────────────────────────────────────────────────┘

3. REMOVER ITEM
   ┌─────────────────────────────────────────────────────────┐
   │ Usuário clica em "Remover"                              │
   │   • POST para /cart/remove                              │
   │   • CartService->removeItem(serviceId)                  │
   │   • Redireciona para /cart                              │
   └─────────────────────────────────────────────────────────┘

4. FINALIZAR COMPRA
   ┌─────────────────────────────────────────────────────────┐
   │ Usuário clica em "Finalizar Compra"                     │
   │   • Verifica se está logado                             │
   │   • Se não: redireciona para /auth/login                │
   │   • Se sim: vai para /cart/checkout                     │
   └─────────────────────────────────────────────────────────┘
                        ↓
   ┌─────────────────────────────────────────────────────────┐
   │ CartController->checkout()                              │
   │   • Cria objeto Order                                   │
   │   • Adiciona itens do carrinho                          │
   │   • Salva no banco (OrderRepository)                    │
   │   • Limpa carrinho (CartService->clear())               │
   │   • Redireciona para /payment/select?order_id=X         │
   └─────────────────────────────────────────────────────────┘
```

### 12.2 Estrutura da Sessão
```
┌─────────────────────────────────────────────────────────────────┐
│                    ESTRUTURA $_SESSION['cart']                  │
└─────────────────────────────────────────────────────────────────┘

$_SESSION['cart'] = [
    1 => 2,    // Serviço ID 1, quantidade 2
    3 => 1,    // Serviço ID 3, quantidade 1
    5 => 1     // Serviço ID 5, quantidade 1
];

EXEMPLO REAL:
┌─────────────────────────────────────────────────────────────┐
│ Carrinho com 3 serviços diferentes:                         │
│                                                             │
│ [1] Site com Hospedagem (x2)    = R$ 599,98                │
│ [3] Google Maps + QR Codes (x1) = R$ 149,99                │
│ [5] Pacote Básico (x1)          = R$ 449,99                │
│                                                             │
│ TOTAL: R$ 1.199,96                                          │
└─────────────────────────────────────────────────────────────┘

CÁLCULO DO TOTAL:
┌─────────────────────────────────────────────────────────────┐
│ foreach ($cart as $serviceId => $quantity) {                │
│     $service = ServiceRepository->findById($serviceId);     │
│     $total += $service->price * $quantity;                  │
│ }                                                           │
└─────────────────────────────────────────────────────────────┘
```

---

## 13. SISTEMA DE PAGAMENTOS

### 13.1 Integração Efí Bank
```
┌─────────────────────────────────────────────────────────────────┐
│                    INTEGRAÇÃO EFÍ BANK                          │
│                  (Antigo Gerencianet)                           │
└─────────────────────────────────────────────────────────────────┘

CONFIGURAÇÃO:
┌─────────────────────────────────────────────────────────────┐
│ Arquivo: config/efi_credentials.php                         │
│                                                             │
│ return [                                                    │
│     'client_id' => 'Client_Id_...',                         │
│     'client_secret' => 'Client_Secret_...',                 │
│     'certificate' => __DIR__.'/certificates/producao.p12',  │
│     'sandbox' => false,  // true para testes                │
│     'debug' => false                                        │
│ ];                                                          │
└─────────────────────────────────────────────────────────────┘

SDK:
┌─────────────────────────────────────────────────────────────┐
│ Composer: efipay/sdk-php-apis-efi                           │
│ Versão: ^2.0                                                │
│                                                             │
│ use Efi\Exception\EfiException;                             │
│ use Efi\EfiPay;                                             │
└─────────────────────────────────────────────────────────────┘
```

### 13.2 Fluxo de Pagamento PIX
```
┌─────────────────────────────────────────────────────────────────┐
│                      FLUXO PAGAMENTO PIX                        │
└─────────────────────────────────────────────────────────────────┘

1. SELEÇÃO DO MÉTODO
   ┌─────────────────────────────────────────────────────────┐
   │ /payment/select?order_id=15                             │
   │   • Exibe opções: PIX, Cartão, Boleto, Carnê           │
   │   • Usuário seleciona PIX                               │
   └─────────────────────────────────────────────────────────┘
                        ↓
2. GERAÇÃO DO PIX
   ┌─────────────────────────────────────────────────────────┐
   │ POST /payment/pix                                       │
   │   • PaymentController->processPix()                     │
   │   • EfiPaymentService->createPixCharge($order)          │
   └─────────────────────────────────────────────────────────┘
                        ↓
3. CHAMADA API EFÍ
   ┌─────────────────────────────────────────────────────────┐
   │ $efi = new EfiPay($credentials);                        │
   │ $body = [                                               │
   │     'calendario' => ['expiracao' => 3600],              │
   │     'valor' => ['original' => '299.99'],                │
   │     'chave' => 'sua-chave-pix',                         │
   │     'infoAdicionais' => [                               │
   │         ['nome' => 'Pedido', 'valor' => '#15']          │
   │     ]                                                   │
   │ ];                                                      │
   │ $response = $efi->pixCreateImmediateCharge([], $body);  │
   └─────────────────────────────────────────────────────────┘
                        ↓
4. RESPOSTA DA API
   ┌─────────────────────────────────────────────────────────┐
   │ {                                                       │
   │     "txid": "7978c0c97ea847e78e8849634473c1f1",         │
   │     "loc": {                                            │
   │         "id": 789,                                      │
   │         "location": "pix.example.com/qr/v2/...",        │
   │         "tipoCob": "cob"                                │
   │     },                                                  │
   │     "status": "ATIVA",                                  │
   │     "pixCopiaECola": "00020126580014br.gov.bcb...",     │
   │     "qrcode": "data:image/png;base64,iVBORw0KGgo..."    │
   │ }                                                       │
   └─────────────────────────────────────────────────────────┘
                        ↓
5. EXIBIÇÃO PARA USUÁRIO
   ┌─────────────────────────────────────────────────────────┐
   │ View: payment/pix.php                                   │
   │                                                         │
   │ ┌───────────────────────────────────────────────────┐   │
   │ │         [QR CODE IMAGE]                           │   │
   │ │                                                   │   │
   │ │  Escaneie o QR Code com seu app de pagamento     │   │
   │ │                                                   │   │
   │ │  OU                                               │   │
   │ │                                                   │   │
   │ │  Código Copia e Cola:                             │   │
   │ │  [00020126580014br.gov.bcb...]  [Copiar]          │   │
   │ │                                                   │   │
   │ │  Valor: R$ 299,99                                 │   │
   │ │  Válido por: 1 hora                               │   │
   │ └───────────────────────────────────────────────────┘   │
   └─────────────────────────────────────────────────────────┘
                        ↓
6. AGUARDANDO PAGAMENTO
   ┌─────────────────────────────────────────────────────────┐
   │ • Página fica aguardando                                │
   │ • Polling a cada 5 segundos (opcional)                  │
   │ • Ou aguarda webhook                                    │
   └─────────────────────────────────────────────────────────┘
                        ↓
7. WEBHOOK RECEBE CONFIRMAÇÃO
   ┌─────────────────────────────────────────────────────────┐
   │ POST /public/webhook/payment.php                        │
   │                                                         │
   │ {                                                       │
   │     "evento": "pix",                                    │
   │     "data_criacao": "2024-01-15T10:30:00",              │
   │     "pix": [{                                           │
   │         "txid": "7978c0c97ea847e78e8849634473c1f1",     │
   │         "valor": "299.99",                              │
   │         "horario": "2024-01-15T10:30:00"                │
   │     }]                                                  │
   │ }                                                       │
   └─────────────────────────────────────────────────────────┘
                        ↓
8. ATUALIZAÇÃO DO PEDIDO
   ┌─────────────────────────────────────────────────────────┐
   │ OrderRepository->updatePaymentStatus(                   │
   │     $orderId,                                           │
   │     'paid',                                             │
   │     $txid                                               │
   │ );                                                      │
   │                                                         │
   │ OrderRepository->updateStatus($orderId, 'processing');  │
   └─────────────────────────────────────────────────────────┘
                        ↓
9. NOTIFICAÇÃO
   ┌─────────────────────────────────────────────────────────┐
   │ NotificationService->notify(                            │
   │     'payment_received',                                 │
   │     $order                                              │
   │ );                                                      │
   │                                                         │
   │ • Email para restaurante (opcional)                     │
   │ • Atualização no dashboard                              │
   └─────────────────────────────────────────────────────────┘
```

### 13.3 Fluxo de Pagamento com Cartão
```
┌─────────────────────────────────────────────────────────────────┐
│                   FLUXO PAGAMENTO CARTÃO                        │
└─────────────────────────────────────────────────────────────────┘

1. FORMULÁRIO DE CARTÃO
   ┌─────────────────────────────────────────────────────────┐
   │ View: payment/credit-card.php                           │
   │                                                         │
   │ CAMPOS:                                                 │
   │   • Número do Cartão (16 dígitos)                       │
   │   • Nome do Titular                                     │
   │   • Validade (MM/AA)                                    │
   │   • CVV (3 dígitos)                                     │
   │   • CPF do Titular                                      │
   │   • Parcelas (1x a 12x)                                 │
   └─────────────────────────────────────────────────────────┘
                        ↓
2. VALIDAÇÃO FRONTEND
   ┌─────────────────────────────────────────────────────────┐
   │ JavaScript (app.js)                                     │
   │   • Valida número do cartão (Luhn Algorithm)            │
   │   • Valida CVV (3 dígitos)                              │
   │   • Valida CPF                                          │
   │   • Valida validade (não expirado)                      │
   └─────────────────────────────────────────────────────────┘
                        ↓
3. ENVIO PARA SERVIDOR
   ┌─────────────────────────────────────────────────────────┐
   │ POST /payment/credit-card                               │
   │   • PaymentController->processCreditCard()              │
   │   • Sanitiza dados do cartão                            │
   │   • EfiPaymentService->processCardPayment()             │
   └─────────────────────────────────────────────────────────┘
                        ↓
4. CHAMADA API EFÍ
   ┌─────────────────────────────────────────────────────────┐
   │ $body = [                                               │
   │     'payment' => [                                      │
   │         'credit_card' => [                              │
   │             'installments' => 1,                        │
   │             'billing_address' => [...],                 │
   │             'payment_token' => $token,                  │
   │             'customer' => [                             │
   │                 'name' => $name,                        │
   │                 'cpf' => $cpf,                          │
   │                 'email' => $email                       │
   │             ]                                           │
   │         ]                                               │
   │     ],                                                  │
   │     'items' => [...]                                    │
   │ ];                                                      │
   │ $response = $efi->createCharge([], $body);              │
   └─────────────────────────────────────────────────────────┘
                        ↓
5. RESPOSTA IMEDIATA
   ┌─────────────────────────────────────────────────────────┐
   │ SE APROVADO:                                            │
   │   • Atualiza pedido para 'paid'                         │
   │   • Redireciona para página de sucesso                  │
   │                                                         │
   │ SE RECUSADO:                                            │
   │   • Exibe mensagem de erro                              │
   │   • Permite tentar novamente                            │
   └─────────────────────────────────────────────────────────┘
```

### 13.4 Webhook de Pagamento
```
┌─────────────────────────────────────────────────────────────────┐
│                    WEBHOOK (public/webhook/payment.php)         │
└─────────────────────────────────────────────────────────────────┘

CONFIGURAÇÃO NA EFÍ:
┌─────────────────────────────────────────────────────────────┐
│ URL: https://seudominio.com/RestifyApp/public/webhook/     │
│      payment.php                                            │
│                                                             │
│ Eventos configurados:                                       │
│   • pix                                                     │
│   • charge                                                  │
│   • carnet                                                  │
└─────────────────────────────────────────────────────────────┘

ESTRUTURA DO WEBHOOK:
┌─────────────────────────────────────────────────────────────┐
│ <?php                                                       │
│ // Recebe notificação da Efí                                │
│ $input = file_get_contents('php://input');                  │
│ $data = json_decode($input, true);                          │
│                                                             │
│ // Log para debug                                           │
│ error_log("Webhook recebido: " . $input);                   │
│                                                             │
│ // Processa baseado no tipo de evento                       │
│ switch ($data['evento']) {                                  │
│     case 'pix':                                             │
│         // PIX recebido                                     │
│         $txid = $data['pix'][0]['txid'];                    │
│         // Busca pedido pelo txid                           │
│         // Atualiza status                                  │
│         break;                                              │
│                                                             │
│     case 'charge':                                          │
│         // Cobrança atualizada                              │
│         break;                                              │
│ }                                                           │
│                                                             │
│ // Retorna 200 OK                                           │
│ http_response_code(200);                                    │
│ ?>                                                          │
└─────────────────────────────────────────────────────────────┘

SEGURANÇA:
┌─────────────────────────────────────────────────────────────┐
│ • Validar origem da requisição                              │
│ • Verificar assinatura (se disponível)                      │
│ • Validar dados antes de processar                          │
│ • Log de todas as requisições                               │
│ • Não expor informações sensíveis                           │
└─────────────────────────────────────────────────────────────┘
```

---

## 14. FRONTEND E JAVASCRIPT

### 14.1 Estrutura do app.js
```
┌─────────────────────────────────────────────────────────────────┐
│                    JAVASCRIPT PRINCIPAL (app.js)                │
└─────────────────────────────────────────────────────────────────┘

FUNÇÕES PRINCIPAIS:

┌─────────────────────────────────────────────────────────────┐
│ addToCart(serviceId, quantity = 1)                          │
├─────────────────────────────────────────────────────────────┤
│ • Envia requisição AJAX para /cart/add                      │
│ • Atualiza contador do carrinho                             │
│ • Exibe alerta de sucesso                                   │
│                                                             │
│ EXEMPLO:                                                    │
│   <button onclick="addToCart(1)">                           │
│       Adicionar ao Carrinho                                 │
│   </button>                                                 │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ updateCartCount(count)                                      │
├─────────────────────────────────────────────────────────────┤
│ • Atualiza badge do carrinho no header                      │
│ • Mostra/oculta badge baseado na quantidade                 │
│                                                             │
│ DOM:                                                        │
│   <span class="cart-count">3</span>                         │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ showAlert(message, type = 'success')                        │
├─────────────────────────────────────────────────────────────┤
│ • Cria alerta flutuante                                     │
│ • Tipos: success, error                                     │
│ • Auto-remove após 3 segundos                               │
│                                                             │
│ VISUAL:                                                     │
│   ┌─────────────────────────────────────┐                  │
│   │ ✓ Item adicionado com sucesso!      │                  │
│   └─────────────────────────────────────┘                  │
│   (canto superior direito)                                  │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ toggleTheme()                                               │
├─────────────────────────────────────────────────────────────┤
│ • Alterna entre tema claro e escuro                         │
│ • Salva preferência no localStorage                         │
│ • Envia para servidor via AJAX                              │
│ • Atualiza ícone (☀️ / 🌙)                                  │
│                                                             │
│ IMPLEMENTAÇÃO:                                              │
│   document.body.classList.toggle('dark-theme');             │
│   localStorage.setItem('theme', newTheme);                  │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ changeLanguage(language)                                    │
├─────────────────────────────────────────────────────────────┤
│ • Envia requisição para /settings/language                  │
│ • Recarrega página para aplicar traduções                   │
│                                                             │
│ IDIOMAS:                                                    │
│   • pt (Português)                                          │
│   • en (English)                                            │
│   • es (Español)                                            │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ startChatPolling(restaurantId = null)                       │
├─────────────────────────────────────────────────────────────┤
│ • Inicia polling de mensagens a cada 3 segundos             │
│ • Atualiza chat em tempo real                               │
│ • Para admin: busca mensagens de restaurante específico     │
│ • Para restaurante: busca suas próprias mensagens           │
│                                                             │
│ FLUXO:                                                      │
│   setInterval(() => {                                       │
│       fetch(url).then(updateChatMessages);                  │
│   }, 3000);                                                 │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ maskPhone(input)                                            │
├─────────────────────────────────────────────────────────────┤
│ • Aplica máscara de telefone                                │
│ • Formato: (11) 99999-9999                                  │
│ • Auto-detecta celular vs fixo                              │
│                                                             │
│ EXEMPLO:                                                    │
│   Input: 11999998888                                        │
│   Output: (11) 99999-8888                                   │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ maskCPF(input)                                              │
├─────────────────────────────────────────────────────────────┤
│ • Aplica máscara de CPF                                     │
│ • Formato: 123.456.789-01                                   │
│ • Valida CPF quando completo                                │
│                                                             │
│ VALIDAÇÃO:                                                  │
│   • Algoritmo de validação de CPF                           │
│   • Rejeita CPFs inválidos                                  │
└─────────────────────────────────────────────────────────────┘
```

### 14.2 Eventos e Inicialização
```
┌─────────────────────────────────────────────────────────────────┐
│              INICIALIZAÇÃO (DOMContentLoaded)                   │
└─────────────────────────────────────────────────────────────────┘

document.addEventListener('DOMContentLoaded', function() {
    
    // 1. INICIALIZAR TEMA
    ┌─────────────────────────────────────────────────────────┐
    │ initTheme();                                            │
    │   • Carrega tema do localStorage                        │
    │   • Aplica classe dark-theme se necessário              │
    │   • Atualiza ícone do botão                             │
    └─────────────────────────────────────────────────────────┘
    
    // 2. APLICAR MÁSCARAS
    ┌─────────────────────────────────────────────────────────┐
    │ const phoneInputs = document.querySelectorAll(          │
    │     'input[name="whatsapp"], input[type="tel"]'         │
    │ );                                                      │
    │ phoneInputs.forEach(input => {                          │
    │     input.addEventListener('input', () => {             │
    │         maskPhone(input);                               │
    │     });                                                 │
    │ });                                                     │
    └─────────────────────────────────────────────────────────┘
    
    // 3. INICIAR CHAT POLLING
    ┌─────────────────────────────────────────────────────────┐
    │ if (document.querySelector('.chat-container')) {        │
    │     const restaurantId = document                       │
    │         .querySelector('[data-restaurant-id]')          │
    │         ?.dataset.restaurantId;                         │
    │     startChatPolling(restaurantId);                     │
    │ }                                                       │
    └─────────────────────────────────────────────────────────┘
    
    // 4. VALIDAÇÃO DE FORMULÁRIOS
    ┌─────────────────────────────────────────────────────────┐
    │ const forms = document.querySelectorAll(                │
    │     'form[data-validate]'                               │
    │ );                                                      │
    │ forms.forEach(form => {                                 │
    │     form.addEventListener('submit', (e) => {            │
    │         if (!validateForm(form)) {                      │
    │             e.preventDefault();                         │
    │         }                                               │
    │     });                                                 │
    │ });                                                     │
    └─────────────────────────────────────────────────────────┘
    
    // 5. AUTO-HIDE ALERTS
    ┌─────────────────────────────────────────────────────────┐
    │ const alerts = document.querySelectorAll('.alert');     │
    │ alerts.forEach(alert => {                               │
    │     setTimeout(() => {                                  │
    │         alert.style.opacity = '0';                      │
    │         setTimeout(() => alert.remove(), 300);          │
    │     }, 5000);                                           │
    │ });                                                     │
    └─────────────────────────────────────────────────────────┘
});
```


---

## 15. SISTEMA DE INTERNACIONALIZAÇÃO

### 15.1 Estrutura i18n
```
┌─────────────────────────────────────────────────────────────────┐
│                  SISTEMA DE INTERNACIONALIZAÇÃO                 │
│                         (i18n - I18n.php)                       │
└─────────────────────────────────────────────────────────────────┘

CLASSE I18n:
┌─────────────────────────────────────────────────────────────┐
│ class I18n {                                                │
│     private static $language = 'pt';                        │
│     private static $translations = [];                      │
│                                                             │
│     public static function init($lang = 'pt') {             │
│         self::$language = $lang;                            │
│         self::loadTranslations();                           │
│     }                                                       │
│                                                             │
│     private static function loadTranslations() {            │
│         $file = __DIR__."/../lang/".self::$language.".php"; │
│         if (file_exists($file)) {                           │
│             self::$translations = include $file;            │
│         }                                                   │
│     }                                                       │
│                                                             │
│     public static function t($key, $params = []) {          │
│         $text = self::$translations[$key] ?? $key;          │
│         foreach ($params as $k => $v) {                     │
│             $text = str_replace('{'.$k.'}', $v, $text);     │
│         }                                                   │
│         return $text;                                       │
│     }                                                       │
│ }                                                           │
│                                                             │
│ // Helper function                                          │
│ function t($key, $params = []) {                            │
│     return I18n::t($key, $params);                          │
│ }                                                           │
└─────────────────────────────────────────────────────────────┘

IDIOMAS SUPORTADOS:
┌─────────────────────────────────────────────────────────────┐
│ • pt (Português - Brasil)  - lang/pt.php                    │
│ • en (English)             - lang/en.php                    │
│ • es (Español)             - lang/es.php                    │
└─────────────────────────────────────────────────────────────┘
```

### 15.2 Arquivo de Tradução (Exemplo)
```
┌─────────────────────────────────────────────────────────────────┐
│                    lang/pt.php (PORTUGUÊS)                      │
└─────────────────────────────────────────────────────────────────┘

<?php
return [
    // Navegação
    'dashboard' => 'Painel',
    'orders' => 'Pedidos',
    'my_orders' => 'Meus Pedidos',
    'services' => 'Serviços',
    'restaurants' => 'Restaurantes',
    'chat' => 'Chat',
    'login' => 'Entrar',
    'register' => 'Cadastrar',
    'logout' => 'Sair',
    
    // Home
    'welcome' => 'Bem-vindo',
    'our_services' => 'Nossos Serviços',
    'add_to_cart' => 'Adicionar ao Carrinho',
    'package' => 'Pacote',
    
    // Carrinho
    'cart' => 'Carrinho',
    'cart_empty' => 'Seu carrinho está vazio',
    'continue_shopping' => 'Continuar Comprando',
    'checkout' => 'Finalizar Compra',
    'total' => 'Total',
    'remove_item' => 'Remover',
    
    // Mensagens
    'item_added_to_cart_success' => 'Item adicionado ao carrinho!',
    'error_adding_item_to_cart' => 'Erro ao adicionar item',
    'fill_all_required_fields' => 'Preencha todos os campos',
    
    // ... mais traduções
];

┌─────────────────────────────────────────────────────────────────┐
│                    lang/en.php (ENGLISH)                        │
└─────────────────────────────────────────────────────────────────┘

<?php
return [
    'dashboard' => 'Dashboard',
    'orders' => 'Orders',
    'my_orders' => 'My Orders',
    'services' => 'Services',
    'restaurants' => 'Restaurants',
    'chat' => 'Chat',
    'login' => 'Login',
    'register' => 'Register',
    'logout' => 'Logout',
    
    'welcome' => 'Welcome',
    'our_services' => 'Our Services',
    'add_to_cart' => 'Add to Cart',
    'package' => 'Package',
    
    // ... more translations
];

┌─────────────────────────────────────────────────────────────────┐
│                    lang/es.php (ESPAÑOL)                        │
└─────────────────────────────────────────────────────────────────┘

<?php
return [
    'dashboard' => 'Panel',
    'orders' => 'Pedidos',
    'my_orders' => 'Mis Pedidos',
    'services' => 'Servicios',
    'restaurants' => 'Restaurantes',
    'chat' => 'Chat',
    'login' => 'Iniciar Sesión',
    'register' => 'Registrarse',
    'logout' => 'Salir',
    
    'welcome' => 'Bienvenido',
    'our_services' => 'Nuestros Servicios',
    'add_to_cart' => 'Añadir al Carrito',
    'package' => 'Paquete',
    
    // ... más traducciones
];
```

### 15.3 Uso nas Views
```
┌─────────────────────────────────────────────────────────────────┐
│                      USO DA FUNÇÃO t()                          │
└─────────────────────────────────────────────────────────────────┘

EXEMPLO 1: Texto simples
┌─────────────────────────────────────────────────────────────┐
│ <h1><?= t('welcome') ?></h1>                                │
│                                                             │
│ RESULTADO:                                                  │
│   PT: Bem-vindo                                             │
│   EN: Welcome                                               │
│   ES: Bienvenido                                            │
└─────────────────────────────────────────────────────────────┘

EXEMPLO 2: Com parâmetros
┌─────────────────────────────────────────────────────────────┐
│ <?= t('welcome_user', ['name' => $userName]) ?>             │
│                                                             │
│ Tradução:                                                   │
│   'welcome_user' => 'Bem-vindo, {name}!'                    │
│                                                             │
│ RESULTADO:                                                  │
│   Bem-vindo, João!                                          │
└─────────────────────────────────────────────────────────────┘

EXEMPLO 3: Em atributos HTML
┌─────────────────────────────────────────────────────────────┐
│ <button title="<?= t('add_to_cart') ?>">                    │
│     <?= t('add') ?>                                         │
│ </button>                                                   │
└─────────────────────────────────────────────────────────────┘

EXEMPLO 4: Em JavaScript
┌─────────────────────────────────────────────────────────────┐
│ <script>                                                    │
│     window.translations = {                                 │
│         'item_added': '<?= t('item_added_to_cart') ?>',     │
│         'error': '<?= t('error_message') ?>'                │
│     };                                                      │
│                                                             │
│     // Uso no JS                                            │
│     alert(window.translations.item_added);                  │
│ </script>                                                   │
└─────────────────────────────────────────────────────────────┘
```

### 15.4 Mudança de Idioma
```
┌─────────────────────────────────────────────────────────────────┐
│                    FLUXO DE MUDANÇA DE IDIOMA                   │
└─────────────────────────────────────────────────────────────────┘

1. SELETOR NO HEADER
   ┌─────────────────────────────────────────────────────────┐
   │ <select onchange="changeLanguage(this.value)">          │
   │     <option value="pt">🇧🇷</option>                     │
   │     <option value="en">🇺🇸</option>                     │
   │     <option value="es">🇪🇸</option>                     │
   │ </select>                                               │
   └─────────────────────────────────────────────────────────┘
                        ↓
2. JAVASCRIPT
   ┌─────────────────────────────────────────────────────────┐
   │ function changeLanguage(language) {                     │
   │     fetch(BASE_URL + '/settings/language', {            │
   │         method: 'POST',                                 │
   │         body: `language=${language}`                    │
   │     }).then(() => location.reload());                   │
   │ }                                                       │
   └─────────────────────────────────────────────────────────┘
                        ↓
3. CONTROLLER
   ┌─────────────────────────────────────────────────────────┐
   │ SettingsController->updateLanguage()                    │
   │   • $_SESSION['language'] = $language                   │
   │   • setcookie('language', $language, ...)               │
   │   • I18n::setLanguage($language)                        │
   └─────────────────────────────────────────────────────────┘
                        ↓
4. RELOAD DA PÁGINA
   ┌─────────────────────────────────────────────────────────┐
   │ • Página recarrega                                      │
   │ • I18n::init() carrega novo idioma                      │
   │ • Todas as traduções são aplicadas                      │
   └─────────────────────────────────────────────────────────┘
```

---

## 16. DESIGN SYSTEM E PALETA DE CORES

### 16.1 Paleta de Cores
```
┌─────────────────────────────────────────────────────────────────┐
│                        PALETA DE CORES                          │
└─────────────────────────────────────────────────────────────────┘

COR PRINCIPAL (Verde):
┌─────────────────────────────────────────────────────────────┐
│ #548A4C                                                     │
│ ████████████████████████████████████████                    │
│                                                             │
│ USO:                                                        │
│   • Header e Footer                                         │
│   • Botões primários                                        │
│   • Bordas de cards principais                              │
│   • Cabeçalhos de tabelas                                   │
│   • Preços destacados                                       │
│   • Hover de botões                                         │
│                                                             │
│ VARIAÇÃO ESCURA: #3d6838 (hover)                            │
└─────────────────────────────────────────────────────────────┘

COR DE DESTAQUE (Laranja):
┌─────────────────────────────────────────────────────────────┐
│ #fb6f24                                                     │
│ ████████████████████████████████████████                    │
│                                                             │
│ USO:                                                        │
│   • Bordas do header e footer                               │
│   • Bordas laterais dos cards de produtos                   │
│   • Botão de logout                                         │
│   • Badges de pacotes                                       │
│   • Elementos hover (navegação, cart-icon)                  │
│   • Contador do carrinho                                    │
│   • Alertas de ação                                         │
│                                                             │
│ VARIAÇÃO ESCURA: #d95a1a (hover)                            │
└─────────────────────────────────────────────────────────────┘

TEMA CLARO:
┌─────────────────────────────────────────────────────────────┐
│ Fundo Principal:    #ffffff (branco)                        │
│ Fundo Cards:        #f2f2f2 (cinza claro)                   │
│ Texto Principal:    #000000 (preto)                         │
│ Texto Secundário:   #666666 (cinza médio)                   │
│ Bordas:             #ddd (cinza claro)                      │
└─────────────────────────────────────────────────────────────┘

TEMA ESCURO:
┌─────────────────────────────────────────────────────────────┐
│ Fundo Principal:    #000000 (preto)                         │
│ Fundo Cards:        #1a1a1a (cinza escuro)                  │
│ Texto Principal:    #ffffff (branco)                        │
│ Texto Secundário:   #cccccc (cinza claro)                   │
│ Bordas:             #333333 (cinza escuro)                  │
└─────────────────────────────────────────────────────────────┘
```

### 16.2 Componentes Visuais
```
┌─────────────────────────────────────────────────────────────────┐
│                      CARD DE PRODUTO                            │
└─────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│                                                               │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │                    [PACOTE]                             │ │
│  │                  (badge laranja)                        │ │
│  └─────────────────────────────────────────────────────────┘ │
│                                                               │
│              Site com Hospedagem                              │
│           (título em negrito, 1.3rem)                         │
│                                                               │
│      Criação de site profissional com                         │
│           hospedagem incluída                                 │
│        (descrição cinza, 0.95rem)                             │
│                                                               │
│                  R$ 299,99                                    │
│            (preço verde, 2rem, bold)                          │
│                                                               │
│         ┌───────────────────────────┐                         │
│         │  Adicionar ao Carrinho    │                         │
│         │  (botão verde, arredondado)│                        │
│         └───────────────────────────┘                         │
│                                                               │
└───────────────────────────────────────────────────────────────┘
  │                                                           │
  └───────────────────────────────────────────────────────────┘
              Borda direita e inferior laranja (4px)

CSS:
┌─────────────────────────────────────────────────────────────┐
│ .service-card {                                             │
│     background: #f2f2f2;                                    │
│     border-radius: 15px;                                    │
│     border-right: 4px solid #fb6f24;                        │
│     border-bottom: 4px solid #fb6f24;                       │
│     box-shadow: 0 4px 8px rgba(0,0,0,0.1);                  │
│     padding: 1.5rem;                                        │
│     text-align: center;                                     │
│     transition: transform 0.3s, box-shadow 0.3s;            │
│ }                                                           │
│                                                             │
│ .service-card:hover {                                       │
│     transform: translateY(-5px);                            │
│     box-shadow: 0 6px 12px rgba(0,0,0,0.15);                │
│ }                                                           │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                         BOTÕES                                  │
└─────────────────────────────────────────────────────────────────┘

BOTÃO PRIMÁRIO (Verde):
┌─────────────────────────────────────────────────────────────┐
│  ┌───────────────────────────┐                              │
│  │   Adicionar ao Carrinho   │                              │
│  └───────────────────────────┘                              │
│                                                             │
│  background: #548A4C                                        │
│  color: #fff                                                │
│  border-radius: 5px                                         │
│  padding: 0.6rem 1.5rem                                     │
│                                                             │
│  HOVER: background: #3d6838                                 │
└─────────────────────────────────────────────────────────────┘

BOTÃO SECUNDÁRIO (Laranja):
┌─────────────────────────────────────────────────────────────┐
│  ┌───────────────────────────┐                              │
│  │      Continuar Comprando  │                              │
│  └───────────────────────────┘                              │
│                                                             │
│  background: #fff                                           │
│  color: #000                                                │
│  border: 1px solid #fb6f24                                  │
│  border-radius: 5px                                         │
│                                                             │
│  HOVER: background: #fb6f24, color: #fff                    │
└─────────────────────────────────────────────────────────────┘

BOTÃO LOGOUT (Laranja):
┌─────────────────────────────────────────────────────────────┐
│  ┌───────────────────────────┐                              │
│  │          Sair             │                              │
│  └───────────────────────────┘                              │
│                                                             │
│  background: #fb6f24                                        │
│  color: #fff                                                │
│  border-radius: 5px                                         │
│                                                             │
│  HOVER: background: #d95a1a                                 │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                         HEADER                                  │
└─────────────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────────┐
│ ┌──────────┐                              ┌──────────────┐   │
│ │  LOGO    │                              │  NAVEGAÇÃO   │   │
│ │ [IMAGE]  │                              │              │   │
│ │logo.png  │                              │ [Dashboard]  │   │
│ │          │                              │ [Pedidos]    │   │
│ │          │                              │ [Chat]       │   │
│ │          │                              │ 🛒 (3)       │   │
│ │          │                              │ ☀️ 🇧🇷 📊    │   │
│ │          │                              │ [Sair]       │   │
│ └──────────┘                              └──────────────┘   │
└───────────────────────────────────────────────────────────────┘
  background: #548A4C
  border-bottom: 2px solid #fb6f24

ELEMENTOS:
┌─────────────────────────────────────────────────────────────┐
│ • Logo: max-height 60px, max-width 200px                    │
│ • Links: brancos com borda arredondada                      │
│ • Hover: fundo laranja (#fb6f24)                            │
│ • Carrinho: badge circular laranja                          │
│ • Controles: botões brancos arredondados                    │
└─────────────────────────────────────────────────────────────┘
```

### 16.3 Responsividade
```
┌─────────────────────────────────────────────────────────────────┐
│                      BREAKPOINTS                                │
└─────────────────────────────────────────────────────────────────┘

DESKTOP (> 768px):
┌─────────────────────────────────────────────────────────────┐
│ • Grid de serviços: 3 colunas                               │
│ • Header: logo à esquerda, nav à direita                    │
│ • Logo: 60px altura                                         │
│ • Tabelas: largura completa                                 │
└─────────────────────────────────────────────────────────────┘

MOBILE (≤ 768px):
┌─────────────────────────────────────────────────────────────┐
│ @media (max-width: 768px) {                                 │
│     .header-content {                                       │
│         flex-direction: column;                             │
│         gap: 1rem;                                          │
│     }                                                       │
│                                                             │
│     .nav {                                                  │
│         flex-wrap: wrap;                                    │
│         justify-content: center;                            │
│     }                                                       │
│                                                             │
│     .services-grid {                                        │
│         grid-template-columns: 1fr;                         │
│     }                                                       │
│                                                             │
│     .logo img {                                             │
│         max-height: 50px;                                   │
│         max-width: 150px;                                   │
│     }                                                       │
│                                                             │
│     .btn {                                                  │
│         width: 100%;                                        │
│         margin-bottom: 0.5rem;                              │
│     }                                                       │
│ }                                                           │
└─────────────────────────────────────────────────────────────┘
```


---

## 17. FLUXO COMPLETO DO SISTEMA

### 17.1 Jornada do Usuário - Restaurante
```
┌─────────────────────────────────────────────────────────────────┐
│              FLUXO COMPLETO: RESTAURANTE                        │
└─────────────────────────────────────────────────────────────────┘

ETAPA 1: CADASTRO
┌───────────────────────────────────────────────────────────────┐
│ 1. Acessa /auth/register                                      │
│ 2. Preenche formulário:                                       │
│    • Nome do restaurante                                      │
│    • Email                                                    │
│    • WhatsApp                                                 │
│    • Endereço                                                 │
│    • Senha                                                    │
│ 3. Submit → AuthController->register()                        │
│ 4. AuthService valida e cria conta                            │
│ 5. Senha é hasheada (password_hash)                           │
│ 6. Salvo no banco via RestaurantRepository                    │
│ 7. Redireciona para /auth/login                               │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 2: LOGIN
┌───────────────────────────────────────────────────────────────┐
│ 1. Acessa /auth/login                                         │
│ 2. Informa email e senha                                      │
│ 3. AuthService->loginRestaurant()                             │
│ 4. Verifica credenciais no banco                              │
│ 5. Cria sessão:                                               │
│    $_SESSION['restaurant_id'] = id                            │
│    $_SESSION['restaurant_name'] = name                        │
│ 6. Redireciona para /restaurant/dashboard                     │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 3: NAVEGAÇÃO E ESCOLHA DE SERVIÇOS
┌───────────────────────────────────────────────────────────────┐
│ 1. Visualiza dashboard com estatísticas                       │
│ 2. Acessa página inicial (/)                                  │
│ 3. Vê grid com todos os serviços disponíveis                  │
│ 4. Analisa opções:                                            │
│    • Serviços individuais                                     │
│    • Pacotes (com desconto)                                   │
│ 5. Clica em "Adicionar ao Carrinho"                           │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 4: CARRINHO
┌───────────────────────────────────────────────────────────────┐
│ 1. JavaScript envia AJAX para /cart/add                       │
│ 2. CartService->addItem() salva na sessão                     │
│ 3. Badge do carrinho atualiza (mostra quantidade)             │
│ 4. Alerta de sucesso aparece                                  │
│ 5. Repete para outros serviços desejados                      │
│ 6. Clica no ícone do carrinho 🛒                              │
│ 7. Visualiza tabela com todos os itens                        │
│ 8. Vê total calculado                                         │
│ 9. Pode remover itens se desejar                              │
│ 10. Clica em "Finalizar Compra"                               │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 5: CHECKOUT
┌───────────────────────────────────────────────────────────────┐
│ 1. Acessa /cart/checkout                                      │
│ 2. Revisa resumo do pedido                                    │
│ 3. Confirma pedido                                            │
│ 4. CartController cria Order no banco                         │
│ 5. Adiciona order_items                                       │
│ 6. Limpa carrinho                                             │
│ 7. Redireciona para /payment/select?order_id=15               │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 6: SELEÇÃO DE PAGAMENTO
┌───────────────────────────────────────────────────────────────┐
│ 1. Visualiza opções de pagamento:                             │
│    ┌─────────────┐  ┌─────────────┐                           │
│    │     PIX     │  │   CARTÃO    │                           │
│    └─────────────┘  └─────────────┘                           │
│    ┌─────────────┐  ┌─────────────┐                           │
│    │   BOLETO    │  │    CARNÊ    │                           │
│    └─────────────┘  └─────────────┘                           │
│ 2. Escolhe método (ex: PIX)                                   │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 7: PAGAMENTO PIX
┌───────────────────────────────────────────────────────────────┐
│ 1. POST para /payment/pix                                     │
│ 2. EfiPaymentService->createPixCharge()                       │
│ 3. API Efí gera:                                              │
│    • QR Code (imagem)                                         │
│    • Código copia e cola                                      │
│    • txid (ID da transação)                                   │
│ 4. Exibe na tela:                                             │
│    ┌─────────────────────────────────────────────┐            │
│    │         [QR CODE IMAGE]                     │            │
│    │                                             │            │
│    │  Escaneie com seu app de pagamento          │            │
│    │                                             │            │
│    │  Código: 00020126580014br.gov.bcb...        │            │
│    │  [Copiar]                                   │            │
│    │                                             │            │
│    │  Valor: R$ 299,99                           │            │
│    └─────────────────────────────────────────────┘            │
│ 5. Restaurante paga via PIX                                   │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 8: CONFIRMAÇÃO DE PAGAMENTO
┌───────────────────────────────────────────────────────────────┐
│ 1. Efí Bank detecta pagamento                                 │
│ 2. Envia webhook para /public/webhook/payment.php             │
│ 3. Webhook processa:                                          │
│    • Identifica pedido pelo txid                              │
│    • Atualiza payment_status = 'paid'                         │
│    • Atualiza status = 'processing'                           │
│ 4. NotificationService notifica (opcional)                    │
│ 5. Dashboard atualiza automaticamente                         │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 9: ACOMPANHAMENTO
┌───────────────────────────────────────────────────────────────┐
│ 1. Acessa /restaurant/orders                                  │
│ 2. Visualiza lista de pedidos:                                │
│    ┌─────────────────────────────────────────────┐            │
│    │ Pedido #15                                  │            │
│    │ Status: Em Processamento                    │            │
│    │ Pagamento: Pago (PIX)                       │            │
│    │ Total: R$ 299,99                            │            │
│    │ Data: 15/01/2024 10:30                      │            │
│    └─────────────────────────────────────────────┘            │
│ 3. Pode acompanhar mudanças de status                         │
│ 4. Recebe serviços quando status = 'completed'                │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 10: SUPORTE VIA CHAT
┌───────────────────────────────────────────────────────────────┐
│ 1. Acessa /restaurant/chat                                    │
│ 2. Visualiza histórico de mensagens                           │
│ 3. Envia mensagem para admin                                  │
│ 4. Chat atualiza a cada 3 segundos (polling)                  │
│ 5. Recebe respostas do admin em tempo real                    │
│ 6. Pode solicitar alterações ou tirar dúvidas                 │
└───────────────────────────────────────────────────────────────┘
```

### 17.2 Jornada do Usuário - Administrador
```
┌─────────────────────────────────────────────────────────────────┐
│              FLUXO COMPLETO: ADMINISTRADOR                      │
└─────────────────────────────────────────────────────────────────┘

ETAPA 1: LOGIN ADMIN
┌───────────────────────────────────────────────────────────────┐
│ 1. Acessa /auth/login                                         │
│ 2. Seleciona tipo: "Administrador"                            │
│ 3. Credenciais:                                               │
│    Email: admin@restify.com                                   │
│    Senha: admin123                                            │
│ 4. AuthService->loginAdmin()                                  │
│ 5. Cria sessão admin                                          │
│ 6. Redireciona para /admin/dashboard                          │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 2: DASHBOARD ADMINISTRATIVO
┌───────────────────────────────────────────────────────────────┐
│ 1. Visualiza estatísticas:                                    │
│    ┌──────────┐  ┌──────────┐  ┌──────────┐                   │
│    │  Total   │  │  Pedidos │  │Restaurantes│                 │
│    │  Pedidos │  │  Hoje    │  │Cadastrados │                 │
│    │   150    │  │    12    │  │     45     │                 │
│    └──────────┘  └──────────┘  └──────────┘                   │
│                                                               │
│ 2. Vê pedidos recentes                                        │
│ 3. Acessa ações rápidas                                       │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 3: GERENCIAMENTO DE PEDIDOS
┌───────────────────────────────────────────────────────────────┐
│ 1. Acessa /admin/orders                                       │
│ 2. Visualiza todos os pedidos do sistema                      │
│ 3. Filtra por status, data, restaurante                       │
│ 4. Para cada pedido pode:                                     │
│    • Ver detalhes completos                                   │
│    • Atualizar status:                                        │
│      - pending → processing                                   │
│      - processing → completed                                 │
│      - qualquer → cancelled                                   │
│ 5. Exporta relatório em CSV                                   │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 4: GERENCIAMENTO DE SERVIÇOS
┌───────────────────────────────────────────────────────────────┐
│ 1. Acessa /admin/services                                     │
│ 2. Visualiza lista de serviços                                │
│ 3. Pode criar novo serviço:                                   │
│    • Nome                                                     │
│    • Descrição                                                │
│    • Preço                                                    │
│    • Tipo (individual/package)                                │
│ 4. Pode editar serviço existente                              │
│ 5. Pode deletar serviço                                       │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 5: GERENCIAMENTO DE RESTAURANTES
┌───────────────────────────────────────────────────────────────┐
│ 1. Acessa /admin/restaurants                                  │
│ 2. Visualiza lista de todos os restaurantes                   │
│ 3. Vê informações:                                            │
│    • Nome                                                     │
│    • Email                                                    │
│    • WhatsApp                                                 │
│    • Endereço                                                 │
│    • Data de cadastro                                         │
│    • Total de pedidos                                         │
│ 4. Exporta lista em CSV                                       │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 6: CHAT COM RESTAURANTES
┌───────────────────────────────────────────────────────────────┐
│ 1. Acessa /admin/chat                                         │
│ 2. Visualiza lista de restaurantes com mensagens              │
│ 3. Seleciona restaurante                                      │
│ 4. Acessa /admin/chat/{id}                                    │
│ 5. Vê histórico de mensagens                                  │
│ 6. Envia respostas                                            │
│ 7. Chat atualiza automaticamente (polling)                    │
│ 8. Pode atender múltiplos restaurantes                        │
└───────────────────────────────────────────────────────────────┘
                        ↓
ETAPA 7: EXPORTAÇÃO DE DADOS
┌───────────────────────────────────────────────────────────────┐
│ 1. Clica no botão de exportação (📊)                          | 
│ 2. Seleciona tipo:                                            │
│    • Exportar Pedidos                                         │
│    • Exportar Restaurantes                                    │
│ 3. SettingsController->exportOrders() ou                      │
│    SettingsController->exportRestaurants()                    │
│ 4. ExportService gera CSV                                     │
│ 5. Download automático do arquivo                             │
└───────────────────────────────────────────────────────────────┘
```

### 17.3 Diagrama de Fluxo Geral
```
┌─────────────────────────────────────────────────────────────────┐
│                    DIAGRAMA DE FLUXO GERAL                      │
└─────────────────────────────────────────────────────────────────┘

                    ┌──────────────┐
                    │   USUÁRIO    │
                    │  (Browser)   │
                    └──────┬───────┘
                           │
                ┌──────────┴──────────┐
                │                     │
                ▼                     ▼
        ┌──────────────┐      ┌──────────────┐
        │  VISITANTE   │      │   LOGADO     │
        └──────┬───────┘      └──────┬───────┘
               │                     │
               │              ┌──────┴──────┐
               │              │             │
               │              ▼             ▼
               │      ┌────────────┐ ┌────────────┐
               │      │RESTAURANTE │ │   ADMIN    │
               │      └─────┬──────┘ └─────┬──────┘
               │            │              │
               ▼            ▼              ▼
        ┌──────────┐ ┌──────────┐  ┌──────────┐
        │   HOME   │ │DASHBOARD │  │DASHBOARD │
        │          │ │RESTAURANTE│  │  ADMIN   │
        └────┬─────┘ └────┬─────┘  └────┬─────┘
             │            │              │
             │            │              │
        ┌────┴────┐  ┌────┴────┐   ┌────┴────┐
        │SERVIÇOS │  │CARRINHO │   │PEDIDOS  │
        └────┬────┘  └────┬────┘   └────┬────┘
             │            │              │
             │            ▼              │
             │      ┌──────────┐         │
             │      │CHECKOUT  │         │
             │      └────┬─────┘         │
             │           │               │
             │           ▼               │
             │      ┌──────────┐         │
             │      │PAGAMENTO │         │
             │      └────┬─────┘         │
             │           │               │
             │           ▼               │
             │      ┌──────────┐         │
             │      │WEBHOOK   │         │
             │      └────┬─────┘         │
             │           │               │
             └───────────┴───────────────┘
                         │
                         ▼
                  ┌──────────────┐
                  │   BANCO DE   │
                  │    DADOS     │
                  └──────────────┘
```

---

## 18. CONCLUSÃO E PRÓXIMOS PASSOS

### 18.1 Resumo do Sistema
```
┌─────────────────────────────────────────────────────────────────┐
│                      RESUMO EXECUTIVO                           │
└─────────────────────────────────────────────────────────────────┘

O RESTIFY é um sistema completo de serviços digitais para
restaurantes, desenvolvido com:

✓ Arquitetura MVC bem estruturada
✓ Design Patterns (Singleton, Repository, Factory, Observer, Strategy)
✓ Integração real com gateway de pagamento (Efí Bank)
✓ Sistema de carrinho de compras funcional
✓ Chat em tempo real (polling)
✓ Internacionalização (PT, EN, ES)
✓ Temas claro/escuro
✓ Design responsivo e moderno
✓ Exportação de dados (CSV)
✓ Sistema de autenticação robusto
✓ Código limpo e bem documentado
```

### 18.2 Melhorias Futuras
```
┌─────────────────────────────────────────────────────────────────┐
│                    ROADMAP DE MELHORIAS                         │
└─────────────────────────────────────────────────────────────────┘

CURTO PRAZO:
┌─────────────────────────────────────────────────────────────┐
│ • Implementar WebSocket para chat real-time                 │
│ • Adicionar upload de logo do restaurante                   │
│ • Sistema de notificações push                              │
│ • Recuperação de senha por email                            │
│ • Validação de email no cadastro                            │
└─────────────────────────────────────────────────────────────┘

MÉDIO PRAZO:
┌─────────────────────────────────────────────────────────────┐
│ • API RESTful para integrações                              │
│ • App mobile (React Native/Flutter)                         │
│ • Sistema de avaliações e reviews                           │
│ • Dashboard com gráficos interativos                        │
│ • Relatórios avançados (PDF)                                │
└─────────────────────────────────────────────────────────────┘

LONGO PRAZO:
┌─────────────────────────────────────────────────────────────┐
│ • Migração para PostgreSQL/MySQL                            │
│ • Microserviços                                             │
│ • Sistema de recomendação (IA)                              │
│ • Integração com mais gateways de pagamento                 │
│ • Marketplace de templates                                  │
└─────────────────────────────────────────────────────────────┘
```

---

## 📚 REFERÊNCIAS E RECURSOS

### Documentação Oficial
- PHP: https://www.php.net/docs.php
- SQLite: https://www.sqlite.org/docs.html
- Efí Bank API: https://dev.efipay.com.br/docs

### Design Patterns
- Repository Pattern
- Factory Pattern
- Singleton Pattern
- Observer Pattern
- Strategy Pattern

### Ferramentas Utilizadas
- Composer (Gerenciador de dependências)
- Git (Controle de versão)
- VS Code (Editor recomendado)

---

**FIM DA DOCUMENTAÇÃO TÉCNICA COMPLETA**

*Documento criado em: 15/11/2025
*Versão do Sistema: 1.0.0*
*Desenvolvido por: Equipe Restify*

