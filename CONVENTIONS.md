# Conventions — underhillnovels.com

How this site is built, so every new book arrives looking like the last one.
Read this before adding or changing anything.

> **This repository is public.** Everything in it, including this file and the
> full commit history, can be read by anyone. Nothing here may name, describe
> or hint at anyone other than Sharyn Underhill.

---

## 1. Identity rules (non-negotiable)

- **Author name:** always **Sharyn Underhill**, in bylines, `<meta name="author">`,
  footers, copyright lines and alt text. No other name appears anywhere.
- **Commits:** author must be `Sharyn Underhill <334414178+underhillsharyn-novels@users.noreply.github.com>`.
  Check with `git config user.email` before the first commit of a session.
- **Search every new file** for names, places, email addresses and links to other
  sites before committing. Manuscripts drafted elsewhere often carry a stray
  byline, `<meta name="author">` or copyright line.
- **Images must be cleaned before they enter `images/`** (see §5). Design-tool
  exports embed account, team and workspace identifiers.
- **Design tools:** before designing anything, confirm the tool is signed in to
  the pen-name account / workspace.
- **No photographs presented as the author.** The author portrait is a pencil
  illustration and stays one.

---

## 2. Site structure

```
index.html                  Home: plum hero with fanned covers, novel cards, pull quote, About, Write band
books/<slug>.html           One self-contained page per book
images/<slug>-cover.jpg     Book cover (full size)
images/<slug>-cover-600.jpg 600px-wide sharpened copy for the home page
images/<slug>-share.jpg     Link-preview card for that book
images/author-portrait.jpg  Pencil portrait (About section)
images/author-mark.png      "su" monogram (source for the icons)
images/icon-64.png, icon-180.png   Browser-tab and home-screen icons
CNAME                       underhillnovels.com — do not edit
sitemap.xml, robots.txt     Search-engine files — update sitemap.xml for every new page
.nojekyll                   Keeps GitHub Pages from processing files
```

- **Slug:** a short, lower-case, hyphenated form of the title, e.g. `rowena-thornhill`.
  File names never contain spaces or capitals: web addresses are case-sensitive.
- Each book page is a **single self-contained HTML file** with its own inline
  `<style>` and `<script>`. There is no build step and no framework.

---

## 3. Design system

Colours are defined as tokens on `:root`, with dark-mode overrides. Copy them
exactly; never hard-code a colour.

| Token      | Light     | Dark      | Use                              |
|------------|-----------|-----------|----------------------------------|
| `--bg`     | `#faf8f4` | `#15140f` | Page (cream paper)               |
| `--ink`    | `#22201d` | `#e6e0d4` | Body text                        |
| `--mute`   | `#6d675f` | `#9a9284` | Secondary text, labels           |
| `--rule`   | `#ddd6ca` | `#33302a` | Hairlines and borders            |
| `--accent` | `#74294f` | `#cf8fae` | Claret-plum: rules, button, progress bar |
| `--card`   | `#fffdf9` | `#1c1a15` | Note boxes, back-to-top button   |
| `--plum`   | `#2b1724` | `#1f1019` | Top bar, hero, Write band, footer |
| `--plum-2` | `#45203a` | `#2f1729` | Gradient partner for plum bands  |
| `--gold`   | `#d9a54e` | `#d9a54e` | Buttons on plum, rules, progress bar, eyebrows |
| `--gold-wash` | `#f4e7cf` | `#2a2116` | Pull-quote band (home only)    |

Each book has its own card colour on the home page: Silk Purse gold-ink
(`#8a5a14`), Rowena dusk blue (`#3d5876`). Pick one for each new book from
its cover.

- **Display (names, book titles, chapter titles, section headings):** `Cormorant Garamond` from Google Fonts (weights 500, 600, italic 500), falling back to the serif stack.
- **Serif (all reading text):** `"Iowan Old Style","Palatino Linotype",Palatino,"Book Antiqua",Georgia,serif`
- **Sans (labels, buttons, meta, notes):** `ui-sans-serif,-apple-system,"Segoe UI",Roboto,Helvetica,Arial,sans-serif`
- Labels are sans, small, uppercase, letter-spaced (`.18em`).
- Reading measure `34rem`; home page wide measure `62rem`; 16px side gutter.
- Dark mode is supported in three ways on every page: system preference,
  `:root[data-theme="dark"]`, and a **Theme** button. The button stores the
  choice in `localStorage` under the key **`su-theme`** (shared site-wide).
- Everything must work at 320px wide with no sideways scrolling.

Tone: literary, warm, sensual but never explicit on the home page. A good publisher's list, not the romance aisle.
The home page describes the work as **"Sensual literary fiction · for adult readers"** and each
novel card carries an **18+** badge. Keep explicit terms off the home page (it is not
adult-tagged, so it stays visible in SafeSearch); book pages carry `rating=adult`
and plain content notes.
No script fonts, no pastels, no stock photos of people.

---

## 4. Book page skeleton

**Start a new book by copying `books/rowena-thornhill.html`** and replacing its
content. That keeps the CSS, script, bar and footer identical. The parts:

### `<head>`
```html
<html lang="en-AU">
<title>BOOK TITLE</title>
<meta name="description" content="ONE-SENTENCE HOOK">
<meta name="author" content="Sharyn Underhill">
<meta name="rating" content="adult">              <!-- only if adult content -->
<meta property="og:title" content="BOOK TITLE">
<meta property="og:type" content="book">
<meta property="og:image" content="https://underhillnovels.com/images/SLUG-share.jpg">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta property="og:url" content="https://underhillnovels.com/books/SLUG.html">
<meta name="twitter:card" content="summary_large_image">
<meta property="og:description" content="ONE-SENTENCE HOOK">
<link rel="icon" type="image/png" href="../images/icon-64.png">
<link rel="apple-touch-icon" href="../images/icon-180.png">
```

### Top bar, title page, note, contents
```html
<div class="progress" id="prog"></div>
<nav class="bar"><div class="wrap">
  <a class="bt" href="../">&larr; Sharyn Underhill</a>
  <a class="bt" href="#contents">Contents</a>
  <span class="sp"></span>
  <button class="bt" id="theme" type="button" aria-label="Toggle light and dark">Theme</button>
</div></nav>

<main class="wrap">
  <div class="titlepage">
    <img class="cover" src="../images/SLUG-cover.jpg" width="W" height="H" alt="Cover of BOOK TITLE: SHORT DESCRIPTION">
    <h1>Title Line One<br>Title Line Two</h1>
    <p class="by">Sharyn Underhill</p>
    <div class="rule"></div>
    <p class="meta">A novel &middot; N chapters &middot; N,NNN words</p>
  </div>

  <aside class="note">
    <b>A note before you begin.</b> CONTENT NOTE: audience and themes, stated plainly.
  </aside>

  <nav class="contents" id="contents">
    <h2>Contents</h2>
    <ol>
<li><a href="#ch1"><span class="n">1</span><span class="t">Chapter Title</span></a></li>
…
<li><a href="#about-this-book"><span class="n">&middot;</span><span class="t">About this book</span></a></li>
    </ol>
  </nav>
```

### Chapters
```html
<section class="chapter" id="ch1">
<header class="ch-head"><p class="ch-num">Chapter 1</p><h2>Chapter Title</h2></header>
<p>First paragraph (no indent, set automatically).</p>
<p>Following paragraphs are indented, with no space between them.</p>
<p class="beat">* * *</p>                  <!-- scene break within a chapter -->
<p class="quote">A quoted or remembered line.</p>
<p class="msg">A text message or note, shown in sans.</p>
<p class="end">&#10022;</p>               <!-- closes every chapter -->
</section>
```

- Chapter ids are `ch1`, `ch2`, … in order, matching the contents list.
- Use `&quot;`, `&#x27;`, `&mdash;` etc. for punctuation, or plain UTF-8; don't mix
  straight and curly quotes within one book.
- One idea per `<p>`. No `<br>` inside paragraphs, no inline styles, no fonts.

### Back matter, footer, script
```html
<section class="colophon" id="about-this-book">
  <p class="ch-num">About this book</p>
  <p><em>BOOK TITLE</em> was developed in consultation with AI. The cover image … created with AI.</p>
</section>
</main>

<footer>
  <p>BOOK TITLE</p>
  <p>&copy; Sharyn Underhill. All rights reserved.</p>
</footer>

<button class="top" id="top" type="button" aria-label="Back to top">&uarr;</button>
<script>/* copy unchanged from rowena-thornhill.html */</script>
```

**Letters from readers** follow the colophon (`section.letters#letters`). Readers
write via the Google Form linked from the "Write to Sharyn" button. Only letters
whose writer ticked a "Yes" publishing option are added, newest first:
```html
<blockquote class="letter">
  <p>Letter text (trimmed or excerpted if needed, never reworded).</p>
  <p class="from">&mdash; Name as given, or &ldquo;A reader&rdquo; if anonymous</p>
</blockquote>
```
Each book's "Write to Sharyn" button pre-selects that book in the form's
"Which book are you writing about?" question by adding
`?usp=pp_url&amp;entry.1533486748=Book+Title` to the form link (spaces as `+`).
For a new book, also add its title as an option in that form question, spelt
exactly as in the link.

Never publish a reader's email address or any detail they didn't ask to share.

**AI disclosure** lives only in the `About this book` colophon at the end of each
book — never on the title page or cover. List every AI-assisted element (text,
cover, portrait, other images).

---

## 5. Images

| Image            | Size (px)        | Format | Quality | File                     |
|------------------|------------------|--------|---------|--------------------------|
| Book cover       | ~1600 × 2400 (2:3) | JPG  | ~84     | `SLUG-cover.jpg`         |
| Link-preview card| 1200 × 630       | JPG    | ~86     | `SLUG-share.jpg`         |
| Home banner      | 2400 × 1000      | JPG    | ~78     | `banner.jpg`             |
| Portrait / marks | 800 × 800        | JPG/PNG| —       | `author-*.jpg/png`       |

- **Clean every image before committing** by re-encoding from pixels only, which
  drops EXIF, XMP and content-credential data:
  ```python
  from PIL import Image
  im = Image.open(src).convert("RGB")
  clean = Image.new("RGB", im.size); clean.paste(im)
  clean.save(dst, quality=84, optimize=True, progressive=True)
  ```
  Then confirm: search the file for `canva`, `c2pa`, `xmp`, `Attrib` and any
  real names — there should be no matches.
- Keep original downloads **outside** this folder (they still carry metadata).
- Covers: no faces, still-life or place, title in cream serif, author name in
  small caps, thin claret rule between them. Author name must be legible at
  thumbnail size.
- Link-preview card: cover on the left, `Sharyn Underhill` in bold serif, claret
  rule, book title in italic, `A NOVEL · FREE TO READ ONLINE` in sans.
- Always give `<img>` real `width`/`height` and a descriptive `alt`.

---

## 6. Adding a new book — checklist

1. Copy `books/rowena-thornhill.html` to `books/SLUG.html`; replace head,
   title page, note, contents, chapters and colophon.
2. Search the manuscript for stray names, places or bylines (§1).
3. Add `images/SLUG-cover.jpg` and `images/SLUG-share.jpg`, cleaned (§5).
4. **Home page (`index.html`):**
   - Hero: the new book's 600px cover becomes `.stack .front`; the previous
     front cover moves to `.stack .back`. Update the eyebrow
     (`New novel · TITLE`) and the gold button (`Read TITLE`).
   - Novels: add an `article.novel` card first in `.novels`, with the `New`
     and `18+` badges, its own `--book` colour, meta, blurb, content note and
     Start reading link. Change the previous book's badge from `New` to
     nothing (or `Second novel` etc.).
   - Optionally swap the pull quote for a line from the new book: suggestive,
     never explicit, under ~30 words.
   - Update `og:image` to the new book's share card.
5. **Search:** add the new page to `sitemap.xml` (and bump `<lastmod>` on the
   home page entry); give the book page its own `<link rel="canonical">` and a
   `Book` JSON-LD block copied from `rowena-thornhill.html`, with its own title,
   url, image, description, datePublished and wordCount.
6. Check the page at desktop width and at 320px, in light and dark mode, and
   confirm there is no sideways scroll.
7. Commit and push (§7), then load the live page to confirm.

---

## 7. Publishing

- Hosting is **GitHub Pages** from the `main` branch root of
  `underhillnovels/underhillnovels.github.io`, served at **https://underhillnovels.com**.
- **Commit and push together**: pushing to `main` is what publishes, usually
  within a minute. There is no staging site; preview locally before pushing.
- Never click **Unpublish site** in the repository's Pages settings, and never
  publish anything through the domain registrar's website builder.
- If a deploy sticks, push an empty commit:
  `git commit --allow-empty -m "Trigger Pages redeploy"; git push`

---

## 8. Paste-in brief for a drafting session

Give this to any writing assistant producing a new manuscript for the site:

```
Format the manuscript for underhillnovels.com as follows.
- Author: Sharyn Underhill (no other name anywhere, including metadata).
- Output plain chapter HTML only, no <html>, <head>, <style> or fonts:
  <section class="chapter" id="chN">
  <header class="ch-head"><p class="ch-num">Chapter N</p><h2>Title</h2></header>
  <p>…</p> for each paragraph; <p class="beat">* * *</p> for scene breaks;
  <p class="quote"> for quoted/remembered lines; <p class="msg"> for texts/notes;
  end each chapter with <p class="end">&#10022;</p></section>
- Also supply: book title, a one-sentence hook, a 40–60 word blurb, a content
  note (audience and themes, stated plainly), chapter count and word count.
- Australian/British spelling. No real people, no identifying places for the author.
```
