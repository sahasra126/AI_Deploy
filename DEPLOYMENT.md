# Deploying to Render and Vercel

Deploy the backend first, then deploy the frontend.

## 1. Push This Folder to GitHub

Use `AI-DIGITAL-FOOTPRINT-main` as the repository root. It now contains:

- `render.yaml` for the FastAPI backend on Render
- `vercel.json` for the Vite frontend on Vercel
- `frontend/.env.example` showing the frontend API variable

## 2. Deploy Backend on Render

1. Open Render and choose **New +** > **Blueprint**.
2. Connect your GitHub repository.
3. Select the repository root that contains `render.yaml`.
4. Render will create `ai-digital-footprint-backend`.
5. Add these environment variables in Render:

```text
MONGODB_URL=mongodb+srv://...
MONGODB_DB_NAME=privacy_analyzer
OPENAI_API_KEY=...
GEMINI_API_KEY=...
GITHUB_TOKEN=...
DEBUG=false
SPACY_MODEL=en_core_web_sm
```

`OPENAI_API_KEY`, `GEMINI_API_KEY`, and `GITHUB_TOKEN` are optional unless you want those features enabled. `SECRET_KEY` is generated automatically by `render.yaml`.

After Render deploys, copy your backend URL. It will look like:

```text
https://ai-digital-footprint-backend.onrender.com
```

Test it in the browser:

```text
https://ai-digital-footprint-backend.onrender.com/health
```

## 3. Deploy Frontend on Vercel

1. Open Vercel and choose **Add New** > **Project**.
2. Import the same GitHub repository.
3. Keep the root as the repository root.
4. Vercel will use `vercel.json`.
5. Add this environment variable:

```text
VITE_API_URL=https://ai-digital-footprint-backend.onrender.com/api
```

Replace the URL with your actual Render backend URL.

Deploy the project. After Vercel gives you a URL, copy it.

## 4. Update Render CORS

Go back to the Render backend environment variables and set:

```text
ALLOWED_ORIGINS=["https://your-vercel-project.vercel.app","http://localhost:5173"]
```

Replace `https://your-vercel-project.vercel.app` with your real Vercel frontend URL.

Redeploy the Render backend after changing this variable.

## 5. Final Test

Open your Vercel URL and test:

- Register/login
- Analyze text
- Privacy chat, if an OpenAI or Gemini key is configured
- GitHub scan, if a GitHub token is configured

If the backend wakes slowly on Render free tier, the first request may take a minute.
