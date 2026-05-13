# GCP-Gmail API

## Sending Emails from Next.js using Google Workspace Gmail API

> **Approach:** Service Account with Domain-Wide Delegation — no OAuth login required, no expiring tokens, fully server-side.

***

### Overview

This guide walks through setting up your Google Workspace account to send transactional emails (notifications, confirmations, alerts) from a Next.js application using the Gmail API.

#### Why this approach?

| Method                           | Pros                                           | Cons                                         |
| -------------------------------- | ---------------------------------------------- | -------------------------------------------- |
| SMTP + App Password              | Simple setup                                   | Requires Admin to enable; 2,000/day limit    |
| OAuth2 + Refresh Token           | User-scoped                                    | Tokens expire; needs Google app verification |
| **Service Account (this guide)** | **No expiry, server-to-server, no user login** | **Needs domain-wide delegation setup**       |

#### What you'll need

* A Google Workspace account with a custom domain (e.g. `yourcompany.com`)
* Admin access to [Google Cloud Console](https://console.cloud.google.com/)
* Admin access to [Google Workspace Admin](https://admin.google.com/)
* A Next.js app (v13+ with App Router recommended)

***

### Phase 1 — Google Cloud Console

#### Step 1 — Create a Google Cloud Project

1. Go to [console.cloud.google.com](https://console.cloud.google.com/)
2. Click the project selector at the top → **New Project**
3. Give it a name (e.g. `MyAppMailer`) and click **Create**
4. Make sure the new project is selected in the top bar

> **Why a separate project?** Keeping your mailer isolated from other infrastructure makes it easier to manage permissions, monitor API usage, and revoke access if needed.

***

#### Step 2 — Enable the Gmail API

1. In your project, go to **APIs & Services → Library**
2. Search for **Gmail API**
3. Click on it → click **Enable**

> The Gmail API allows your server to send, read, and manage Gmail messages programmatically. For this guide, you only need send access — you'll restrict the scope to `gmail.send` later.

***

#### Step 3 — Create a Service Account

A **Service Account** is a special Google account that represents your application, not a human user. It authenticates using a private key (JSON file) instead of a password.

1. Go to **IAM & Admin → Service Accounts**
2. Click **+ Create Service Account**
3. Fill in:
   * **Name:** e.g. `mailer-service`
   * **ID:** auto-filled (e.g. `mailer-service@your-project.iam.gserviceaccount.com`)
   * **Description:** e.g. `Sends transactional emails via Gmail API`
4. Click **Create and Continue**

**Permissions step — skip it**

On the **"Grant this service account access"** screen, **do not select any IAM role**. Click **Done**.

> **Why skip IAM roles here?** IAM roles control access to Google Cloud resources (Storage, Compute, BigQuery, etc.). Gmail sending permissions are granted separately through Google Workspace domain-wide delegation — not through Cloud IAM. Adding a Cloud IAM role here would give the service account unnecessary access to your cloud infrastructure.

***

#### Step 4 — Fix the Org Policy (if blocked)

When you try to create a JSON key, you may see:

```
Service account key creation is disabled.
Policy: iam.disableServiceAccountKeyCreation
```

This means your organization has a security policy blocking JSON key creation. You need to temporarily disable it.

**Fix:**

1. Go to **IAM & Admin → Service Accounts** → click your service account
2. Go to the Permissions tab
3. click **Manage access**
4. Add role to "owner" temporarily&#x20;
5. Click **Save**

> **Security note:** Once you've downloaded your JSON key, you can delete this role. The key will continue working — the policy only blocks _creation_ of new keys, not the use of existing ones.

***

#### Step 5 — Download the JSON Key

This is your application's credential file. It contains a private key that proves your app is the service account.

1. Go to **IAM & Admin → Service Accounts** → click your service account
2. Go to the **Keys** tab
3. Click **Add Key → Create new key**
4. Select **JSON** → click **Create**
5. The file downloads automatically — keep it safe

**What's inside the JSON key file**

```json
{
  "type": "service_account",
  "project_id": "your-project-id",
  "private_key_id": "abc123...",
  "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEv...\n-----END PRIVATE KEY-----\n",
  "client_email": "mailer-service@your-project.iam.gserviceaccount.com",
  "client_id": "123456789012345678901",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token"
}
```

You'll use `client_email` and `private_key` in your Next.js app.

> **Never commit this file to Git.** Add it to `.gitignore` immediately. Store credentials only in environment variables or a secrets manager (e.g. AWS Secrets Manager, Vercel Environment Variables).

***

#### Step 6 — Enable Domain-Wide Delegation

Domain-wide delegation allows your service account to **impersonate** a Google Workspace user (e.g. `info@yourcompany.com`) and send email on their behalf — without that user ever logging in.

1. Click on your service account → go to the **Details** tab
2. Scroll down to **"Domain-wide delegation"**
3. Check **"Enable Google Workspace Domain-wide Delegation"**
4. Enter a product name (e.g. `MyApp Mailer`)
5. Click **Save**
6. You'll now see a **Client ID** — a long numeric string like `123456789012345678901`

**Copy this Client ID** — you'll need it in the next phase.

> **How impersonation works technically:** When your app makes a Gmail API request, it uses the service account's private key to sign a JWT (JSON Web Token). That JWT includes a `sub` field set to the Workspace user's email address. Google verifies the JWT signature against the service account's public key, checks that domain-wide delegation is authorized for this Client ID and scope, and then issues a short-lived OAuth access token scoped to that user's Gmail. Your app uses that access token to call the Gmail API. This all happens server-side in milliseconds — no user interaction required.

***

### Phase 2 — Google Workspace Admin Console

#### Step 7 — Authorize the Service Account

This is where you tell Google Workspace: _"This service account is allowed to send emails on behalf of users in our organization."_

1. Go to [admin.google.com](https://admin.google.com/)
2. Navigate to **Security → Access and data control → API controls**
3. Click **Manage Domain-Wide Delegation**
4. Click **Add new**
5. Fill in:
   * **Client ID:** paste the numeric Client ID from Step 6
   * **OAuth scopes:** paste exactly:

```
https://www.googleapis.com/auth/gmail.send
```

6. Click **Authorize**

> **Why just `gmail.send` scope?** The principle of least privilege — your service account gets only the permission it needs. The broader `https://mail.google.com/` scope would give read/write/delete access to the entire mailbox, which is far more than needed for sending emails.

> **Propagation delay:** Changes to domain-wide delegation can take up to 30 minutes to propagate across Google's systems. If you get permission errors immediately after adding the scope, wait and try again.

***

### Phase 3 — Next.js Application

#### Step 8 — Add credentials to `.env.local`

Open your JSON key file and copy the `client_email` and `private_key` values.

```env
# .env.local

GOOGLE_SERVICE_ACCOUNT_EMAIL=mailer-service@your-project.iam.gserviceaccount.com
GOOGLE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nMIIEvgIBADANBgkq...\n-----END PRIVATE KEY-----\n"
GMAIL_SENDER=info@yourcompany.com
```

> **Important — private key formatting:** The private key in the JSON file contains literal `\n` characters representing newlines. When you paste it into `.env.local`, keep those `\n` sequences — don't expand them into real line breaks. In your code, you'll call `.replace(/\\n/g, '\n')` to convert them back. This is a common gotcha when deploying to platforms like Vercel.

For Vercel deployments, add these as **Environment Variables** in your project settings (not in the file — the file is for local development only).

***

#### Step 9 — Install the Google API client library

```bash
npm install googleapis
```

The `googleapis` package is Google's official Node.js client library. It handles JWT signing, token exchange, and all Gmail API calls.

***

#### Step 10 — Create the Gmail utility (`lib/gmail.ts`)

This file contains all the Gmail API logic. It's kept separate from the API route so it can be reused across your app.

```typescript
// lib/gmail.ts
import { google } from 'googleapis';

/**
 * Creates an authenticated Gmail API client using the service account.
 * The `subject` field impersonates a Workspace user — the email will
 * appear to come from that address in the recipient's inbox.
 */
function getGmailClient(senderEmail: string) {
  const auth = new google.auth.JWT({
    email: process.env.GOOGLE_SERVICE_ACCOUNT_EMAIL,
    key: process.env.GOOGLE_PRIVATE_KEY?.replace(/\\n/g, '\n'),
    scopes: ['https://www.googleapis.com/auth/gmail.send'],
    subject: senderEmail, // impersonate this Workspace user
  });

  return google.gmail({ version: 'v1', auth });
}

/**
 * Builds a base64url-encoded RFC 2822 email message.
 *
 * The Gmail API's messages.send endpoint expects a raw RFC 2822 message
 * encoded in base64url format (not standard base64 — uses - and _ instead
 * of + and /, and no padding = characters).
 */
function buildRawEmail(
  to: string,
  subject: string,
  html: string,
  from: string
): string {
  const message = [
    `From: ${from}`,
    `To: ${to}`,
    `Subject: ${subject}`,
    `MIME-Version: 1.0`,
    `Content-Type: text/html; charset=utf-8`,
    ``,
    html,
  ].join('\r\n'); // RFC 2822 requires CRLF line endings

  // Convert to base64url (Gmail API requirement)
  return Buffer.from(message)
    .toString('base64')
    .replace(/\+/g, '-')   // base64url: + → -
    .replace(/\//g, '_')   // base64url: / → _
    .replace(/=+$/, '');   // base64url: strip padding
}

/**
 * Sends an HTML email via the Gmail API.
 *
 * @param to - Recipient email address
 * @param subject - Email subject line
 * @param html - HTML body content
 */
export async function sendEmail(
  to: string,
  subject: string,
  html: string
): Promise<void> {
  const sender = process.env.GMAIL_SENDER!;
  const gmail = getGmailClient(sender);

  const raw = buildRawEmail(to, subject, html, sender);

  await gmail.users.messages.send({
    userId: 'me', // 'me' refers to the impersonated user (GMAIL_SENDER)
    requestBody: { raw },
  });
}
```

**How the JWT auth flow works**

```
Your app                     Google Auth Server            Gmail API
    |                               |                          |
    |-- Sign JWT with private key ->|                          |
    |   (includes subject: sender) |                          |
    |                               |-- Verify signature       |
    |                               |-- Check delegation scope |
    |<-- Short-lived access token --|                          |
    |                                                          |
    |-- API request + access token --------------------------->|
    |<-- Email sent -------------------------------------------|
```

The JWT is signed locally using the private key from your JSON file. No password is ever sent over the network.

***

#### Step 11 — Create the API Route

```typescript
// app/api/send-email/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { sendEmail } from '@/lib/gmail';

export async function POST(req: NextRequest) {
  try {
    const { to, subject, html } = await req.json();

    // Basic validation
    if (!to || !subject || !html) {
      return NextResponse.json(
        { error: 'Missing required fields: to, subject, html' },
        { status: 400 }
      );
    }

    await sendEmail(to, subject, html);

    return NextResponse.json({ success: true });

  } catch (error) {
    console.error('[send-email] Error:', error);
    return NextResponse.json(
      { error: 'Failed to send email' },
      { status: 500 }
    );
  }
}
```

> **Security tip:** In production, add authentication to this route so only your own frontend (or authenticated users) can trigger email sends. Without it, anyone who discovers the endpoint could send emails through your Workspace account. Use session checks, API keys, or middleware.

***

#### Step 12 — Call from your frontend

```typescript
// Example: sending a confirmation email after a form submission

async function sendConfirmationEmail(clientEmail: string, orderNumber: string) {
  const response = await fetch('/api/send-email', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      to: clientEmail,
      subject: `Order #${orderNumber} confirmed`,
      html: `
        <h1>Your order is confirmed!</h1>
        <p>Order number: <strong>${orderNumber}</strong></p>
        <p>We'll notify you when it ships.</p>
      `,
    }),
  });

  if (!response.ok) {
    const error = await response.json();
    throw new Error(error.message || 'Failed to send email');
  }

  return response.json();
}
```

***

### File structure summary

```
your-nextjs-app/
├── .env.local                        # credentials (never commit)
├── .gitignore                        # must include .env.local
├── lib/
│   └── gmail.ts                      # Gmail API utility
└── app/
    └── api/
        └── send-email/
            └── route.ts              # POST /api/send-email
```

***

### Sending limits

| Plan                         | Daily limit      | Notes                          |
| ---------------------------- | ---------------- | ------------------------------ |
| Google Workspace (all plans) | 2,000 emails/day | Per user sending via API       |
| Gmail free accounts          | 500 emails/day   | Not recommended for production |

For higher volume (newsletters, bulk notifications), consider a dedicated email delivery service like Resend or SendGrid. For typical transactional email (order confirmations, password resets, alerts), 2,000/day is sufficient for most apps.
