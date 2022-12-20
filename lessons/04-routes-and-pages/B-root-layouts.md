Both of our layouts use a glass pane UI component that we have yet to make. Let's create that in the components dir.

```tsx
import clsx from "clsx";

const GlassPane = ({ children, className }) => {
  return (
    <div
      className={clsx(
        "glass rounded-2xl border-solid border-2 border-gray-200",
        className
      )}
    >
      {children}
    </div>
  );
};

export default GlassPane;
```

Now for our Auth root layout

```tsx
import "@/styles/global.css";
import { Inter } from "@next/font/google";

const inter = Inter({
  variable: "--font-inter",
});

export default function AuthRootLayout({ children }) {
  return (
    <html lang="en" className={inter.variable}>
      <head />
      <body className="h-screen w-screen rainbow-mesh p-6">
        <GlassPane className="w-full h-full flex items-center justify-center">
          {children}
        </GlassPane>
      </body>
    </html>
  );
}
```

And our Dashboard root layout

```tsx
import "@/styles/global.css";
import { Inter } from "@next/font/google";
import Sidebar from "@/components/Sidebar";
import clsx from "clsx";
import GlassPane from "@/components/GlassPane";

const inter = Inter({
  variable: "--font-inter",
});

export default function DashboardRootLayout({ children }) {
  return (
    <html lang="en" className={clsx(inter.variable, "dark")}>
      <head />
      <body className="h-screen w-screen candy-mesh p-6">
        <GlassPane className="w-full h-full p-6 flex align-center container mx-auto">
          <Sidebar />
          <main className="w-full pl-6 h-full">{children}</main>
        </GlassPane>
      </body>
    </html>
  );
}
```
