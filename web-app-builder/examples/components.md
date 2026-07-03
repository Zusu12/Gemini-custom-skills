# Button Component

A versatile button component with multiple variants and states.

```jsx
// src/components/ui/Button.js
'use client';

import { forwardRef } from 'react';
import { clsx } from 'clsx';

const variants = {
  primary:
    'bg-gradient-to-r from-primary-600 to-primary-500 text-white shadow-lg shadow-primary-500/25 hover:shadow-xl hover:shadow-primary-500/40',
  secondary:
    'border border-white/10 bg-white/5 text-surface-200 backdrop-blur-sm hover:bg-white/10 hover:border-white/20',
  ghost:
    'text-surface-300 hover:text-white hover:bg-white/5',
  danger:
    'bg-gradient-to-r from-red-600 to-red-500 text-white shadow-lg shadow-red-500/25 hover:shadow-xl hover:shadow-red-500/40',
  accent:
    'bg-gradient-to-r from-accent-600 to-accent-500 text-white shadow-lg shadow-accent-500/25 hover:shadow-xl hover:shadow-accent-500/40',
};

const sizes = {
  sm: 'px-3 py-1.5 text-sm rounded-lg',
  md: 'px-5 py-2.5 text-sm rounded-xl',
  lg: 'px-6 py-3 text-base rounded-xl',
  xl: 'px-8 py-4 text-lg rounded-2xl',
  icon: 'p-2.5 rounded-xl',
};

const Button = forwardRef(function Button(
  {
    children,
    variant = 'primary',
    size = 'md',
    className,
    disabled,
    loading,
    icon: Icon,
    iconPosition = 'left',
    ...props
  },
  ref
) {
  return (
    <button
      ref={ref}
      disabled={disabled || loading}
      className={clsx(
        'relative inline-flex items-center justify-center gap-2 font-semibold',
        'transition-all duration-300',
        'hover:translate-y-[-2px] active:translate-y-0',
        'focus:outline-none focus:ring-2 focus:ring-primary-400/50',
        'disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:translate-y-0',
        variants[variant],
        sizes[size],
        className
      )}
      {...props}
    >
      {loading && (
        <svg
          className="animate-spin h-4 w-4"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
        >
          <circle
            className="opacity-25"
            cx="12"
            cy="12"
            r="10"
            stroke="currentColor"
            strokeWidth="4"
          />
          <path
            className="opacity-75"
            fill="currentColor"
            d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4z"
          />
        </svg>
      )}
      {Icon && iconPosition === 'left' && !loading && <Icon className="h-4 w-4" />}
      {children}
      {Icon && iconPosition === 'right' && <Icon className="h-4 w-4" />}
    </button>
  );
});

export default Button;
```

---

# Card Component

A glassmorphism card with optional hover glow effect.

```jsx
// src/components/ui/Card.js
'use client';

import { clsx } from 'clsx';

export default function Card({
  children,
  className,
  hover = true,
  glow = false,
  padding = 'md',
  ...props
}) {
  const paddings = {
    none: '',
    sm: 'p-4',
    md: 'p-6',
    lg: 'p-8',
  };

  return (
    <div
      className={clsx(
        'glass-card',
        hover && 'transition-all duration-300 hover:translate-y-[-4px] hover:border-white/20',
        glow && 'glow',
        paddings[padding],
        className
      )}
      {...props}
    >
      {children}
    </div>
  );
}
```

---

# Input Component

A styled input with floating label, validation states, and icons.

```jsx
// src/components/ui/Input.js
'use client';

import { forwardRef, useState } from 'react';
import { clsx } from 'clsx';

const Input = forwardRef(function Input(
  {
    label,
    error,
    icon: Icon,
    type = 'text',
    className,
    id,
    ...props
  },
  ref
) {
  const [focused, setFocused] = useState(false);

  return (
    <div className="w-full">
      {label && (
        <label
          htmlFor={id}
          className={clsx(
            'block text-sm font-medium mb-1.5 transition-colors duration-200',
            error ? 'text-red-400' : focused ? 'text-primary-400' : 'text-surface-300'
          )}
        >
          {label}
        </label>
      )}
      <div className="relative">
        {Icon && (
          <Icon
            className={clsx(
              'absolute left-3 top-1/2 -translate-y-1/2 h-4 w-4 transition-colors',
              error ? 'text-red-400' : focused ? 'text-primary-400' : 'text-surface-500'
            )}
          />
        )}
        <input
          ref={ref}
          id={id}
          type={type}
          onFocus={() => setFocused(true)}
          onBlur={() => setFocused(false)}
          className={clsx(
            'w-full rounded-xl border bg-white/5 px-4 py-3 text-surface-100',
            'placeholder:text-surface-500',
            'backdrop-blur-sm transition-all duration-300',
            'focus:outline-none focus:ring-2',
            Icon && 'pl-10',
            error
              ? 'border-red-500/50 focus:border-red-400 focus:ring-red-400/20'
              : 'border-white/10 focus:border-primary-400 focus:ring-primary-400/20 hover:border-white/20',
            className
          )}
          {...props}
        />
      </div>
      {error && (
        <p className="mt-1.5 text-sm text-red-400 animate-fade-in">{error}</p>
      )}
    </div>
  );
});

export default Input;
```

---

# Modal Component

An animated modal overlay using framer-motion.

```jsx
// src/components/ui/Modal.js
'use client';

import { useEffect } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { X } from 'lucide-react';

export default function Modal({
  isOpen,
  onClose,
  title,
  children,
  size = 'md',
}) {
  const sizes = {
    sm: 'max-w-sm',
    md: 'max-w-md',
    lg: 'max-w-lg',
    xl: 'max-w-xl',
    full: 'max-w-4xl',
  };

  // Lock body scroll when modal is open
  useEffect(() => {
    if (isOpen) {
      document.body.style.overflow = 'hidden';
    } else {
      document.body.style.overflow = '';
    }
    return () => { document.body.style.overflow = ''; };
  }, [isOpen]);

  return (
    <AnimatePresence>
      {isOpen && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-4">
          {/* Backdrop */}
          <motion.div
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            onClick={onClose}
            className="absolute inset-0 bg-black/60 backdrop-blur-sm"
          />

          {/* Modal content */}
          <motion.div
            initial={{ opacity: 0, scale: 0.95, y: 20 }}
            animate={{ opacity: 1, scale: 1, y: 0 }}
            exit={{ opacity: 0, scale: 0.95, y: 20 }}
            transition={{ type: 'spring', damping: 25, stiffness: 300 }}
            className={`relative w-full ${sizes[size]} glass-card`}
          >
            {/* Header */}
            <div className="flex items-center justify-between mb-4">
              <h2 className="text-xl font-display font-bold text-white">
                {title}
              </h2>
              <button
                onClick={onClose}
                className="p-1.5 rounded-lg text-surface-400 hover:text-white
                           hover:bg-white/10 transition-all duration-200"
              >
                <X className="h-5 w-5" />
              </button>
            </div>

            {/* Body */}
            {children}
          </motion.div>
        </div>
      )}
    </AnimatePresence>
  );
}
```

---

# Badge Component

Status badges with color variants.

```jsx
// src/components/ui/Badge.js
import { clsx } from 'clsx';

const variants = {
  default: 'bg-surface-700 text-surface-200',
  primary: 'bg-primary-500/20 text-primary-300 border border-primary-500/30',
  success: 'bg-emerald-500/20 text-emerald-300 border border-emerald-500/30',
  warning: 'bg-amber-500/20 text-amber-300 border border-amber-500/30',
  danger: 'bg-red-500/20 text-red-300 border border-red-500/30',
  info: 'bg-sky-500/20 text-sky-300 border border-sky-500/30',
  accent: 'bg-accent-500/20 text-accent-300 border border-accent-500/30',
};

export default function Badge({
  children,
  variant = 'default',
  size = 'md',
  dot = false,
  className,
}) {
  const sizes = {
    sm: 'px-2 py-0.5 text-xs',
    md: 'px-2.5 py-1 text-xs',
    lg: 'px-3 py-1.5 text-sm',
  };

  return (
    <span
      className={clsx(
        'inline-flex items-center gap-1.5 rounded-full font-medium',
        variants[variant],
        sizes[size],
        className
      )}
    >
      {dot && (
        <span
          className={clsx('h-1.5 w-1.5 rounded-full', {
            'bg-surface-400': variant === 'default',
            'bg-primary-400': variant === 'primary',
            'bg-emerald-400': variant === 'success',
            'bg-amber-400': variant === 'warning',
            'bg-red-400': variant === 'danger',
            'bg-sky-400': variant === 'info',
            'bg-accent-400': variant === 'accent',
          })}
        />
      )}
      {children}
    </span>
  );
}
```

---

# Skeleton Component

Loading skeleton with shimmer animation.

```jsx
// src/components/ui/Skeleton.js
import { clsx } from 'clsx';

export function Skeleton({ className, ...props }) {
  return <div className={clsx('skeleton', className)} {...props} />;
}

export function SkeletonText({ lines = 3, className }) {
  return (
    <div className={clsx('space-y-3', className)}>
      {Array.from({ length: lines }).map((_, i) => (
        <Skeleton
          key={i}
          className={clsx(
            'h-4 rounded',
            i === lines - 1 ? 'w-3/4' : 'w-full'
          )}
        />
      ))}
    </div>
  );
}

export function SkeletonCard({ className }) {
  return (
    <div className={clsx('glass-card space-y-4', className)}>
      <Skeleton className="h-40 w-full rounded-xl" />
      <Skeleton className="h-6 w-3/4 rounded" />
      <SkeletonText lines={2} />
      <div className="flex gap-2">
        <Skeleton className="h-8 w-20 rounded-lg" />
        <Skeleton className="h-8 w-20 rounded-lg" />
      </div>
    </div>
  );
}
```

---

# Toast Component

Notification toasts with auto-dismiss.

```jsx
// src/components/ui/Toast.js
'use client';

import { useEffect, useState } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { X, CheckCircle, AlertTriangle, AlertCircle, Info } from 'lucide-react';

const icons = {
  success: CheckCircle,
  warning: AlertTriangle,
  error: AlertCircle,
  info: Info,
};

const styles = {
  success: 'border-emerald-500/30 bg-emerald-500/10',
  warning: 'border-amber-500/30 bg-amber-500/10',
  error: 'border-red-500/30 bg-red-500/10',
  info: 'border-sky-500/30 bg-sky-500/10',
};

const iconColors = {
  success: 'text-emerald-400',
  warning: 'text-amber-400',
  error: 'text-red-400',
  info: 'text-sky-400',
};

export default function Toast({
  message,
  type = 'info',
  duration = 4000,
  onClose,
  id,
}) {
  const Icon = icons[type];

  useEffect(() => {
    if (duration > 0) {
      const timer = setTimeout(onClose, duration);
      return () => clearTimeout(timer);
    }
  }, [duration, onClose]);

  return (
    <motion.div
      layout
      initial={{ opacity: 0, x: 50, scale: 0.95 }}
      animate={{ opacity: 1, x: 0, scale: 1 }}
      exit={{ opacity: 0, x: 50, scale: 0.95 }}
      className={`flex items-center gap-3 rounded-xl border p-4 backdrop-blur-xl shadow-glass ${styles[type]}`}
    >
      <Icon className={`h-5 w-5 flex-shrink-0 ${iconColors[type]}`} />
      <p className="text-sm text-surface-200 flex-1">{message}</p>
      <button
        onClick={onClose}
        className="p-1 rounded-lg text-surface-400 hover:text-white
                   hover:bg-white/10 transition-all duration-200"
      >
        <X className="h-4 w-4" />
      </button>
    </motion.div>
  );
}

// Toast Container — place in root layout
export function ToastContainer({ toasts, removeToast }) {
  return (
    <div className="fixed bottom-6 right-6 z-[100] flex flex-col gap-3 max-w-sm w-full">
      <AnimatePresence mode="popLayout">
        {toasts.map((toast) => (
          <Toast
            key={toast.id}
            {...toast}
            onClose={() => removeToast(toast.id)}
          />
        ))}
      </AnimatePresence>
    </div>
  );
}
```

---

# Navbar Component

```jsx
// src/components/layout/Navbar.js
'use client';

import { useState } from 'react';
import Link from 'next/link';
import { motion, AnimatePresence } from 'framer-motion';
import { Menu, X } from 'lucide-react';
import Button from '@/components/ui/Button';

export default function Navbar({ user, onSignOut }) {
  const [mobileOpen, setMobileOpen] = useState(false);

  const navLinks = [
    { label: 'Home', href: '/' },
    { label: 'Features', href: '/#features' },
    { label: 'Pricing', href: '/#pricing' },
  ];

  return (
    <nav className="fixed top-0 left-0 right-0 z-50">
      <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div className="flex h-16 items-center justify-between rounded-2xl mt-4 px-6
                        border border-white/10 bg-surface-900/80 backdrop-blur-xl">
          {/* Logo */}
          <Link href="/" className="text-xl font-display font-bold text-gradient">
            AppName
          </Link>

          {/* Desktop nav */}
          <div className="hidden md:flex items-center gap-6">
            {navLinks.map((link) => (
              <Link
                key={link.href}
                href={link.href}
                className="text-sm text-surface-300 hover:text-white transition-colors"
              >
                {link.label}
              </Link>
            ))}
          </div>

          {/* Auth buttons */}
          <div className="hidden md:flex items-center gap-3">
            {user ? (
              <>
                <Link href="/dashboard">
                  <Button variant="ghost" size="sm">Dashboard</Button>
                </Link>
                <Button variant="secondary" size="sm" onClick={onSignOut}>
                  Sign Out
                </Button>
              </>
            ) : (
              <>
                <Link href="/login">
                  <Button variant="ghost" size="sm">Log In</Button>
                </Link>
                <Link href="/signup">
                  <Button variant="primary" size="sm">Get Started</Button>
                </Link>
              </>
            )}
          </div>

          {/* Mobile toggle */}
          <button
            onClick={() => setMobileOpen(!mobileOpen)}
            className="md:hidden p-2 text-surface-300 hover:text-white"
          >
            {mobileOpen ? <X className="h-5 w-5" /> : <Menu className="h-5 w-5" />}
          </button>
        </div>
      </div>

      {/* Mobile menu */}
      <AnimatePresence>
        {mobileOpen && (
          <motion.div
            initial={{ opacity: 0, y: -10 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -10 }}
            className="md:hidden mx-4 mt-2 rounded-2xl border border-white/10
                       bg-surface-900/95 backdrop-blur-xl p-4 space-y-3"
          >
            {navLinks.map((link) => (
              <Link
                key={link.href}
                href={link.href}
                onClick={() => setMobileOpen(false)}
                className="block px-3 py-2 rounded-lg text-surface-300
                           hover:text-white hover:bg-white/5 transition-all"
              >
                {link.label}
              </Link>
            ))}
            <div className="pt-3 border-t border-white/10 flex flex-col gap-2">
              {user ? (
                <>
                  <Link href="/dashboard">
                    <Button variant="secondary" size="sm" className="w-full">Dashboard</Button>
                  </Link>
                  <Button variant="ghost" size="sm" className="w-full" onClick={onSignOut}>
                    Sign Out
                  </Button>
                </>
              ) : (
                <>
                  <Link href="/login">
                    <Button variant="secondary" size="sm" className="w-full">Log In</Button>
                  </Link>
                  <Link href="/signup">
                    <Button variant="primary" size="sm" className="w-full">Get Started</Button>
                  </Link>
                </>
              )}
            </div>
          </motion.div>
        )}
      </AnimatePresence>
    </nav>
  );
}
```
