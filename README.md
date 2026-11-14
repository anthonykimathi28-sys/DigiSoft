# DigiSoft E-Learning: setup

## Files
- `index.html`: the whole website (no build step)
- `config.js`: your Supabase URL and anon key, prices and payment instructions

## 1. Connect Supabase
1. Run `digisoft_supabase_schema.sql` in Supabase SQL Editor (once).
2. Project Settings > API: copy **Project URL** and **anon public key** into `config.js`.
3. Authentication > URL Configuration: set **Site URL** to your live website address.
4. Register on the site, then promote yourself (SQL Editor):
   `update public.profiles set role='admin' where id=(select id from auth.users where email='you@example.com');`

## 2. Host it (free)
- **Netlify**: drag the `digisoft-site` folder onto app.netlify.com/drop
- or **Vercel / Cloudflare Pages / GitHub Pages**: upload the folder.
Add a domain (e.g. digisoft.co.ke) in the host's domain settings.

## 3. Run it day to day (Admin page)
- Add course, publish it.
- Add material: upload a PDF/document, or paste an unlisted Vimeo/YouTube link.
- Learners start a free trial themselves. For paid plans, confirm the M-Pesa payment, then use **Grant access**.
- Approve Organization sign-ups under Organizations.

## Not included yet
Automatic M-Pesa/card payments (needs a Supabase Edge Function plus a Daraja or Pesapal account), quizzes, and an organization dashboard for managing many learners.
