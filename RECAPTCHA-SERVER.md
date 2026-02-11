# reCAPTCHA – Server-Side Verification

The contact form (index.html) and promo form (wrought-iron-promo.html) both use reCAPTCHA v2. The widget is loaded with a **site key** in the HTML. On submit, the form includes a `g-recaptcha-response` token.

## Frontend (already done)

- Each form has a reCAPTCHA widget with `data-sitekey="YOUR_RECAPTCHA_SITE_KEY"`.
- **Replace `YOUR_RECAPTCHA_SITE_KEY`** in both `index.html` and `wrought-iron-promo.html` with your **reCAPTCHA site key** from the [Google reCAPTCHA admin console](https://www.google.com/recaptcha/admin). The site key is safe to use in the browser.

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

4. Only accept the form submission if the verify response contains `"success": true`.

Documentation: https://developers.google.com/recaptcha/docs/verify
