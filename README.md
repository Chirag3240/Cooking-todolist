# 🍳 Cooking To-Do — AI Daily Meal Planner

Built for **PromptWars** (Google for Developers H2S · Build with AI)

A simple AI micro-app that helps a user generate a personal cooking to-do list based on their day. The user tells it how much time they have, their budget, their diet preference, and household size — the app returns:

- A structured **Breakfast / Lunch / Dinner plan**
- A **grocery list** (scaled to household size)
- **Substitutions** for cheaper or easier alternatives
- **Budget feasibility logic** (feasible / tight / over budget, with estimated cost)

The AI logic is powered by the **Claude API** (Anthropic), called securely from a Vercel serverless function — the API key never touches the browser.

---

## 🗂️ Project structure

```
cooking-todo-app/
├── api/
│   └── generate-plan.js     ← Vercel serverless function (calls Claude API)
├── public/
│   ├── index.html           ← Main page / form
│   ├── style.css            ← Styling
│   └── app.js                ← Frontend logic (calls /api/generate-plan)
├── package.json
├── vercel.json
├── .gitignore
├── .env.example
└── README.md
```

---

## 🚀 Step 1 — Get a Claude API key

1. Go to https://console.anthropic.com/
2. Sign up / log in
3. Go to **API Keys** → **Create Key**
4. Copy the key (starts with `sk-ant-...`). Keep it secret — don't paste it into any file you commit.

---

## 🐙 Step 2 — Upload this project to GitHub

If you already have these files locally:

```bash
cd cooking-todo-app
git init
git add .
git commit -m "Initial commit: Cooking To-Do AI micro-app"
```

Create a new repo on GitHub (via the GitHub website: **New Repository** → don't initialize with a README since you already have one), then:

```bash
git remote add origin https://github.com/<your-username>/<your-repo-name>.git
git branch -M main
git push -u origin main
```

> ✅ Your `.gitignore` already excludes `.env` and `.env.local`, so your API key will **not** be pushed to GitHub. Never commit your real key.

---

## ▲ Step 3 — Deploy to Vercel

1. Go to https://vercel.com and log in (you can sign in with your GitHub account directly).
2. Click **Add New → Project**.
3. Select the GitHub repo you just pushed.
4. Vercel will auto-detect it as a generic project — that's fine, no special build settings needed (no build command required, output is the `public/` folder + `api/` functions).
5. **Before clicking Deploy**, open **Environment Variables** and add:
   - **Key:** `ANTHROPIC_API_KEY`
   - **Value:** your real key from Step 1
   - Apply to: Production, Preview, and Development
6. Click **Deploy**.

Vercel will build and give you a live URL like:
```
https://cooking-todo-app.vercel.app
```

That's it — it's live. 🎉

---

## 🧪 Step 4 — Test it

Open your Vercel URL, fill in:
- Time available
- Budget (pick currency + amount)
- Diet preference
- Number of people

Click **Generate My Cooking Plan**. Within a few seconds you should see the meal plan, grocery list, substitutions, and budget verdict.

---

## 🛠️ Local development (optional)

If you want to test locally before pushing:

```bash
npm install -g vercel
cp .env.example .env.local
# edit .env.local and paste your real API key
vercel dev
```

This runs the same serverless function locally at `http://localhost:3000`.

---

## 🔒 Security notes

- The Claude API key lives **only** as a Vercel environment variable — it's never sent to the browser or committed to GitHub.
- `/api/generate-plan.js` runs server-side only (Vercel serverless function).
- If you ever accidentally commit a real key, revoke it immediately in the Anthropic console and generate a new one.

---

## 🧩 How it works (architecture)

1. User fills the form in `public/index.html`.
2. `public/app.js` sends a POST request to `/api/generate-plan` with the user's inputs.
3. `api/generate-plan.js` (running on Vercel's serverless infrastructure) builds a prompt and calls the Claude API with the secret key from environment variables, instructing the model to return **strict JSON only**.
4. The function parses and returns that JSON to the browser.
5. `app.js` renders it into the meal plan, grocery list, substitutions, and budget feasibility sections.

---

## 📋 Challenge brief (for reference)

> Build a simple AI micro-app that helps a user generate a personal cooking to-do list based on their day.
> - A structured meal planning flow that produces:
>   - Breakfast/Lunch/Dinner plan
>   - Grocery list
>   - Substitutions
>   - Budget feasibility logic
