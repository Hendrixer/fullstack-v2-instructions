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
