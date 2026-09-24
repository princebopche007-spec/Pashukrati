# Pashukrati website

Plain HTML/CSS/JS (no build step) + Supabase (login, database, storage, payment functions) + Razorpay (payments).

## Setup
1. **Supabase**: create a project → SQL Editor → run `supabase/migrations/001_init.sql`.
2. **Config**: put your Project URL and anon key in `js/config.js`.
3. **Auth**: Supabase → Authentication → URL Configuration → set Site URL to your live site (and add `http://localhost:3000` for testing). For Google login, enable the Google provider.
4. **Razorpay** (start in Test mode): copy Key ID + Key Secret.
5. **Functions**: install the Supabase CLI, then
   ```
   supabase login
   supabase link --project-ref YOUR_PROJECT_REF
   supabase secrets set RAZORPAY_KEY_ID=rzp_test_xxx RAZORPAY_KEY_SECRET=xxx RAZORPAY_WEBHOOK_SECRET=choose-a-long-random-string
   supabase functions deploy create-order
   supabase functions deploy verify-payment
   supabase functions deploy razorpay-webhook --no-verify-jwt
   ```
6. **Webhook**: Razorpay Dashboard → Settings → Webhooks → URL `https://YOUR_PROJECT_REF.supabase.co/functions/v1/razorpay-webhook`, same secret as above, events `payment.captured`, `order.paid`, `payment.failed`.
7. **Admin**: sign up on the site, then run the commented statement at the bottom of the SQL file with your email.
8. **Host**: connect this repo to Vercel, Netlify or Cloudflare Pages (no build command, output directory = repo root).

Never commit the service_role key or the Razorpay secrets.
