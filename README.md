# Swades AI — Multi-Agent Support System

AI-powered customer support with a Router Agent that delegates to specialized sub-agents (Support, Order, Billing), each with database-backed tools.

## Architecture

```
User → Frontend (Next.js) → Backend (Hono.js, Port 8000)
                                 ├── Auth (JWT + bcrypt)
                                 ├── Rate Limiting (IP-based, sliding window)
                                 └── Multi-Agent System (Vercel AI SDK)
                                      ├── Router Agent → classifies intent
                                      ├── Support Agent → queryConversationHistory
                                      ├── Order Agent → fetchOrder, trackOrder, cancelOrder, createOrder
                                      └── Billing Agent → getInvoice, checkRefund
                                           ↕
                                      PostgreSQL (Drizzle ORM)
```

## Tech Stack

| Layer | Tech |
|---|---|
| Frontend | Next.js, React, Tailwind CSS, shadcn/ui |
| Backend | Hono.js, TypeScript |
| Database | PostgreSQL, Drizzle ORM |
| AI | Vercel AI SDK, Groq (LLaMA 3.3 70B) |
| Auth | JWT, bcrypt |

## Setup

### Prerequisites
- Node.js 18+
- PostgreSQL running locally

### Backend

```bash
cd backend
npm install
```

Create `.env`:
```
DATABASE_URL=postgresql://user:password@localhost:5432/swades_ai
GROQ_API_KEY=your_groq_api_key
```

```bash
npm run db:push    # Push schema to DB
npm run db:seed    # Seed demo data
npm run dev        # Start on port 8000
```

### Frontend

```bash
cd frontend
npm install
npm run dev        # Start on port 3000
``

### Demo Credentials
```
Email: demo@swades.ai
Password: password123
```

## API Routes

```
/api/auth       POST /signup, /login, /logout
/api/chat       POST /messages, /messages/stream
                GET /conversations, /conversations/:id
                DELETE /conversations/:id
/api/agents     GET /, /:type/capabilities
/api/health     GET /
/docs           Swagger UI
```

## Key Features
- **Multi-Agent Routing** — AI classifies intent, keyword fallback
- **Agent Tools** — 7 tools querying real PostgreSQL data
- **Streaming (SSE)** — Real-time token-by-token AI responses
- **Rate Limiting** — 100/min API, 10/min auth, 20/min chat
- **Rich UI Cards** — Order & invoice cards rendered from tool results
- **Conversation Persistence** — Full history stored in DB
- **Prompt Optimization** — Strict capability/limitation boundaries per agent

# AI-Powered Multi-Agent Support System

A full-stack AI support system with a Node.js/Hono backend and Next.js frontend that leverages multi-agent AI for intelligent customer support.

## Project Structure

```
.
├── frontend/
│   ├── backend/          # Node.js API (Hono framework)
│   │   ├── src/
│   │   ├── package.json
│   │   └── tsconfig.json
│   └── frontend/         # Next.js web application
│       ├── app/
│       ├── components/
│       ├── package.json
│       └── tsconfig.json
├── render.yaml           # Render deployment configuration
├── package.json          # Monorepo root configuration
└── README.md
```

## Local Development

### Prerequisites
- Node.js 18+ and npm
- PostgreSQL database (or use provided Supabase connection)

### Setup

1. **Install dependencies:**
   ```bash
   npm run install-all
   ```

2. **Configure environment variables:**
   - Copy `.env.example` to `.env` at the root
   - Update `frontend/backend/.env` with your credentials:
     ```bash
     DATABASE_URL=your_postgres_url
     GROQ_API_KEY=your_groq_api_key
     ```

3. **Run development servers:**
   ```bash
   npm run dev
   ```
   - Backend: http://localhost:8000
   - Frontend: http://localhost:3000
   - API Docs: http://localhost:8000/docs

### Available Scripts

**Backend:**
```bash
cd frontend/backend
npm run dev          # Start development server
npm run build        # Build TypeScript
npm run db:push      # Push database schema
npm run db:seed      # Seed database
```

**Frontend:**
```bash
cd frontend/frontend
npm run dev          # Start development server
npm run build        # Build Next.js app
npm start            # Start production server
```

## Deployment on Render

### Prerequisites
- Render account
- GitHub repository connected to Render
- Environment variables configured in Render dashboard

### Environment Variables Required

**Backend:**
- `NODE_ENV=production`
- `DATABASE_URL`: PostgreSQL connection string
- `GROQ_API_KEY`: Groq API key for AI models

**Frontend:**
- `NEXT_PUBLIC_API_URL`: Backend API URL (e.g., https://ai-support-backend.onrender.com)

### Deploy via render.yaml

The `render.yaml` file automatically configures:
- **Backend Service**: Runs on Node.js with Hono
- **Frontend Service**: Runs Next.js with production optimizations

**To deploy:**
1. Push code to GitHub
2. Connect repo to Render dashboard
3. Select "Web Services" → "New +" → "Build and deploy from Git"
4. Render will automatically use `render.yaml` configuration

### Manual Deployment

If using dashboard:

**Backend Service:**
- Root Directory: `frontend/backend`
- Build Command: `npm install && npm run build`
- Start Command: `npm start`
- Port: Auto-detected from `PORT` environment variable

**Frontend Service:**
- Root Directory: `frontend/frontend`
- Build Command: `npm install && npm run build`
- Start Command: `npm start`

## API Documentation

Access Swagger UI at: `{BACKEND_URL}/docs`

### Key Endpoints

- `GET /api/health` - Health check
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `POST /api/chat/message` - Send chat message
- `GET /api/agents` - List available agents

## Database

Using Drizzle ORM with PostgreSQL (Supabase):

```bash
# Generate migration files
npm run db:generate

# Apply migrations
npm run db:migrate

# Push schema to database
npm run db:push

# Open Drizzle Studio
npm run db:studio

# Seed database
npm run db:seed
```

## Features

- **Multi-Agent AI**: Leverages multiple AI agents for specialized support tasks
- **Real-time Chat**: WebSocket support for real-time messaging
- **Authentication**: JWT-based auth with rate limiting
- **Rate Limiting**: Per-endpoint and per-user rate limits
- **Responsive UI**: Built with Next.js and Radix UI components
- **API Documentation**: Swagger/OpenAPI documentation

## Tech Stack

**Backend:**
- Hono (Web framework)
- Drizzle ORM (Database)
- Groq API (AI models)
- bcryptjs (Password hashing)
- Zod (Schema validation)

**Frontend:**
- Next.js 15+
- React 18+
- TailwindCSS
- Radix UI
- TypeScript

## Troubleshooting

### Build Fails with "Cannot find package.json"
- Ensure `render.yaml` has correct `rootDir` paths
- Verify `package.json` exists in specified directories

### Database Connection Issues
- Check `DATABASE_URL` format: `postgresql://user:pass@host:port/db`
- Verify IP allowlist in database settings

### Environment Variables Not Loading
- Set all required vars in Render dashboard
- Backend will fail to start without `DATABASE_URL` and `GROQ_API_KEY`

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make changes and test locally
4. Push and create a pull request

## License

ISC

## Support

For issues and questions, please open an issue on GitHub.
