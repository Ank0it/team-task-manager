Team Task Manager

A full-stack team task manager for organizing projects, members, and work items in one place.
Built for clarity: role-based access, validated APIs, and a focused dashboard for progress at a glance.

--------------------------------------------------
FEATURES
--------------------------------------------------

Authentication
- Sign up
- Sign in (JWT session cookie)
- Sign out
- Passwords hashed with bcrypt

Roles
- ADMIN
- MEMBER
Admins manage projects, members, and tasks.
Members update status on assigned work.

Projects
- List
- Search
- Create
- View
- Update
- Delete (admin only)
- Membership management
- Activity timeline

Tasks
- Create
- Assign
- Filter
- Pagination
- Status and priority
- Overdue filters
- Comments and mentions

Dashboard
- Total tasks
- Pending tasks
- Overdue tasks
- Per-project completion tracking

Activity
- Project-scoped activity log for auditing changes

--------------------------------------------------
TECH STACK
--------------------------------------------------

Framework: Next.js 16 (App Router)
Language: TypeScript
UI: React 19, Tailwind CSS 4
Database: PostgreSQL
ORM: Prisma 7 with pg adapter
Authentication: NextAuth.js v4 (JWT + credentials)
Validation: Zod 4

--------------------------------------------------
ARCHITECTURE
--------------------------------------------------

Browser
   |
   v
Next.js (App Router + Route Handlers)
   |
   v
getServerSession / JWT (NextAuth)
   |
   v
Prisma + PostgreSQL

Pages under src/app use server components where helpful.
Interactive areas use client components in src/components.

API routes under src/app/api enforce auth via shared helpers in src/lib/rbac.ts.

Middleware (src/middleware.ts) protects routes and APIs.
Public routes:
- /
- /login
- /signup
- /api/auth/*

--------------------------------------------------
PREREQUISITES
--------------------------------------------------

- Node.js 20+
- PostgreSQL 14+
- npm

--------------------------------------------------
GETTING STARTED
--------------------------------------------------

1. Clone and install

git clone <your-repo-url> task-manager
cd task-manager
npm install

2. Environment variables

Copy the example file:

cp .env.example .env

Required variables:

DATABASE_URL
- PostgreSQL connection string

NEXTAUTH_URL
- App origin
Example:
http://localhost:3000
https://your-domain

NEXTAUTH_SECRET
- Random secret for signing JWTs

Important:
NEXTAUTH_URL must match the browser URL exactly.

3. Database setup

Generate Prisma client:

npx prisma generate

Apply migrations:

npx prisma migrate deploy

For fresh local database:

npx prisma migrate dev

4. Run development server

npm run dev

Open:
http://localhost:3000

5. First account = admin

The first registered user becomes ADMIN.
All subsequent users become MEMBER.

--------------------------------------------------
SCRIPTS
--------------------------------------------------

npm run dev
- Start development server

npm run build
- Production build

npm run start
- Start production server

npm run lint
- Run ESLint

--------------------------------------------------
DEPLOYMENT (RAILWAY)
--------------------------------------------------

1. Create Railway project with PostgreSQL and web service
2. Set DATABASE_URL
3. Set NEXTAUTH_URL to Railway public HTTPS URL
4. Set NEXTAUTH_SECRET
5. Run migrations on deploy:
   npx prisma migrate deploy
6. Start app:
   npm run start

Suggested build command:

npx prisma generate && npm run build

--------------------------------------------------
PROJECT STRUCTURE
--------------------------------------------------

task-manager/
├── prisma/
│   ├── schema.prisma
│   └── migrations/
├── prisma.config.ts
├── src/
│   ├── app/
│   │   ├── api/
│   │   ├── dashboard/
│   │   ├── login/
│   │   ├── signup/
│   │   ├── projects/
│   │   └── tasks/
│   ├── components/
│   ├── lib/
│   ├── middleware.ts
│   └── types/
├── .env.example
└── package.json

--------------------------------------------------
API OVERVIEW
--------------------------------------------------

POST   /api/auth/signup
- Register (public)

POST   /api/auth/login
- Sign in

POST   /api/auth/logout
- Sign out

*      /api/auth/[...nextauth]
- NextAuth handler

GET    /api/projects
- List/search projects

POST   /api/projects
- Create project (admin)

GET/PATCH/DELETE   /api/projects/[id]
- Project CRUD

POST   /api/projects/[id]/members
- Add member (admin)

DELETE /api/projects/[id]/members/[memberId]
- Remove member (admin)

GET    /api/projects/[id]/timeline
- Activity log

GET/POST /api/tasks
- List/create tasks

PATCH/DELETE /api/tasks/[id]
- Update/delete task

GET/POST /api/tasks/[id]/comments
- Task comments

GET /api/dashboard
- Dashboard aggregates

Request bodies are validated using Zod schemas.

--------------------------------------------------
SECURITY NOTES
--------------------------------------------------

- Passwords are hashed using bcrypt
- Use strong NEXTAUTH_SECRET
- Prefer HTTPS in production
- First-user-admin bootstrap is useful for demos
- Restrict signup in production if needed

--------------------------------------------------
LICENSE
--------------------------------------------------

Private / unpublished

Add a LICENSE file when you decide how to share the project.

--------------------------------------------------

Team Task Manager
Ship work with clarity.