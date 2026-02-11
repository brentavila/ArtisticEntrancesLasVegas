# reCAPTCHA – Server-Side Verification

The contact form (index.html) and promo form (wrought-iron-promo.html) both use **reCAPTCHA v3** (invisible). On submit, the frontend gets a token and sends it in the hidden field `g-recaptcha-response`.

## Frontend (already done)

- The script loads with `?render=YOUR_SITE_KEY`. The site key is in the HTML/JS.
- On submit, `grecaptcha.execute()` gets a token and sets it on a hidden input; your server receives it with the form.

## Server-side (your backend)

When you process the form (e.g. GoHighLevel webhook, Formspree, or your own API):

1. Read the `g-recaptcha-response` value from the submitted form.
2. Verify it with Google’s API using your **secret key**:

   ```
   POST https://www.google.com/recaptcha/api/siteverify
   Content-Type: application/x-www-form-urlencoded

   secret=YOUR_SECRET_KEY&response=TOKEN_FROM_FORM
   ```

3. Use the secret key from your reCAPTCHA admin console. A copy is stored locally in `recaptcha-secret.txt` (that file is in `.gitignore` and must **never** be committed or exposed in client code).

4. Only accept the form submission if the verify response contains `"success": true`. For v3, the response also includes a `"score"` (0.0–1.0); you can require a minimum score (e.g. ≥ 0.5) to block likely bots.

Documentation: https://developers.google.com/recaptcha/docs/verify
