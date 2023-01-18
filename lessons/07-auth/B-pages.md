## Sign In & Register Pages

Now to add the `AuthForm` to the right pages

```ts
// signin/page.tsx
import AuthForm from "@/components/AuthForm";

export default function Register() {
  return <AuthForm mode="signin" />;
}
```

```ts
// register/page.tsx
import AuthForm from "@/components/AuthForm";

export default function Register() {
  return <AuthForm mode="register" />;
}
```

> ✔️ Code Checkpoint: The current code for the application can be found on the [auth-pages branch](https://github.com/Hendrixer/fullstack-app-v2-app/tree/auth-pages).
