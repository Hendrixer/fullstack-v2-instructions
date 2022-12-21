Before we can create the API routes to handle user auth, we need some utility functions.

Create a new file, `auth` in the `lib` folder and add this to it. These functions will help us hash and check passwords.

```ts
import bcrypt from "bcrypt";
import { SignJWT, jwtVerify } from "jose";
import { db } from "./db";

export const hashPassword = (password) => bcrypt.hash(password, 10);

export const comparePasswords = (plainTextPassword, hashedPassword) =>
  bcrypt.compare(plainTextPassword, hashedPassword);
```

This file might be imported by a server component so we must tell Next.js about is dependencies, so in the `next.config.js` we must add `bcrypt` there.

```js
experimental: {
  appDir: true,
  serverComponentsExternalPackages: ['bcrypt'],
},
```
