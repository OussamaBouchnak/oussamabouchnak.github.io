# Blog — How to Use

This is a [Jekyll](https://jekyllrb.com) site hosted for free on
[GitHub Pages](https://pages.github.com). This file is only for you —
it is excluded from the published site (see `_config.yml`).

## How posts work

Every post is a Markdown file inside the `_posts/` folder. The filename
**must** follow this pattern, otherwise Jekyll silently ignores it and
it never shows up on the page:

```
_posts/YYYY-MM-DD-short-title.md
```

Example:

```
_posts/2026-07-08-berufe.md
```

The date in the filename (and the `date:` line) controls when it is
published and how it is sorted on the homepage/blog. Only posts dated
today or earlier appear.

## Adding a new post

1. Create a file in `_posts/` named like `2026-07-08-my-title.md`
   (use a date, lowercase letters, hyphens instead of spaces).
2. Start the file with front matter:

   ```yaml
   ---
   layout: post
   title: "My Post Title"
   date: 2026-07-08
   category: "German"
   excerpt: "A short summary shown on the homepage"
   ---
   ```

   - `layout` is always `post`.
   - `title` is what readers see (it may contain spaces, umlauts, etc.).
   - `date` should match the date in the filename.
   - `category` and `excerpt` are optional but recommended.

3. Write the content below the front matter in normal Markdown.
4. Each post gets its own page at a URL like
   `https://oussamabouchnak.github.io/2026/07/my-title/`.

## Linking between posts

Use a normal Markdown link with the post's URL (no `[[...]]`
Obsidian links — those do not work on GitHub Pages):

```markdown
[Berufe](/2026/07/berufe/)
```

## Previewing locally (optional)

If you have Ruby installed:

```
bundle install
bundle exec jekyll serve
```

Then open http://localhost:4000.

## Publishing

1. Commit your changes.
2. Push to GitHub:

   ```
   git add .
   git commit -m "Add a new post"
   git push
   ```

3. GitHub Pages builds the site automatically (it takes ~1 minute).
   Check the **Actions** tab of the repo if the page does not update.

## Troubleshooting

- **Post not showing up?** The filename must start with `YYYY-MM-DD-`
  and the file must be in `_posts/`. Double-check there are no typos.
- **Date in the future?** The post is hidden until that date passes.
- **Umlauts in filenames?** Avoid them in filenames; keep them in the
  `title:` instead (it keeps URLs clean).
- **Broken links?** Only link to posts that actually exist. Use the URL
  pattern `/YYYY/MM/your-slug/`.
