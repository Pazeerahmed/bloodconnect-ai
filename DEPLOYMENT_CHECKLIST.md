# BloodConnect AI — deployment checklist

- [ ] Change GitHub repository visibility to **PUBLIC** before submission.
- [ ] Create MongoDB Atlas free cluster and database user.
- [ ] Configure Atlas Network Access; create a unique index on `users.email`.
- [ ] Configure Cloudflare `MONGODB_URI` and `JWT_SECRET` as Worker **secrets**.
- [ ] Check Cloudflare Workers + MongoDB Atlas driver compatibility in the deployed runtime.
- [ ] Verify Workers AI binding and currently available model.
- [ ] Deploy backend by Thursday November 26, 2026.
- [ ] Import `client/` on Vercel and configure `VITE_API_URL`.
- [ ] Update `FRONTEND_URL` in Worker config to match Vercel production origin.
- [ ] Test full signup → login → donor CRUD → request CRUD → AI → logout journey **online**.
- [ ] Test mobile responsiveness.
- [ ] Add genuine screenshots and live URLs to README.
- [ ] Submit all four links/files before November 29, 2026.
