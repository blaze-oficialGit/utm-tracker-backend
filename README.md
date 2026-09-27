# 📊 UTM Tracker - Plataforma SaaS de Rastreamento e Atribuição

Plataforma completa para rastreamento de UTMs, atribuição de vendas e analytics em tempo real. Inspirada na lógica de ferramentas como UTMify, mas com implementação e identidade visual próprias.

## 🎯 Objetivo

Permitir que usuários conectem anúncios, gerem links rastreáveis e acompanhem em tempo quase real quais campanhas, conjuntos, anúncios e UTMs estão gerando cliques, leads, vendas e faturamento — sem depender do atraso dos gerenciadores de anúncios.

## ✨ Funcionalidades

- ✅ **Login e Contas**: Cadastro, login JWT, dashboard individual por usuário
- ✅ **Dashboard Principal**: Métricas em tempo real (Investimento, Receita, Vendas, Leads, Cliques, Conversão, CPA, CPL, ROAS, Ticket Médio, Lucro)
- ✅ **Gráficos Interativos**: Receita, Vendas, Cliques e ROAS ao longo do tempo (Chart.js)
- ✅ **Gerador de Links UTM**: Cria links rastreáveis com UTMs + parâmetros de plataforma (campaign_id, ad_id, placement, etc.)
- ✅ **Captura de Visitantes**: Registra device, browser, OS, país, referrer, landing page
- ✅ **Persistência de Atribuição**: UTMs sobrevivem à navegação via localStorage
- ✅ **Webhooks de Vendas**: Endpoint POST /api/webhooks/purchase para Hotmart, Kiwify, Stripe, etc.
- ✅ **Atribuição Automática**: Relaciona vendas a visitantes por session_id, click_id, email ou cookie
- ✅ **Proteção contra Duplicatas**: transaction_id único prevende vendas duplicadas
- ✅ **Script de Tracking**: JavaScript leve (< 5KB) com sendBeacon, auto-tracking de cliques, suporte a SPA
- ✅ **API Completa**: Endpoints RESTful para eventos, stats, campaigns, orders, visitors
- ✅ **Jornada do Cliente**: Visualização completa do funil (Anúncio → Clique → Lead → Checkout → Compra)
- ✅ **Página de Vendas**: Tabela filtrável com origem, campanha, anúncio, valor, status
- ✅ **Integrações Preparadas**: Estrutura para Meta Ads, TikTok Ads, Google Ads (requer credenciais)
- ✅ **Modelos de Atribuição**: Last Click, First Click, Linear (configurável)
- ✅ **Teste de Tracking**: Ferramenta integrada para validar UTMs e cookies
- ✅ **Alertas de Vendas**: Notificações em tempo real no painel
- ✅ **Modo Demonstração**: Dados fictícios claramente identificados quando banco está vazio
- ✅ **Segurança**: Helmet, rate limiting, CORS, bcrypt, JWT, sanitização
- ✅ **Multiusuário**: Workspaces isolados, cada usuário vê apenas seus dados
- ✅ **Responsivo**: Desktop e mobile

## 🏗️ Arquitetura

```
utm-tracker/
├── backend/                 # API Node.js + Express
│   ├── src/
│   │   ├── server.js       # Servidor principal
│   │   ├── db/
│   │   │   ├── schema.sql  # Schema PostgreSQL (17 tabelas)
│   │   │   ├── pool.js     # Pool de conexões
│   │   │   ├── init.js     # Inicialização do banco
│   │   │   └── seed-demo.js # Dados de demonstração
│   │   ├── middleware/
│   │   │   └── auth.js     # JWT authentication
│   │   └── routes/
│   │       ├── auth.js         # Login/cadastro
│   │       ├── tracking.js     # Captura de visitantes
│   │       ├── webhooks.js     # Recebimento de vendas
│   │       ├── stats.js        # Dashboard metrics
│   │       ├── campaigns.js    # Campanhas
│   │       ├── links.js        # Gerador de links
│   │       ├── visitors.js     # Visitantes
│   │       ├── events.js       # Eventos
│   │       ├── integrations.js # Plataformas
│   │       └── orders.js       # Vendas
│   ├── public/
│   │   └── tracking.js     # Script de tracking (client-side)
│   ├── package.json
│   └── .env.example
├── frontend/                # Interface SaaS
│   ├── index.html          # SPA com sidebar
│   ├── styles.css          # Design system (teal/emerald)
│   └── app.js              # Lógica do frontend
└── README.md
```

## 🚀 Instalação e Deploy

### Pré-requisitos

- Node.js 18+
- PostgreSQL 14+
- npm ou yarn

### 1. Backend

```bash
cd backend
npm install
cp .env.example .env
# Edite .env com suas credenciais
npm run db:init        # Cria as tabelas
npm run db:seed        # (opcional) Gera dados de demonstração
npm run dev            # Roda em http://localhost:3000
```

### 2. Frontend

```bash
cd frontend
# Abra index.html diretamente no navegador
# Ou sirva com qualquer servidor estático:
npx serve .
```

### 3. Variáveis de Ambiente (.env)

```env
PORT=3000
NODE_ENV=development
DATABASE_URL=postgresql://usuario:senha@localhost:5432/utm_tracker
JWT_SECRET=sua-chave-secreta-muito-longa-e-aleatoria-aqui
JWT_EXPIRES_IN=7d
FRONTEND_URL=http://localhost:5173
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100
WEBHOOK_SECRET=sua-chave-secreta-para-webhooks
DEMO_MODE=false
```

### 4. Deploy em Produção

**Backend (Railway/Render):**
```bash
git init
git add .
git commit -m "Initial commit"
# Conecte ao Railway/Render, adicione variáveis de ambiente
# Use PostgreSQL gerenciado (Railway/Supabase/Neon)
```

**Frontend (Vercel/Netlify):**
```bash
# Suba a pasta frontend/ para Vercel
# Configure a variável de ambiente API_URL apontando para o backend
```

## 📡 API Endpoints

### Autenticação
- `POST /api/auth/register` - Criar conta
- `POST /api/auth/login` - Login
- `GET /api/auth/me` - Usuário atual
- `PUT /api/auth/me` - Atualizar perfil
- `PUT /api/auth/change-password` - Alterar senha

### Tracking
- `POST /api/track` - Registrar evento (page_view, click, lead, purchase)
- `GET /api/track/pixel` - Pixel de tracking (1x1 gif)

### Webhooks
- `POST /api/webhooks/purchase` - Receber venda de plataforma externa
- `POST /api/webhooks/lead` - Receber lead
- `GET /api/webhooks/logs` - Logs de webhooks recebidos

### Dashboard
- `GET /api/stats/dashboard?period=7d` - Métricas do dashboard
- `GET /api/stats/revenue?period=30d` - Receita ao longo do tempo

### Links
- `POST /api/links/generate` - Gerar link rastreável
- `GET /api/links` - Listar links
- `DELETE /api/links/:id` - Deletar link
- `GET /r/:code` - Redirecionar short URL

### Vendas
- `GET /api/orders` - Listar vendas
- `GET /api/orders/:id` - Detalhes + jornada
- `PATCH /api/orders/:id/status` - Atualizar status

### Visitantes
- `GET /api/visitors` - Listar visitantes
- `GET /api/visitors/:id/journey` - Jornada completa

### Eventos
- `GET /api/events` - Listar eventos
- `POST /api/events` - Registrar evento autenticado

### Integrações
- `GET /api/integrations` - Listar integrações
- `POST /api/integrations/connect` - Conectar plataforma
- `DELETE /api/integrations/:platform` - Desconectar

### Campanhas
- `GET /api/campaigns` - Listar campanhas
- `GET /api/campaigns/:id` - Detalhes + ad groups + ads
- `POST /api/campaigns/sync` - Sincronizar da plataforma

## 🔗 Como Usar

### 1. Instalar o Script de Tracking

Cole no seu site antes do `</body>`:

```html
<script src="https://seu-backend.com/tracking.js" data-api="https://seu-backend.com/api/track"></script>
```

O script automaticamente:
- Captura UTMs da URL
- Cria visitor_id e session_id
- Persiste atribuição no localStorage
- Envia page views e cliques via sendBeacon
- Expõe `window.UTMTracker.track('evento', dados)` para eventos customizados

### 2. Configurar Webhook de Vendas

Na sua plataforma de pagamento (Hotmart, Kiwify, Stripe), configure o webhook:

```
POST https://seu-backend.com/api/webhooks/purchase
```

Formato esperado:

```json
{
  "transaction_id": "ABC123",
  "customer": {
    "name": "Cliente",
    "email": "cliente@email.com"
  },
  "amount": 47.00,
  "currency": "BRL",
  "status": "approved",
  "product": "Produto Teste",
  "session_id": "optional-session-id",
  "click_id": "optional-click-id"
}
```

O sistema automaticamente:
- Valida transaction_id (previne duplicatas)
- Busca sessão por session_id, click_id ou email
- Atribui a venda aos UTMs originais
- Registra no dashboard em tempo real

### 3. Gerar Links Rastreáveis

No painel, vá em **Links** → **Gerar Link Rastreável**:

- URL de destino: `https://meusite.com/oferta`
- utm_source: `tiktok`
- utm_medium: `paid`
- utm_campaign: `campanha_setembro`
- campaign_id: `123456`
- ad_id: `789`

Resultado:
```
https://meusite.com/oferta?utm_source=tiktok&utm_medium=paid&utm_campaign=campanha_setembro&campaign_id=123456&ad_id=789
```

### 4. Acompanhar em Tempo Real

- **Dashboard**: Métricas agregadas (HOJE / 7D / 30D)
- **Vendas**: Tabela com origem, campanha, valor, status
- **Visitantes**: Lista com device, país, última visita
- **Eventos**: Feed em tempo real de page views, cliques, leads, purchases
- **Jornada**: Clique em uma venda para ver o funil completo

## 🔒 Segurança

- ✅ Senhas hash com bcrypt (salt rounds 10)
- ✅ JWT com expiração configurável
- ✅ Rate limiting (100 req/15min por IP)
- ✅ Helmet (headers de segurança)
- ✅ CORS configurado por origem
- ✅ Validação de input em todos os endpoints
- ✅ Proteção contra SQL injection (prepared statements)
- ✅ Transaction_id único previne vendas duplicadas
- ✅ Workspaces isolados (multiusuário seguro)
- ✅ Variáveis de ambiente para secrets

## 📊 Banco de Dados

17 tabelas PostgreSQL otimizadas com índices:

- `users` - Usuários
- `workspaces` - Espaços de trabalho
- `tracking_links` - Links gerados
- `visitors` - Visitantes únicos
- `sessions` - Sessões com UTMs
- `events` - Eventos (page_view, click, lead, purchase, refund)
- `clicks` - Cliques detalhados
- `leads` - Leads capturados
- `orders` - Vendas/pedidos
- `order_items` - Itens do pedido
- `campaigns` - Campanhas (cache das plataformas)
- `ad_groups` - Conjuntos de anúncios
- `ads` - Anúncios
- `integrations` - Credenciais de plataformas
- `webhooks` - Webhooks configurados
- `webhook_logs` - Logs de webhooks recebidos

Índices em: workspace_id, visitor_id, session_id, transaction_id, event_type, timestamp, created_at

## 🎨 Design System

Paleta teal/emerald com slate neutro:

- Primary: `#0d9488` (teal-600)
- Secondary: `#0ea5e9` (sky-500)
- Success: `#10b981` (emerald-500)
- Warning: `#f59e0b` (amber-500)
- Danger: `#ef4444` (red-500)
- Background: `#f8fafc` (slate-50)
- Surface: `#ffffff`
- Text: `#0f172a` (slate-900)

Tipografia: System font stack (-apple-system, BlinkMacSystemFont, Segoe UI, Roboto)

Componentes: Cards com shadow sutil, bordas arredondadas (8px), sidebar escura, métricas com borda lateral colorida

## 🧪 Modo Demonstração

Quando `DEMO_MODE=true` no .env:

- Badge "🎭 Dados de Demonstração" aparece no dashboard
- Dados fictícios são claramente identificados
- Nunca misturados com dados reais
- Útil para testes e apresentações

Para gerar dados demo:

```bash
cd backend
node src/db/seed-demo.js
```

Login demo: `demo@utmtracker.com` / `demo123`

## 🔮 Roadmap (Futuro)

- [ ] Integração real com Meta Ads API (OAuth + sync de campanhas)
- [ ] Integração real com TikTok Ads API
- [ ] Integração real com Google Ads API
- [ ] Planos pagos (Free / Pro / Enterprise)
- [ ] Cobrança recorrente (Stripe)
- [ ] Exportação de relatórios (CSV/PDF)
- [ ] Alertas por email/Telegram
- [ ] White-label (domínio customizado)
- [ ] API pública para desenvolvedores
- [ ] Mobile app (React Native)
- [ ] Atribuição multi-touch avançada (Markov chains)
- [ ] Previsão de ROAS com ML

## 📝 Licença

MIT

## 🤝 Contribuindo

Contribuições são bem-vindas! Abra uma issue ou PR.

## 📞 Suporte

Para dúvidas ou problemas, abra uma issue no GitHub.

---

**Construído com foco em velocidade de atribuição.** Quando o webhook de pagamento informa "Venda aprovada", ela aparece no dashboard imediatamente — sem esperar a plataforma de anúncios registrar a conversão.