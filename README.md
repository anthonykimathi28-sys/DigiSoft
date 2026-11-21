# DigiSoft E-Learning: setup

## Files
- `index.html`: the whole website (no build step)
- `config.js`: your Supabase URL and anon key, prices and payment instructions
- `netlify.toml`: Netlify static-site publish directory and fallback routing

## 1. Connect Supabase
1. Run `digisoft_supabase_schema.sql` in Supabase SQL Editor (once). **That schema file is not currently present in this repository**, so obtain/restore the matching schema before expecting profiles, roles, and admin SQL to work. Do not substitute an unreviewed schema on a live project.
2. Project Settings > API: copy **Project URL** and **anon public key** into `config.js`.
3. In Supabase, open **Authentication > URL Configuration**:
   - Set **Site URL** to the exact production Netlify origin, for example `https://your-site.netlify.app` (no path or trailing slash).
   - Add the production origin and any development/preview origins you actually use to **Redirect URLs** (for example `https://your-site.netlify.app/**`). Supabase requires the redirect destination to be allow-listed.
   - The registration code does not specify `emailRedirectTo`, so Supabase falls back to **Site URL**. If that value is still localhost, an old domain, or a Netlify deploy-preview address that is not allow-listed, confirmation links can send users to the wrong place or be rejected. Update these settings after changing domains.
4. Check **Authentication > Email Templates > Confirm signup**. Keep the confirmation link pointed at Supabase's `{{ .ConfirmationURL }}` so Supabase can verify the token; the verification then redirects to the configured Site URL. Do not replace it with a raw link to the app, which would skip verification.
5. Deploy the site, register using an email you can access, open the newest confirmation email, and then log in. Check spam and Supabase Auth logs if the email is not delivered. Supabase email confirmation must be enabled/configured for the project.
6. Promote your own account only after signup and confirmation created a row in `auth.users` and the schema's profile trigger created a matching `public.profiles` row. In Supabase SQL Editor, replace the example email with your own:
   ```sql
   update public.profiles
   set role = 'admin'
   where id = (select id from auth.users where email = 'you@example.com');
   ```
   Verify the affected-row count and then log out and back in so the app reloads the profile role. Never expose a Supabase `service_role` key in this static site; keep admin promotion in the trusted SQL editor.

## 2. Host it (free)
- **Netlify**: deploy the repository root (or drag the site folder onto app.netlify.com/drop).
- or **Vercel / Cloudflare Pages / GitHub Pages**: upload the site files.
Add a domain (e.g. digisoft.co.ke) in the host's domain settings, and add that exact origin to Supabase's Redirect URLs.

## 3. Run it day to day (Admin page)
- Add course, publish it.
- Add material: upload a PDF/document, or paste an unlisted Vimeo/YouTube link.
- Learners start a free trial themselves. For paid plans, confirm the M-Pesa payment, then use **Grant access**.
- Approve Organization sign-ups under Organizations.

## Not included yet
Automatic M-Pesa/card payments (needs a Supabase Edge Function plus a Daraja or Pesapal account), quizzes, and an organization dashboard for managing many learners.
