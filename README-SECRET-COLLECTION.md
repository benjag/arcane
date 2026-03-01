# Secret Collection & Restricted Product — Admin Guide

Purpose
- Server-side Liquid gating for collections and products using a metafield value and a customer tag.
- Collection or product with metafield custom.private_access = "cheech" will be hidden from unauthorized visitors; only customers tagged `cheech` can view.

Quick setup (admin)
1. Create metafield definition (one-time)
   - Admin → Settings → Custom data → Collections → Add definition
   - Namespace: `custom`  Key: `private_access`  Type: single line text
   - Repeat for Products if you plan to set product-level restrictions.

2. Mark a collection as secret
   - Admin → Products → Collections → open the collection
   - In Custom data / Metafields set `custom.private_access` = `cheech` and save

3. (Optional) Mark individual products
   - Admin → Products → open a product → set `custom.private_access` = `cheech`
   - Use when you need product-level restrictions independent of collection

4. Tag authorized customers
   - Admin → Customers → open customer record → add tag `cheech`
   - Bulk tag via CSV or an app for many customers

5. (Optional) Header link
   - Online Store → Themes → Customize → Header section → set the secret collection URL (e.g. `/collections/secret-handle`)
   - Theme will only render the link for authorized customers (UI convenience)

How it works (technical summary)
- Collection template now checks `collection.metafields.custom.private_access == 'cheech'` (or uses the metafield `.value`).
- Product template checks `product.metafields.custom.private_access == 'cheech'` OR any parent collection metafield.
- If restricted and user is not logged in with tag `cheech`, templates render a friendly restricted message and a `<meta name="robots" content="noindex">`.
- Authorized customers (tagged) see full content.

Testing checklist
- Anonymous user: secret collection/product → restricted message (no products/content)
- Logged-in without tag: same restricted behavior
- Logged-in with `cheech` tag: full collection and product content visible; header link visible
- Test search, product direct URLs, and theme editor preview

Admin notes & caveats
- This is server-side gating (Liquid) — it prevents HTML from being rendered to unauthorized visitors. Good for basic access control.
- Hiding links in the header is NOT access control; always rely on the template guards.
- For stronger/accessible features (time-limited access, invite links, advanced rules) use a dedicated app (e.g., Locksmith).
- Keep tag name and metafield value consistent (`cheech`); avoid extra spaces/case mismatches.

Localization & assets
- If you customize the restricted messages, add matching keys to locales/en.default.json (keys used in templates: `collections.restricted.*`, `products.restricted.*`).
- Ensure any icons or snippets referenced (icon-tick, icon-close) exist in `snippets/`.

Troubleshooting
- Authorized user still restricted: ensure the customer record has tag `cheech` and user is logged in.
- Collection still visible to anonymous: confirm `custom.private_access` metafield is set correctly and templates deployed.
- Missing snippets or locale keys will cause template errors; add minimal snippets/locales if needed.

Admin copy/paste checklist
- Metafield key: `custom.private_access` = `cheech` (collection or product)
- Customer tag: `cheech`
- Test: anonymous / logged-in without tag / logged-in with tag

Contact
- Add instructions here for your team on who to contact for help or which internal process to follow when granting access.

