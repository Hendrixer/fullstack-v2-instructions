## GreetingsSkeleton Component

Now we'll create the loading component for the greetings card. Create `/components/GreetingsSkeleton.tsx`:

```ts
import Card from "./Card";

const GreetingsSkeleton = () => {
  return (
    <Card className="w-full py-14">
      <div className="animate-pulse flex space-x-4">
        <div className="rounded-full bg-gray-300 h-10 w-10"></div>
        <div className="flex-1 space-y-6 py-1">
          <div className="h-2 bg-gray-300 rounded"></div>
          <div className="space-y-3">
            <div className="grid grid-cols-3 gap-4">
              <div className="h-2 bg-gray-300 rounded col-span-2"></div>
              <div className="h-2 bg-gray-300 rounded col-span-1"></div>
            </div>
            <div className="h-2 bg-gray-300 rounded"></div>
          </div>
        </div>
      </div>
    </Card>
  );
};

export default GreetingsSkeleton;
```

Using `<Suspense>`, we can show this loader while the greetings component is loading in the dashboard home page

```ts
<Suspense fallback={<GreetingsSkeleton />}>
  <Greetings />
</Suspense>
```

> ✔️ Code Checkpoint: The current code for the application can be found on the [middleware branch](https://github.com/Hendrixer/fullstack-app-v2-app/tree/middleware).
