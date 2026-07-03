---
name: web-app-builder
description: >
  Builds modern, production-ready full-stack web applications with a premium
  design aesthetic. Uses Next.js (React) for the frontend with Tailwind CSS v3,
  Supabase as the primary backend (auth, Postgres, storage), and optional
  FastAPI or Express.js API layers. Enforces modern design patterns —
  glassmorphism, gradients, micro-animations, dark mode — with built-in SEO,
  responsive design, testing, and deployment to Vercel.
  Use this skill when the user wants to build, scaffold, or architect a
  full-stack web application, SPA, dashboard, or any frontend-heavy project.
  Do NOT use for simple static HTML pages with no interactivity, CLI tools,
  mobile-native apps, or backend-only microservices.
---

# 🚀 Web App Builder Skill

Build **modern, production-ready, full-stack web applications** with a premium
design aesthetic that wows users at first glance. This skill covers the entire
lifecycle — from scaffolding to deployment.

---

## Tech Stack Overview

| Layer           | Technology                                    |
|-----------------|-----------------------------------------------|
| **Frontend**    | Next.js 14+ (React, App Router)               |
| **Styling**     | Tailwind CSS v3, Google Fonts (Inter/Outfit)   |
| **Backend**     | Supabase (primary) — Auth, Postgres, Storage   |
| **API Layer**   | FastAPI (Python) or Express.js (Node) optional |
| **Package Mgr** | npm                                           |
| **Language**    | JavaScript (ES2022+)                          |
| **Deployment**  | Vercel (primary), with guides for alternatives |
| **Testing**     | Jest + React Testing Library + Playwright      |

---

## When to Use This Skill

Activate this skill when the user says things like:

- "Build me a web app for ..."
- "Create a full-stack application with ..."
- "Scaffold a new Next.js project"
- "I need a dashboard / SPA / landing page with backend"
- "Set up a project with Supabase authentication"
- "Build a modern web app with dark mode and animations"

**Do NOT use when:**

- The user wants a CLI tool, mobile-native app, or backend-only API
- The user wants a simple static HTML page with zero interactivity
- The user explicitly requests a different framework (Angular, Vue, Svelte)
  — ask if they want to proceed with Next.js or switch

---

## Step-by-Step Workflow

### Step 1 — Understand the Requirements

Before writing any code, clarify the following with the user:

1. **App purpose** — What does the app do? (e.g., "a task manager", "a blog")
2. **Core features** — List the 3–5 most important features for the MVP
3. **Authentication needed?** — Does it need user login/signup? (default: yes)
4. **Database tables** — What data models are needed? (e.g., users, posts, tasks)
5. **Design preference** — Confirm dark mode / glassmorphism / color palette
   preference, or use the default premium dark theme
6. **Pages needed** — List all pages/routes (e.g., Home, Dashboard, Settings)

> **Tip:** If the user is vague, propose a reasonable MVP scope and ask for
> confirmation before proceeding.

---

### Step 2 — Scaffold the Project

Use the following command to create a new Next.js project:

```bash
npx -y create-next-app@latest ./ --js --tailwind --eslint --app --src-dir --import-alias "@/*" --use-npm
```

**Flags explained:**
- `--js` — JavaScript (not TypeScript)
- `--tailwind` — Pre-configure Tailwind CSS
- `--eslint` — Include ESLint
- `--app` — Use the App Router (not Pages Router)
- `--src-dir` — Use `src/` directory
- `--import-alias "@/*"` — Clean import paths
- `--use-npm` — Use npm as the package manager

After scaffolding, install additional dependencies:

```bash
npm install @supabase/supabase-js @supabase/auth-helpers-nextjs framer-motion lucide-react clsx
```

**Optional (if user needs API layer):**

For FastAPI:
```bash
pip install fastapi uvicorn python-dotenv supabase
```

For Express:
```bash
npm install express cors dotenv @supabase/supabase-js nodemon
```

---

### Step 3 — Set Up Project Structure

Use the following **recommended** project structure (adapt as needed):

```
project-root/
├── public/
│   ├── fonts/
│   └── images/
├── src/
│   ├── app/
│   │   ├── layout.js           ← Root layout (fonts, metadata, providers)
│   │   ├── page.js             ← Home / landing page
│   │   ├── globals.css         ← Global styles + Tailwind directives
│   │   ├── (auth)/
│   │   │   ├── login/page.js
│   │   │   └── signup/page.js
│   │   ├── dashboard/
│   │   │   ├── layout.js
│   │   │   └── page.js
│   │   └── api/                ← Next.js API routes (if no separate backend)
│   │       └── [...]/route.js
│   ├── components/
│   │   ├── ui/                 ← Reusable UI primitives
│   │   │   ├── Button.js
│   │   │   ├── Card.js
│   │   │   ├── Input.js
│   │   │   ├── Modal.js
│   │   │   └── Badge.js
│   │   ├── layout/             ← Layout components
│   │   │   ├── Navbar.js
│   │   │   ├── Sidebar.js
│   │   │   └── Footer.js
│   │   └── features/           ← Feature-specific components
│   │       └── [feature-name]/
│   ├── lib/
│   │   ├── supabase/
│   │   │   ├── client.js       ← Browser Supabase client
│   │   │   ├── server.js       ← Server-side Supabase client
│   │   │   └── middleware.js   ← Auth middleware helper
│   │   ├── utils.js            ← Shared utility functions
│   │   └── constants.js        ← App-wide constants
│   ├── hooks/                  ← Custom React hooks
│   │   ├── useAuth.js
│   │   └── useSupabase.js
│   ├── context/                ← React context providers
│   │   └── AuthProvider.js
│   └── styles/                 ← Additional style modules (if needed)
├── api/                        ← Optional FastAPI/Express backend
│   ├── main.py                 ← FastAPI entry (or index.js for Express)
│   ├── routes/
│   ├── models/
│   └── requirements.txt        ← (or package.json for Express)
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
├── .env.local                  ← Supabase keys + secrets (NEVER commit)
├── .env.example                ← Template for env vars
├── tailwind.config.js
├── next.config.js
├── jsconfig.json
└── package.json
```

> **Important:** Always create `.env.example` with placeholder values AND
> add `.env.local` to `.gitignore`.

---

### Step 4 — Configure the Design System

#### 4.1 — Tailwind Configuration

Update `tailwind.config.js` with a premium design system:

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './src/pages/**/*.{js,jsx}',
    './src/components/**/*.{js,jsx}',
    './src/app/**/*.{js,jsx}',
  ],
  darkMode: 'class',
  theme: {
    extend: {
      colors: {
        // Primary palette — deep indigo/purple
        primary: {
          50:  '#eef2ff',
          100: '#e0e7ff',
          200: '#c7d2fe',
          300: '#a5b4fc',
          400: '#818cf8',
          500: '#6366f1',
          600: '#4f46e5',
          700: '#4338ca',
          800: '#3730a3',
          900: '#312e81',
          950: '#1e1b4b',
        },
        // Accent palette — vibrant cyan
        accent: {
          50:  '#ecfeff',
          100: '#cffafe',
          200: '#a5f3fc',
          300: '#67e8f9',
          400: '#22d3ee',
          500: '#06b6d4',
          600: '#0891b2',
          700: '#0e7490',
          800: '#155e75',
          900: '#164e63',
        },
        // Surface colors for dark mode
        surface: {
          50:  '#f8fafc',
          100: '#f1f5f9',
          200: '#e2e8f0',
          700: '#1e293b',
          800: '#0f172a',
          900: '#020617',
          950: '#010313',
        },
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', '-apple-system', 'sans-serif'],
        display: ['Outfit', 'Inter', 'system-ui', 'sans-serif'],
        mono: ['JetBrains Mono', 'Fira Code', 'monospace'],
      },
      borderRadius: {
        '4xl': '2rem',
      },
      backdropBlur: {
        xs: '2px',
      },
      animation: {
        'fade-in': 'fadeIn 0.5s ease-out',
        'fade-in-up': 'fadeInUp 0.6s ease-out',
        'slide-in-right': 'slideInRight 0.4s ease-out',
        'scale-in': 'scaleIn 0.3s ease-out',
        'glow-pulse': 'glowPulse 2s ease-in-out infinite',
        'shimmer': 'shimmer 2s linear infinite',
        'float': 'float 6s ease-in-out infinite',
      },
      keyframes: {
        fadeIn: {
          '0%': { opacity: '0' },
          '100%': { opacity: '1' },
        },
        fadeInUp: {
          '0%': { opacity: '0', transform: 'translateY(20px)' },
          '100%': { opacity: '1', transform: 'translateY(0)' },
        },
        slideInRight: {
          '0%': { opacity: '0', transform: 'translateX(-20px)' },
          '100%': { opacity: '1', transform: 'translateX(0)' },
        },
        scaleIn: {
          '0%': { opacity: '0', transform: 'scale(0.95)' },
          '100%': { opacity: '1', transform: 'scale(1)' },
        },
        glowPulse: {
          '0%, 100%': { boxShadow: '0 0 20px rgba(99, 102, 241, 0.3)' },
          '50%': { boxShadow: '0 0 40px rgba(99, 102, 241, 0.6)' },
        },
        shimmer: {
          '0%': { backgroundPosition: '-200% 0' },
          '100%': { backgroundPosition: '200% 0' },
        },
        float: {
          '0%, 100%': { transform: 'translateY(0)' },
          '50%': { transform: 'translateY(-10px)' },
        },
      },
      boxShadow: {
        'glass': '0 8px 32px 0 rgba(31, 38, 135, 0.15)',
        'glass-lg': '0 16px 48px 0 rgba(31, 38, 135, 0.25)',
        'neon': '0 0 20px rgba(99, 102, 241, 0.4), 0 0 60px rgba(99, 102, 241, 0.1)',
        'neon-accent': '0 0 20px rgba(6, 182, 212, 0.4), 0 0 60px rgba(6, 182, 212, 0.1)',
      },
    },
  },
  plugins: [],
};
```

#### 4.2 — Global CSS (`globals.css`)

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Google Fonts — loaded via next/font in layout.js for performance */

@layer base {
  :root {
    --glass-bg: rgba(255, 255, 255, 0.08);
    --glass-border: rgba(255, 255, 255, 0.12);
    --glass-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.15);
  }

  body {
    @apply bg-surface-950 text-surface-100 antialiased;
  }

  ::selection {
    @apply bg-primary-500/30 text-white;
  }

  /* Smooth scrolling */
  html {
    scroll-behavior: smooth;
  }

  /* Custom scrollbar */
  ::-webkit-scrollbar {
    width: 6px;
  }
  ::-webkit-scrollbar-track {
    @apply bg-surface-900;
  }
  ::-webkit-scrollbar-thumb {
    @apply bg-surface-700 rounded-full;
  }
  ::-webkit-scrollbar-thumb:hover {
    @apply bg-surface-600;
  }
}

@layer components {
  /* Glassmorphism card */
  .glass-card {
    @apply relative overflow-hidden rounded-2xl border border-white/10
           bg-white/5 p-6 backdrop-blur-xl;
    box-shadow: var(--glass-shadow);
  }

  .glass-card::before {
    content: '';
    @apply absolute inset-0 rounded-2xl;
    background: linear-gradient(
      135deg,
      rgba(255, 255, 255, 0.06) 0%,
      transparent 50%,
      transparent 100%
    );
    pointer-events: none;
  }

  /* Gradient text */
  .text-gradient {
    @apply bg-gradient-to-r from-primary-400 via-accent-400 to-primary-300
           bg-clip-text text-transparent;
  }

  /* Premium button */
  .btn-primary {
    @apply relative inline-flex items-center justify-center gap-2
           rounded-xl bg-gradient-to-r from-primary-600 to-primary-500
           px-6 py-3 font-semibold text-white
           shadow-lg shadow-primary-500/25
           transition-all duration-300
           hover:shadow-xl hover:shadow-primary-500/40
           hover:translate-y-[-2px]
           active:translate-y-0
           focus:outline-none focus:ring-2 focus:ring-primary-400/50
           disabled:opacity-50 disabled:cursor-not-allowed
           disabled:hover:translate-y-0;
  }

  .btn-secondary {
    @apply relative inline-flex items-center justify-center gap-2
           rounded-xl border border-white/10 bg-white/5
           px-6 py-3 font-semibold text-surface-200
           backdrop-blur-sm
           transition-all duration-300
           hover:bg-white/10 hover:border-white/20
           hover:translate-y-[-2px]
           active:translate-y-0
           focus:outline-none focus:ring-2 focus:ring-white/20;
  }

  /* Glow effect */
  .glow {
    @apply relative;
  }
  .glow::after {
    content: '';
    @apply absolute -inset-1 -z-10 rounded-2xl opacity-0
           transition-opacity duration-500;
    background: linear-gradient(135deg, theme('colors.primary.500'), theme('colors.accent.500'));
    filter: blur(20px);
  }
  .glow:hover::after {
    @apply opacity-40;
  }

  /* Shimmer loading skeleton */
  .skeleton {
    @apply relative overflow-hidden rounded-lg bg-surface-800;
  }
  .skeleton::after {
    content: '';
    @apply absolute inset-0 animate-shimmer;
    background: linear-gradient(
      90deg,
      transparent 0%,
      rgba(255, 255, 255, 0.04) 50%,
      transparent 100%
    );
    background-size: 200% 100%;
  }
}

@layer utilities {
  .text-balance {
    text-wrap: balance;
  }
}
```

#### 4.3 — Root Layout (`layout.js`)

```jsx
import { Inter, Outfit } from 'next/font/google';
import './globals.css';

const inter = Inter({
  subsets: ['latin'],
  variable: '--font-inter',
});

const outfit = Outfit({
  subsets: ['latin'],
  variable: '--font-outfit',
});

export const metadata = {
  title: 'App Name — Tagline',
  description: 'A brief, compelling description of the application for SEO.',
  keywords: ['relevant', 'keywords', 'here'],
  authors: [{ name: 'Your Name' }],
  openGraph: {
    title: 'App Name — Tagline',
    description: 'A brief description for social sharing.',
    type: 'website',
  },
};

export default function RootLayout({ children }) {
  return (
    <html lang="en" className={`${inter.variable} ${outfit.variable} dark`}>
      <body className="font-sans min-h-screen bg-surface-950 text-surface-100">
        {children}
      </body>
    </html>
  );
}
```

---

### Step 5 — Set Up Supabase

#### 5.1 — Environment Variables

Create `.env.local`:
```env
NEXT_PUBLIC_SUPABASE_URL=your-project-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
```

Create `.env.example`:
```env
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key-here
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key-here
```

#### 5.2 — Supabase Client (`lib/supabase/client.js`)

```javascript
import { createBrowserClient } from '@supabase/auth-helpers-nextjs';

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY
  );
}
```

#### 5.3 — Supabase Server Client (`lib/supabase/server.js`)

```javascript
import { createServerClient } from '@supabase/auth-helpers-nextjs';
import { cookies } from 'next/headers';

export function createServerSupabaseClient() {
  const cookieStore = cookies();
  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY,
    {
      cookies: {
        get(name) {
          return cookieStore.get(name)?.value;
        },
        set(name, value, options) {
          cookieStore.set({ name, value, ...options });
        },
        remove(name, options) {
          cookieStore.set({ name, value: '', ...options });
        },
      },
    }
  );
}
```

#### 5.4 — Auth Middleware (`middleware.js` at project root)

```javascript
import { createMiddlewareClient } from '@supabase/auth-helpers-nextjs';
import { NextResponse } from 'next/server';

export async function middleware(req) {
  const res = NextResponse.next();
  const supabase = createMiddlewareClient({ req, res });

  const {
    data: { session },
  } = await supabase.auth.getSession();

  // Redirect unauthenticated users from protected routes
  if (!session && req.nextUrl.pathname.startsWith('/dashboard')) {
    return NextResponse.redirect(new URL('/login', req.url));
  }

  // Redirect authenticated users away from auth pages
  if (session && (req.nextUrl.pathname === '/login' || req.nextUrl.pathname === '/signup')) {
    return NextResponse.redirect(new URL('/dashboard', req.url));
  }

  return res;
}

export const config = {
  matcher: ['/dashboard/:path*', '/login', '/signup'],
};
```

---

### Step 6 — Build Reusable UI Components

Create the following **base UI components** in `src/components/ui/`. Each
component should use the design system classes from Step 4.

Refer to the component templates in `examples/components/` for full
implementations of:

| Component        | File                  | Description                                  |
|------------------|-----------------------|----------------------------------------------|
| `Button`         | `Button.js`           | Primary, secondary, ghost, icon variants      |
| `Card`           | `Card.js`             | Glass card with hover glow effect             |
| `Input`          | `Input.js`            | Styled input with label, error states         |
| `Modal`          | `Modal.js`            | Animated overlay modal with framer-motion     |
| `Badge`          | `Badge.js`            | Status badges (success, warning, error, info) |
| `Avatar`         | `Avatar.js`           | User avatar with fallback initials            |
| `Dropdown`       | `Dropdown.js`         | Animated dropdown menu                        |
| `Toast`          | `Toast.js`            | Notification toasts with auto-dismiss         |
| `Skeleton`       | `Skeleton.js`         | Loading skeleton with shimmer animation       |
| `EmptyState`     | `EmptyState.js`       | Illustrated empty state placeholder           |

> **Design Rules:**
> - Every interactive element must have a `transition-all duration-300`
> - Hover states should include `translate-y-[-2px]` lift effect
> - Use `backdrop-blur` and semi-transparent backgrounds for glass effects
> - Shadows should use colored shadows (e.g., `shadow-primary-500/25`)
> - All components must have unique `id` attributes for testing

---

### Step 7 — Build Pages

For each page, follow this pattern:

1. **SEO**: Set page-specific `metadata` export
2. **Layout**: Use semantic HTML (`<main>`, `<section>`, `<nav>`, `<footer>`)
3. **Single `<h1>`**: One per page, with proper heading hierarchy
4. **Animations**: Use `framer-motion` for page transitions and element reveals
5. **Responsiveness**: Mobile-first with `sm:`, `md:`, `lg:` breakpoints
6. **Loading states**: Use `Skeleton` components for async data
7. **Error states**: Handle and display errors gracefully

**Example page with framer-motion animations:**

```jsx
'use client';

import { motion } from 'framer-motion';

const fadeInUp = {
  initial: { opacity: 0, y: 20 },
  animate: { opacity: 1, y: 0 },
  transition: { duration: 0.5 },
};

const staggerContainer = {
  animate: {
    transition: { staggerChildren: 0.1 },
  },
};

export default function DashboardPage() {
  return (
    <motion.main
      variants={staggerContainer}
      initial="initial"
      animate="animate"
      className="min-h-screen p-6 md:p-10"
    >
      <motion.h1
        variants={fadeInUp}
        className="text-4xl font-display font-bold text-gradient mb-8"
      >
        Dashboard
      </motion.h1>

      <motion.div
        variants={fadeInUp}
        className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"
      >
        {/* Cards, stats, content here */}
      </motion.div>
    </motion.main>
  );
}
```

---

### Step 8 — API Integration Patterns

#### 8.1 — Supabase Direct (Client-Side)

```javascript
'use client';

import { createClient } from '@/lib/supabase/client';
import { useEffect, useState } from 'react';

export function useItems() {
  const [items, setItems] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  const supabase = createClient();

  useEffect(() => {
    async function fetchItems() {
      try {
        const { data, error } = await supabase
          .from('items')
          .select('*')
          .order('created_at', { ascending: false });

        if (error) throw error;
        setItems(data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    }
    fetchItems();
  }, []);

  return { items, loading, error };
}
```

#### 8.2 — Next.js API Route (Server-Side)

```javascript
// src/app/api/items/route.js
import { createServerSupabaseClient } from '@/lib/supabase/server';
import { NextResponse } from 'next/server';

export async function GET() {
  const supabase = createServerSupabaseClient();

  const { data, error } = await supabase
    .from('items')
    .select('*')
    .order('created_at', { ascending: false });

  if (error) {
    return NextResponse.json({ error: error.message }, { status: 500 });
  }

  return NextResponse.json(data);
}

export async function POST(request) {
  const supabase = createServerSupabaseClient();
  const body = await request.json();

  const { data, error } = await supabase
    .from('items')
    .insert(body)
    .select()
    .single();

  if (error) {
    return NextResponse.json({ error: error.message }, { status: 400 });
  }

  return NextResponse.json(data, { status: 201 });
}
```

#### 8.3 — FastAPI Backend (Optional)

```python
# api/main.py
from fastapi import FastAPI, HTTPException
from fastapi.middleware.cors import CORSMiddleware
from supabase import create_client
import os
from dotenv import load_dotenv

load_dotenv()

app = FastAPI(title="App API", version="1.0.0")

app.add_middleware(
    CORSMiddleware,
    allow_origins=[os.getenv("FRONTEND_URL", "http://localhost:3000")],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

supabase = create_client(
    os.getenv("SUPABASE_URL"),
    os.getenv("SUPABASE_SERVICE_ROLE_KEY"),
)

@app.get("/api/items")
async def get_items():
    response = supabase.table("items").select("*").execute()
    return response.data

@app.post("/api/items")
async def create_item(item: dict):
    response = supabase.table("items").insert(item).execute()
    if not response.data:
        raise HTTPException(status_code=400, detail="Failed to create item")
    return response.data[0]
```

#### 8.4 — Express.js Backend (Optional)

```javascript
// api/index.js
const express = require('express');
const cors = require('cors');
const { createClient } = require('@supabase/supabase-js');
require('dotenv').config();

const app = express();
app.use(cors({ origin: process.env.FRONTEND_URL || 'http://localhost:3000' }));
app.use(express.json());

const supabase = createClient(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_SERVICE_ROLE_KEY
);

app.get('/api/items', async (req, res) => {
  const { data, error } = await supabase
    .from('items')
    .select('*')
    .order('created_at', { ascending: false });

  if (error) return res.status(500).json({ error: error.message });
  res.json(data);
});

app.post('/api/items', async (req, res) => {
  const { data, error } = await supabase
    .from('items')
    .insert(req.body)
    .select()
    .single();

  if (error) return res.status(400).json({ error: error.message });
  res.status(201).json(data);
});

const PORT = process.env.PORT || 4000;
app.listen(PORT, () => console.log(`API running on port ${PORT}`));
```

---

### Step 9 — Authentication Flow

Implement the following auth pages using Supabase Auth:

#### Login Page (`src/app/(auth)/login/page.js`)

- Email + password form
- Social login buttons (Google, GitHub — configurable)
- "Forgot password?" link
- Redirect to `/dashboard` on success
- Glass card design with gradient background

#### Signup Page (`src/app/(auth)/signup/page.js`)

- Email + password + confirm password
- Terms of service checkbox
- Social signup options
- Redirect to `/login` with success message after signup
- Email verification flow

#### Auth Hook (`src/hooks/useAuth.js`)

```javascript
'use client';

import { createClient } from '@/lib/supabase/client';
import { useEffect, useState } from 'react';
import { useRouter } from 'next/navigation';

export function useAuth() {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const supabase = createClient();
  const router = useRouter();

  useEffect(() => {
    const { data: { subscription } } = supabase.auth.onAuthStateChange(
      (event, session) => {
        setUser(session?.user ?? null);
        setLoading(false);
      }
    );

    return () => subscription.unsubscribe();
  }, []);

  const signIn = async (email, password) => {
    const { error } = await supabase.auth.signInWithPassword({ email, password });
    if (error) throw error;
    router.push('/dashboard');
  };

  const signUp = async (email, password) => {
    const { error } = await supabase.auth.signUp({ email, password });
    if (error) throw error;
  };

  const signOut = async () => {
    await supabase.auth.signOut();
    router.push('/login');
  };

  const signInWithOAuth = async (provider) => {
    const { error } = await supabase.auth.signInWithOAuth({
      provider,
      options: { redirectTo: `${window.location.origin}/dashboard` },
    });
    if (error) throw error;
  };

  return { user, loading, signIn, signUp, signOut, signInWithOAuth };
}
```

---

### Step 10 — Testing Setup

#### 10.1 — Unit & Integration Tests (Jest + RTL)

Install:
```bash
npm install -D jest @testing-library/react @testing-library/jest-dom jest-environment-jsdom
```

Create `jest.config.js`:
```javascript
const nextJest = require('next/jest');

const createJestConfig = nextJest({ dir: './' });

const customJestConfig = {
  setupFilesAfterSetup: ['<rootDir>/tests/setup.js'],
  testEnvironment: 'jest-environment-jsdom',
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/src/$1',
  },
};

module.exports = createJestConfig(customJestConfig);
```

Create `tests/setup.js`:
```javascript
import '@testing-library/jest-dom';
```

Add to `package.json` scripts:
```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage"
  }
}
```

#### 10.2 — E2E Tests (Playwright)

Install:
```bash
npm install -D @playwright/test
npx playwright install
```

Create `playwright.config.js`:
```javascript
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './tests/e2e',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: 'html',
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
  },
  webServer: {
    command: 'npm run dev',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
  },
});
```

Add to `package.json` scripts:
```json
{
  "scripts": {
    "test:e2e": "playwright test",
    "test:e2e:ui": "playwright test --ui"
  }
}
```

---

### Step 11 — SEO Optimization

Every page MUST include:

1. **Metadata export** — Title, description, keywords, OpenGraph
2. **Single `<h1>`** — One per page, descriptive
3. **Semantic HTML** — `<main>`, `<section>`, `<article>`, `<nav>`, `<footer>`
4. **Image alt text** — All images must have descriptive `alt` attributes
5. **Structured data** — Add JSON-LD for rich results where applicable

**Per-page metadata pattern:**

```javascript
export const metadata = {
  title: 'Page Title — App Name',
  description: 'A clear 150-160 character description of this page.',
  openGraph: {
    title: 'Page Title — App Name',
    description: 'Social sharing description.',
    images: [{ url: '/og-image.png', width: 1200, height: 630 }],
  },
};
```

**Sitemap** — Create `src/app/sitemap.js`:

```javascript
export default function sitemap() {
  return [
    { url: 'https://yourdomain.com', lastModified: new Date(), priority: 1 },
    { url: 'https://yourdomain.com/login', lastModified: new Date(), priority: 0.5 },
    // Add more pages
  ];
}
```

**Robots.txt** — Create `src/app/robots.js`:

```javascript
export default function robots() {
  return {
    rules: { userAgent: '*', allow: '/', disallow: '/dashboard/' },
    sitemap: 'https://yourdomain.com/sitemap.xml',
  };
}
```

---

### Step 12 — Deployment

#### 12.1 — Vercel (Primary)

1. Push code to GitHub
2. Connect repo to [Vercel](https://vercel.com)
3. Set environment variables in Vercel dashboard:
   - `NEXT_PUBLIC_SUPABASE_URL`
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
   - `SUPABASE_SERVICE_ROLE_KEY`
4. Deploy — Vercel auto-detects Next.js

**`vercel.json` (optional customization):**
```json
{
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "X-Frame-Options", "value": "DENY" },
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "Referrer-Policy", "value": "strict-origin-when-cross-origin" }
      ]
    }
  ]
}
```

#### 12.2 — Alternative Deployments

**Netlify:**
```bash
npm install -D @netlify/plugin-nextjs
# Add netlify.toml with build settings
```

**Railway (for FastAPI/Express backends):**
```bash
# Procfile for FastAPI
web: uvicorn api.main:app --host 0.0.0.0 --port $PORT

# Procfile for Express
web: node api/index.js
```

---

## Design Checklist ✅

Before considering any page "done", verify:

- [ ] **Dark mode** — Entire app uses dark theme by default
- [ ] **Glassmorphism** — Cards use `glass-card` class or equivalent
- [ ] **Gradients** — Headlines use `text-gradient`, CTAs use gradient backgrounds
- [ ] **Micro-animations** — Hover lifts, fade-ins, stagger reveals
- [ ] **Custom shadows** — Colored shadows (`shadow-primary-500/25`)
- [ ] **Typography** — Using Inter (body) and Outfit (display) fonts
- [ ] **Responsive** — Works on 320px to 1440px+ screens
- [ ] **Loading states** — Skeleton shimmer animations for async content
- [ ] **Empty states** — Illustrated empty states, not blank pages
- [ ] **Error states** — User-friendly error messages with retry actions
- [ ] **No browser defaults** — No default checkboxes, selects, scrollbars
- [ ] **Unique IDs** — All interactive elements have unique `id` attributes
- [ ] **SEO metadata** — Title, description, OpenGraph for every page

---

## Tools Used by This Skill

| Tool               | Purpose                                              |
|--------------------|------------------------------------------------------|
| `run_command`      | Scaffold project, install deps, run dev server        |
| `write_to_file`   | Create project files, components, configs             |
| `view_file`       | Read existing code for context                        |
| `replace_file_content` | Modify existing files                            |
| `search_web`      | Look up latest API docs, package versions             |
| `generate_image`  | Create assets (logos, OG images, illustrations)       |
| `browser_subagent` | Preview and test the running app                     |

---

## Limitations & Notes

- ❌ **Not for mobile-native apps** — Use React Native or Flutter instead
- ❌ **Not for CLI tools** — This skill is for web UIs only
- ❌ **Not for Vue/Angular/Svelte** — Focused on Next.js (React) stack
- ✅ **Supabase free tier** — Works within Supabase free tier limits
- ✅ **Vercel free tier** — Deployable on Vercel Hobby plan
- ⚠️ **API keys** — Always use `.env.local`, never hardcode secrets

---

## Example Workflow

**User says:** "Build me a task management app with teams and real-time updates"

1. **Clarify**: Confirm features — tasks CRUD, teams, real-time, auth
2. **Scaffold**: `npx create-next-app@latest` with all configurations
3. **Database**: Design Supabase tables — users, teams, tasks, team_members
4. **Design system**: Apply Tailwind config + global CSS
5. **Components**: Build Card, Button, Input, Modal, Sidebar, TaskCard
6. **Pages**: Home, Login, Signup, Dashboard, Team, Settings
7. **Auth**: Implement Supabase auth flow with middleware protection
8. **Real-time**: Set up Supabase real-time subscriptions for tasks
9. **API routes**: Create CRUD endpoints for tasks and teams
10. **Testing**: Write unit tests for components, E2E for auth flow
11. **SEO**: Add metadata, sitemap, robots.txt
12. **Deploy**: Push to GitHub → Deploy to Vercel
13. **Report**: Summarize what was built and provide access URLs
