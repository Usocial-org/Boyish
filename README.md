# Boyish

A self-hosted media nest: an admin panel to embed videos, drop in picture URLs (one or many at once),
write blog posts, and save weblinks — with a public site where visitors browse everything in four
sections: Pictures, Videos, Blog, and Links.

## Running locally

```bash
npm install
ADMIN_PASSWORD=yourpassword npm start
```

Then open http://localhost:3000. Click the gear-like icon in the top right to sign in as admin.

## Deploying on Render

1. Push this folder to a GitHub repo.
2. On Render: **New → Web Service**, connect the repo.
3. Build command: `npm install`
4. Start command: `npm start`
5. Add environment variables (Render dashboard → Environment):
  - `ADMIN_PASSWORD` — required only on the first start. It is hashed into `data/admin-auth.json`; change it later from the admin panel's Security tab.
   - `PEXELS_API_KEY` — optional, enables the "Fetch pictures" AI tool (free key at pexels.com/api).
   - `YOUTUBE_API_KEY` — optional, enables the "Fetch videos" AI tool (free key via Google Cloud Console, enable "YouTube Data API v3").
6. Deploy. Render gives you a URL for your Boyish site.

## How content storage works

Content (pictures, videos, blog posts, links) is stored in `data/content.json` on the server —
not in the visitor's browser — so everything you add in the admin panel is visible to every visitor.

One thing to know about Render's **free** tier: its disk is not persistent across deploys/restarts,
so `data/content.json` can reset when the service restarts. If you want content to survive restarts:
- Upgrade to a Render plan with a persistent disk and mount it at `/data`, then point `DATA_FILE`
  at that path (small code change in `server.js`), **or**
- Swap the JSON file for a real database later (Render's free Postgres works well) — ask me if you
  want that done.

## Adding content

- **Embed video**: paste a Vimeo or YouTube link (or a full `<iframe>` embed code) — Boyish figures out
  how to play it.
- **Add pictures**: paste one image URL, or several separated by new lines or commas, to add them all
  at once.
- **Write post**: title + body, published immediately.
- **Add link**: URL, title, and an optional description — shown to visitors as a card with a "Visit" button.
- **AI fetch**: search Pexels for pictures or YouTube for videos by keyword, preview the results, and
  add the ones you want with one click. Needs the optional API keys above.

## Admin access

There's a single admin login — no visitor accounts, since only you manage content. Change the password any
time from the Security tab in the admin panel.
