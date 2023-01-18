## Adding Tasks to Home Page

Add the `TaskCard` component to the home page. Find the placeholder in `/app/(dashboard)/home/page.tsx`:

```jsx
<div className="mt-6 flex-2 grow w-full flex">
  <div className="w-full">{/* tasks here */}</div>
</div>
```

Add the `TaskCard` component:

```jsx
<div className="mt-6 flex-2 grow w-full flex">
  <div className="w-full">
    <TaskCard />
  </div>
</div>
```
