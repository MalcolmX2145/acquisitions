# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Development Commands

### Server Management
- `npm run dev` - Start development server with hot reload using Node's --watch flag
- `npm start` - Start production server (via `node src/index.js`)

### Code Quality
- `npm run lint` - Run ESLint to check for code issues
- `npm run lint:fix` - Automatically fix linting issues where possible
- `npm run format` - Format code with Prettier
- `npm run format:check` - Check if code follows Prettier formatting

### Database Operations
- `npm run db:generate` - Generate Drizzle schema migrations from model changes
- `npm run db:migrate` - Apply pending database migrations
- `npm run db:studio` - Launch Drizzle Studio for database inspection

## Architecture Overview

This is a Node.js REST API built with Express.js, using modern ES modules and a clean layered architecture:

### Core Technologies
- **Runtime**: Node.js with ES modules (`"type": "module"`)
- **Framework**: Express.js v5 with security middleware (Helmet, CORS)
- **Database**: PostgreSQL via Neon serverless with Drizzle ORM
- **Validation**: Zod for schema validation
- **Authentication**: JWT tokens with secure HTTP-only cookies
- **Logging**: Winston with file and console transports
- **Password Hashing**: bcrypt

### Project Structure
The codebase follows a clean architecture pattern with path mapping via Node.js imports:

```
src/
├── config/          # Database and logger configuration
├── controllers/     # HTTP request handlers
├── models/          # Drizzle ORM database schemas
├── routes/          # Express route definitions
├── services/        # Business logic layer
├── utils/           # Shared utilities (JWT, cookies, formatting)
├── validations/     # Zod validation schemas
├── app.js          # Express app configuration
├── server.js       # Server startup
└── index.js        # Main entry point
```

### Path Mapping
The project uses Node.js import maps for clean imports:
- `#src/*` → `./src/*`
- `#config/*` → `./src/config/*`
- `#controllers/*` → `./src/controllers/*`
- `#middleware/*` → `./src/middleware/*`
- `#models/*` → `./src/models/*`
- `#routes/*` → `./src/routes/*`
- `#services/*` → `./src/services/*`
- `#utils/*` → `./src/utils/*`
- `#validations/*` → `./src/validations/*`

### Database Layer
- **ORM**: Drizzle with PostgreSQL dialect
- **Database**: Neon serverless PostgreSQL
- **Schema Location**: `src/models/*.js`
- **Migrations**: Generated to `./drizzle/` directory
- **Configuration**: `drizzle.config.js`

### Authentication System
The API implements JWT-based authentication with:
- Secure HTTP-only cookies for token storage
- bcrypt password hashing with salt rounds of 10
- Role-based access control (user/admin roles)
- Input validation with Zod schemas

### Code Standards
- **Linting**: ESLint with recommended rules + custom Node.js globals
- **Formatting**: Prettier with single quotes, semicolons, 2-space tabs
- **Style**: Modern ES6+ syntax with arrow functions, async/await
- **Error Handling**: Structured error responses with Winston logging

## Development Patterns

### Adding New Features
1. Define Zod validation schemas in `src/validations/`
2. Create Drizzle models in `src/models/`
3. Implement business logic in `src/services/`
4. Build controllers in `src/controllers/`
5. Define routes in `src/routes/`
6. Generate and run database migrations

### Database Workflow
1. Modify models in `src/models/`
2. Run `npm run db:generate` to create migration files
3. Run `npm run db:migrate` to apply changes to database
4. Use `npm run db:studio` to inspect database schema and data

### Environment Configuration
The project uses environment variables for configuration:
- `PORT` - Server port (default: 3000)
- `NODE_ENV` - Environment (development/production)
- `DATABASE_URL` - PostgreSQL connection string
- `JWT_SECRET` - JWT signing secret
- `LOG_LEVEL` - Winston logging level

### Security Considerations
- Passwords are hashed with bcrypt before storage
- JWT tokens are stored in secure HTTP-only cookies
- CORS and security headers are configured via middleware
- Database queries use parameterized statements through Drizzle ORM
- Input validation occurs at the controller level using Zod schemas

## Testing & Debugging
- Server starts on `http://localhost:3000` by default
- Health check endpoint available at `/health`
- Logs are written to `logs/` directory (error.log, combined.log)
- Development logs also output to console with colors