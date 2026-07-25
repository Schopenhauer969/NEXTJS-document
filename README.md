# Next.js — Writing Data, Beginner to Advanced

A complete, practical guide to **writing data in Next.js** (App Router) — from your first API route to production-grade Server Actions, optimistic updates, file uploads, and error handling.

Examples target **Next.js 14+** with the **App Router** (`app/` directory) and TypeScript.

```bash
npx create-next-app@latest my-app
cd my-app
```

---

## Table of Contents

1. [Setup & Project Structure](#1-setup--project-structure)
2. [Beginner: API Route Handlers](#2-beginner-api-route-handlers)
3. [Beginner: Server Actions](#3-beginner-server-actions)
4. [Beginner: Writing from a Form](#4-beginner-writing-from-a-form)
5. [Intermediate: Validation with Zod](#5-intermediate-validation-with-zod)
6. [Intermediate: useFormState & useFormStatus](#6-intermediate-useformstate--useformstatus)
7. [Intermediate: Revalidating Data After a Write](#7-intermediate-revalidating-data-after-a-write)
8. [Intermediate: Database Writes (Prisma)](#8-intermediate-database-writes-prisma)
9. [Advanced: Optimistic Updates (useOptimistic)](#9-advanced-optimistic-updates-useoptimistic)
10. [Advanced: File & Image Uploads](#10-advanced-file--image-uploads)
11. [Advanced: Transactions Across Multiple Writes](#11-advanced-transactions-across-multiple-writes)
12. [Advanced: Rate Limiting & Auth Guards](#12-advanced-rate-limiting--auth-guards)
13. [Advanced: Error Handling Patterns](#13-advanced-error-handling-patterns)
14. [Advanced: Testing Writes](#14-advanced-testing-writes)
15. [Best Practices Cheat Sheet](#15-best-practices-cheat-sheet)

---

## 1. Setup & Project Structure

```
app/
├── actions/
│   └── posts.ts          # Server Actions ("use server")
├── api/
│   └── posts/
│       └── route.ts      # API Route Handler
├── posts/
│   ├── new/
│   │   └── page.tsx       # Form page
│   └── page.tsx
lib/
├── db.ts                  # Database client
└── validation.ts          # Zod schemas
```

```bash
npm install zod
```

---

## 2. Beginner: API Route Handlers

Route handlers live in `route.ts`/`route.js` files and export functions named after HTTP methods.

```typescript
// app/api/posts/route.ts
import { NextRequest, NextResponse } from "next/server";

// In-memory store for this example — replace with a real database
let posts: { id: number; title: string; body: string }[] = [];
let nextId = 1;

export async function POST(request: NextRequest) {
  const data = await request.json();

  if (!data.title || !data.body) {
    return NextResponse.json(
      { error: "title and body are required" },
      { status: 400 }
    );
  }

  const newPost = { id: nextId++, title: data.title, body: data.body };
  posts.push(newPost);

  return NextResponse.json(newPost, { status: 201 });
}

export async function GET() {
  return NextResponse.json(posts);
}
```

### Calling it from the Client

```typescript
// app/posts/create-button.tsx
"use client";

export function CreatePostButton() {
  async function handleClick() {
    const res = await fetch("/api/posts", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({ title: "Hello World", body: "My first post" })
    });

    if (!res.ok) {
      const err = await res.json();
      console.error("Failed to create post:", err.error);
      return;
    }

    const post = await res.json();
    console.log("Created post:", post);
  }

  return <button onClick={handleClick}>Create Post</button>;
}
```

---

## 3. Beginner: Server Actions

Server Actions let you write server-side mutation code that's callable directly from components — no manual `fetch` or API route needed.

```typescript
// app/actions/posts.ts
"use server";

import { revalidatePath } from "next/cache";

export async function createPost(formData: FormData) {
  const title = formData.get("title") as string;
  const body = formData.get("body") as string;

  if (!title || !body) {
    throw new Error("title and body are required");
  }

  // Replace with a real database write
  console.log("Saving post:", { title, body });

  revalidatePath("/posts"); // refresh the posts list after writing
}
```

### Using a Server Action Directly in a Server Component Form

```tsx
// app/posts/new/page.tsx
import { createPost } from "@/app/actions/posts";

export default function NewPostPage() {
  return (
    <form action={createPost}>
      <input name="title" placeholder="Title" required />
      <textarea name="body" placeholder="Body" required />
      <button type="submit">Create Post</button>
    </form>
  );
}
```

> ✅ No `onSubmit`, no `fetch`, no API route required — the `action` prop wires the form directly to server code. The form works even with JavaScript disabled.

---

## 4. Beginner: Writing from a Form

### Client Component Calling a Server Action

```tsx
// app/posts/new-post-form.tsx
"use client";

import { createPost } from "@/app/actions/posts";
import { useRef } from "react";

export function NewPostForm() {
  const formRef = useRef<HTMLFormElement>(null);

  async function handleSubmit(formData: FormData) {
    await createPost(formData);
    formRef.current?.reset();
  }

  return (
    <form ref={formRef} action={handleSubmit}>
      <input name="title" placeholder="Title" required />
      <textarea name="body" placeholder="Body" required />
      <button type="submit">Create Post</button>
    </form>
  );
}
```

---

## 5. Intermediate: Validation with Zod

Never trust `FormData` or JSON bodies directly — validate before writing.

```typescript
// lib/validation.ts
import { z } from "zod";

export const createPostSchema = z.object({
  title: z.string().min(3, "Title must be at least 3 characters").max(200),
  body: z.string().min(10, "Body must be at least 10 characters"),
  published: z.coerce.boolean().optional().default(false)
});

export type CreatePostInput = z.infer<typeof createPostSchema>;
```

```typescript
// app/actions/posts.ts
"use server";

import { revalidatePath } from "next/cache";
import { createPostSchema } from "@/lib/validation";

export async function createPost(prevState: unknown, formData: FormData) {
  const parsed = createPostSchema.safeParse({
    title: formData.get("title"),
    body: formData.get("body"),
    published: formData.get("published")
  });

  if (!parsed.success) {
    return {
      success: false,
      errors: parsed.error.flatten().fieldErrors
    };
  }

  // Replace with a real database write
  console.log("Validated data:", parsed.data);

  revalidatePath("/posts");
  return { success: true, errors: null };
}
```

---

## 6. Intermediate: useFormState & useFormStatus

Show validation errors and pending state without any client-side fetch logic.

```tsx
// app/posts/new-post-form.tsx
"use client";

import { useFormState, useFormStatus } from "react-dom";
import { createPost } from "@/app/actions/posts";

const initialState = { success: false, errors: null as Record<string, string[]> | null };

function SubmitButton() {
  const { pending } = useFormStatus();
  return (
    <button type="submit" disabled={pending}>
      {pending ? "Saving..." : "Create Post"}
    </button>
  );
}

export function NewPostForm() {
  const [state, formAction] = useFormState(createPost, initialState);

  return (
    <form action={formAction}>
      <input name="title" placeholder="Title" />
      {state.errors?.title && <p className="error">{state.errors.title[0]}</p>}

      <textarea name="body" placeholder="Body" />
      {state.errors?.body && <p className="error">{state.errors.body[0]}</p>}

      <SubmitButton />

      {state.success && <p className="success">Post created!</p>}
    </form>
  );
}
```

---

## 7. Intermediate: Revalidating Data After a Write

Next.js caches aggressively — after any write, tell it what's now stale.

```typescript
// app/actions/posts.ts
"use server";

import { revalidatePath, revalidateTag } from "next/cache";

export async function publishPost(postId: string) {
  // Replace with a real database write
  console.log(`Publishing post ${postId}`);

  // Option A: revalidate a specific path
  revalidatePath(`/posts/${postId}`);
  revalidatePath("/posts");

  // Option B: revalidate by cache tag (pairs with fetch(..., { next: { tags: ["posts"] } }))
  revalidateTag("posts");
}
```

```typescript
// Fetching with a tag so revalidateTag("posts") knows what to invalidate
async function getPosts() {
  const res = await fetch("https://api.example.com/posts", {
    next: { tags: ["posts"] }
  });
  return res.json();
}
```

---

## 8. Intermediate: Database Writes (Prisma)

```bash
npm install prisma @prisma/client
npx prisma init
```

```prisma
// prisma/schema.prisma
model Post {
  id        String   @id @default(cuid())
  title     String
  body      String
  published Boolean  @default(false)
  createdAt DateTime @default(now())
}
```

```typescript
// lib/db.ts
import { PrismaClient } from "@prisma/client";

// Prevent creating a new client on every hot reload in development
const globalForPrisma = globalThis as unknown as { prisma?: PrismaClient };

export const db = globalForPrisma.prisma ?? new PrismaClient();

if (process.env.NODE_ENV !== "production") {
  globalForPrisma.prisma = db;
}
```

```typescript
// app/actions/posts.ts
"use server";

import { db } from "@/lib/db";
import { createPostSchema } from "@/lib/validation";
import { revalidatePath } from "next/cache";

export async function createPost(prevState: unknown, formData: FormData) {
  const parsed = createPostSchema.safeParse({
    title: formData.get("title"),
    body: formData.get("body")
  });

  if (!parsed.success) {
    return { success: false, errors: parsed.error.flatten().fieldErrors };
  }

  const post = await db.post.create({
    data: {
      title: parsed.data.title,
      body: parsed.data.body
    }
  });

  revalidatePath("/posts");
  return { success: true, errors: null, postId: post.id };
}

export async function updatePost(id: string, formData: FormData) {
  await db.post.update({
    where: { id },
    data: {
      title: formData.get("title") as string,
      body: formData.get("body") as string
    }
  });

  revalidatePath(`/posts/${id}`);
}

export async function deletePost(id: string) {
  await db.post.delete({ where: { id } });
  revalidatePath("/posts");
}
```

---

## 9. Advanced: Optimistic Updates (useOptimistic)

Update the UI instantly while the server write is still in flight.

```tsx
// app/posts/post-list.tsx
"use client";

import { useOptimistic, useState, useTransition } from "react";
import { toggleLike } from "@/app/actions/posts";

type Post = { id: string; title: string; likes: number };

export function PostList({ initialPosts }: { initialPosts: Post[] }) {
  const [posts, setPosts] = useState(initialPosts);
  const [isPending, startTransition] = useTransition();

  const [optimisticPosts, addOptimisticLike] = useOptimistic(
    posts,
    (state, postId: string) =>
      state.map(p => (p.id === postId ? { ...p, likes: p.likes + 1 } : p))
  );

  function handleLike(postId: string) {
    startTransition(async () => {
      addOptimisticLike(postId);          // instant UI update
      const updated = await toggleLike(postId); // real server write
      setPosts(prev =>
        prev.map(p => (p.id === postId ? updated : p))
      );
    });
  }

  return (
    <ul>
      {optimisticPosts.map(post => (
        <li key={post.id}>
          {post.title} — {post.likes} likes
          <button onClick={() => handleLike(post.id)} disabled={isPending}>
            👍 Like
          </button>
        </li>
      ))}
    </ul>
  );
}
```

```typescript
// app/actions/posts.ts
"use server";

import { db } from "@/lib/db";

export async function toggleLike(postId: string) {
  const post = await db.post.update({
    where: { id: postId },
    data: { likes: { increment: 1 } }
  });
  return post;
}
```

---

## 10. Advanced: File & Image Uploads

```tsx
// app/upload/upload-form.tsx
"use client";

import { uploadAvatar } from "@/app/actions/upload";

export function UploadForm() {
  return (
    <form action={uploadAvatar}>
      <input type="file" name="avatar" accept="image/*" required />
      <button type="submit">Upload</button>
    </form>
  );
}
```

```typescript
// app/actions/upload.ts
"use server";

import { writeFile, mkdir } from "fs/promises";
import path from "path";
import { randomUUID } from "crypto";

const MAX_SIZE_BYTES = 5 * 1024 * 1024; // 5 MB
const ALLOWED_TYPES = ["image/png", "image/jpeg", "image/webp"];

export async function uploadAvatar(formData: FormData) {
  const file = formData.get("avatar") as File;

  if (!file || file.size === 0) {
    throw new Error("No file provided");
  }
  if (!ALLOWED_TYPES.includes(file.type)) {
    throw new Error("Unsupported file type");
  }
  if (file.size > MAX_SIZE_BYTES) {
    throw new Error("File too large (max 5MB)");
  }

  const buffer = Buffer.from(await file.arrayBuffer());
  const ext = file.type.split("/")[1];
  const filename = `${randomUUID()}.${ext}`;

  const uploadDir = path.join(process.cwd(), "public", "uploads");
  await mkdir(uploadDir, { recursive: true });
  await writeFile(path.join(uploadDir, filename), buffer);

  return { url: `/uploads/${filename}` };
}
```

> ⚠️ Writing to the local filesystem only works reliably on servers with persistent disks. On serverless platforms (Vercel, etc.), use **object storage** (S3, Cloudflare R2, Vercel Blob) instead — the filesystem there is ephemeral.

### Uploading to Vercel Blob (serverless-friendly alternative)

```bash
npm install @vercel/blob
```

```typescript
// app/actions/upload.ts
"use server";

import { put } from "@vercel/blob";

export async function uploadAvatar(formData: FormData) {
  const file = formData.get("avatar") as File;

  if (!file || file.size === 0) {
    throw new Error("No file provided");
  }

  const blob = await put(`avatars/${crypto.randomUUID()}-${file.name}`, file, {
    access: "public"
  });

  return { url: blob.url };
}
```

---

## 11. Advanced: Transactions Across Multiple Writes

```typescript
// app/actions/orders.ts
"use server";

import { db } from "@/lib/db";
import { revalidatePath } from "next/cache";

export async function placeOrder(userId: string, items: { sku: string; qty: number }[]) {
  try {
    const order = await db.$transaction(async (tx) => {
      // 1. Create the order
      const newOrder = await tx.order.create({
        data: { userId, status: "pending" }
      });

      // 2. Decrement stock for each item, failing the whole transaction if any is short
      for (const item of items) {
        const product = await tx.product.update({
          where: { sku: item.sku, stock: { gte: item.qty } },
          data: { stock: { decrement: item.qty } }
        }).catch(() => null);

        if (!product) {
          throw new Error(`Insufficient stock for SKU ${item.sku}`);
        }

        await tx.orderItem.create({
          data: { orderId: newOrder.id, sku: item.sku, qty: item.qty }
        });
      }

      return newOrder;
    });

    revalidatePath("/orders");
    return { success: true, orderId: order.id };
  } catch (error) {
    console.error("Order failed:", error);
    return { success: false, error: (error as Error).message };
  }
}
```

> ✅ `db.$transaction` (Prisma) rolls back **every** write inside the callback if any step throws — critical for multi-table writes like orders + inventory.

---

## 12. Advanced: Rate Limiting & Auth Guards

```typescript
// lib/rate-limit.ts
const requestLog = new Map<string, number[]>();

export function isRateLimited(key: string, limit = 5, windowMs = 60_000): boolean {
  const now = Date.now();
  const timestamps = (requestLog.get(key) ?? []).filter(t => now - t < windowMs);
  timestamps.push(now);
  requestLog.set(key, timestamps);
  return timestamps.length > limit;
}
```

```typescript
// app/actions/posts.ts
"use server";

import { auth } from "@/lib/auth";
import { isRateLimited } from "@/lib/rate-limit";
import { db } from "@/lib/db";

export async function createPost(formData: FormData) {
  const session = await auth();
  if (!session?.user) {
    throw new Error("Unauthorized: you must be signed in to post");
  }

  if (isRateLimited(session.user.id)) {
    throw new Error("Too many requests — please slow down");
  }

  const title = formData.get("title") as string;
  const body = formData.get("body") as string;

  return db.post.create({
    data: { title, body, authorId: session.user.id }
  });
}
```

> ℹ️ For production rate limiting across multiple server instances, use a shared store like Redis (e.g. `@upstash/ratelimit`) instead of an in-memory `Map`, which resets per server instance.

---

## 13. Advanced: Error Handling Patterns

### Structured Return Values (recommended for form-bound Server Actions)

```typescript
// app/actions/posts.ts
"use server";

import { db } from "@/lib/db";
import { Prisma } from "@prisma/client";

type ActionResult =
  | { success: true; postId: string }
  | { success: false; error: string };

export async function createPost(formData: FormData): Promise<ActionResult> {
  try {
    const post = await db.post.create({
      data: {
        title: formData.get("title") as string,
        body: formData.get("body") as string
      }
    });
    return { success: true, postId: post.id };
  } catch (error) {
    if (error instanceof Prisma.PrismaClientKnownRequestError) {
      if (error.code === "P2002") {
        return { success: false, error: "A post with this title already exists" };
      }
    }
    console.error("Unexpected error creating post:", error);
    return { success: false, error: "Something went wrong. Please try again." };
  }
}
```

### `error.tsx` — Catching Uncaught Errors at the Route Segment Level

```tsx
// app/posts/error.tsx
"use client";

export default function PostsError({
  error,
  reset
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <div>
      <h2>Something went wrong loading or saving posts.</h2>
      <p>{error.message}</p>
      <button onClick={() => reset()}>Try again</button>
    </div>
  );
}
```

### API Route Error Handling

```typescript
// app/api/posts/route.ts
import { NextRequest, NextResponse } from "next/server";
import { db } from "@/lib/db";
import { createPostSchema } from "@/lib/validation";

export async function POST(request: NextRequest) {
  try {
    const body = await request.json();
    const parsed = createPostSchema.safeParse(body);

    if (!parsed.success) {
      return NextResponse.json(
        { error: "Validation failed", details: parsed.error.flatten() },
        { status: 400 }
      );
    }

    const post = await db.post.create({ data: parsed.data });
    return NextResponse.json(post, { status: 201 });
  } catch (error) {
    console.error("POST /api/posts failed:", error);
    return NextResponse.json(
      { error: "Internal server error" },
      { status: 500 }
    );
  }
}
```

---

## 14. Advanced: Testing Writes

```bash
npm install -D vitest @testing-library/react
```

```typescript
// app/actions/posts.test.ts
import { describe, it, expect, vi, beforeEach } from "vitest";
import { createPost } from "./posts";
import { db } from "@/lib/db";

vi.mock("@/lib/db", () => ({
  db: {
    post: {
      create: vi.fn()
    }
  }
}));

vi.mock("next/cache", () => ({
  revalidatePath: vi.fn()
}));

describe("createPost", () => {
  beforeEach(() => {
    vi.clearAllMocks();
  });

  it("rejects a title that is too short", async () => {
    const formData = new FormData();
    formData.set("title", "hi");
    formData.set("body", "This is a valid body length.");

    const result = await createPost(undefined, formData);

    expect(result.success).toBe(false);
    expect(result.errors?.title).toBeDefined();
    expect(db.post.create).not.toHaveBeenCalled();
  });

  it("creates a post with valid data", async () => {
    (db.post.create as any).mockResolvedValue({ id: "1", title: "Valid Title" });

    const formData = new FormData();
    formData.set("title", "A Valid Title");
    formData.set("body", "This is a valid body with enough characters.");

    const result = await createPost(undefined, formData);

    expect(result.success).toBe(true);
    expect(db.post.create).toHaveBeenCalledTimes(1);
  });
});
```

---

## 15. Best Practices Cheat Sheet

- ✅ Prefer **Server Actions** over API routes for form-driven writes — less boilerplate, works without JS, and integrates with `useFormState`/`useOptimistic`.
- ✅ Always validate input server-side (Zod or similar) — client-side validation alone can be bypassed.
- ✅ Call `revalidatePath`/`revalidateTag` after every write that affects cached data, or users will see stale content.
- ✅ Wrap multi-step database writes in a transaction (`db.$transaction`) so partial failures don't corrupt data.
- ✅ Return structured `{ success, error }` objects from Server Actions instead of throwing where possible — easier to render in the UI.
- ✅ Use `useOptimistic` for actions where perceived speed matters (likes, toggles, reactions).
- ✅ Guard every mutating Server Action / Route Handler with an auth check — Server Actions are public HTTP endpoints under the hood.
- ✅ Use object storage (S3, Vercel Blob, R2) for file uploads on serverless deployments — the local filesystem is ephemeral there.
- ✅ Rate-limit public write endpoints, using a shared store (Redis) in multi-instance deployments.
- ❌ Don't trust `FormData`/JSON bodies without validation, even from your own frontend.
- ❌ Don't forget `"use server"` at the top of Server Action files/functions — without it, code runs on the client.
- ❌ Don't perform writes inside Server Components during render — use Server Actions or Route Handlers instead.

---

## License

Free to use in any project — copy, adapt, and drop straight into your own `README.md`.
