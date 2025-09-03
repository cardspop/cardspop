# Minimal GitHub Pages Blog (Jekyll + Minima)

This is a dead-simple blog that builds on GitHub Pages using Jekyll's built-in **minima** theme. Write posts in Markdown; push to `main`; the site updates automatically.

This copy is configured for a User Page.
It will be served at `https://cardspop.github.io/` once Pages is enabled.

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

## Deploy to GitHub Pages (User Page)

1. Create a repository named `cardspop.github.io` (public).
2. Push the site to the `main` branch of that repo.
3. In GitHub: Settings → Pages → Build and deployment → Deploy from a branch → Branch: `main`, Folder: `/ (root)`.
4. Visit `https://cardspop.github.io/` after a minute or two.

## Preview locally

Option A — Ruby/Jekyll (matches GitHub Pages)

1. Install Ruby (3.x) and Bundler.
2. In this folder, run:
   ```bash
   bundle install
   bundle exec jekyll serve --livereload
   ```
3. Open: http://127.0.0.1:4000/

Option B — Docker (no Ruby on your machine)

```bash
docker run --rm -it \
  -p 4000:4000 \
  -v "$PWD":/srv/jekyll \
  -w /srv/jekyll \
  jekyll/jekyll:4 \
  sh -lc "bundle install && jekyll serve --host 0.0.0.0 --livereload"
```
Then open: http://127.0.0.1:4000/

Optional: add a `CNAME` file with your custom domain (e.g., `blog.yourdomain.com`) and configure Pages to use it.
