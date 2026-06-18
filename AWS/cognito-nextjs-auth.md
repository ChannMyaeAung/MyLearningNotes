# 🔐 Enterprise Authentication with AWS Cognito & Next.js

This document outlines the end-to-end integration of Amazon Cognito with a Next.js (App Router) frontend using AWS Amplify, detailing both the infrastructure setup and client state hydration lifecycle.

---

## 🏗️ Architecture & Authentication Flow

The application implements a secure, decoupled authentication architecture across the client, identity provider, and backend server layers:


1. **Handshake:** The frontend client initiates authentication via the AWS Amplify SDK directly to the **Amazon Cognito User Pool**.
2. **Token Issuance:** Upon successful verification, Cognito returns cryptographically signed JSON Web Tokens (JWTs: ID, Access, and Refresh tokens).
3. **Session Interception:** A custom React wrapper (`authProvider.tsx`) intercepts the authentication state change and manages client-side redirection.
4. **API Hydration:** Global API queries (via Redux Toolkit Query) use a network interceptor to inject the JWT into the `Authorization: Bearer <TOKEN>` header.
5. **Backend Verification:** The server middleware validates the token signature and maps the verified `cognitoId` to local database application records.

---

## ⚙️ Configuration & Environment Setup

### 1. Client Environment Layer (`client/.env.local`)
Expose the identity provider variables to the browser context using Next.js public prefixes:

```env
NEXT_PUBLIC_AWS_COGNITO_USER_POOL_ID=ap-southeast-1_xxxxxxxxx
NEXT_PUBLIC_AWS_COGNITO_USER_POOL_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
```

### 2. Core Authentication Wrapper (`app/(auth)/authProvider.tsx`)

This client-side provider initializes the Amplify SDK context and enforces global styling rules for the pre-built interface engine.

TypeScript

```typescript
"use client";

import React from "react";
import { Amplify } from "aws-amplify";
import { Authenticator, useAuthenticator } from "@aws-amplify/ui-react";
import "@aws-amplify/ui-react/styles.css";

Amplify.configure({
  Auth: {
    Cognito: {
      userPoolId: process.env.NEXT_PUBLIC_AWS_COGNITO_USER_POOL_ID!,
      userPoolClientId: process.env.NEXT_PUBLIC_AWS_COGNITO_USER_POOL_CLIENT_ID!,
    },
  },
});

export default function Auth({ children }: { children: React.ReactNode }) {
  const { user } = useAuthenticator((context) => [context.user]);

  return (
    <div className="h-full">
      <Authenticator>
        {() => <>{children}</>}
      </Authenticator>
    </div>
  );
}
```

### 3. Application Provider Tree (`app/providers.tsx`)

TypeScript

```typescript
"use client";

import StoreProvider from "@/state/redux";
import { Authenticator } from "@aws-amplify/ui-react";
import Auth from "./(auth)/authProvider";

export default function Providers({ children }: { children: React.ReactNode }) {
  return (
    <StoreProvider>
      <Authenticator.Provider>
        <Auth>{children}</Auth>
      </Authenticator.Provider>
    </StoreProvider>
  );
}
```

## 💡 Engineering Takeaways & Debugging

- **Token Inspection:** During local development, capture the raw JWT string out of the network browser tab or application storage, and use **[jwt.io](https://jwt.io/)** to decode the header and payload claims safely.
- **Custom User Schema:** Custom user metadata profiles (like structural access control `roles`) must be defined as explicitly mapped custom attributes within the Cognito sign-up configuration stage before production pooling begins.