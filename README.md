# 🚀 Next.js — Beginner to Advanced

> A complete Next.js learning guide from beginner to advanced, with English + Khmer explanations and practical examples.

---

## 📚 Table of Contents

* [1. What is Next.js?](#1-what-is-nextjs)
* [2. Why Next.js?](#2-why-nextjs)
* [3. Requirements](#3-requirements)
* [4. Create a Next.js Project](#4-create-a-nextjs-project)
* [5. Project Structure](#5-project-structure)
* [6. App Router](#6-app-router)
* [7. Pages](#7-pages)
* [8. Layouts](#8-layouts)
* [9. Components](#9-components)
* [10. Server Components](#10-server-components)
* [11. Client Components](#11-client-components)
* [12. Routing](#12-routing)
* [13. Dynamic Routes](#13-dynamic-routes)
* [14. Catch-All Routes](#14-catch-all-routes)
* [15. Navigation](#15-navigation)
* [16. Link Component](#16-link-component)
* [17. Images](#17-images)
* [18. Fonts](#18-fonts)
* [19. Metadata](#19-metadata)
* [20. Loading UI](#20-loading-ui)
* [21. Error Handling](#21-error-handling)
* [22. Not Found](#22-not-found)
* [23. API Route Handlers](#23-api-route-handlers)
* [24. GET API](#24-get-api)
* [25. POST API](#25-post-api)
* [26. PUT API](#26-put-api)
* [27. DELETE API](#27-delete-api)
* [28. Forms](#28-forms)
* [29. Server Actions](#29-server-actions)
* [30. Data Fetching](#30-data-fetching)
* [31. Database](#31-database)
* [32. Environment Variables](#32-environment-variables)
* [33. Authentication](#33-authentication)
* [34. Middleware / Proxy](#34-middleware--proxy)
* [35. Cookies](#35-cookies)
* [36. Headers](#36-headers)
* [37. Search Params](#37-search-params)
* [38. Dynamic Metadata](#38-dynamic-metadata)
* [39. Static Generation](#39-static-generation)
* [40. Dynamic Rendering](#40-dynamic-rendering)
* [41. Caching](#41-caching)
* [42. Revalidation](#42-revalidation)
* [43. Parallel Routes](#43-parallel-routes)
* [44. Intercepting Routes](#44-intercepting-routes)
* [45. Route Groups](#45-route-groups)
* [46. Suspense](#46-suspense)
* [47. Streaming](#47-streaming)
* [48. TypeScript](#48-typescript)
* [49. Tailwind CSS](#49-tailwind-css)
* [50. Production Build](#50-production-build)
* [51. Deployment](#51-deployment)
* [52. Recommended Project Structure](#52-recommended-project-structure)
* [53. Advanced Architecture](#53-advanced-architecture)
* [54. Best Practices](#54-best-practices)
* [55. Complete Mini Project](#55-complete-mini-project)
* [46. Learning Roadmap](#46-learning-roadmap)
* 
---

# 1. What is Next.js?

## English

Next.js is a React framework for building modern web applications.

It provides features such as:

* Routing
* Server Components
* Client Components
* Server-side rendering
* Static rendering
* Data fetching
* API Route Handlers
* Server Actions
* Image optimization
* Font optimization
* Metadata
* Streaming
* Caching
* Middleware / Proxy
* TypeScript support

## ខ្មែរ

Next.js គឺជា **React Framework** សម្រាប់បង្កើត Web Application ទំនើបៗ។

វាជួយផ្តល់មុខងារសំខាន់ៗដូចជា៖

* Routing
* Server Components
* Client Components
* Server Rendering
* Static Rendering
* API
* Database integration
* Authentication
* Image optimization
* SEO Metadata
* Caching
* Server Actions

---

# 2. Why Next.js?

## English

React is mainly a UI library.

Next.js adds application-level features around React.

```text
React
  │
  ├── Components
  ├── State
  └── UI

Next.js
  │
  ├── React
  ├── Routing
  ├── Server Components
  ├── API
  ├── Rendering
  ├── Caching
  ├── SEO
  └── Deployment
```

## ខ្មែរ

React ផ្តោតសំខាន់ទៅលើ UI។

Next.js បន្ថែមមុខងារសម្រាប់បង្កើត Application ពេញលេញ។

---

# 3. Requirements

Before learning Next.js, you should know:

* HTML
* CSS
* JavaScript
* ES6+
* React
* npm
* Git

Recommended:

```bash
node -v
npm -v
git --version
```

---

# 4. Create a Next.js Project

Create a new project:

```bash
npx create-next-app@latest my-next-app
```

Then:

```bash
cd my-next-app
npm run dev
```

Open:

```text
http://localhost:3000
```

## ខ្មែរ

បង្កើត Project ថ្មី៖

```bash
npx create-next-app@latest my-next-app
```

ចូលទៅក្នុង Project:

```bash
cd my-next-app
```

Run:

```bash
npm run dev
```

បន្ទាប់មកបើក Browser:

```text
http://localhost:3000
```

---

# 5. Project Structure

Typical App Router project:

```text
my-next-app/
│
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── globals.css
│   │
│   ├── about/
│   │   └── page.tsx
│   │
│   └── products/
│       ├── page.tsx
│       └── [id]/
│           └── page.tsx
│
├── components/
│   └── Navbar.tsx
│
├── public/
│   └── images/
│
├── .env.local
├── next.config.ts
├── package.json
└── tsconfig.json
```

## Important folders

| Folder       | Purpose             |
| ------------ | ------------------- |
| `app`        | Application routes  |
| `components` | Reusable components |
| `public`     | Static assets       |
| `app/api`    | API Route Handlers  |

---

# 6. App Router

Modern Next.js applications use the **App Router**.

The App Router is based on the filesystem.

```text
app/
├── page.tsx
├── about/
│   └── page.tsx
└── contact/
    └── page.tsx
```

Creates:

```text
/
/about
/contact
```

## ខ្មែរ

App Router ប្រើ Folder និង File ដើម្បីបង្កើត Route។

ឧទាហរណ៍៖

```text
app/about/page.tsx
```

មានន័យថា៖

```text
/about
```

---

# 7. Pages

A `page.tsx` file creates a route.

## `app/page.tsx`

```tsx
export default function HomePage() {
  return (
    <main>
      <h1>Welcome to Next.js</h1>
      <p>This is the home page.</p>
    </main>
  );
}
```

URL:

```text
/
```

## `app/about/page.tsx`

```tsx
export default function AboutPage() {
  return (
    <main>
      <h1>About</h1>
      <p>This is the about page.</p>
    </main>
  );
}
```

URL:

```text
/about
```

---

# 8. Layouts

A layout is shared UI between pages.

## `app/layout.tsx`

```tsx
import type { Metadata } from "next";
import "./globals.css";

export const metadata: Metadata = {
  title: "My Next.js App",
  description: "My Next.js application",
};

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

## Nested Layout

```text
app/
├── layout.tsx
└── dashboard/
    ├── layout.tsx
    └── page.tsx
```

`dashboard/layout.tsx`:

```tsx
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <section>
      <nav>Dashboard Navigation</nav>

      <main>{children}</main>
    </section>
  );
}
```

## ខ្មែរ

Layout គឺជា UI ដែលអាចប្រើរួមគ្នាជាមួយ Page ជាច្រើន។

ឧទាហរណ៍៖

```text
Navbar
Sidebar
Footer
```

អាចដាក់ក្នុង Layout បាន។

---

# 9. Components

Create:

```text
components/Button.tsx
```

```tsx
type ButtonProps = {
  children: React.ReactNode;
};

export default function Button({ children }: ButtonProps) {
  return (
    <button type="button">
      {children}
    </button>
  );
}
```

Use it:

```tsx
import Button from "@/components/Button";

export default function HomePage() {
  return (
    <main>
      <h1>Hello</h1>
      <Button>Click Me</Button>
    </main>
  );
}
```

## ខ្មែរ

Component គឺជា UI ដែលអាចយកមកប្រើឡើងវិញ។

ឧទាហរណ៍៖

```text
Button
Navbar
Card
Modal
Input
Table
```

---

# 10. Server Components

In the App Router, components are Server Components by default.

```tsx
export default async function UsersPage() {
  const response = await fetch("https://jsonplaceholder.typicode.com/users");

  const users = await response.json();

  return (
    <main>
      <h1>Users</h1>

      {users.map((user: { id: number; name: string }) => (
        <p key={user.id}>{user.name}</p>
      ))}
    </main>
  );
}
```

## English

Server Components can perform server-side work without sending the component's JavaScript to the browser in the same way as Client Components.

## ខ្មែរ

Server Component ដំណើរការនៅ Server ជាចម្បង ហើយមានប្រយោជន៍សម្រាប់៖

* Fetch data
* Database access
* Server-side logic
* Reduce client JavaScript

---

# 11. Client Components

When you need browser interactivity, use:

```tsx
"use client";
```

Example:

```tsx
"use client";

import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>

      <button
        type="button"
        onClick={() => setCount(count + 1)}
      >
        Increment
      </button>
    </div>
  );
}
```

Client Components are useful for:

* `useState`
* `useEffect`
* Event handlers
* Browser APIs
* Interactive UI

## ខ្មែរ

ប្រើ `"use client"` នៅពេល Component ត្រូវការ Browser interaction។

ឧទាហរណ៍៖

```text
Click
Form interaction
State
useEffect
window
localStorage
```

---

# 12. Routing

Example:

```text
app/
├── page.tsx
├── about/
│   └── page.tsx
├── contact/
│   └── page.tsx
└── products/
    └── page.tsx
```

Routes:

```text
/
/about
/contact
/products
```

---

# 13. Dynamic Routes

Create:

```text
app/products/[id]/page.tsx
```

```tsx
type ProductPageProps = {
  params: Promise<{
    id: string;
  }>;
};

export default async function ProductPage({
  params,
}: ProductPageProps) {
  const { id } = await params;

  return (
    <main>
      <h1>Product ID: {id}</h1>
    </main>
  );
}
```

Example URLs:

```text
/products/1
/products/25
/products/100
```

## ខ្មែរ

`[id]` គឺ Dynamic Segment។

ឧទាហរណ៍៖

```text
/products/[id]
```

អាចទទួលបាន៖

```text
/products/1
/products/2
/products/abc
```

---

# 14. Catch-All Routes

Create:

```text
app/docs/[...slug]/page.tsx
```

```tsx
type DocsPageProps = {
  params: Promise<{
    slug: string[];
  }>;
};

export default async function DocsPage({
  params,
}: DocsPageProps) {
  const { slug } = await params;

  return (
    <main>
      <h1>Documentation</h1>

      <p>{slug.join(" / ")}</p>
    </main>
  );
}
```

Matches:

```text
/docs/javascript
/docs/javascript/react
/docs/javascript/react/hooks
```

---

# 15. Navigation

Use Next.js `Link`.

```tsx
import Link from "next/link";

export default function Navbar() {
  return (
    <nav>
      <Link href="/">Home</Link>
      <Link href="/about">About</Link>
      <Link href="/contact">Contact</Link>
    </nav>
  );
}
```

---

# 16. Link Component

Dynamic navigation:

```tsx
import Link from "next/link";

export default function ProductList() {
  const products = [
    { id: 1, name: "Laptop" },
    { id: 2, name: "Phone" },
  ];

  return (
    <ul>
      {products.map((product) => (
        <li key={product.id}>
          <Link href={`/products/${product.id}`}>
            {product.name}
          </Link>
        </li>
      ))}
    </ul>
  );
}
```

---

# 17. Images

Use `next/image`.

```tsx
import Image from "next/image";

export default function Profile() {
  return (
    <Image
      src="/profile.jpg"
      alt="Profile"
      width={300}
      height={300}
    />
  );
}
```

For external images, configure the allowed image source in your Next.js configuration.

---

# 18. Fonts

Next.js provides font optimization through `next/font`.

Example:

```tsx
import { Geist } from "next/font/google";

const geist = Geist({
  subsets: ["latin"],
});

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={geist.className}>
      <body>{children}</body>
    </html>
  );
}
```

---

# 19. Metadata

Static metadata:

```tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
  title: "Home",
  description: "Welcome to my website",
};

export default function HomePage() {
  return <h1>Home</h1>;
}
```

## ខ្មែរ

Metadata មានសារៈសំខាន់សម្រាប់ SEO និង Browser information។

---

# 20. Loading UI

Create:

```text
app/loading.tsx
```

```tsx
export default function Loading() {
  return (
    <main>
      <p>Loading...</p>
    </main>
  );
}
```

Next.js can automatically show this UI while the route is loading.

---

# 21. Error Handling

Create:

```text
app/error.tsx
```

```tsx
"use client";

export default function ErrorPage({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <main>
      <h1>Something went wrong</h1>

      <p>{error.message}</p>

      <button type="button" onClick={() => reset()}>
        Try again
      </button>
    </main>
  );
}
```

---

# 22. Not Found

Create:

```text
app/not-found.tsx
```

```tsx
export default function NotFound() {
  return (
    <main>
      <h1>404 - Page Not Found</h1>
      <p>The page you requested does not exist.</p>
    </main>
  );
}
```

---

# 23. API Route Handlers

Create:

```text
app/api/users/route.ts
```

```tsx
export async function GET() {
  return Response.json({
    message: "Hello from Next.js API",
  });
}
```

URL:

```text
/api/users
```

---

# 24. GET API

```tsx
export async function GET() {
  const users = [
    {
      id: 1,
      name: "Dara",
    },
    {
      id: 2,
      name: "Sokha",
    },
  ];

  return Response.json(users);
}
```

---

# 25. POST API

```tsx
export async function POST(request: Request) {
  const body = await request.json();

  return Response.json(
    {
      message: "User created",
      user: body,
    },
    {
      status: 201,
    },
  );
}
```

Example request:

```json
{
  "name": "Dara",
  "email": "dara@example.com"
}
```

---

# 26. PUT API

```tsx
export async function PUT(request: Request) {
  const body = await request.json();

  return Response.json({
    message: "User updated",
    user: body,
  });
}
```

---

# 27. DELETE API

```tsx
export async function DELETE() {
  return Response.json({
    message: "User deleted",
  });
}
```

---

# 28. Forms

Client-side form:

```tsx
"use client";

import { FormEvent, useState } from "react";

export default function LoginForm() {
  const [email, setEmail] = useState("");

  function handleSubmit(event: FormEvent<HTMLFormElement>) {
    event.preventDefault();

    console.log({
      email,
    });
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={(event) => setEmail(event.target.value)}
        placeholder="Email"
        required
      />

      <button type="submit">
        Login
      </button>
    </form>
  );
}
```

---

# 29. Server Actions

Server Actions allow server-side functions to be called from forms and other supported contexts.

Example:

```tsx
"use server";

export async function createUser(formData: FormData) {
  const name = formData.get("name");

  console.log("Creating user:", name);
}
```

Use it:

```tsx
import { createUser } from "./actions";

export default function UserForm() {
  return (
    <form action={createUser}>
      <input
        name="name"
        placeholder="Name"
        required
      />

      <button type="submit">
        Create User
      </button>
    </form>
  );
}
```

## ខ្មែរ

Server Action ជួយឱ្យយើងធ្វើ Server-side mutation ដូចជា៖

```text
Create
Update
Delete
Database operation
```

ដោយមិនចាំបាច់បង្កើត API endpoint សម្រាប់គ្រប់ mutation ទាំងអស់។

---

# 30. Data Fetching

Server Component:

```tsx
type User = {
  id: number;
  name: string;
  email: string;
};

export default async function UsersPage() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/users",
  );

  if (!response.ok) {
    throw new Error("Failed to fetch users");
  }

  const users: User[] = await response.json();

  return (
    <main>
      <h1>Users</h1>

      {users.map((user) => (
        <article key={user.id}>
          <h2>{user.name}</h2>
          <p>{user.email}</p>
        </article>
      ))}
    </main>
  );
}
```

---

# 31. Database

Next.js can work with databases such as:

* PostgreSQL
* MySQL
* MongoDB
* SQLite
* SQL Server

Popular tools include:

* Prisma
* Drizzle
* Mongoose
* Database-specific drivers

Example architecture:

```text
Browser
   ↓
Next.js
   ↓
Server Component / Server Action
   ↓
Database
```

Example Prisma-style query:

```tsx
const users = await prisma.user.findMany();
```

> Keep database credentials on the server. Never expose private database credentials to Client Components.

---

# 32. Environment Variables

Create:

```text
.env.local
```

```env
DATABASE_URL="your-database-url"
API_SECRET="your-secret"
NEXT_PUBLIC_API_URL="https://example.com"
```

Server-only variable:

```tsx
const databaseUrl = process.env.DATABASE_URL;
```

Public variable:

```tsx
const apiUrl = process.env.NEXT_PUBLIC_API_URL;
```

## Important

Do not commit:

```text
.env.local
```

to Git.

Add:

```text
.env*.local
```

to `.gitignore` when appropriate.

## ខ្មែរ

Environment Variables ប្រើសម្រាប់រក្សាទុក Configuration និង Secret។

ឧទាហរណ៍៖

```text
Database password
API secret
Private API key
```

`NEXT_PUBLIC_` មានន័យថាតម្លៃអាចត្រូវបានបញ្ជូនទៅ Client។

---

# 33. Authentication

Authentication usually contains:

```text
Register
Login
Logout
Session
Cookies
Authorization
Protected Routes
```

A common architecture:

```text
Browser
   ↓
Login
   ↓
Authentication system
   ↓
Session
   ↓
Protected page
```

Example protected server page:

```tsx
import { redirect } from "next/navigation";

async function getCurrentUser() {
  // Replace this with your real authentication logic.
  return null;
}

export default async function DashboardPage() {
  const user = await getCurrentUser();

  if (!user) {
    redirect("/login");
  }

  return (
    <main>
      <h1>Dashboard</h1>
    </main>
  );
}
```

---

# 34. Middleware / Proxy

In current Next.js versions, request interception terminology and APIs may evolve. Follow the version-specific Next.js documentation for whether your project should use `proxy.ts` or legacy `middleware.ts`.

Conceptually, request interception can be used for:

* Authentication checks
* Redirects
* Request-based logic
* Headers
* Rewrites

Example concept:

```text
Request
   ↓
Proxy / Middleware
   ↓
Authentication check
   ↓
Route
```

---

# 35. Cookies

Server-side cookie access:

```tsx
import { cookies } from "next/headers";

export default async function ProfilePage() {
  const cookieStore = await cookies();

  const session = cookieStore.get("session");

  return (
    <main>
      <p>
        Session: {session?.value ?? "Not logged in"}
      </p>
    </main>
  );
}
```

---

# 36. Headers

Read request headers:

```tsx
import { headers } from "next/headers";

export default async function Page() {
  const headersList = await headers();

  const userAgent = headersList.get("user-agent");

  return (
    <main>
      <p>User Agent: {userAgent}</p>
    </main>
  );
}
```

---

# 37. Search Params

URL:

```text
/products?search=laptop
```

Page:

```tsx
type ProductsPageProps = {
  searchParams: Promise<{
    search?: string;
  }>;
};

export default async function ProductsPage({
  searchParams,
}: ProductsPageProps) {
  const params = await searchParams;

  const search = params.search ?? "";

  return (
    <main>
      <h1>Products</h1>

      <p>Search: {search}</p>
    </main>
  );
}
```

---

# 38. Dynamic Metadata

For dynamic pages:

```tsx
import type { Metadata } from "next";

type ProductPageProps = {
  params: Promise<{
    id: string;
  }>;
};

export async function generateMetadata({
  params,
}: ProductPageProps): Promise<Metadata> {
  const { id } = await params;

  return {
    title: `Product ${id}`,
    description: `Details for product ${id}`,
  };
}

export default async function ProductPage({
  params,
}: ProductPageProps) {
  const { id } = await params;

  return <h1>Product {id}</h1>;
}
```

---

# 39. Static Generation

A page can be statically generated when its data and rendering strategy allow it.

Example:

```tsx
export default function AboutPage() {
  return (
    <main>
      <h1>About Us</h1>
      <p>Our company information.</p>
    </main>
  );
}
```

Static pages are useful for content that does not need per-request rendering.

---

# 40. Dynamic Rendering

Dynamic rendering is useful when the result depends on request-time information.

Examples:

```text
Cookies
Headers
Authentication
Request-specific data
```

Example:

```tsx
import { cookies } from "next/headers";

export default async function Page() {
  const cookieStore = await cookies();

  const theme = cookieStore.get("theme")?.value;

  return (
    <main>
      <h1>Theme: {theme ?? "default"}</h1>
    </main>
  );
}
```

---

# 41. Caching

Caching can improve application performance.

Think about:

```text
Request
   ↓
Cache
   ↓
Data
```

Before configuring caching behavior, understand the Next.js version you are using because caching behavior and APIs can change between major releases.

Do not assume every `fetch()` has the same caching semantics across versions.

---

# 42. Revalidation

Revalidation allows cached data to be refreshed.

Example pattern:

```tsx
const response = await fetch(
  "https://api.example.com/products",
  {
    next: {
      revalidate: 60,
    },
  },
);
```

Concept:

```text
Cache
  ↓
60 seconds
  ↓
Refresh data
```

---

# 43. Parallel Routes

Parallel Routes allow multiple route segments to render into slots.

Concept:

```text
app/
├── @dashboard/
├── @analytics/
└── layout.tsx
```

Layout:

```tsx
export default function Layout({
  children,
  dashboard,
  analytics,
}: {
  children: React.ReactNode;
  dashboard: React.ReactNode;
  analytics: React.ReactNode;
}) {
  return (
    <div>
      {children}

      <section>
        {dashboard}
      </section>

      <section>
        {analytics}
      </section>
    </div>
  );
}
```

---

# 44. Intercepting Routes

Intercepting Routes allow a route to load another route within the current layout context.

A common use case:

```text
Product list
     ↓
Click product
     ↓
Product opens as modal
```

While direct navigation can still show the full product page.

Concept:

```text
/products
    ↓
/products/1
```

with the product page intercepted into a modal UI.

---

# 45. Route Groups

Route Groups organize routes without affecting the URL.

Example:

```text
app/
├── (marketing)/
│   ├── page.tsx
│   └── about/
│       └── page.tsx
│
└── (dashboard)/
    └── dashboard/
        └── page.tsx
```

Parentheses:

```text
(marketing)
(dashboard)
```

do not appear in URLs.

---

# 46. Suspense

React Suspense can display fallback UI while part of the UI is waiting.

```tsx
import { Suspense } from "react";

async function UserList() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/users",
  );

  const users = await response.json();

  return (
    <ul>
      {users.map((user: { id: number; name: string }) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

export default function Page() {
  return (
    <main>
      <h1>Users</h1>

      <Suspense fallback={<p>Loading users...</p>}>
        <UserList />
      </Suspense>
    </main>
  );
}
```

---

# 47. Streaming

Streaming allows parts of a page to become available without waiting for the entire page.

Concept:

```text
Request
   ↓
Page starts rendering
   ↓
Fast content → Browser
   ↓
Slow content → Browser later
```

This can improve perceived performance.

---

# 48. TypeScript

Next.js works very well with TypeScript.

Example:

```tsx
type User = {
  id: number;
  name: string;
  email: string;
};

type UserCardProps = {
  user: User;
};

export default function UserCard({
  user,
}: UserCardProps) {
  return (
    <article>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </article>
  );
}
```

---

# 49. Tailwind CSS

A simple component:

```tsx
export default function Card() {
  return (
    <div className="rounded-lg border p-6 shadow">
      <h2 className="text-xl font-bold">
        Next.js
      </h2>

      <p className="mt-2 text-gray-600">
        Learn Next.js from beginner to advanced.
      </p>

      <button
        type="button"
        className="mt-4 rounded bg-black px-4 py-2 text-white"
      >
        Learn More
      </button>
    </div>
  );
}
```

---

# 50. Production Build

Development:

```bash
npm run dev
```

Production build:

```bash
npm run build
```

Start production server:

```bash
npm start
```

Lint:

```bash
npm run lint
```

A typical workflow:

```text
Development
    ↓
Test
    ↓
Lint
    ↓
Build
    ↓
Deploy
```

---

# 51. Deployment

Next.js applications can be deployed to various platforms.

Common options include:

```text
Vercel
Docker
Node.js hosting
Cloud platforms
```

Typical production process:

```bash
npm install
npm run build
npm start
```

---

# 52. Recommended Project Structure

For a medium/large project:

```text
src/
├── app/
│   ├── api/
│   │   └── users/
│   │       └── route.ts
│   │
│   ├── dashboard/
│   │   ├── layout.tsx
│   │   └── page.tsx
│   │
│   ├── login/
│   │   └── page.tsx
│   │
│   ├── products/
│   │   ├── page.tsx
│   │   └── [id]/
│   │       └── page.tsx
│   │
│   ├── layout.tsx
│   ├── page.tsx
│   ├── loading.tsx
│   ├── error.tsx
│   └── not-found.tsx
│
├── components/
│   ├── ui/
│   │   ├── Button.tsx
│   │   ├── Input.tsx
│   │   └── Card.tsx
│   │
│   ├── Navbar.tsx
│   └── Footer.tsx
│
├── lib/
│   ├── db.ts
│   ├── auth.ts
│   └── utils.ts
│
├── types/
│   └── index.ts
│
└── styles/
    └── globals.css
```

---

# 53. Advanced Architecture

A production application can follow:

```text
                    Browser
                       │
                       ▼
                Next.js App
                       │
          ┌────────────┴────────────┐
          │                         │
          ▼                         ▼
    Server Components        Client Components
          │                         │
          ▼                         ▼
    Server Actions              UI State
          │
          ▼
       Services
          │
          ▼
      Database
```

Example:

```text
app/
components/
features/
lib/
services/
types/
```

A feature-based structure:

```text
src/
├── app/
│
├── components/
│
├── features/
│   ├── auth/
│   │   ├── components/
│   │   ├── actions/
│   │   └── types.ts
│   │
│   ├── products/
│   │   ├── components/
│   │   ├── actions/
│   │   └── types.ts
│   │
│   └── users/
│       ├── components/
│       ├── actions/
│       └── types.ts
│
├── lib/
│
├── services/
│
└── types/
```

---

# 54. Best Practices

## 1. Prefer Server Components by default

Use Server Components when you do not need browser interactivity.

```tsx
export default async function Page() {
  const data = await getData();

  return <div>{data.name}</div>;
}
```

---

## 2. Use Client Components only when needed

Avoid putting:

```tsx
"use client";
```

everywhere.

Use it when you need:

```text
useState
useEffect
onClick
Browser APIs
Interactive UI
```

---

## 3. Keep database code server-side

Bad:

```tsx
"use client";

const users = await database.user.findMany();
```

Better:

```text
Server Component
       ↓
Database
```

---

## 4. Validate user input

Never trust input from:

```text
Forms
URL
API
Cookies
Headers
```

Example:

```tsx
const name = formData.get("name");

if (typeof name !== "string" || name.length < 2) {
  throw new Error("Invalid name");
}
```

For production applications, use a dedicated validation library when appropriate.

---

## 5. Protect secrets

Never expose:

```text
DATABASE_PASSWORD
PRIVATE_API_KEY
JWT_SECRET
AUTH_SECRET
```

to the browser.

---

## 6. Use TypeScript

Prefer:

```tsx
type Product = {
  id: number;
  name: string;
  price: number;
};
```

instead of:

```tsx
const product: any = {};
```

---

## 7. Use reusable components

Instead of repeating:

```tsx
<button>Save</button>
<button>Save</button>
<button>Save</button>
```

create:

```tsx
<Button>Save</Button>
```

---

# 55. Complete Mini Project

Let's build a small product application.

## Project Structure

```text
src/
├── app/
│   ├── products/
│   │   ├── page.tsx
│   │   └── [id]/
│   │       └── page.tsx
│   │
│   ├── layout.tsx
│   └── page.tsx
│
└── components/
    └── ProductCard.tsx
```

---

## Step 1 — Product Type

`src/types/product.ts`

```tsx
export type Product = {
  id: number;
  name: string;
  price: number;
  description: string;
};
```

---

## Step 2 — Product Card

`src/components/ProductCard.tsx`

```tsx
import Link from "next/link";
import type { Product } from "@/types/product";

type ProductCardProps = {
  product: Product;
};

export default function ProductCard({
  product,
}: ProductCardProps) {
  return (
    <article>
      <h2>{product.name}</h2>

      <p>${product.price.toFixed(2)}</p>

      <p>{product.description}</p>

      <Link href={`/products/${product.id}`}>
        View Product
      </Link>
    </article>
  );
}
```

---

## Step 3 — Product List

`src/app/products/page.tsx`

```tsx
import ProductCard from "@/components/ProductCard";
import type { Product } from "@/types/product";

const products: Product[] = [
  {
    id: 1,
    name: "Laptop",
    price: 999,
    description: "A powerful laptop.",
  },
  {
    id: 2,
    name: "Phone",
    price: 699,
    description: "A modern smartphone.",
  },
  {
    id: 3,
    name: "Keyboard",
    price: 99,
    description: "A mechanical keyboard.",
  },
];

export default function ProductsPage() {
  return (
    <main>
      <h1>Products</h1>

      {products.map((product) => (
        <ProductCard
          key={product.id}
          product={product}
        />
      ))}
    </main>
  );
}
```

---

## Step 4 — Dynamic Product Page

`src/app/products/[id]/page.tsx`

```tsx
type ProductPageProps = {
  params: Promise<{
    id: string;
  }>;
};

const products = [
  {
    id: 1,
    name: "Laptop",
    price: 999,
    description: "A powerful laptop.",
  },
  {
    id: 2,
    name: "Phone",
    price: 699,
    description: "A modern smartphone.",
  },
  {
    id: 3,
    name: "Keyboard",
    price: 99,
    description: "A mechanical keyboard.",
  },
];

export default async function ProductPage({
  params,
}: ProductPageProps) {
  const { id } = await params;

  const product = products.find(
    (item) => item.id === Number(id),
  );

  if (!product) {
    return <h1>Product not found</h1>;
  }

  return (
    <main>
      <h1>{product.name}</h1>

      <p>Price: ${product.price.toFixed(2)}</p>

      <p>{product.description}</p>
    </main>
  );
}
```

Now:

```text
/products
/products/1
/products/2
/products/3
```

work as separate routes.

---

# 🧠 Next.js Learning Roadmap

## Beginner

Learn:

```text
HTML
CSS
JavaScript
React
↓
Next.js
↓
Pages
Layouts
Components
Routing
Link
Images
Fonts
Metadata
```

---

## Intermediate

Learn:

```text
Server Components
Client Components
Data Fetching
API Route Handlers
Forms
Server Actions
Dynamic Routes
Search Params
Loading
Error Handling
TypeScript
Tailwind CSS
```

---

## Advanced

Learn:

```text
Authentication
Authorization
Database
Caching
Revalidation
Streaming
Suspense
Parallel Routes
Intercepting Routes
Route Groups
Advanced Metadata
Performance
Security
Production Architecture
Deployment
```

---

# 🏗️ Full-Stack Next.js Architecture

A typical full-stack application:

```text
                    FRONTEND
                       │
                       ▼
                ┌─────────────┐
                │   Next.js   │
                └──────┬──────┘
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
      Components   Server Actions   APIs
                       │
                       ▼
                  Service Layer
                       │
                       ▼
                    Database
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
      PostgreSQL     MySQL       MongoDB
```

---

# 🔐 Security Checklist

Before deploying:

```text
[ ] Validate input
[ ] Protect authentication
[ ] Protect authorization
[ ] Keep secrets server-side
[ ] Configure environment variables
[ ] Avoid exposing private API keys
[ ] Validate API requests
[ ] Protect database credentials
[ ] Use HTTPS in production
[ ] Keep dependencies updated
[ ] Handle errors safely
```

---

# ⚡ Performance Checklist

```text
[ ] Prefer Server Components
[ ] Minimize Client Components
[ ] Use next/image
[ ] Use next/font
[ ] Cache appropriate data
[ ] Revalidate appropriate data
[ ] Use Suspense when useful
[ ] Stream slow sections
[ ] Avoid unnecessary JavaScript
[ ] Optimize database queries
```

---

# 📌 Important Next.js Concepts

Remember these:

```text
page.tsx
    ↓
Creates a route

layout.tsx
    ↓
Shared UI

loading.tsx
    ↓
Loading UI

error.tsx
    ↓
Error boundary

not-found.tsx
    ↓
404 UI

route.ts
    ↓
API Route Handler

[ id ]
    ↓
Dynamic route

[ ... slug ]
    ↓
Catch-all route

( group )
    ↓
Route organization

@slot
    ↓
Parallel route

"use client"
    ↓
Client Component
```

---

# 🎯 Final Learning Order

The recommended order is:

```text
1. JavaScript
       ↓
2. React
       ↓
3. Next.js Fundamentals
       ↓
4. App Router
       ↓
5. Components
       ↓
6. Server Components
       ↓
7. Client Components
       ↓
8. Routing
       ↓
9. Data Fetching
       ↓
10. API Route Handlers
       ↓
11. Forms
       ↓
12. Server Actions
       ↓
13. TypeScript
       ↓
14. Database
       ↓
15. Authentication
       ↓
16. Authorization
       ↓
17. Caching
       ↓
18. Performance
       ↓
19. Security
       ↓
20. Production Deployment
```

---

# 🏆 Final Goal

After completing this guide, you should be able to build applications such as:

```text
✅ Portfolio
✅ Blog
✅ E-commerce
✅ Admin Dashboard
✅ Inventory System
✅ Authentication System
✅ REST API
✅ Full-Stack CRUD Application
✅ SaaS Application
✅ Dashboard + Database Application
```

A strong Next.js developer should understand not only how to write components, but also:

```text
UI
+
Routing
+
Rendering
+
Server/Client boundaries
+
Data
+
Database
+
Authentication
+
Security
+
Caching
+
Performance
+
Deployment
```

---

# 📖 Quick Reference

## Start

```bash
npx create-next-app@latest my-app
cd my-app
npm run dev
```

## Development

```bash
npm run dev
```

## Build

```bash
npm run build
```

## Production

```bash
npm start
```

## Main files

```text
app/page.tsx
app/layout.tsx
app/loading.tsx
app/error.tsx
app/not-found.tsx
app/api/*/route.ts
```

## Main concepts

```text
Server Component
Client Component
Dynamic Route
Route Handler
Server Action
Metadata
Caching
Revalidation
Streaming
Suspense
Authentication
Database
```

---

# 🇰🇭 សង្ខេបជាភាសាខ្មែរ

Next.js គឺជា React Framework ដែលសមស្របសម្រាប់បង្កើត Full-Stack Web Application។

ចាប់ផ្តើមពី៖

```text
React
↓
Next.js Basics
↓
App Router
↓
Components
↓
Server Components
↓
Client Components
↓
Routing
↓
API
↓
Forms
↓
Server Actions
↓
Database
↓
Authentication
↓
Security
↓
Caching
↓
Performance
↓
Deployment
```

ចំណុចសំខាន់បំផុតគឺត្រូវយល់ពីភាពខុសគ្នារវាង៖

```text
Server Component
        VS
Client Component
```

និង៖

```text
Page
Layout
Route
API Route Handler
Server Action
```

ពេលយល់ចំណុចទាំងនេះហើយ អ្នកអាចបន្តទៅ Database, Authentication, Security និង Production Architecture បាន។
