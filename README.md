# Repro steps

Clone this repo

Run `vercel dev`
Open up `localhost:3000` in your browser

You'll now notice that inside the `vercel dev` terminal that it gives a ts error after you load the page
If you run `npx tsc` - it won't give an error, because it's a newer version of typescript.

# Suggested fixes

I find it unusual that `vercel dev` is running typescript at API execution time. I would expect
it to
- Run the version of TS in my repo's package.json (not the version bundled with vercel)
- Run typescript on save (rather than after API execution)

Until these are fixed, I'd prefer it not to run typescript at all as it spams the terminal.

# Steps to create this repository

```bash
npm create vite@latest
```
Select React + Typescript

```bash
npm add @vercel/node
```

Add `api/hello.ts` (as in this repo). Notably the reference to Typescript 4.5 feature `Awaited`
```typescript
import type { VercelRequest, VercelResponse } from "@vercel/node";

export default async (request: VercelRequest, response: VercelResponse) => {
  response.status(200).send(await hello());
};

async function hello(): Promise<Awaited<string>> {
  return `Hello World!`;
}
```

Stick hello world API call in App.tsx
```typescript
// Hello World.
fetch("/api/hello")
  .then(response => response.text())
  .then(text => console.log(text))
  .catch(error => console.log(error));
```

