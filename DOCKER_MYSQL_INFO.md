# Docker and MySQL Information

## Important Notice About This Project

This portfolio website is built on **Lovable Cloud** which uses:
- **Frontend**: React + Vite (static site)
- **Backend**: Supabase (PostgreSQL database, not MySQL)
- **Authentication**: Supabase Auth
- **Hosting**: Optimized for static site deployment (Render.com, Vercel, Netlify, etc.)

## Why Not MySQL?

This project uses **PostgreSQL** (via Supabase) instead of MySQL because:
1. Supabase provides a complete backend solution (database, auth, storage, APIs)
2. PostgreSQL is more feature-rich and better for web applications
3. The entire codebase is built around Supabase's PostgreSQL database
4. All authentication, RLS policies, and database functions are PostgreSQL-specific

**Switching to MySQL would require a complete rewrite of the backend.**

## Docker Deployment Options

While this project isn't designed for Docker by default, you have two options:

### Option 1: Keep Supabase Backend (Recommended)

Deploy only the frontend in Docker:

```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy source code
COPY . .

# Build the app
RUN npm run build

# Install serve to run the static site
RUN npm install -g serve

# Expose port
EXPOSE 3000

# Start the app
CMD ["serve", "-s", "dist", "-l", "3000"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  frontend:
    build: .
    ports:
      - "3000:3000"
    environment:
      - VITE_SUPABASE_URL=${VITE_SUPABASE_URL}
      - VITE_SUPABASE_PUBLISHABLE_KEY=${VITE_SUPABASE_PUBLISHABLE_KEY}
      - VITE_SUPABASE_PROJECT_ID=${VITE_SUPABASE_PROJECT_ID}
```

The backend (Supabase) continues running in the cloud.

### Option 2: Self-Host Everything (Advanced)

If you want to host everything yourself (including the database):

1. **Self-host Supabase** using Docker:
   - Follow: https://supabase.com/docs/guides/self-hosting
   - This requires Docker Compose with PostgreSQL, Auth, Storage, etc.
   - Complex setup with multiple containers

2. **OR Switch to a completely different backend**:
   - Rewrite all database code for MySQL
   - Implement your own authentication system
   - Rebuild all APIs and database functions
   - **This would be a major project (100+ hours of work)**

## Current Deployment (Easiest)

The simplest approach is what's already set up:

1. **Frontend**: Deploy to Render.com (see DEPLOYMENT.md)
2. **Backend**: Keep using Lovable Cloud (Supabase)

This gives you:
- ✅ Free hosting for frontend
- ✅ Managed database (no maintenance)
- ✅ Built-in authentication
- ✅ Automatic backups
- ✅ SSL certificates
- ✅ Global CDN

## Need MySQL Specifically?

If you absolutely need MySQL, here's what would be required:

1. Remove all Supabase integration code
2. Set up a new MySQL database
3. Rewrite all SQL queries (PostgreSQL → MySQL syntax)
4. Implement custom authentication system
5. Rewrite Row Level Security as application logic
6. Create REST API endpoints
7. Update all frontend data fetching code
8. Set up database migrations
9. Configure Docker Compose with MySQL

**Estimated effort**: 2-3 weeks of full-time development

## Recommendation

**Keep the current setup** with Supabase/PostgreSQL. It's:
- Production-ready
- Scalable
- Secure
- Free tier available
- Minimal maintenance

If you specifically need Docker for deployment, use **Option 1** above (Docker frontend + Supabase backend).
