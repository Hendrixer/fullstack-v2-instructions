Next we need API routes for auth

For register `/api/register`

```ts
import { NextApiRequest, NextApiResponse } from "next";
import { db } from "@/lib/db";
import { createJWT, hashPassword } from "@/lib/auth";
import { serialize } from "cookie";

export default async function register(
  req: NextApiRequest,
  res: NextApiResponse
) {
  if (req.method === "POST") {
    const user = await db.user.create({
      data: {
        email: req.body.email,
        password: await hashPassword(req.body.password),
        firstName: req.body.firstName,
        lastName: req.body.lastName,
      },
    });

    const jwt = await createJWT(user);
    res.setHeader(
      "Set-Cookie",
      serialize(process.env.COOKIE_NAME, jwt, {
        httpOnly: true,
        path: "/",
        maxAge: 60 * 60 * 24 * 7,
      })
    );
    res.status(201);
    res.end();
  } else {
    res.status(402);
    res.end();
  }
}
```

For sigin `/api/signin`

```ts
import { NextApiRequest, NextApiResponse } from "next";
import { db } from "@/lib/db";
import { comparePasswords, createJWT } from "@/lib/auth";
import { serialize } from "cookie";

export default async function signin(
  req: NextApiRequest,
  res: NextApiResponse
) {
  if (req.method === "POST") {
    const user = await db.user.findUnique({
      where: {
        email: req.body.email,
      },
    });

    if (!user) {
      res.status(401);
      res.json({ error: "Invalid login" });
      return;
    }

    const isUser = await comparePasswords(req.body.password, user.password);

    if (isUser) {
      const jwt = await createJWT(user);
      res.setHeader(
        "Set-Cookie",
        serialize(process.env.COOKIE_NAME, jwt, {
          httpOnly: true,
          path: "/",
          maxAge: 60 * 60 * 24 * 7,
        })
      );
      res.status(201);
      res.end();
    } else {
      res.status(401);
      res.json({ error: "Invalid login" });
    }
  } else {
    res.status(402);
    res.end();
  }
}
```
