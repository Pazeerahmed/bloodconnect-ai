# 🩸 BloodConnect AI — Blood Donor Finder

An individual MERN + AI final project connecting willing blood donors with people looking for help in their city.

**Deadline:** November 29, 2026

## Problem
People can struggle to find voluntary blood donors nearby, especially when they need assistance quickly. Messages in social groups are difficult to search and contact information may be outdated.

## Target audience
Patients' families, volunteer blood donors and local communities.

## Solution and features
- Signup/login with hashed passwords and signed JWTs
- Donor registration, viewing, editing and deleting (including consent and availability)
- Search public, opted-in donors by blood group and city
- Create/read/update/delete blood requests
- **One AI feature:** generate a shareable, compassionate blood request message
- Responsive React UI, Zod validation and helpful error responses
- Ownership checks: users may only edit and delete their own records

## Tech stack
React + Vite frontend, Hono + Node.js tooling on Cloudflare Workers backend, MongoDB Atlas, bcryptjs, jose JWT, Zod, Cloudflare Workers AI, Vercel and GitHub.

**Database collections:** `users` (authentication + donor profile) and `requests`. Combining donor fields into the user document keeps the app at **two collections** while supporting full donor-profile and request CRUD.

## Six screens
Home / find donors; Login; Signup; Dashboard; Become a Donor / donor profile; Blood Requests + AI generator.

## AI feature
A logged-in user fills in blood group, city, hospital, requested units and urgency, then clicks **Generate AI message**. The frontend calls `POST /api/ai/generate`. The Worker validates the input and calls Cloudflare Workers AI to draft a concise appeal. It does not assess blood compatibility, eligibility or any other medical question.

## API endpoints
| Method | Route | Description |
|---|---|---|
| GET | `/api/health` | Health check |
| POST | `/api/auth/signup` | Signup |
| POST | `/api/auth/login` | JWT login |
| GET | `/api/me` | Own account |
| GET | `/api/donors` | Search donors |
| GET | `/api/donors/:id` | View donor |
| POST | `/api/me/donor` | Create donor profile |
| PUT | `/api/me/donor` | Edit donor profile |
| DELETE | `/api/me/donor` | Remove donor profile |
| GET | `/api/requests` | Public open requests |
| GET | `/api/requests/mine` | Own requests |
| GET | `/api/requests/:id` | Single request |
| POST | `/api/requests` | Create request |
| PUT | `/api/requests/:id` | Edit owned request |
| DELETE | `/api/requests/:id` | Delete owned request |
| POST | `/api/ai/generate` | AI request message |

## Run locally
Requirements: Node.js LTS, MongoDB Atlas cluster, Cloudflare account.

**Backend**
```bash
cd server
npm install
cp .dev.vars.example .dev.vars
# Fill .dev.vars with a real MONGODB_URI and random JWT_SECRET (32+ characters)
npm run dev
```
Backend URL: http://localhost:8787; verify http://localhost:8787/api/health.

**Frontend** (separate terminal)
```bash
cd client
npm install
cp .env.example .env
npm run dev
```
Frontend URL: http://localhost:5173.

In MongoDB Atlas, create database user, configure Network Access and create a unique index for users.email, e.g. `db.users.createIndex({email:1},{unique:true})`. Database `bloodconnect` and collections appear when records are first inserted.

## Deploy backend (Cloudflare Workers)
Check your MongoDB driver and Workers compatibility on a real deployed Worker. The app uses the `nodejs_compat` flag but actual MongoDB connectivity must be tested.

```bash
cd server
npx wrangler login
npx wrangler secret put MONGODB_URI
npx wrangler secret put JWT_SECRET
npx wrangler deploy
```
Set `FRONTEND_URL` in `wrangler.toml` to the exact deployed Vercel origin and redeploy. Never commit real secrets. Confirm the Workers AI binding and model remain available.

## Deploy frontend (Vercel)
Import this GitHub repository, set **Root Directory** to `client`, build command `npm run build`, output directory `dist`, and environment variable `VITE_API_URL` to your real Worker URL. Deploy again after environment updates. Test signup, login, both CRUD resources, AI, phone display and logout on the **live** site.

## Screenshots
Add screenshots from your **deployed** app before submission:
- Home and donor search
- Signup/login
- Donor profile
- Blood requests
- AI-generated appeal

## Deployment links
- **GitHub:** https://github.com/Pazeerahmed/bloodconnect-ai
- **Frontend (Vercel):** _Pending deployment_
- **Backend (Cloudflare Workers):** _Pending deployment_

## Viva points
**JWT:** signed login token verified by auth middleware. **bcryptjs:** hashes passwords instead of saving plaintext. **Zod:** validates user input. **CRUD:** create, read, update, delete donor profiles and blood requests. **Ownership:** authenticated user ID limits edits/deletes. **AI:** one Worker route drafts donation request text. **CORS:** restricts browser origins to local frontend and configured live frontend.

## Safety, privacy and limitations
Donors opt in before their name, blood group, city and phone become searchable. This student app is a contact directory, not a medical service. **Donation eligibility, medical compatibility, screening and hospital coordination must be determined by qualified healthcare professionals.** Never share private patient medical records or guarantee donor availability.