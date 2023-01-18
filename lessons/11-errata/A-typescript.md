---
title: TypeScript
---

## Adding Types

Throughout the course `.ts/.tsx` files were used by types mostly omitted. As a challenge, try to add types to the application. There are many correct ways to do this. Check out the [typescript branch](https://github.com/Hendrixer/fullstack-app-v2-app/tree/typescript) for one possibly solution.

In the solution, you see a few TypeScript comments for server components:

```jsx
{
  /* @ts-expect-error Server Component */
}
<Greetings />;
```

This is a workaround for end-to-end type safety with server compoents. See the [Next.js Beta Docs](https://beta.nextjs.org/docs/configuring/typescript#end-to-end-type-safety) for more information.
