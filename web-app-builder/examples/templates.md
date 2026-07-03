# Starter Templates

This file contains complete starter page templates that the skill can scaffold
for common page types. Each template follows the design system defined in the
main SKILL.md.

---

## Landing Page Template

A premium dark landing page with hero, features grid, CTA, and footer.

```jsx
// src/app/page.js
'use client';

import { motion } from 'framer-motion';
import {
  Sparkles, Zap, Shield, BarChart3, ArrowRight, Github, Twitter,
} from 'lucide-react';
import Link from 'next/link';
import Button from '@/components/ui/Button';
import Card from '@/components/ui/Card';
import Navbar from '@/components/layout/Navbar';

const fadeInUp = {
  initial: { opacity: 0, y: 30 },
  animate: { opacity: 1, y: 0 },
};

const stagger = {
  animate: { transition: { staggerChildren: 0.12 } },
};

const features = [
  {
    icon: Zap,
    title: 'Lightning Fast',
    description: 'Built on Next.js for blazing performance and instant page loads.',
  },
  {
    icon: Shield,
    title: 'Secure by Default',
    description: 'Enterprise-grade auth and row-level security with Supabase.',
  },
  {
    icon: BarChart3,
    title: 'Real-Time Data',
    description: 'Live subscriptions and instant updates across all connected clients.',
  },
  {
    icon: Sparkles,
    title: 'Beautiful UI',
    description: 'Glassmorphism, gradients, and micro-animations out of the box.',
  },
];

export default function LandingPage() {
  return (
    <div className="min-h-screen">
      <Navbar />

      {/* Hero Section */}
      <section className="relative pt-32 pb-20 px-4 overflow-hidden">
        {/* Gradient orbs (background decoration) */}
        <div className="absolute top-20 left-1/4 w-96 h-96 bg-primary-500/20 rounded-full blur-[128px]" />
        <div className="absolute bottom-10 right-1/4 w-80 h-80 bg-accent-500/15 rounded-full blur-[100px]" />

        <motion.div
          variants={stagger}
          initial="initial"
          animate="animate"
          className="relative max-w-4xl mx-auto text-center"
        >
          {/* Badge */}
          <motion.div variants={fadeInUp} className="mb-6">
            <span className="inline-flex items-center gap-2 px-4 py-1.5 rounded-full
                             bg-primary-500/10 border border-primary-500/20 text-primary-300 text-sm">
              <Sparkles className="h-3.5 w-3.5" />
              Now in Public Beta
            </span>
          </motion.div>

          {/* Headline */}
          <motion.h1
            variants={fadeInUp}
            className="text-5xl sm:text-6xl lg:text-7xl font-display font-bold
                       leading-tight tracking-tight mb-6"
          >
            Build apps that{' '}
            <span className="text-gradient">wow your users</span>
          </motion.h1>

          {/* Subtitle */}
          <motion.p
            variants={fadeInUp}
            className="text-lg sm:text-xl text-surface-300 max-w-2xl mx-auto mb-10 text-balance"
          >
            The modern full-stack platform for building beautiful, real-time
            web applications — powered by Next.js and Supabase.
          </motion.p>

          {/* CTA Buttons */}
          <motion.div variants={fadeInUp} className="flex flex-wrap gap-4 justify-center">
            <Link href="/signup">
              <Button size="lg" icon={ArrowRight} iconPosition="right">
                Get Started Free
              </Button>
            </Link>
            <Link href="https://github.com" target="_blank">
              <Button variant="secondary" size="lg" icon={Github}>
                View on GitHub
              </Button>
            </Link>
          </motion.div>
        </motion.div>
      </section>

      {/* Features Section */}
      <section id="features" className="py-24 px-4">
        <div className="max-w-6xl mx-auto">
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            whileInView={{ opacity: 1, y: 0 }}
            viewport={{ once: true }}
            className="text-center mb-16"
          >
            <h2 className="text-3xl sm:text-4xl font-display font-bold mb-4">
              Everything you need
            </h2>
            <p className="text-surface-400 text-lg max-w-xl mx-auto">
              A complete toolkit for building modern web applications without compromise.
            </p>
          </motion.div>

          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            {features.map((feature, i) => (
              <motion.div
                key={feature.title}
                initial={{ opacity: 0, y: 20 }}
                whileInView={{ opacity: 1, y: 0 }}
                viewport={{ once: true }}
                transition={{ delay: i * 0.1 }}
              >
                <Card glow className="h-full">
                  <div className="flex items-start gap-4">
                    <div className="flex-shrink-0 p-3 rounded-xl bg-primary-500/10
                                    border border-primary-500/20">
                      <feature.icon className="h-6 w-6 text-primary-400" />
                    </div>
                    <div>
                      <h3 className="text-lg font-semibold text-white mb-2">
                        {feature.title}
                      </h3>
                      <p className="text-surface-400">{feature.description}</p>
                    </div>
                  </div>
                </Card>
              </motion.div>
            ))}
          </div>
        </div>
      </section>

      {/* CTA Section */}
      <section className="py-24 px-4">
        <div className="max-w-3xl mx-auto text-center">
          <motion.div
            initial={{ opacity: 0, scale: 0.95 }}
            whileInView={{ opacity: 1, scale: 1 }}
            viewport={{ once: true }}
            className="glass-card p-12 relative overflow-hidden"
          >
            <div className="absolute -top-20 -right-20 w-60 h-60 bg-primary-500/20 rounded-full blur-[80px]" />
            <div className="absolute -bottom-20 -left-20 w-60 h-60 bg-accent-500/15 rounded-full blur-[80px]" />

            <div className="relative">
              <h2 className="text-3xl sm:text-4xl font-display font-bold mb-4">
                Ready to get started?
              </h2>
              <p className="text-surface-400 text-lg mb-8 max-w-lg mx-auto">
                Join thousands of developers building the future of the web.
              </p>
              <Link href="/signup">
                <Button size="xl" icon={ArrowRight} iconPosition="right">
                  Start Building Now
                </Button>
              </Link>
            </div>
          </motion.div>
        </div>
      </section>

      {/* Footer */}
      <footer className="border-t border-white/5 py-12 px-4">
        <div className="max-w-6xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-4">
          <p className="text-sm text-surface-500">
            © {new Date().getFullYear()} AppName. All rights reserved.
          </p>
          <div className="flex items-center gap-4">
            <a href="#" className="text-surface-500 hover:text-white transition-colors">
              <Twitter className="h-5 w-5" />
            </a>
            <a href="#" className="text-surface-500 hover:text-white transition-colors">
              <Github className="h-5 w-5" />
            </a>
          </div>
        </div>
      </footer>
    </div>
  );
}
```

---

## Login Page Template

```jsx
// src/app/(auth)/login/page.js
'use client';

import { useState } from 'react';
import { motion } from 'framer-motion';
import Link from 'next/link';
import { Mail, Lock, Eye, EyeOff } from 'lucide-react';
import Button from '@/components/ui/Button';
import Input from '@/components/ui/Input';
import { useAuth } from '@/hooks/useAuth';

export default function LoginPage() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [showPassword, setShowPassword] = useState(false);
  const [error, setError] = useState('');
  const [loading, setLoading] = useState(false);
  const { signIn, signInWithOAuth } = useAuth();

  const handleSubmit = async (e) => {
    e.preventDefault();
    setError('');
    setLoading(true);
    try {
      await signIn(email, password);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center px-4 relative">
      {/* Background decorations */}
      <div className="absolute top-1/4 left-1/4 w-96 h-96 bg-primary-500/15 rounded-full blur-[128px]" />
      <div className="absolute bottom-1/4 right-1/3 w-72 h-72 bg-accent-500/10 rounded-full blur-[100px]" />

      <motion.div
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
        className="relative w-full max-w-md"
      >
        <div className="glass-card p-8">
          {/* Header */}
          <div className="text-center mb-8">
            <Link href="/" className="text-2xl font-display font-bold text-gradient inline-block mb-2">
              AppName
            </Link>
            <p className="text-surface-400">Welcome back! Sign in to continue.</p>
          </div>

          {/* Social Login */}
          <div className="grid grid-cols-2 gap-3 mb-6">
            <Button
              variant="secondary"
              size="md"
              onClick={() => signInWithOAuth('google')}
              className="w-full"
            >
              Google
            </Button>
            <Button
              variant="secondary"
              size="md"
              onClick={() => signInWithOAuth('github')}
              className="w-full"
            >
              GitHub
            </Button>
          </div>

          <div className="relative mb-6">
            <div className="absolute inset-0 flex items-center">
              <div className="w-full border-t border-white/10" />
            </div>
            <div className="relative flex justify-center text-xs">
              <span className="px-3 text-surface-500 bg-surface-900">
                or continue with email
              </span>
            </div>
          </div>

          {/* Form */}
          <form onSubmit={handleSubmit} className="space-y-4">
            {error && (
              <motion.div
                initial={{ opacity: 0, y: -10 }}
                animate={{ opacity: 1, y: 0 }}
                className="p-3 rounded-xl bg-red-500/10 border border-red-500/20
                           text-red-300 text-sm text-center"
              >
                {error}
              </motion.div>
            )}

            <Input
              id="login-email"
              label="Email"
              type="email"
              icon={Mail}
              placeholder="you@example.com"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              required
            />

            <div className="relative">
              <Input
                id="login-password"
                label="Password"
                type={showPassword ? 'text' : 'password'}
                icon={Lock}
                placeholder="Enter your password"
                value={password}
                onChange={(e) => setPassword(e.target.value)}
                required
              />
              <button
                type="button"
                onClick={() => setShowPassword(!showPassword)}
                className="absolute right-3 top-[38px] text-surface-500 hover:text-white transition-colors"
              >
                {showPassword ? <EyeOff className="h-4 w-4" /> : <Eye className="h-4 w-4" />}
              </button>
            </div>

            <div className="flex justify-end">
              <Link
                href="/forgot-password"
                className="text-sm text-primary-400 hover:text-primary-300 transition-colors"
              >
                Forgot password?
              </Link>
            </div>

            <Button
              type="submit"
              loading={loading}
              className="w-full"
              size="lg"
            >
              Sign In
            </Button>
          </form>

          {/* Footer */}
          <p className="text-center text-sm text-surface-400 mt-6">
            Don't have an account?{' '}
            <Link
              href="/signup"
              className="text-primary-400 hover:text-primary-300 transition-colors font-medium"
            >
              Sign up
            </Link>
          </p>
        </div>
      </motion.div>
    </div>
  );
}
```

---

## Dashboard Layout Template

```jsx
// src/app/dashboard/layout.js
'use client';

import { useState } from 'react';
import Link from 'next/link';
import { usePathname } from 'next/navigation';
import { motion } from 'framer-motion';
import {
  LayoutDashboard, FolderKanban, Users, Settings,
  LogOut, ChevronLeft, ChevronRight,
} from 'lucide-react';
import { useAuth } from '@/hooks/useAuth';
import { clsx } from 'clsx';

const menuItems = [
  { label: 'Dashboard', href: '/dashboard', icon: LayoutDashboard },
  { label: 'Projects', href: '/dashboard/projects', icon: FolderKanban },
  { label: 'Team', href: '/dashboard/team', icon: Users },
  { label: 'Settings', href: '/dashboard/settings', icon: Settings },
];

export default function DashboardLayout({ children }) {
  const [collapsed, setCollapsed] = useState(false);
  const pathname = usePathname();
  const { user, signOut } = useAuth();

  return (
    <div className="flex h-screen overflow-hidden bg-surface-950">
      {/* Sidebar */}
      <motion.aside
        animate={{ width: collapsed ? 72 : 260 }}
        transition={{ type: 'spring', damping: 25, stiffness: 200 }}
        className="flex flex-col border-r border-white/5 bg-surface-900/50 backdrop-blur-xl"
      >
        {/* Logo */}
        <div className="flex items-center h-16 px-4 border-b border-white/5">
          {!collapsed && (
            <Link href="/" className="text-lg font-display font-bold text-gradient">
              AppName
            </Link>
          )}
          <button
            onClick={() => setCollapsed(!collapsed)}
            className={clsx(
              'p-1.5 rounded-lg text-surface-400 hover:text-white hover:bg-white/5 transition-all',
              collapsed ? 'mx-auto' : 'ml-auto'
            )}
          >
            {collapsed ? <ChevronRight className="h-4 w-4" /> : <ChevronLeft className="h-4 w-4" />}
          </button>
        </div>

        {/* Menu */}
        <nav className="flex-1 py-4 px-2 space-y-1">
          {menuItems.map((item) => {
            const isActive = pathname === item.href;
            return (
              <Link
                key={item.href}
                href={item.href}
                className={clsx(
                  'flex items-center gap-3 px-3 py-2.5 rounded-xl transition-all duration-200',
                  isActive
                    ? 'bg-primary-500/10 text-primary-300 border border-primary-500/20'
                    : 'text-surface-400 hover:text-white hover:bg-white/5'
                )}
              >
                <item.icon className={clsx('h-5 w-5 flex-shrink-0', isActive && 'text-primary-400')} />
                {!collapsed && <span className="text-sm font-medium">{item.label}</span>}
              </Link>
            );
          })}
        </nav>

        {/* User section */}
        <div className="p-2 border-t border-white/5">
          <button
            onClick={signOut}
            className="flex items-center gap-3 w-full px-3 py-2.5 rounded-xl
                       text-surface-400 hover:text-red-400 hover:bg-red-500/5
                       transition-all duration-200"
          >
            <LogOut className="h-5 w-5 flex-shrink-0" />
            {!collapsed && <span className="text-sm">Sign Out</span>}
          </button>
        </div>
      </motion.aside>

      {/* Main content */}
      <main className="flex-1 overflow-y-auto">
        <div className="p-6 md:p-10">
          {children}
        </div>
      </main>
    </div>
  );
}
```

---

## Dashboard Home Template

```jsx
// src/app/dashboard/page.js
'use client';

import { motion } from 'framer-motion';
import { TrendingUp, Users, FolderKanban, Activity } from 'lucide-react';
import Card from '@/components/ui/Card';
import Badge from '@/components/ui/Badge';

const fadeInUp = {
  initial: { opacity: 0, y: 20 },
  animate: { opacity: 1, y: 0 },
};

const stagger = {
  animate: { transition: { staggerChildren: 0.08 } },
};

const stats = [
  { label: 'Total Revenue', value: '$12,450', change: '+12.5%', icon: TrendingUp, trend: 'up' },
  { label: 'Active Users', value: '1,234', change: '+8.2%', icon: Users, trend: 'up' },
  { label: 'Projects', value: '23', change: '+3', icon: FolderKanban, trend: 'up' },
  { label: 'Uptime', value: '99.9%', change: 'Healthy', icon: Activity, trend: 'neutral' },
];

export default function DashboardPage() {
  return (
    <motion.div variants={stagger} initial="initial" animate="animate">
      {/* Header */}
      <motion.div variants={fadeInUp} className="mb-8">
        <h1 className="text-3xl font-display font-bold text-white mb-1">
          Dashboard
        </h1>
        <p className="text-surface-400">
          Welcome back! Here's an overview of your app.
        </p>
      </motion.div>

      {/* Stats Grid */}
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-8">
        {stats.map((stat) => (
          <motion.div key={stat.label} variants={fadeInUp}>
            <Card className="flex items-start justify-between">
              <div>
                <p className="text-sm text-surface-400 mb-1">{stat.label}</p>
                <p className="text-2xl font-bold text-white">{stat.value}</p>
                <Badge
                  variant={stat.trend === 'up' ? 'success' : 'info'}
                  size="sm"
                  dot
                  className="mt-2"
                >
                  {stat.change}
                </Badge>
              </div>
              <div className="p-2.5 rounded-xl bg-primary-500/10">
                <stat.icon className="h-5 w-5 text-primary-400" />
              </div>
            </Card>
          </motion.div>
        ))}
      </div>

      {/* Content area — add charts, tables, recent activity here */}
      <motion.div variants={fadeInUp}>
        <Card padding="lg">
          <h2 className="text-lg font-semibold text-white mb-4">
            Recent Activity
          </h2>
          <p className="text-surface-400">
            Activity feed and charts will go here.
          </p>
        </Card>
      </motion.div>
    </motion.div>
  );
}
```
