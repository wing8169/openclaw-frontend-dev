---
name: ui-development
description: Generate production-ready Next.js projects with TypeScript, Tailwind CSS, shadcn/ui, and API integration. Use when the user asks to build, create, develop, or scaffold a Next.js application, web app, full-stack project, or frontend with backend integration. Prioritizes modern stack (Next.js 14+, TypeScript, shadcn/ui, axios, react-query) and best practices. Also triggers on requests to add features, integrate APIs, or extend existing Next.js projects.
---

# UI Development

Generate production-ready Next.js projects from natural language, with shadcn/ui components, API integration, type safety, and modern tooling.

## Requirements & Optional Features

### Required Dependencies
- **Node.js 18+** and **npm/yarn/pnpm**
- **Git** (for project initialization)

### Optional Features (user can decline)

#### 1. Auto-Revision with Visual Review (requires Chromium)
- **What it does**: Takes screenshots during development to visually review designs and auto-fix issues
- **Installation**: `sudo apt-get install chromium-browser` (Debian/Ubuntu)
- **Privileges**: Read/write access to project files, execute chromium in headless mode
- **If declined**: Manual review only (you describe, user verifies)

#### 2. Live Preview Server (requires Nginx)
- **What it does**: Serves project on local network for live preview during development
- **Installation**: `sudo apt-get install nginx`
- **Nginx setup**:
  ```nginx
  server {
    listen <port>;
    server_name _;
    location / {
      proxy_pass http://localhost:3000;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection 'upgrade';
      proxy_set_header Host $host;
      proxy_cache_bypass $http_upgrade;
    }
  }
  ```
- **Privileges**: Read nginx config (`/etc/nginx/sites-available/`), reload nginx (`sudo systemctl reload nginx`)
- **If declined**: Use Next.js dev server only (`npm run dev` on localhost:3000)

**Before starting, ask user if they want to enable optional features.**

## Tech Stack

**Core:**
- Next.js 14+ (App Router)
- TypeScript
- Tailwind CSS v3
- **shadcn/ui** (recommended UI component library)
- ESLint + Prettier

**API Integration (default):**
- axios (HTTP client)
- @tanstack/react-query (data fetching, caching, state management)

**Optional (based on needs):**
- Zustand (client-side state management)
- Zod (runtime validation)
- next-auth (authentication)
- Prisma (database ORM)

## Project Structure

**Industry-standard Next.js 14+ App Router structure with feature-based organization:**

```
<project-name>/
├── app/                                # Next.js 14 App Router
│   ├── (auth)/                         # Route group (auth pages)
│   │   ├── login/
│   │   │   └── page.tsx
│   │   ├── register/
│   │   │   └── page.tsx
│   │   └── layout.tsx                  # Auth-specific layout
│   ├── (dashboard)/                    # Route group (protected pages)
│   │   ├── dashboard/
│   │   │   ├── page.tsx
│   │   │   └── loading.tsx
│   │   ├── profile/
│   │   │   └── page.tsx
│   │   ├── settings/
│   │   │   └── page.tsx
│   │   └── layout.tsx                  # Dashboard layout with sidebar
│   ├── api/                            # API routes
│   │   ├── auth/
│   │   │   └── [...nextauth]/route.ts
│   │   └── users/
│   │       └── route.ts
│   ├── layout.tsx                      # Root layout
│   ├── page.tsx                        # Home page
│   ├── loading.tsx                     # Root loading UI
│   ├── error.tsx                       # Root error boundary
│   ├── not-found.tsx                   # 404 page
│   └── providers.tsx                   # Client providers (React Query, etc.)
│
├── components/
│   ├── ui/                             # shadcn/ui components (auto-generated)
│   │   ├── button.tsx
│   │   ├── card.tsx
│   │   ├── input.tsx
│   │   ├── form.tsx
│   │   └── ...
│   ├── layout/                         # Layout components
│   │   ├── header.tsx
│   │   ├── footer.tsx
│   │   ├── sidebar.tsx
│   │   └── mobile-nav.tsx
│   ├── features/                       # Feature-specific components
│   │   ├── auth/
│   │   │   ├── login-form.tsx
│   │   │   └── register-form.tsx
│   │   ├── dashboard/
│   │   │   ├── stats-card.tsx
│   │   │   └── recent-activity.tsx
│   │   └── profile/
│   │       ├── profile-header.tsx
│   │       └── edit-profile-form.tsx
│   └── shared/                         # Shared/common components
│       ├── data-table.tsx
│       ├── search-bar.tsx
│       └── pagination.tsx
│
├── lib/                                # Utility functions & configurations
│   ├── api.ts                          # Axios instance + interceptors
│   ├── react-query.ts                  # React Query client config
│   ├── utils.ts                        # Utility functions (cn, formatters)
│   ├── validations.ts                  # Zod schemas
│   ├── constants.ts                    # App constants
│   └── auth.ts                         # Auth utilities (if using next-auth)
│
├── hooks/                              # Custom React hooks
│   ├── use-auth.ts                     # Authentication hook
│   ├── use-user.ts                     # User data hook (React Query)
│   ├── use-posts.ts                    # Posts data hook (React Query)
│   ├── use-media-query.ts              # Responsive design hook
│   └── use-toast.ts                    # Toast notifications (shadcn)
│
├── types/                              # TypeScript type definitions
│   ├── index.ts                        # Common types
│   ├── api.ts                          # API response types
│   ├── user.ts                         # User-related types
│   └── database.ts                     # Database types (Prisma generated)
│
├── actions/                            # Server Actions (Next.js 14+)
│   ├── auth.ts                         # Auth actions
│   ├── user.ts                         # User actions
│   └── posts.ts                        # Posts actions
│
├── config/                             # Configuration files
│   ├── site.ts                         # Site metadata (name, description, etc.)
│   └── navigation.ts                   # Navigation menu config
│
├── prisma/                             # Prisma ORM (if using database)
│   ├── schema.prisma                   # Database schema
│   └── migrations/                     # Database migrations
│
├── public/                             # Static assets
│   ├── images/
│   ├── icons/
│   └── fonts/
│
├── styles/                             # Global styles
│   └── globals.css                     # Tailwind imports + custom styles
│
├── .env.local                          # Environment variables (gitignored)
├── .env.example                        # Environment variables template
├── .eslintrc.json                      # ESLint config
├── .prettierrc                         # Prettier config
├── components.json                     # shadcn/ui config
├── next.config.js                      # Next.js config
├── tailwind.config.ts                  # Tailwind config
├── tsconfig.json                       # TypeScript config
├── package.json                        # Dependencies
└── README.md                           # Project documentation
```

### Directory Purpose

**`app/`** - Next.js 14 App Router pages and layouts. Use route groups `(name)` for logical grouping without affecting URLs.

**`components/`** - All React components, organized by type:
- `ui/` - shadcn/ui components (copy-paste, customizable)
- `layout/` - Shared layout components (header, footer, sidebar)
- `features/` - Feature-specific components (scoped to one feature)
- `shared/` - Reusable components used across features

**`lib/`** - Utility functions, configurations, and third-party library setups.

**`hooks/`** - Custom React hooks, especially React Query hooks for API calls.

**`types/`** - TypeScript type definitions and interfaces.

**`actions/`** - Server Actions for form handling and server-side operations (Next.js 14+).

**`config/`** - App configuration (site metadata, navigation menus, constants).

**`prisma/`** - Database schema and migrations (if using Prisma).

**`public/`** - Static files served at root URL.

**`styles/`** - Global CSS (Tailwind imports + custom styles).

## Workflow

**Keep user informed at every step — this is a live build log.**

### Step 1: Project Setup
Ask:
- Project name
- Description/purpose
- Optional features (chromium review, nginx preview)

Create Next.js project:
```bash
npx create-next-app@latest <project-name> \
  --typescript \
  --tailwind \
  --app \
  --no-src-dir \
  --import-alias "@/*"
```

**→ Message user: "Next.js project initialized ✓"**

### Step 2: Create Directory Structure

Create all necessary directories following industry best practices:

```bash
cd <project-name>

# Create app route groups
mkdir -p app/\(auth\)/login app/\(auth\)/register
mkdir -p app/\(dashboard\)/dashboard app/\(dashboard\)/profile app/\(dashboard\)/settings
mkdir -p app/api/auth app/api/users

# Create component directories
mkdir -p components/ui components/layout components/features components/shared
mkdir -p components/features/auth components/features/dashboard components/features/profile

# Create utility directories
mkdir -p lib hooks types actions config

# Create static asset directories
mkdir -p public/images public/icons public/fonts

# Create styles directory
mkdir styles

# Create Prisma directory (if using database)
# mkdir -p prisma
```

Create essential config files:

**`config/site.ts`** - Site metadata
```typescript
export const siteConfig = {
  name: '<Project Name>',
  description: '<Project Description>',
  url: process.env.NEXT_PUBLIC_APP_URL || 'http://localhost:3000',
  links: {
    github: 'https://github.com/...',
  },
};
```

**`config/navigation.ts`** - Navigation menu
```typescript
export const mainNav = [
  { title: 'Home', href: '/' },
  { title: 'Dashboard', href: '/dashboard' },
  { title: 'Profile', href: '/profile' },
];

export const dashboardNav = [
  { title: 'Overview', href: '/dashboard' },
  { title: 'Profile', href: '/profile' },
  { title: 'Settings', href: '/settings' },
];
```

**`.env.example`** - Environment variables template
```
NEXT_PUBLIC_APP_URL=http://localhost:3000
NEXT_PUBLIC_API_BASE_URL=http://localhost:3000/api
DATABASE_URL=postgresql://...
NEXTAUTH_SECRET=...
NEXTAUTH_URL=http://localhost:3000
```

**→ Message user: "Directory structure created ✓"**

### Step 3: Install Dependencies

**Core dependencies:**
```bash
cd <project-name>
npm install axios @tanstack/react-query
npm install -D @types/node
```

**shadcn/ui setup (recommended):**
```bash
npx shadcn-ui@latest init
```

This will prompt for configuration. Recommended answers:
- Style: Default
- Base color: Slate
- CSS variables: Yes

**Install essential shadcn components:**
```bash
npx shadcn-ui@latest add button card input label select textarea
npx shadcn-ui@latest add dropdown-menu dialog sheet tabs
npx shadcn-ui@latest add table form avatar badge separator toast
```

**Install form dependencies (for shadcn/ui forms):**
```bash
npm install react-hook-form @hookform/resolvers zod
```

**Optional (ask user based on needs):**
```bash
npm install zustand  # State management
npm install next-auth  # Authentication
npm install prisma @prisma/client  # Database ORM
```

**→ Message user: "Dependencies + shadcn/ui installed ✓"**

### Step 4: Configure Base Files

#### `lib/api.ts` (axios instance)
```typescript
import axios from 'axios';

export const api = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_BASE_URL || 'http://localhost:3000/api',
  timeout: 10000,
  headers: { 'Content-Type': 'application/json' }
});

// Request interceptor (add auth tokens, etc.)
api.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('token');
    if (token) config.headers.Authorization = `Bearer ${token}`;
    return config;
  },
  (error) => Promise.reject(error)
);

// Response interceptor (handle errors globally)
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      // Handle unauthorized
    }
    return Promise.reject(error);
  }
);
```

#### `lib/react-query.ts` (query client)
```typescript
import { QueryClient } from '@tanstack/react-query';

export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 60 * 1000, // 1 minute
      refetchOnWindowFocus: false,
      retry: 1,
    },
  },
});
```

#### `app/providers.tsx` (wrap app with providers)
```typescript
'use client';

import { QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';
import { queryClient } from '@/lib/react-query';

export function Providers({ children }: { children: React.ReactNode }) {
  return (
    <QueryClientProvider client={queryClient}>
      {children}
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}
```

Update `app/layout.tsx` to use Providers.

**→ Message user: "Base configuration complete ✓"**

### Step 5: Generate Features

Ask what features/pages to build. For each feature:

1. **Create route** (`app/<feature>/page.tsx`)
2. **Create components** (`components/features/<feature>/`)
3. **Create API hooks** (`hooks/use<Feature>.ts`) using react-query
4. **Create types** (`types/<feature>.ts`)
5. **Optionally create API routes** (`app/api/<feature>/route.ts`)

**Example: User Profile Feature**

```typescript
// types/user.ts
export interface User {
  id: string;
  name: string;
  email: string;
}

// hooks/useUser.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { api } from '@/lib/api';
import type { User } from '@/types/user';

export const useUser = (id: string) => {
  return useQuery({
    queryKey: ['user', id],
    queryFn: async () => {
      const { data } = await api.get<User>(`/users/${id}`);
      return data;
    },
  });
};

export const useUpdateUser = () => {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: async (user: Partial<User>) => {
      const { data } = await api.patch<User>(`/users/${user.id}`, user);
      return data;
    },
    onSuccess: (data) => {
      queryClient.invalidateQueries({ queryKey: ['user', data.id] });
    },
  });
};

// app/profile/[id]/page.tsx
'use client';

import { useUser, useUpdateUser } from '@/hooks/useUser';

export default function ProfilePage({ params }: { params: { id: string } }) {
  const { data: user, isLoading, error } = useUser(params.id);
  const updateUser = useUpdateUser();

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;

  return (
    <div>
      <h1>{user?.name}</h1>
      <p>{user?.email}</p>
    </div>
  );
}
```

**→ Message user after each feature: "Profile page complete ✓"**

### Step 6: Build UI with shadcn/ui Components

Use shadcn/ui components (already installed) for consistent, accessible UI. Apply Design Principles (see below).

**Example: Profile page with shadcn/ui**
```typescript
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar';

export default function ProfilePage({ params }: { params: { id: string } }) {
  const { data: user, isLoading } = useUser(params.id);

  if (isLoading) return <Card className="w-full max-w-2xl mx-auto"><CardContent>Loading...</CardContent></Card>;

  return (
    <Card className="w-full max-w-2xl mx-auto">
      <CardHeader>
        <div className="flex items-center gap-4">
          <Avatar className="h-20 w-20">
            <AvatarImage src={user?.avatar} />
            <AvatarFallback>{user?.name[0]}</AvatarFallback>
          </Avatar>
          <div>
            <CardTitle>{user?.name}</CardTitle>
            <p className="text-sm text-muted-foreground">{user?.email}</p>
          </div>
        </div>
      </CardHeader>
      <CardContent>
        <Button>Edit Profile</Button>
      </CardContent>
    </Card>
  );
}
```

**When to add more components:**
- Forms → `npx shadcn-ui@latest add form input label`
- Data tables → `npx shadcn-ui@latest add table`
- Navigation → `npx shadcn-ui@latest add navigation-menu`
- Feedback → `npx shadcn-ui@latest add toast alert`

**→ Message user: "UI built with shadcn/ui ✓"**

### Step 7: Visual Review (if chromium enabled)

Run dev server:
```bash
npm run dev > /tmp/project-dev.log 2>&1 &
```

**Wait for server to be fully ready** (critical - avoid white screen screenshots):
```bash
# Wait for "Ready in" message (usually 5-15 seconds)
timeout=30
elapsed=0
while [ $elapsed -lt $timeout ]; do
  if grep -q "Ready in" /tmp/project-dev.log 2>/dev/null; then
    echo "Server ready!"
    sleep 3  # Extra buffer for module loading
    break
  fi
  sleep 1
  elapsed=$((elapsed + 1))
done
```

Take screenshots (requires chromium):
```bash
bash scripts/screenshot.sh "http://localhost:3000" /tmp/review-desktop.png 1400 900
bash scripts/screenshot.sh "http://localhost:3000" /tmp/review-mobile.png 390 844
```

Analyze with `image` tool. Fix issues. Re-run if needed.

**→ Message user: "Review complete, sending preview..."**

### Step 8: Environment Setup

Create `.env.local`:
```
NEXT_PUBLIC_API_BASE_URL=https://api.example.com
DATABASE_URL=postgresql://...
NEXTAUTH_SECRET=...
```

Create `.env.example` (template for user).

**→ Message user: "Environment template created ✓"**

### Step 9: Scripts & Documentation

Update `package.json` scripts:
```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint",
    "type-check": "tsc --noEmit"
  }
}
```

Create `README.md` with:
- Setup instructions
- Environment variables needed
- Development commands
- API integration guide

**→ Message user: "Documentation complete ✓"**

### Step 10: Export & Deploy Guidance

Zip the project:
```bash
cd .. && zip -r /tmp/<project-name>.zip <project-name>/
```

Send via message tool with `filePath`.

Provide deployment options:
- **Vercel** (recommended): `npx vercel`
- **Netlify**: `npm run build && netlify deploy`
- **Docker**: Provide Dockerfile
- **Self-hosted**: Provide systemd service + nginx config

**→ Message user: "Project ready! 🚀"**

## API Integration Patterns

### Pattern 1: REST API (default)

Use axios + react-query:

```typescript
// hooks/usePosts.ts
import { useQuery, useMutation } from '@tanstack/react-query';
import { api } from '@/lib/api';

export const usePosts = () => {
  return useQuery({
    queryKey: ['posts'],
    queryFn: async () => {
      const { data } = await api.get('/posts');
      return data;
    },
  });
};

export const useCreatePost = () => {
  return useMutation({
    mutationFn: async (post: { title: string; body: string }) => {
      const { data } = await api.post('/posts', post);
      return data;
    },
  });
};
```

### Pattern 2: GraphQL (optional)

Install:
```bash
npm install @apollo/client graphql
```

Setup Apollo Client, use `useQuery` and `useMutation` from Apollo.

### Pattern 3: tRPC (optional)

For Next.js API routes with type safety:
```bash
npm install @trpc/server @trpc/client @trpc/react-query @trpc/next
```

### Pattern 4: Server Actions (Next.js 14+)

For form handling without API routes:
```typescript
// app/actions.ts
'use server';

export async function createPost(formData: FormData) {
  const title = formData.get('title');
  // ...
}
```

**Always ask user which pattern they prefer for their use case.**

## Design Principles

Apply these consistently. These are quality standards.

### Layout & Spacing
- Consistent Tailwind spacing scale (4, 6, 8, 12, 16, 20, 24)
- Max content width: max-w-5xl or max-w-6xl
- Vertical rhythm: py-16 for sections, py-8 for subsections
- Mobile: minimum px-4 padding

### Typography
- Clear hierarchy (h1 → h2 → h3, max 3-4 sizes)
- Line length: max 65-75 characters (max-w-prose)
- Font weight contrast (bold headings, regular body)
- Text color hierarchy (slate-900 → slate-700 → slate-500)

### Color & Contrast
- WCAG AA minimum (4.5:1 contrast)
- Limit palette (1 primary + 1 accent + neutrals)
- Consistent accent usage (CTAs, links, active states)

### Responsive Design
- Mobile-first (390px → 768px → 1024px)
- Touch targets: min 44x44px on mobile
- Stack on mobile (grids collapse to single column)
- Hamburger menu on mobile

### Components (Use shadcn/ui)
- **Icons**: Use Lucide React (comes with shadcn/ui), never emoji
- **Buttons**: Use `<Button>` component with variants (default, destructive, outline, ghost)
- **Forms**: Use shadcn `<Form>` with react-hook-form integration
- **Cards**: Use `<Card>` component for content sections
- **Dialogs/Modals**: Use `<Dialog>` or `<Sheet>` components
- **Loading states**: Use shadcn `<Skeleton>` component for loading UI
- **Error handling**: Use `<Alert>` component for error messages
- **Data display**: Use `<Table>` component for tabular data

**shadcn/ui benefits:** Accessible, customizable, copy-paste friendly, works with Tailwind

### TypeScript Best Practices
- Strict mode enabled
- Explicit return types for functions
- Interface over type for objects
- Avoid `any` (use `unknown` if needed)
- Use discriminated unions for variants

### Performance
- Use Next.js Image component (`next/image`)
- Lazy load below-the-fold content
- Code splitting (dynamic imports)
- Memoize expensive computations (useMemo, useCallback)

## Common Patterns

### Form Handling (with shadcn/ui)
```typescript
'use client';

import { zodResolver } from '@hookform/resolvers/zod';
import { useForm } from 'react-hook-form';
import * as z from 'zod';
import { useMutation } from '@tanstack/react-query';
import { Button } from '@/components/ui/button';
import { Form, FormControl, FormField, FormItem, FormLabel, FormMessage } from '@/components/ui/form';
import { Input } from '@/components/ui/input';
import { useToast } from '@/components/ui/use-toast';

const formSchema = z.object({
  name: z.string().min(2, 'Name must be at least 2 characters'),
  email: z.string().email('Invalid email address'),
});

export default function ContactForm() {
  const { toast } = useToast();
  const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
    defaultValues: { name: '', email: '' },
  });

  const mutation = useMutation({
    mutationFn: async (data: z.infer<typeof formSchema>) => {
      const res = await api.post('/contact', data);
      return res.data;
    },
    onSuccess: () => {
      toast({ title: 'Success', description: 'Message sent!' });
      form.reset();
    },
    onError: (error) => {
      toast({ title: 'Error', description: error.message, variant: 'destructive' });
    },
  });

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit((data) => mutation.mutate(data))} className="space-y-4">
        <FormField
          control={form.control}
          name="name"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Name</FormLabel>
              <FormControl>
                <Input placeholder="John Doe" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <FormField
          control={form.control}
          name="email"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Email</FormLabel>
              <FormControl>
                <Input type="email" placeholder="john@example.com" {...field} />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <Button type="submit" disabled={mutation.isPending}>
          {mutation.isPending ? 'Sending...' : 'Send Message'}
        </Button>
      </form>
    </Form>
  );
}
```

**Note:** Run `npx shadcn-ui@latest add form toast` and install `npm install react-hook-form @hookform/resolvers zod` for this pattern.

### Pagination
```typescript
const usePaginatedPosts = (page: number) => {
  return useQuery({
    queryKey: ['posts', page],
    queryFn: async () => {
      const { data } = await api.get(`/posts?page=${page}`);
      return data;
    },
    keepPreviousData: true, // Smooth transitions
  });
};
```

### Infinite Scroll
```typescript
import { useInfiniteQuery } from '@tanstack/react-query';

const useInfinitePosts = () => {
  return useInfiniteQuery({
    queryKey: ['posts'],
    queryFn: async ({ pageParam = 1 }) => {
      const { data } = await api.get(`/posts?page=${pageParam}`);
      return data;
    },
    getNextPageParam: (lastPage, pages) => lastPage.nextPage,
  });
};
```

## Common Mistakes to Avoid
- ❌ Not wrapping app with QueryClientProvider
- ❌ Using axios without interceptors (no error handling)
- ❌ Forgetting loading/error states in components
- ❌ Not invalidating queries after mutations
- ❌ Using `any` instead of proper TypeScript types
- ❌ Client components when server components would work
- ❌ Not using Next.js Image component (performance loss)
- ❌ Missing error boundaries
- ❌ Hardcoding API URLs (use env vars)
- ❌ No mobile testing (always check responsive)
- ❌ Building custom components when shadcn/ui has them (Button, Card, Dialog, etc.)
- ❌ Using emoji for icons (use Lucide React icons from shadcn/ui)
- ❌ Not installing `@hookform/resolvers` and `zod` before using shadcn forms
- ❌ Forgetting to add `<Toaster />` component when using toast notifications
- ❌ **Taking screenshots before dev server is fully ready** (causes white screens)
- ❌ **Not waiting for module loading** (causes "Module not found" errors in screenshots)

## Troubleshooting

### White Screen Screenshots
**Problem:** Screenshots show blank white page
**Cause:** Dev server not fully initialized before screenshot
**Solution:** 
- Wait for "Ready in" message in dev server logs
- Add 3-5 second buffer after "Ready" message
- Verify localhost:3000 loads in browser before taking screenshot

### Module Not Found Errors
**Problem:** React error "Module not found: Can't resolve @tanstack/react-query"
**Cause:** Dev server started before all packages loaded
**Solution:**
- Restart dev server: `pkill -f "next dev" && npm run dev`
- Verify packages in node_modules: `ls node_modules/@tanstack/`
- Wait 10-15 seconds after `npm install` before starting dev server

### Dev Server Won't Start
**Problem:** Port 3000 already in use
**Solution:**
```bash
# Kill existing process
pkill -f "next dev"
# Or use different port
PORT=3001 npm run dev
```

## Iteration & Updates

When user requests changes:
1. Identify affected files
2. Make changes
3. Run type check: `npm run type-check`
4. Test locally: `npm run dev`
5. If chromium enabled: take new screenshot
6. Report changes to user

**Always explain what changed and why.**
