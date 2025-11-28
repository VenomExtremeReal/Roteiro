# 🎤 APRESENTAÇÃO RESTIFY - Roteiro (7 minutos)

---

## 1️⃣ INTRODUÇÃO (30 seg)

**Nome do projeto:** Restify  
**Turma:** Engenharia de Software  
**Integrantes:**
- Artur Braga de Azevedo - 22302123
- Lucas Ferreira Santos - 22301410
- Júlia Braga Lemos - 22302344
- Júlio Andrade Nogueira - 22300988
- Bernardo de Oliveira Andrade - 22300783
- Matheus Pedro Procopio - 22403337

**Resumo:**
"O Restify é um sistema web que permite pequenos restaurantes contratarem serviços de digitalização como criação de sites, gerenciamento de redes sociais e cardápios online, resolvendo o problema da falta de presença digital."

---

## 2️⃣ MODELAGEM DE DOMÍNIO (1 min)

**Mostrar:** Diagrama de classes

**Falar:**
"Nosso modelo de domínio reflete o problema real. Temos 4 entidades principais:

- **Restaurant:** Representa o cliente, com dados de cadastro e autenticação
- **Service:** Serviços digitais oferecidos (sites, redes sociais, cardápios)
- **Order:** Pedidos realizados pelos restaurantes
- **Message:** Sistema de chat para suporte

As relações são:
- Restaurant **tem muitos** Orders (composição)
- Restaurant **tem muitas** Messages (composição)
- Order **contém muitos** Services (agregação)

Montamos o modelo desta forma porque reflete exatamente o fluxo de negócio: restaurante contrata serviços, gera pedidos e se comunica via chat."

---

## 3️⃣ CASOS DE USO E DEMONSTRAÇÃO (2 min)

**Mostrar:** Diagrama de casos de uso

**Explicar:**
"Nosso sistema possui 11 casos de uso principais envolvendo restaurantes e administradores.

Vamos demonstrar 2 casos de uso funcionando ao vivo:

### **Caso de Uso 1: Chat com Suporte**

**Ator:** Restaurante

**Pré-condições:** Restaurante autenticado

**Fluxo Principal:**
1. Restaurante acessa menu "Chat"
2. Sistema exibe histórico de mensagens
3. Restaurante digita mensagem no campo de texto
4. Restaurante clica em "Enviar"
5. Sistema salva mensagem no banco via Repository
6. Sistema exibe mensagem na tela com animação
7. Admin recebe notificação de nova mensagem
8. Sistema atualiza chat automaticamente (polling a cada 3s)
9. Admin responde pelo painel administrativo
10. Restaurante vê resposta em tempo real

**Fluxos Alternativos:**
- **3a.** Mensagem vazia: Sistema não permite envio
- **8a.** Erro no polling: Sistema continua funcionando, tenta novamente

**Pós-condições:** Mensagem registrada e visível para ambas as partes

**Demonstração ao vivo:**
[Login restaurante → Chat → Enviar mensagem → Abrir painel admin → Responder → Mostrar atualização automática]

---

### **Caso de Uso 2: Contratar Serviço**

**Ator:** Restaurante

**Pré-condições:** Restaurante autenticado

**Fluxo Principal:**
1. Restaurante visualiza catálogo de serviços
2. Seleciona serviço desejado (ex: Site + Instagram)
3. Clica em "Adicionar ao Carrinho"
4. Sistema adiciona item ao carrinho (sessão)
5. Restaurante acessa carrinho
6. Clica em "Finalizar Pedido"
7. Sistema exibe opções de pagamento:
   - PIX (integração Efí Bank)
   - Cartão de crédito
   - Boleto
   - Carnê
8. Restaurante escolhe PIX
9. Sistema gera QR Code

**Fluxos Alternativos:**
- **3a.** Carrinho vazio: Sistema exibe mensagem
- **9a.** Erro na geração PIX: Sistema oferece outras formas

**Pós-condições:** Pedido criado, aguardando pagamento

**Demonstração ao vivo:**
[Login → Catálogo → Adicionar carrinho → Checkout → Escolher PIX → Mostrar QR Code → Dashboard com pedido]

---

**Sistema em homologação:** https://restify.onrender.com

---

## 4️⃣ ARQUITETURA MVC + REPOSITORY (2 min)

**Mostrar:** Estrutura de pastas do projeto

**Falar:**
"Nossa arquitetura segue MVC com Repository Pattern. A estrutura é:

```
app/
├── controllers/  (7 controllers)
├── models/       (4 models)
├── repositories/ (4 repositories)
├── services/     (6 services)
└── views/        (interface)
```

Vou demonstrar o fluxo completo de **Chat com Suporte**:

1. **View** (chat.php): Usuário digita mensagem
2. **Controller** (RestaurantController): Recebe POST da mensagem
3. **Repository** (MessageRepository): Salva mensagem no banco
4. **Model** (Message): Representa a entidade
5. **JavaScript** (app.js): Polling busca novas mensagens a cada 3s
6. **View**: Atualiza chat automaticamente

As camadas estão completamente separadas:
- Controller coordena o fluxo
- Repository acessa o banco de dados
- Model representa a regra de negócio
- Service implementa lógica complexa

Além disso, implementamos 5 Design Patterns GoF:
- **Singleton** no Database (única conexão)
- **Repository** para persistência
- **Factory** para criar serviços de pagamento
- **Observer** para notificações
- **Strategy** para exportação de dados"

---

## 5️⃣ FUNCIONALIDADES E ÉTICA (1 min)

**Mostrar:** Lista de funcionalidades + Termos

**Falar:**
"Implementamos 20 requisitos funcionais completos:

**Autenticação:**
1. Cadastro de restaurantes
2. Login/Logout
3. Perfis diferenciados (Admin/Restaurante)

**Serviços:**
4. Catálogo de serviços
5. Carrinho de compras
6. Sistema de pedidos
7. Acompanhamento de status

**Pagamentos:**
8. PIX (estrutura completa implementada, simulação por falta de CNPJ)
9. Cartão de crédito
10. Boleto bancário
11. Carnê parcelado

**IMPORTANTE:** Implementamos toda a estrutura para integração com Efí Bank (SDK instalado, Factory Pattern, serviços completos), mas não conseguimos ativar a API real por falta de CNPJ. O sistema gera pagamentos simulados com QR Codes inválidos.

**Comunicação:**
12. Chat em tempo real
13. Notificações de status

**Administração:**
14. Dashboard admin
15. Gerenciamento de restaurantes
16. Gerenciamento de pedidos
17. Criação de serviços

**Extras:**
18. Exportação CSV
19. Internacionalização (PT/EN/ES)
20. Tema claro/escuro

**Funcionalidade diferencial:** Estrutura completa de integração com gateway de pagamento (SDK Efí Bank instalado e configurado).

**Ética e LGPD:**
Implementamos Termo de Uso e Política de Privacidade que garantem:
- Transparência no uso de dados pessoais
- Direito de acesso, correção e exclusão de dados
- Segurança no armazenamento (criptografia de senhas)
- Consentimento explícito do usuário

Júlia Braga Lemos é nossa DPO (Data Protection Officer) responsável pela conformidade com LGPD."

---

## 6️⃣ ENCERRAMENTO (30 seg)

**Falar:**
"Para resumir:

**O que resolvemos:** Falta de presença digital de pequenos restaurantes

**Principal aprendizado:** Implementação prática de arquitetura MVC, design patterns GoF e integração com APIs externas

**Maturidade da entrega:**
- Arquitetura sólida com separação de camadas
- Código limpo e documentado
- Sistema funcional em produção
- Conformidade ética com LGPD

O Restify é apenas um protótipo e planejamos que siga assim.

Obrigado pela atenção. Estamos prontos para perguntas."

---

## 📊 ESTRUTURA DA APRESENTAÇÃO

| Seção | Tempo | Conteúdo |
|---------|-------|----------|
| 1. Introdução | 30s | Nome, turma, integrantes, resumo |
| 2. Modelagem | 1min | Diagrama de classes, entidades, relações |
| 3. Casos de Uso | 2min | Diagrama + Demo de Chat e Contratação |
| 4. Arquitetura | 2min | MVC + Repository + 5 Design Patterns |
| 5. Funcionalidades | 1min | 20 requisitos + Ética/LGPD |
| 6. Encerramento | 30s | Resumo, aprendizados, agradecimento |

**TOTAL: 7 minutos**

**Dividam entre vocês como preferirem!**

---

## 📋 MATERIAIS NECESSÁRIOS

### Slides:
1. Capa (nome, turma, integrantes)
2. Diagrama de Classes
3. Diagrama de Casos de Uso
4. Estrutura de Pastas (print do projeto)
5. Fluxo MVC (diagrama)
6. Lista de 20 Funcionalidades
7. Termos de Uso e Privacidade
8. Encerramento (links e contato)

### Demonstração:
- Sistema aberto: https://restify.onrender.com
- Login admin: admin@restify.com / admin123
- Conta restaurante demo criada
- **Backup:** Vídeo curto (sem áudio) caso internet falhe

---

## ✅ CHECKLIST DE PREPARAÇÃO

### 1 dia antes:
- [ ] Repositório atualizado e rodando
- [ ] Sistema testado em produção
- [ ] Diagramas revisados e coerentes com código
- [ ] Termos éticos no repositório
- [ ] Falas ensaiadas e cronometradas (7 min)
- [ ] Vídeo de backup gravado

### 1 hora antes:
- [ ] Sistema online funcionando
- [ ] Slides prontos
- [ ] Login admin testado
- [ ] Conta demo criada
- [ ] Todos presentes

### Durante apresentação:
- [ ] Falar devagar e claro
- [ ] Mostrar confiança
- [ ] Demonstrar sistema funcionando
- [ ] Respeitar tempo (7 min)

---

## 🎯 PERGUNTAS INDIVIDUAIS (5 min)

**Cada um deve saber explicar:**

### Sobre Modelagem:

**P: Por que escolheram essas entidades?**
R: "Escolhemos Restaurant, Service, Order e Message porque representam os elementos centrais do negócio. Restaurant é o cliente que contrata serviços, Service representa os produtos digitais que oferecemos, Order registra as transações comerciais e Message permite comunicação para suporte. São as entidades mínimas necessárias para o sistema funcionar."

**P: Como as relações refletem o problema real?**
R: "As relações espelham o fluxo de negócio: um restaurante pode fazer vários pedidos (1:N), cada pedido contém vários serviços (N:M), e restaurantes podem enviar várias mensagens para suporte (1:N). Usamos composição entre Restaurant e Order porque o pedido não existe sem o restaurante."

**P: Diferença entre composição e agregação no projeto?**
R: "Composição: Restaurant-Order. Se deletarmos o restaurante, seus pedidos também são deletados (dependência forte). Agregação: Order-Service. Se deletarmos um pedido, os serviços continuam existindo no catálogo (dependência fraca)."

### Sobre Casos de Uso:

**P: Quais são os atores do sistema?**
R: "Temos 3 atores principais: Visitante (não autenticado, pode se cadastrar), Restaurante (cliente autenticado, contrata serviços) e Admin (gerencia sistema, restaurantes e pedidos). Cada ator tem permissões específicas."

**P: Explique o fluxo de um caso de uso específico**
R: "No caso 'Contratar Serviço': restaurante loga, navega pelo catálogo, adiciona serviços ao carrinho, finaliza pedido, aceita termos de uso e consentimento, escolhe forma de pagamento (PIX/boleto/cartão), sistema gera cobrança e exibe confirmação. O pedido fica visível no dashboard com status 'Pendente'."

**P: Como tratam exceções nos casos de uso?**
R: "Implementamos fluxos alternativos: se carrinho estiver vazio, sistema exibe alerta. Se pagamento falhar, oferecemos outras formas. Se sessão expirar, redirecionamos para login. Todas as exceções são tratadas com mensagens claras ao usuário."

### Sobre Arquitetura:

**P: Explique o fluxo MVC completo**
R: "Exemplo de login: View (login.php) exibe formulário → Controller (AuthController) recebe POST → chama Service (AuthService) para validar credenciais → Service usa Repository (RestaurantRepository) para buscar no banco → Repository retorna Model (Restaurant) → Controller cria sessão e redireciona para View (dashboard.php)."

**P: Qual a responsabilidade de cada camada?**
R: "Controller: coordena requisições HTTP e respostas. Model: representa entidades e regras de negócio. View: renderiza interface. Repository: abstrai acesso ao banco. Service: implementa lógica complexa (autenticação, pagamentos, notificações)."

**P: Por que usaram Repository Pattern?**
R: "Para separar lógica de negócio da persistência. Se mudarmos de SQLite para MySQL, só alteramos o Repository, sem tocar Controllers ou Models. Facilita testes unitários e manutenção."

**P: Explique cada um dos 5 design patterns**
R: "1) Singleton (Database): garante uma única conexão ao banco. 2) Repository: abstrai persistência. 3) Factory (PaymentServiceFactory): cria serviços de pagamento baseado no tipo (PIX/boleto). 4) Observer (NotificationService): notifica restaurantes sobre mudanças de status. 5) Strategy (ExportService): permite exportar dados em diferentes formatos (CSV/JSON/XML)."

### Sobre Funcionalidades:

**P: Como funciona a integração com Efí Bank?**
R: "Implementamos toda a estrutura: SDK instalado via Composer, EfiPaymentService com métodos para PIX/boleto/cartão, Factory Pattern para criar instâncias, configuração de credenciais. Porém, não conseguimos ativar a API real por falta de CNPJ (Efí exige conta PJ). O sistema gera pagamentos simulados com QR Codes válidos para demonstração."

**P: Como implementaram internacionalização?**
R: "Criamos arquivos de idioma em lang/ (pt.php, en.php, es.php) com arrays de traduções. Função t() busca texto no idioma ativo. Usuário pode alternar idioma no header. Sessão armazena preferência. Suportamos Português, Inglês e Espanhol."

**P: Como funciona o chat em tempo real?**
R: "Chat usa polling: JavaScript faz requisições AJAX a cada 3 segundos para buscar novas mensagens. MessageRepository retorna mensagens não lidas. Não usamos WebSocket por limitações de hospedagem, mas o efeito é similar para o usuário."

### Sobre Ética:

**P: Como garantem privacidade dos dados?**
R: "Seguimos LGPD: senhas criptografadas com bcrypt, conexão HTTPS obrigatória, prepared statements contra SQL injection, validação de inputs, termo de consentimento explícito. Júlia Braga Lemos é nossa DPO responsável."

**P: O que o termo de uso garante?**
R: "Define direitos e deveres: uso permitido (contratar serviços), uso proibido (atividades ilegais), política de cancelamento e reembolso, limitação de responsabilidade, lei aplicável (brasileira). Usuário deve aceitar antes de finalizar pedido."

**P: Como tratam dados sensíveis?**
R: "Dados pessoais (email, telefone, endereço) são coletados apenas com consentimento. Senhas nunca são armazenadas em texto plano (bcrypt). Dados de pagamento são processados pela Efí (não armazenamos cartões). Usuário pode solicitar exclusão via DPO."

---

**Se não souber:** "Boa pergunta, implementamos X mas não exploramos Y em profundidade ainda"

---

## 💡 DICAS GERAIS

### Para quem fizer a demonstração:
- Ensaie a demo várias vezes antes
- Tenha o fluxo decorado
- Se der erro, mantenha calma e explique o que deveria acontecer
- Tenha vídeo de backup caso internet falhe

### Para quem explicar arquitetura:
- Mostre código real, não apenas diagramas
- Explique com clareza a responsabilidade de cada camada
- Destaque os 5 design patterns com exemplos

### Para todos:
- Falar devagar e com confiança
- Olhar para a plateia, não só para tela
- Diagramas devem ser legíveis
- Respeitar o tempo (7 minutos)

---
