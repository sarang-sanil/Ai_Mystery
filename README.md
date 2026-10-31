<<<<<<< HEAD
# AI-Mystery
Created using codex
=======
# Casefile — AI Mystery Room

An AI powered investigation game. Create a fictional case from a premise, inspect evidence, use a limited number of hints, and submit one final accusation. The Express API is authoritative for case state and scoring; Gemini credentials and the culprit remain on the server.

## Stack

- React 18, Vite, React Router, Tailwind CSS
- Node.js, Express, Mongoose, MongoDB
- Google Gemini through the official `@google/genai` SDK
- Zod request and generated-case validation
- Vitest and Supertest for backend unit and API tests

## Requirements

- Node.js 20.19+ or 22.12+
- MongoDB local or hosted connection string
- Google Gemini API key for AI case generation

## Local development

1. Copy the root `.env.example` to `server/.env`.
2. Set `MONGODB_URI` and `GEMINI_API_KEY` in `server/.env`. The server loads this file directly, independent of the terminal's working directory. Keep it private; it is ignored by Git.
3. Install dependencies with `npm install`.
4. Start MongoDB, then run `npm run dev`.
5. Open <http://localhost:5173>. The API health endpoint is <http://localhost:4000/api/health>.

The client uses `VITE_API_URL` (defaults to `http://localhost:4000/api`); if you need to change it, put that variable in `client/.env`. The server accepts `CLIENT_ORIGIN`, `PORT`, `MONGODB_URI`, `GEMINI_API_KEY`, and `GEMINI_MODEL` from `server/.env` or the process environment. The default Gemini model is `gemini-3.5-flash-lite`.

## Gameplay API

All endpoints use `/api` and JSON. `POST /cases` starts generation and returns a case ID. Poll `GET /cases/:id/status`; use `GET /cases/:id` when ready. Discover evidence with `POST /cases/:id/clues/:clueId/discover`, request the next hint with `POST /cases/:id/hints`, and submit one accusation with `POST /cases/:id/accuse`. The solution is available only through `GET /cases/:id/result` after completion.

The score is computed server side: `max(0, correct × base + unique clues × 5 − hints × 10)`. Difficulty configurations and counts are Easy 3/5/2/100, Medium 4/7/3/150, and Hard 5/9/3/200 (suspects/clues/hints/base score).

## Build and checks

- `npm run build` builds the static frontend to `client/dist`.
- `npm test` runs backend unit and API tests.
- `npm start` starts the API after MongoDB configuration is present.

## Deployment

The steps below use Render for the frontend and API, MongoDB Atlas for persistence, and a Gemini API key for case generation. The same settings apply on other hosts that provide a static site and a persistent Node.js web service.

### 1. Prepare MongoDB Atlas

1. Create an Atlas cluster and a database user with read/write access to the app database.
2. In **Network Access**, allow connections from your API host. For a first Render deployment, Atlas may require `0.0.0.0/0` because Render outbound IPs are not fixed on all plans; use a paid static outbound IP option and narrow the Atlas rule when available.
3. Copy the driver connection string. Replace its username, password, and database name, and URL-encode special characters in the password.

### 2. Create the API service

1. Push this repository to a Git provider and create a **Web Service** in Render from it. Set the root directory to the repository root.
2. Use build command `npm install` and start command `npm start`. The root package starts the API workspace; the service must be a persistent Node service, not a short-lived function, because generation runs after the request returns.
3. Add these environment variables in the service settings:
   - `MONGODB_URI`: your Atlas connection string
   - `GEMINI_API_KEY`: a newly issued Gemini key
   - `GEMINI_MODEL`: `gemini-3.5-flash-lite` (or a model enabled for your key)
   - `CLIENT_ORIGIN`: the exact public frontend origin, such as `https://your-app.onrender.com` (no trailing slash)
   - `NODE_ENV`: `production`
   - `PORT` is supplied by Render; leave it unset unless your host requires a fixed value.
4. Deploy and open `https://<your-api-host>/api/health`. Confirm the JSON reports `database: "connected"`.

### 3. Create the frontend static site

1. Create a **Static Site** in Render from the same repository, with the repository root as its root directory.
2. Set build command to `npm install && npm run build` and publish directory to `client/dist`.
3. Add `VITE_API_URL=https://<your-api-host>/api` as a frontend build environment variable, substituting your API's actual HTTPS origin. Vite embeds this value at build time, so set it before deployment and redeploy after changing it.
4. Deploy. Add the final frontend origin (including `https://`, without a trailing slash) to the API's `CLIENT_ORIGIN`, then redeploy the API.

### 4. Verify the deployment

Open the frontend, create a case, wait for generation, discover a clue, request a hint, and finish an accusation. If the browser reports a CORS error, check that `CLIENT_ORIGIN` exactly matches the frontend origin. If case creation or generation fails, inspect the API logs and verify Atlas network access, `MONGODB_URI`, and `GEMINI_API_KEY`. Keep secrets in the hosting provider's environment settings; never commit `.env` files. `.env.example` contains placeholders only.

### Deploying both parts on one host

The frontend and API can also use different hostnames on any provider. The API needs a persistent Node.js process, the frontend must build with `VITE_API_URL` set to the API's `/api` URL, and the API's `CLIENT_ORIGIN` must contain the frontend origin. Both services should use HTTPS.

This repository contains deployable application code and deployment instructions. A public URL requires hosting accounts, a MongoDB deployment, and a Gemini credential; those are deployment inputs and are not embedded in source control.
>>>>>>> 035aa6b (first commit)
