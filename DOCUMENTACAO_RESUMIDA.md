# DOCUMENTACAO RESUMIDA - RESTIFY

Sistema de Servicos Digitais para Restaurantes

---

## VISAO GERAL

Sistema web PHP com arquitetura MVC para oferecer servicos digitais para restaurantes.

**Stack**: PHP 7.4+, SQLite, JavaScript Vanilla, Efi Bank SDK

**Funcionalidades**: Autenticacao, Carrinho, Pagamentos (PIX/Cartao/Boleto/Carne), Chat, i18n, Temas

---

## ARQUITETURA MVC

```
Browser -> index.php -> Controllers -> Services -> Repositories -> Database
                            |
                          Views
```

**Design Patterns**: Singleton, Repository, Factory, Observer, Strategy

---

## ESTRUTURA DE DIRETORIOS

```
RestifyApp/
├── app/
│   ├── controllers/     # HomeController, AuthController, CartController, PaymentController, 
│   │                    # RestaurantController, AdminController, SettingsController
│   ├── models/          # Restaurant, Service, Order, Message
│   ├── repositories/    # RestaurantRepository, ServiceRepository, OrderRepository, MessageRepository
│   ├── services/        # AuthService, CartService, EfiPaymentService, ExportService, 
│   │                    # NotificationService, PaymentServiceFactory
│   └── views/           # home.php, auth/, cart/, payment/, restaurant/, admin/, layout/
├── config/              # database.php, config.php, i18n.php, efi_credentials.php, certificates/
├── database/            # restify.db, schema.sql
├── lang/                # pt.php, en.php, es.php
└── public/              # index.php, css/, js/, webhook/
```

---

## BANCO DE DADOS

### Schema

```
restaurants
├── id (PK)
├── name
├── email (UNIQUE)
├── whatsapp
├── address
├── password (hash)
└── created_at

orders
├── id (PK)
├── restaurant_id (FK)
├── total_amount
├── status (pending/processing/completed/cancelled)
├── payment_method (pix/credit_card/boleto/carne)
├── payment_id
├── payment_status (pending/paid/failed)
├── created_at
└── updated_at

order_items
├── id (PK)
├── order_id (FK)
├── service_id (FK)
├── quantity
└── price

services
├── id (PK)
├── name
├── description
├── price
└── type (individual/package)

messages
├── id (PK)
├── restaurant_id (FK)
├── sender_type (restaurant/admin)
├── message
└── created_at
```

### Relacionamentos

- Restaurant 1:N Orders
- Restaurant 1:N Messages
- Order 1:N OrderItems
- OrderItems N:1 Service

---

## CONTROLLERS

### HomeController
- **index()**: Lista todos os servicos disponiveis na home

### AuthController
- **login()**: GET exibe form, POST processa credenciais
- **register()**: GET exibe form, POST cria restaurante
- **logout()**: Destroi sessao e redireciona

### CartController
- **index()**: Exibe carrinho com itens e total
- **add()**: POST AJAX adiciona item ao carrinho
- **remove()**: POST remove item do carrinho
- **checkout()**: Cria pedido e redireciona para pagamento

### PaymentController
- **selectMethod()**: Exibe opcoes de pagamento
- **processPix()**: Gera cobranca PIX (QR Code)
- **processCreditCard()**: Processa pagamento com cartao
- **generateBoleto()**: Gera boleto bancario
- **generateCarne()**: Gera carne parcelado

### RestaurantController
- **dashboard()**: Estatisticas do restaurante
- **orders()**: Lista pedidos do restaurante
- **chat()**: Chat com admin

### AdminController
- **dashboard()**: Estatisticas gerais do sistema
- **orders()**: Lista todos os pedidos
- **updateOrderStatus()**: Atualiza status de pedido
- **restaurants()**: Lista todos os restaurantes
- **services()**: CRUD de servicos
- **chat()**: Chat com restaurantes

### SettingsController
- **changeLanguage()**: Altera idioma (PT/EN/ES)
- **changeTheme()**: Altera tema (claro/escuro)
- **exportOrders()**: Exporta pedidos em CSV
- **exportRestaurants()**: Exporta restaurantes em CSV

---

## MODELS

### Restaurant
```php
class Restaurant {
    public $id;
    public $name;
    public $email;
    public $whatsapp;
    public $address;
    public $password;
    public $created_at;
}
```

### Service
```php
class Service {
    public $id;
    public $name;
    public $description;
    public $price;
    public $type; // individual ou package
}
```

### Order
```php
class Order {
    public $id;
    public $restaurant_id;
    public $total_amount;
    public $status;
    public $payment_method;
    public $payment_id;
    public $payment_status;
    public $created_at;
    public $updated_at;
    public $items = [];
}
```

### Message
```php
class Message {
    public $id;
    public $restaurant_id;
    public $sender_type; // restaurant ou admin
    public $message;
    public $created_at;
}
```

---

## SERVICES

### AuthService
- **loginRestaurant($email, $password)**: Autentica restaurante
- **loginAdmin($email, $password)**: Autentica admin
- **register($data)**: Cria novo restaurante
- **logout()**: Destroi sessao

### CartService
- **addItem($serviceId, $quantity)**: Adiciona item ao carrinho (sessao)
- **removeItem($serviceId)**: Remove item do carrinho
- **getItems()**: Retorna array de itens
- **getTotal()**: Calcula total do carrinho
- **clear()**: Limpa carrinho
- **getItemCount()**: Retorna quantidade total de itens

### EfiPaymentService
- **createPixCharge($order)**: Gera cobranca PIX (retorna QR Code e codigo)
- **processCardPayment($order, $cardData)**: Processa pagamento com cartao
- **generateBoleto($order)**: Gera boleto (retorna PDF e codigo de barras)
- **generateCarne($order, $installments)**: Gera carne parcelado

### ExportService (Strategy Pattern)
- **exportToCSV($data, $filename)**: Exporta dados em CSV
- **exportToJSON($data, $filename)**: Exporta dados em JSON
- **exportToXML($data, $filename)**: Exporta dados em XML

### NotificationService (Observer Pattern)
- **attach($observer)**: Adiciona observador
- **detach($observer)**: Remove observador
- **notify($event, $data)**: Notifica todos os observadores
- **sendEmail($to, $subject, $message)**: Envia email
- **sendSMS($phone, $message)**: Envia SMS

### PaymentServiceFactory (Factory Pattern)
- **create($type)**: Cria instancia de servico de pagamento baseado no tipo

---

## REPOSITORIES

### RestaurantRepository
- **create($restaurant)**: Cria novo restaurante
- **findByEmail($email)**: Busca por email
- **findById($id)**: Busca por ID
- **getAll()**: Lista todos
- **getTotalCount()**: Conta total
- **getOrdersCount($id)**: Conta pedidos do restaurante

### ServiceRepository
- **getAll()**: Lista todos os servicos
- **getById($id)**: Busca servico por ID
- **create($service)**: Cria novo servico
- **update($service)**: Atualiza servico
- **delete($id)**: Deleta servico
- **getByIds($ids)**: Busca multiplos servicos por IDs

### OrderRepository
- **create($order)**: Cria pedido (com transacao para order_items)
- **addItem($orderId, $item)**: Adiciona item ao pedido
- **getById($id)**: Busca pedido por ID
- **getByRestaurant($restaurantId)**: Lista pedidos do restaurante
- **getAll()**: Lista todos os pedidos
- **updateStatus($id, $status)**: Atualiza status
- **updatePaymentStatus($id, $status, $paymentId)**: Atualiza status de pagamento
- **getTotalRevenue()**: Calcula receita total
- **getRecentOrders($limit)**: Lista pedidos recentes

### MessageRepository
- **create($message)**: Cria mensagem
- **getByRestaurant($restaurantId)**: Lista mensagens do restaurante
- **getRestaurantsWithMessages()**: Lista restaurantes com mensagens
- **markAsRead($restaurantId)**: Marca mensagens como lidas

---

## SISTEMA DE ROTAS

### Publicas
```
GET  /                          -> HomeController@index
GET  /auth/login                -> AuthController@login
POST /auth/login                -> AuthController@login
GET  /auth/register             -> AuthController@register
POST /auth/register             -> AuthController@register
GET  /auth/logout               -> AuthController@logout
GET  /cart                      -> CartController@index
POST /cart/add                  -> CartController@add
POST /cart/remove               -> CartController@remove
```

### Restaurante (requer autenticacao)
```
GET  /restaurant/dashboard      -> RestaurantController@dashboard
GET  /restaurant/orders         -> RestaurantController@orders
GET  /restaurant/chat           -> RestaurantController@chat
POST /restaurant/chat           -> RestaurantController@sendMessage
GET  /cart/checkout             -> CartController@checkout
POST /cart/checkout             -> CartController@checkout
```

### Pagamentos (requer autenticacao)
```
GET  /payment/select            -> PaymentController@selectMethod
POST /payment/pix               -> PaymentController@processPix
POST /payment/credit-card       -> PaymentController@processCreditCard
POST /payment/boleto            -> PaymentController@generateBoleto
POST /payment/carne             -> PaymentController@generateCarne
```

### Admin (requer autenticacao admin)
```
GET  /admin/dashboard           -> AdminController@dashboard
GET  /admin/orders              -> AdminController@orders
POST /admin/orders              -> AdminController@updateOrderStatus
GET  /admin/restaurants         -> AdminController@restaurants
GET  /admin/services            -> AdminController@services
POST /admin/services            -> AdminController@createService
GET  /admin/services/edit/{id}  -> AdminController@editService
POST /admin/services/edit/{id}  -> AdminController@updateService
GET  /admin/chat                -> AdminController@chat
GET  /admin/chat/{id}           -> AdminController@chatWithRestaurant
POST /admin/chat/{id}           -> AdminController@sendMessage
```

### Configuracoes
```
POST /settings/language         -> SettingsController@changeLanguage
POST /settings/theme            -> SettingsController@changeTheme
GET  /export/orders             -> SettingsController@exportOrders
GET  /export/restaurants        -> SettingsController@exportRestaurants
```

### Webhook
```
POST /webhook/payment.php       -> Processa notificacoes Efi Bank
```

---

## FLUXO DE COMPRA COMPLETO

### 1. Visualizacao de Servicos
```
Usuario -> Home (/) -> Lista servicos -> Clica "Adicionar ao Carrinho"
```

### 2. Adicionar ao Carrinho
```
JavaScript -> AJAX POST /cart/add -> CartService->addItem() -> Sessao atualizada
-> Badge carrinho atualizado -> Alerta de sucesso
```

### 3. Visualizar Carrinho
```
Usuario -> Carrinho (/cart) -> CartController->index() -> Busca itens da sessao
-> Calcula total -> Renderiza tabela
```

### 4. Checkout
```
Usuario -> "Finalizar Compra" -> Verifica login -> CartController->checkout()
-> Cria Order no banco -> Adiciona order_items -> Limpa carrinho
-> Redireciona /payment/select
```

### 5. Selecao de Pagamento
```
Usuario -> Escolhe metodo (PIX/Cartao/Boleto/Carne) -> POST para endpoint especifico
```

### 6. Pagamento PIX
```
PaymentController->processPix() -> EfiPaymentService->createPixCharge()
-> API Efi retorna QR Code + codigo -> Exibe para usuario
-> Usuario paga -> Efi envia webhook -> Atualiza order.payment_status = 'paid'
-> Atualiza order.status = 'processing'
```

### 7. Acompanhamento
```
Usuario -> /restaurant/orders -> Lista pedidos com status atualizado
```

---

## SISTEMA DE PAGAMENTOS (EFI BANK)

### Configuracao
```php
// config/efi_credentials.php
return [
    'client_id' => 'Client_Id_...',
    'client_secret' => 'Client_Secret_...',
    'certificate' => __DIR__.'/certificates/producao.p12',
    'sandbox' => false
];
```

### Fluxo PIX
```
1. Cria cobranca PIX
2. Retorna: QR Code (base64), codigo copia-e-cola, txid
3. Exibe para usuario
4. Usuario paga
5. Webhook recebe confirmacao
6. Atualiza pedido
```

### Fluxo Cartao
```
1. Usuario preenche dados do cartao
2. Valida no frontend (Luhn, CVV, CPF)
3. Envia para Efi
4. Resposta imediata (aprovado/recusado)
5. Atualiza pedido
```

### Webhook
```php
// public/webhook/payment.php
$input = file_get_contents('php://input');
$data = json_decode($input, true);

if ($data['evento'] === 'pix') {
    $txid = $data['pix'][0]['txid'];
    // Busca pedido pelo txid
    // Atualiza payment_status = 'paid'
    // Atualiza status = 'processing'
}
```

---

## SISTEMA DE CHAT

### Estrutura
- Polling a cada 3 segundos
- Mensagens armazenadas no banco
- sender_type: 'restaurant' ou 'admin'

### Fluxo Restaurante
```
1. Acessa /restaurant/chat
2. JavaScript inicia polling (setInterval 3s)
3. Fetch /restaurant/messages
4. Atualiza DOM com novas mensagens
5. Usuario envia mensagem -> POST /restaurant/chat
6. Mensagem salva no banco
```

### Fluxo Admin
```
1. Acessa /admin/chat
2. Lista restaurantes com mensagens
3. Seleciona restaurante -> /admin/chat/{id}
4. Polling busca mensagens daquele restaurante
5. Admin responde -> POST /admin/chat/{id}
```

---

## INTERNACIONALIZACAO (i18n)

### Estrutura
```php
// config/i18n.php
class I18n {
    private static $language = 'pt';
    private static $translations = [];
    
    public static function t($key, $params = []) {
        $text = self::$translations[$key] ?? $key;
        foreach ($params as $k => $v) {
            $text = str_replace('{'.$k.'}', $v, $text);
        }
        return $text;
    }
}

function t($key, $params = []) {
    return I18n::t($key, $params);
}
```

### Uso nas Views
```php
<h1><?= t('welcome') ?></h1>
<button><?= t('add_to_cart') ?></button>
<p><?= t('welcome_user', ['name' => $userName]) ?></p>
```

### Arquivos de Traducao
```php
// lang/pt.php
return [
    'welcome' => 'Bem-vindo',
    'add_to_cart' => 'Adicionar ao Carrinho',
    'welcome_user' => 'Bem-vindo, {name}!'
];

// lang/en.php
return [
    'welcome' => 'Welcome',
    'add_to_cart' => 'Add to Cart',
    'welcome_user' => 'Welcome, {name}!'
];
```

### Mudanca de Idioma
```
Usuario -> Seleciona idioma -> POST /settings/language
-> $_SESSION['language'] = $lang -> Reload pagina
-> I18n::init() carrega novo idioma
```

---

## DESIGN SYSTEM

### Paleta de Cores
```
Principal: #548A4C (verde)
- Header, footer, botoes primarios, precos

Destaque: #fb6f24 (laranja)
- Bordas de cards, badges, botao logout, hover

Tema Claro:
- Fundo: #ffffff
- Cards: #f2f2f2
- Texto: #000000
- Texto secundario: #666666

Tema Escuro:
- Fundo: #000000
- Cards: #1a1a1a
- Texto: #ffffff
- Texto secundario: #cccccc
```

### Componentes

#### Card de Servico
```
- Background: #f2f2f2 (claro) / #1a1a1a (escuro)
- Border-radius: 15px
- Border-right: 4px solid #fb6f24
- Border-bottom: 4px solid #fb6f24
- Box-shadow: 0 4px 8px rgba(0,0,0,0.1)
- Hover: translateY(-5px)
```

#### Botoes
```
Primario (Verde):
- Background: #548A4C
- Color: #fff
- Hover: #3d6838

Secundario (Laranja):
- Background: #fff
- Border: 1px solid #fb6f24
- Hover: background #fb6f24, color #fff

Logout (Laranja):
- Background: #fb6f24
- Color: #fff
- Hover: #d95a1a
```

#### Header
```
- Background: #548A4C
- Border-bottom: 2px solid #fb6f24
- Logo: max-height 60px
- Links: brancos com hover laranja
- Badge carrinho: circular laranja
```

### Responsividade
```css
@media (max-width: 768px) {
    .services-grid {
        grid-template-columns: 1fr;
    }
    .header-content {
        flex-direction: column;
    }
    .logo img {
        max-height: 50px;
    }
}
```

---

## JAVASCRIPT (app.js)

### Funcoes Principais

#### addToCart(serviceId, quantity)
```javascript
function addToCart(serviceId, quantity = 1) {
    fetch(BASE_URL + '/cart/add', {
        method: 'POST',
        body: `service_id=${serviceId}&quantity=${quantity}`
    })
    .then(res => res.json())
    .then(data => {
        if (data.success) {
            updateCartCount(data.count);
            showAlert('Item adicionado com sucesso!');
        }
    });
}
```

#### toggleTheme()
```javascript
function toggleTheme() {
    document.body.classList.toggle('dark-theme');
    const theme = document.body.classList.contains('dark-theme') ? 'dark' : 'light';
    localStorage.setItem('theme', theme);
    
    fetch(BASE_URL + '/settings/theme', {
        method: 'POST',
        body: `theme=${theme}`
    });
}
```

#### startChatPolling(restaurantId)
```javascript
function startChatPolling(restaurantId = null) {
    const url = restaurantId 
        ? `/admin/messages/${restaurantId}`
        : '/restaurant/messages';
    
    setInterval(() => {
        fetch(BASE_URL + url)
            .then(res => res.json())
            .then(messages => updateChatMessages(messages));
    }, 3000);
}
```

#### changeLanguage(language)
```javascript
function changeLanguage(language) {
    fetch(BASE_URL + '/settings/language', {
        method: 'POST',
        body: `language=${language}`
    }).then(() => location.reload());
}
```

---

## SEGURANCA

### Autenticacao
- Senhas hasheadas com `password_hash(PASSWORD_DEFAULT)`
- Verificacao com `password_verify()`
- Sessoes seguras com `session_start()`

### Validacao de Entrada
```php
// Sanitizacao
$email = filter_var($_POST['email'], FILTER_SANITIZE_EMAIL);

// Validacao
if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
    // erro
}

// Prepared Statements
$stmt = $db->prepare("SELECT * FROM restaurants WHERE email = ?");
$stmt->execute([$email]);
```

### Protecao de Rotas
```php
function isLoggedIn() {
    return isset($_SESSION['restaurant_id']) || isset($_SESSION['admin']);
}

function isAdmin() {
    return isset($_SESSION['admin']) && $_SESSION['admin'] === true;
}

// Uso
if (!isLoggedIn()) {
    header('Location: /auth/login');
    exit;
}
```

### Webhook Efi
- Validar origem da requisicao
- Verificar assinatura (se disponivel)
- Log de todas as requisicoes
- Validar dados antes de processar

---

## INSTALACAO

### Pre-requisitos
- PHP 7.4+
- Apache/Nginx com mod_rewrite
- SQLite (incluido no PHP)
- Composer

### Passos
```bash
# 1. Clone
git clone https://github.com/usuario/RestifyApp.git
cd RestifyApp

# 2. Instale dependencias
composer install

# 3. Configure servidor
# Apache: DocumentRoot -> public/
# Nginx: root -> public/

# 4. Acesse
http://localhost/RestifyApp/public

# 5. Login admin
Email: admin@restify.com
Senha: admin123
```

### Configuracao Efi Bank
```
1. Obtenha credenciais em https://sejaefi.com.br
2. Baixe certificado .p12
3. Coloque em config/certificates/
4. Atualize config/efi_credentials.php
5. Configure webhook URL no painel Efi
```

---

## DADOS INICIAIS

### Admin
```
ID: 999
Email: admin@restify.com
Senha: admin123
```

### Servicos Padrao
```
1. Site com Hospedagem      - R$ 299,99 (individual)
2. Instagram + 5 Posts       - R$ 199,99 (individual)
3. Google Maps + QR Codes    - R$ 149,99 (individual)
4. Cardapio Online           - R$ 99,99  (individual)
5. Pacote Basico             - R$ 449,99 (package)
6. Pacote Completo           - R$ 649,99 (package)
```

---

## PROXIMOS PASSOS

### Curto Prazo
- WebSocket para chat real-time
- Upload de logo do restaurante
- Sistema de notificacoes push
- Recuperacao de senha por email
- Validacao de email no cadastro

### Medio Prazo
- API RESTful para integracoes
- App mobile (React Native/Flutter)
- Sistema de avaliacoes e reviews
- Dashboard com graficos interativos (Chart.js)
- Relatorios avancados (PDF)

### Longo Prazo
- Migracao para PostgreSQL/MySQL
- Arquitetura de microservicos
- Sistema de recomendacao com IA
- Integracao com mais gateways de pagamento
- Marketplace de templates

---

## ESTATISTICAS DO PROJETO

```
Controllers:        7
Models:             4
Repositories:       4
Services:           6
Views:              20+
Tabelas DB:         5
Design Patterns:    5
Idiomas:            3
Metodos Pagamento:  4
Linhas de Codigo:   ~5000
```

---

## REFERENCIAS

- **PHP**: https://www.php.net/docs.php
- **SQLite**: https://www.sqlite.org/docs.html
- **Efi Bank**: https://dev.efipay.com.br/docs
- **Composer**: https://getcomposer.org/doc/

---

**Equipe Restify**
- Artur Braga de Azevedo - 22302123
- Lucas Ferreira Santos - 22301410
- Julia Braga Lemos - 22302344 (DPO)
- Julio Andrade Nogueira - 22300988
- Bernardo de Oliveira Andrade - 22300783
- Matheus Pedro Procopio - 22403337

*Versao 1.0.0 - Janeiro 2025*
