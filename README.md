# Gold Price Calculator

Simple Vue 3 + Vite app that requires user registration before using the calculator.

Install and run:

```bash
npm install
npm run dev
```

Usage:
- Open the app in the browser (Vite dev server will show the URL).
- Register with a name and email — registration is stored in `localStorage`.
- After registration you'll be redirected to the calculator.

Calculation formula used:

Subtotal = Grams × Gold Rate + Make charge
Tax = 12% of Subtotal
Total = Subtotal + Tax

If you want, I can install dependencies and run the dev server for you, or add `vue-router` for proper routing.
