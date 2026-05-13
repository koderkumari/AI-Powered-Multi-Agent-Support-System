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
