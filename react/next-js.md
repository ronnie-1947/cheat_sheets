# Next JS

Next JS introduced some features that can run code on the server using Server Actions

Link to app\_1: [https://github.com/ronnie-1947/nextjs\_v14\_struc\_app](https://github.com/ronnie-1947/nextjs\_v14\_struc\_app)

## Server Actions

An example shown to create a server action in TypeScript.

```javascript
async function createSnippet(formData: FormData) {

  // This needs to be a server action!
  "use server";

  // Check the user's input and make sure they're valid
  const inputs: string[] = ["title", "code"];
  const [title, code] = inputs.map((c) => formData.get(c) as string);

  // Create a new record in the database
  await db.snippet.create({ data: { code, title } });

  // Redirect the user back to the root route
  redirect("/");
}
```

## Server Component & Client Component

#### Server Component

By default all components are server components in NEXT JS. We can directly fetch data from database.

**Limitations**

* Cannot use any kind of hook
* Cannot assign any event handlers

#### Client Component

All that we are using in react apps. These components supports events, hooks etc. Basically that run on the client browser. A client can't show directly a server component

```javascript
'use client'
```

### Use Form State

It is a hook for client components. It is used to show error messages in forms during validations. It works with server actions and client components together.

It adds a state to a form that is controlled by server-side. Form state is used to change error message or send successful message.

#### UseFormState Hook

Note: This form hook runs fine without running JS in the browser

```javascript
const SnippetCreatePage = () => {
  const [formState, action] = useFormState(createSnippetAction, {
    message: "",
  });

  return (
    <form action={action}></form>
  )}
```

#### Create server Action

```javascript
'use server'

export async function createSnippetAction(formState: { message: string }, formData: FormData) {

  try {
    // Check the user's input and make sure they're valid
    const inputs: string[] = ["title", "code"];
    const [title, code] = inputs.map((c) => formData.get(c));

    if (typeof (title) !== 'string' || title.length < 3) return { message: 'Title must be longer' }
    if (typeof (code) !== 'string' || code.length < 3) return { message: 'Code must be longer' }

    // Create a new record in the database
    await db.snippet.create({ data: { code, title } });

  } catch (err: unknown) {
    if (err instanceof Error) {
      return {
        message: err.message
      }
    } else {
      return {
        message: 'Something went wrong'
      }
    }
  }
  
  // Redirect the user back to the root route
  redirect("/");
}
```

## Use Server Actions in Client components

### 1. Pass server action as props to client component

```javascript
export default function SnippetEditPage(){
    async function serverAction_update(id, code){
        'use server'
        //...
    }
    
    return(
        <ClientComponent onSubmit={serverAction}/>
    )
}
```

### 2. Export server actions from action.ts page

```javascript
'user server'

export async function serverAction_update(id, code){
    //...
}
```

### 1. Server Action in client component (Bind method)

This <mark style="color:green;">**option works**</mark>, even someone is <mark style="color:red;">**not using JS**</mark> in the client side

```javascript
'use client'

import {useState} from 'react'
import * as action from '@action.ts'

export default function EditSnippet(){
    
    const [code, setCode] = useState('')
    
    // Modify the action to get the code as an argument
    const editCode = action.editCode.bind(null, code)
    return(
        <form action={editCode} /form>
    )
}
```

### 2. React style use server Actions in client components

```javascript
'use client'

import {useState} from 'react'
import * as action from '@action.ts'

export default function EditSnippet(){
    
    const [code, setCode] = useState('')
    
    const editCodeHandler = ()=>{
    
// Start transition makes sure not to navigate before server response
        startTransition( async ()=>{
            await action.editCode(code)
        })
    }
    return(
        <button onClick={editCodeHandler} >Submit</button>
    )
}
```

## Show Error page

Show error page  if something wrong goes out in server actions.

```typescript
"use client";

import React from "react";

interface Props {
  error: Error;
  reset: () => void;
}

export default function ErrorPage({ error }: Props) {
  return (
    <>
      <div>{error.message}</div>
    </>
  );
}
```

## Cache & Static or Dynamic page

"Yarn build" will produce build outputs of the files. By default all the files are static.

#### Disable caching

To make it dynamic, write in the page file. This means after every 0 seconds fetch latest data from database.

Revalidate option makes the page to re-render after N seconds. Here 👇 N = 0

```javascript
export const revalidate = 0; 
```

#### On Demand revalidation

Revalidate with latest data whenever needed. This is the best way to show new data. Do it in server actions

```javascript
import { revalidatePath } from "next/cache"

export const deleteSnippetAction = async (id: number) => {
  await db.snippet.delete({ where: { id } })

  revalidatePath('/')
  redirect('/')
}
```

#### Caching in dynamic paths

To cache dynamic paths. Do this below. Add this to page.tsx file in \[id]/page.tsx route.

```javascript
export async function generateStaticParams() {
  const snippets = await db.snippet.findMany()

  return snippets.map((snippet)=>({
    id: snippet.id.toString()
  }))
}
```

## Next Authentication

### Install necessary packages

```sh
yarn add next-auth@5.0.0-beta.19 @auth/prisma-adapter @auth/core
```

### Get client ID and client Secret from GitHub/Google

Go to 👉 [github.com/settings/applications/new](https://github.com/settings/applications/new) and register a new application

<table><thead><tr><th width="300">Field</th><th>Entry</th></tr></thead><tbody><tr><td>Application Name</td><td>Any name</td></tr><tr><td>Homepage URL</td><td>localhost:3000 for dev</td></tr><tr><td>Authorization callback URL</td><td><a href="http://localhost:3000/api/auth/callback/github">http://localhost:3000/api/auth/callback/github</a> use it for GitHub</td></tr></tbody></table>

Get 3 things and place it in .env.local

1. GITHUB\_CLIENT\_ID
2. GITHUB\_CLIENT\_SECRET
3. AUTH\_SECRET

### Configure Prisma Schema

#### Add to the prisma.schema file

```typescript
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}

model Account {
  id                String  @id @default(cuid())
  userId            String
  type              String
  provider          String
  providerAccountId String
  refresh_token     String?
  access_token      String?
  expires_at        Int?
  token_type        String?
  scope             String?
  id_token          String?
  session_state     String?

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@unique([provider, providerAccountId])
}

model Session {
  id           String   @id @default(cuid())
  sessionToken String   @unique
  userId       String
  expires      DateTime
  user         User     @relation(fields: [userId], references: [id], onDelete: Cascade)
}

model User {
  id            String    @id @default(cuid())
  name          String?
  email         String?   @unique
  emailVerified DateTime?
  image         String?
  accounts      Account[]
  sessions      Session[]
}

model VerificationToken {
  identifier String
  token      String   @unique
  expires    DateTime

  @@unique([identifier, token])
}
```

#### Migrate Prisma

```sh
npx prisma generate
npx prisma migrate dev
```

### Setup auth file

#### Add auth.ts file in the src directory

```typescript
import NextAuth from "next-auth";

import Github from "next-auth/providers/github";
import { PrismaAdapter } from "@auth/prisma-adapter";
import { db } from "./db";

const GITHUB_CLIENT_ID = process.env.GITHUB_CLIENT_ID;
const GITHUB_CLIENT_SECRET = process.env.GITHUB_CLIENT_SECRET;

if (!GITHUB_CLIENT_ID || !GITHUB_CLIENT_SECRET)
  throw new Error("Missing github oauth credentials");

export const authOptions = {
  adapter: PrismaAdapter(db),
  providers: [
    Github({ clientId: GITHUB_CLIENT_ID, clientSecret: GITHUB_CLIENT_SECRET }),
  ],
  trustHost: true,
  callbacks: {
    // Usually not needed, here we are fixing a bug in nextauth
    async session({ session, user }: any) {
      if (session && user) {
        session.user.id = user.id;
      }

      return session;
    },
  },
};

export const {
  handlers: { GET, POST },
  auth,
  signOut,
  signIn,
} = NextAuth(authOptions);
```

Make a new file src/app/api/\[...nextauth]/route.ts

```typescript
export {GET, POST} from '@/auth'
```

### Setup SessionProvider for client components

Wrap entire layout in Session Provider

<pre class="language-typescript"><code class="lang-typescript"><strong>// provider.ts file
</strong><strong>"use client";
</strong>
import { NextUIProvider } from "@nextui-org/react";
import { SessionProvider } from "next-auth/react";

interface Props {
  children: React.ReactNode;
}

function Providers({ children }: Props) {

  return (
    &#x3C;SessionProvider >
      &#x3C;NextUIProvider>{children}&#x3C;/NextUIProvider>
    &#x3C;/SessionProvider>
  );
}

export default Providers;

</code></pre>

```typescript
// layout.tsx file

import type { Metadata } from "next";
import { Inter } from "next/font/google";
import "./globals.css";
import Providers from "./providers";

const inter = Inter({ subsets: ["latin"] });

export const metadata: Metadata = {
  title: "Create Next App",
  description: "Generated by create next app",
};

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="en">
      <body className={inter.className}>
        <Providers>
          <main className="container mx-auto mx-w-6xl">{children}</main>
        </Providers>
      </body>
    </html>
  );
}
```

### Use session in the app

#### In server components

```typescript
'use server'

import {auth} from '@/auth'

export default async function Create (){
    const session = await auth()
    
    if(!session) return 'Not logged in'
    return 'Logged in'
}
```

#### In Client components

```typescript
'use client'

import useSession from 'next-auth/react'

export default async function Auth() {
    const session = useSession()
    const {data: {user, email}, status} = session
    
    return ()
}
```
