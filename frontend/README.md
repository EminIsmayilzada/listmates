# ListMates frontend

React + TypeScript (Vite), Tailwind CSS, shadcn/ui, TanStack Query, React Router and the BlockNote editor.

This folder is created in the "Set up the frontend app" issue:

```bash
npm create vite@latest . -- --template react-ts
npm install
npm install react-router-dom @tanstack/react-query @blocknote/core @blocknote/react @blocknote/mantine
npx shadcn@latest init
```

During development, `vite.config.ts` proxies `/api` to `http://localhost:8000`, so the login cookie works without CORS problems.
