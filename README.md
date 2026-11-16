# Agenda multi-salão

Plataforma SaaS de marcações para barbearias e salões. Cada salão tem o seu espaço, a sua equipa,
os seus serviços e a sua agenda, tudo na mesma base de código.

## Porque existe

Barbearias e salões pequenos ainda marcam muitos clientes ao telefone. Um sistema feito à medida para
cada um seria caro de construir e de manter. Esta plataforma serve vários salões em simultâneo,
com os dados de cada um isolados dos restantes.

## Funcionalidades

- **Multi-tenant por subdomínio:** `barbeariaj.dominio.pt` e `outro-salao.dominio.pt` partilham o servidor, mas nunca os dados.
- **Marcação online:** o cliente escolhe serviço, profissional e hora, e confirma a marcação por email.
- **Painel de administração:** calendário semanal, gestão de serviços, equipa, horário de funcionamento e imagem do salão.
- **Estatísticas:** indicadores por salão no painel.
- **Planos:** limites diferentes por plano (basic, premium, enterprise).
- **Segurança:** autenticação JWT, palavras-passe com bcrypt, Helmet, CORS e limite de pedidos.

## Stack

| Camada | Tecnologias |
| --- | --- |
| Frontend | React 18, Vite, Tailwind CSS, React Router, react-big-calendar, date-fns |
| Backend | Node.js, Express, MongoDB com Mongoose, JWT, express-validator |
| Email | Resend / Nodemailer |
| Testes | Jest, Supertest |

## Estrutura

```
backend/
  server.js              arranque da API
  config/                ligação ao MongoDB
  middleware/            deteção do salão (tenant) e tratamento de erros
  models/                Tenant, TenantService, TenantStaff, TenantAppointment, User
  routes/                auth, tenants, services, staff, appointments, analytics
  utils/emailService.js  emails de confirmação
  seed_*.js              dados de arranque de salões
frontend/
  src/pages/             Home, Admin, ConfirmAppointment, ConfirmPage
  src/components/        calendário do admin, modal de marcação, cartões de serviço
  src/hooks/             useAppointments, useServices, useStaff, useAuth, useSalonSettings
```

## API

| Recurso | Rota base | Exemplos |
| --- | --- | --- |
| Autenticação | `/api/auth` | `POST /login`, `GET /me` |
| Salões | `/api/tenants` | `GET /current`, `PUT /branding`, `PUT /working-hours` |
| Serviços | `/api/services` | `GET /`, `POST /`, `DELETE /:id` |
| Equipa | `/api/staff` | `GET /`, `POST /` |
| Marcações | `/api/appointments` | `GET /`, `POST /`, `POST /confirm` |
| Estatísticas | `/api/analytics` | `GET /dashboard`, `GET /dashboard-stats` |

Em desenvolvimento (`localhost`), o salão é escolhido com `?tenant=nome` ou com o cabeçalho `X-Tenant`.
Em produção, é lido do subdomínio.

## Começar

Precisas de Node.js 18+ e de uma base de dados MongoDB (local ou Atlas).

```bash
# Backend
cd backend
npm install
# cria um ficheiro .env (ver abaixo)
npm run dev            # http://localhost:5000

# Frontend, noutro terminal
cd frontend
npm install
npm run dev            # http://localhost:5173
```

### Variáveis de ambiente (`backend/.env`)

```env
PORT=5000
NODE_ENV=development
MONGODB_URI=mongodb://localhost:27017/salon-booking
JWT_SECRET=troca-por-uma-string-longa-e-aleatoria
RESEND_API_KEY=
ADMIN_SEED_SECRET=
```

### Dados de exemplo

Os scripts `seed_*.js` e `create_*.js` criam salões, equipa e um administrador. As contas criadas
usam uma palavra-passe de exemplo: **muda-a logo depois de correr os scripts**, sobretudo em produção.

```bash
cd backend
node seed_barbeariaj.js
node create_admin.js
```

## Autor

**David França** · [Portefólio](https://github.com/DSCF15/portfolio-david) · [LinkedIn](https://www.linkedin.com/in/david-fran%C3%A7a-281418260/)

Licença MIT.
