# Minimal GitHub Pages Blog (Jekyll + Minima)

This is a dead-simple blog that builds on GitHub Pages using Jekyll's built-in **minima** theme. Write posts in Markdown; push to `main`; the site updates automatically.

This copy is configured for a Project Page.
It will be served at `https://cardspop.github.io/cardspop/` once Pages is enabled.

## Structure

- `_config.yml` — site settings (title, theme, plugins)
- `index.md` — homepage listing posts
- `about.md` — simple about page
- `_posts/` — your blog posts (`YYYY-MM-DD-title.md`)
- `assets/styles.css` — small optional CSS tweaks

## Add a post

1. Create a file in `_posts/` named like `2025-01-15-my-post.md`.
2. Start with:
   ```yaml
   ---
   layout: post
   title: My Post Title
   ---
   ```
3. Write Markdown below the front matter. Commit and push.

## Heavy resources (simple approach)

- Video: upload to YouTube (public/unlisted) and embed the player.
- Big downloads: attach to a GitHub **Release** and link to it.
- Images: keep in `assets/`, add `loading="lazy"` where helpful.

## Deploy to GitHub Pages (Project Page)

1. Create or use the repository `cardspop/cardspop` (public).
2. Push the site to the `main` branch of that repo.
3. In GitHub: Settings → Pages → Build and deployment → Deploy from a branch → Branch: `main`, Folder: `/ (root)`.
4. Visit `https://cardspop.github.io/cardspop/` after a minute or two.

Optional: add a `CNAME` file with your custom domain (e.g., `blog.yourdomain.com`) and configure Pages to use it.
