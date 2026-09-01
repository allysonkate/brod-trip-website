# The Bro'd Trip — Static Site Builder Rules

This file defines the authoritative rules for rebuilding brod-trip.com as a static HTML site for GitHub Pages. Read this before every session. No re-teaching required.

---

## 1. Project Overview

- Pure static HTML/CSS/JS — no build tools, no server, no frameworks
- All images are **base64-encoded and embedded directly** in HTML files (no external image references)
- Hosted on GitHub Pages from `/Users/allyson/Desktop/Brod Trip Website/`
- Source archives: https://web.archive.org/web/*/brod-trip.com

---

## 2. File Naming Conventions

- Lowercase, hyphens only, no spaces: `my-post-title.html`
- Match the original WordPress slug when possible
- Images go in the same folder; encode to base64 before embedding

---

## 3. Critical: Unicode Apostrophe Rule

**The posts[] array in index.html uses curly/smart apostrophes: `'` (Unicode `’`)**

When adding entries to `builtPages` or `postThumbs` in index.html, the **key must exactly match** the title string in `posts[]` — including the curly apostrophe.

✅ Correct: `"FRICKE’S FLIGHTS | DOGFISH HEAD BREWERY | MILTON, DE": "frickes-flights-dogfish-head-brewery.html"`
❌ Wrong:   `"FRICKE'S FLIGHTS | DOGFISH HEAD BREWERY | MILTON, DE": "..."` (straight apostrophe — will break linking)

To check: search the `posts[]` array for the exact title string first, then copy it verbatim as the key.

---

## 4. index.html — How to Add a New Post

Two places to update in `index.html`:

### A. `builtPages` object (inside `filterPosts()`)
```js
const builtPages = {
  // ... existing entries ...
  "EXACT TITLE FROM posts[] ARRAY": "filename.html",
};
```

### B. `postThumbs` object (inside `filterPosts()`)
```js
const postThumbs = {
  // ... existing entries ...
  "EXACT TITLE FROM posts[] ARRAY": "data:image/jpeg;base64,...",
};
```

The thumbnail base64 string comes from encoding the post's image file (or a smaller version of it).

---

## 5. Always Check Wayback Machine for Comments

**Before building any post**, check the Wayback Machine for archived comments:

```
https://web.archive.org/web/[TIMESTAMP]/http://brod-trip.com/[SLUG]/
```

Or search: `https://web.archive.org/cdx/search/cdx?url=brod-trip.com/[SLUG]/&output=text&fl=timestamp,statuscode&filter=statuscode:200`

Include all archived comments in the post HTML. Do not skip this step.

---

## 6. Complete HTML Template

Every post must use **this exact structure**. Do not use `.layout`, `.main-col`, or any other CSS classes not defined here.

```html
<!DOCTYPE html><html lang="en"><head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>[POST TITLE] | The Bro'd Trip</title>
<link href="https://fonts.googleapis.com/css2?family=Lato:ital,wght@0,400;0,700;1,400&family=Raleway:wght@400;600;700;900&family=Playfair+Display:wght@700&display=swap" rel="stylesheet">
  <style>
  *,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
  :root{--white:#ffffff;--bg:#f8f8f8;--light-gray:#f0f0f0;--rule:#d8d8d8;--text:#333333;--meta:#999999;--black:#111111;--sidebar-header-bg:#1a1a1a;--accent:#c0392b;--border:#e5e5e5}
  body{font-family:'Lato',sans-serif;background:var(--bg);color:var(--text);font-size:17px;line-height:1.8}
  a{text-decoration:none;color:inherit} a:hover{color:var(--accent)}
  .site-wrapper{background:var(--white);max-width:1200px;margin:0 auto;box-shadow:0 0 40px rgba(0,0,0,0.07)}
  .top-rule{border-top:3px solid var(--black)}
  .site-header{padding:32px 50px 0;display:grid;grid-template-columns:1fr auto 1fr;align-items:center;gap:20px}
  .nav-left{display:flex;justify-content:flex-start} .nav-right{display:flex;justify-content:flex-end}
  .nav-left a,.nav-right a{font-family:'Raleway',sans-serif;font-size:11px;font-weight:700;letter-spacing:0.18em;text-transform:uppercase;color:var(--black);padding:4px 14px;white-space:nowrap;transition:color 0.2s}
  .nav-left a:first-child{padding-left:0} .nav-right a:last-child{padding-right:0}
  .nav-left a:hover,.nav-right a:hover{color:var(--accent)}
  .logo-center{text-align:center}
  .nav-second{display:flex;justify-content:center;padding:10px 50px 0}
  .nav-second a{font-family:'Raleway',sans-serif;font-size:11px;font-weight:700;letter-spacing:0.18em;text-transform:uppercase;color:var(--black);padding:4px 20px;transition:color 0.2s}
  .nav-second a:hover{color:var(--accent)}
  .header-rule{margin:16px 50px 0;border:none;border-top:1px solid var(--rule)}
  .site-body{display:grid;grid-template-columns:1fr 290px;padding:0 50px 60px;align-items:start}
  .article{padding-top:50px;border-right:1px solid var(--border);padding-right:52px}
  .post-series{text-align:center;font-family:'Raleway',sans-serif;font-size:10px;font-weight:700;letter-spacing:0.25em;text-transform:uppercase;color:var(--accent);margin-bottom:8px}
  .post-title{font-family:'Raleway',sans-serif;font-size:26px;font-weight:700;letter-spacing:0.14em;text-transform:uppercase;text-align:center;color:var(--black);line-height:1.3;margin-bottom:12px}
  .post-byline{text-align:center;font-family:'Raleway',sans-serif;font-size:10px;font-weight:500;letter-spacing:0.18em;text-transform:uppercase;color:var(--meta);margin-bottom:36px}
  .post-byline .by-author{color:var(--text);font-weight:700}
  .post-body{font-size:17px;line-height:1.85;color:var(--text)}
  .post-body p{margin-bottom:22px} .post-body p:last-child{margin-bottom:0}
  .post-img{width:100%;height:auto;display:block;margin:28px 0;border-radius:2px}
  .pic-specs{margin-top:36px;padding:24px 28px;border:1px solid var(--border);background:#fafafa}
  .pic-specs-title{font-family:'Raleway',sans-serif;font-size:10px;font-weight:700;letter-spacing:0.22em;text-transform:uppercase;color:var(--meta);margin-bottom:14px}
  .pic-specs-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:12px 20px}
  .spec-label{font-family:'Raleway',sans-serif;font-size:9px;font-weight:700;letter-spacing:0.18em;text-transform:uppercase;color:var(--meta);display:block;margin-bottom:2px}
  .spec-value{font-family:'Lato',sans-serif;font-size:15px;font-weight:700;color:var(--black)}
  .author-bio{margin-top:36px;padding-top:30px;border-top:1px solid var(--rule);display:flex;gap:20px;align-items:flex-start}
  .author-bio img{width:80px;height:80px;object-fit:cover;border-radius:50%;flex-shrink:0;border:2px solid var(--border)}
  .author-bio-text{flex:1}
  .author-bio-name{font-family:'Raleway',sans-serif;font-size:14px;font-weight:700;letter-spacing:0.08em;color:var(--black);margin-bottom:6px}
  .author-bio-quote{font-style:italic;font-size:15px;color:var(--text);line-height:1.6}
  .post-nav{margin-top:40px;padding-top:28px;border-top:1px solid var(--rule);display:flex;justify-content:space-between;align-items:flex-start;gap:20px}
  .post-nav .nav-label{font-size:9px;letter-spacing:0.18em;color:var(--meta);display:block;margin-bottom:4px;font-family:'Raleway',sans-serif;text-transform:uppercase}
  .post-nav a{font-family:'Raleway',sans-serif;font-size:11px;font-weight:700;letter-spacing:0.12em;text-transform:uppercase;color:var(--black);border-bottom:2px solid var(--black);padding-bottom:2px;transition:all 0.2s;display:inline-block}
  .post-nav a:hover{color:var(--accent);border-color:var(--accent)}
  .post-nav-next{text-align:right}
  .sidebar{padding-top:50px;padding-left:40px}
  .sw{margin-bottom:32px}
  .sw-head{background:var(--sidebar-header-bg);color:#fff;font-family:'Raleway',sans-serif;font-size:11px;font-weight:700;letter-spacing:0.2em;text-transform:uppercase;padding:13px 16px;text-align:center}
  .sw-body{border:1px solid var(--border);border-top:none;padding:20px}
  .sw-body img{width:100%;height:auto;display:block;margin-bottom:14px}
  .sw-body p{font-size:14px;color:var(--text);line-height:1.75;margin-bottom:12px}
  .sw-body ul{list-style:disc;padding-left:18px;margin:0}
  .sw-body ul li{margin-bottom:10px;font-size:14px;line-height:1.4}
  .sw-body ul li a{color:var(--text);transition:color 0.2s}
  .sw-body ul li a:hover{color:var(--accent)}
  .back-link{display:inline-block;margin-top:8px;font-family:'Raleway',sans-serif;font-size:10px;font-weight:700;letter-spacing:0.14em;text-transform:uppercase;color:var(--black);border-bottom:2px solid var(--black);padding-bottom:2px;transition:all 0.2s}
  .back-link:hover{color:var(--accent);border-color:var(--accent)}
  .stats-grid{display:grid;grid-template-columns:1fr 1fr;gap:1px;background:var(--border)}
  .stat-cell{background:var(--white);padding:16px 8px;text-align:center}
  .stat-num{font-family:'Playfair Display',serif;font-size:28px;font-weight:700;color:var(--black);display:block;line-height:1;margin-bottom:4px}
  .stat-lbl{font-family:'Raleway',sans-serif;font-size:9px;font-weight:700;letter-spacing:0.16em;text-transform:uppercase;color:var(--meta)}
  .fueled-item{padding:14px 0;border-bottom:1px solid var(--border);text-align:center}
  .fueled-item:first-child{padding-top:0}
  .fueled-item:last-child{border-bottom:none;padding-bottom:0}
  .fueled-name{display:block;font-family:'Raleway',sans-serif;font-size:20px;font-weight:900;letter-spacing:0.08em;color:#c8960c;margin-bottom:2px}
  .fueled-tagline{display:block;font-family:'Lato',sans-serif;font-size:12px;color:var(--meta)}
  .site-footer{border-top:3px solid var(--black);padding:28px 50px;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:12px}
  .footer-brand{font-family:'Playfair Display',serif;font-size:17px;font-weight:700;color:var(--black)}
  .footer-sub{font-family:'Raleway',sans-serif;font-size:10px;font-weight:600;letter-spacing:0.18em;text-transform:uppercase;color:var(--meta);margin-top:3px}
  .footer-note{font-family:'Raleway',sans-serif;font-size:11px;color:var(--meta);text-align:right}
  @media(max-width:820px){
    .site-header{grid-template-columns:1fr;padding:20px 24px 0;text-align:center}
    .nav-left,.nav-right{justify-content:center;flex-wrap:wrap}
    .nav-second,.header-rule{padding-left:24px;padding-right:24px}
    .site-body{grid-template-columns:1fr;padding:0 24px 40px}
    .article{border-right:none;padding-right:0}
    .sidebar{padding-left:0}
  }
  .comments-section{margin-top:48px;padding-top:36px;border-top:1px solid var(--rule);}
  .comments-heading{font-family:'Raleway',sans-serif;font-size:13px;font-weight:700;letter-spacing:0.2em;text-transform:uppercase;color:var(--black);margin-bottom:28px;}
  .comment{padding:20px 0;border-bottom:1px solid var(--light-gray);}
  .comment:last-child{border-bottom:none;}
  .comment-meta{display:flex;align-items:baseline;gap:12px;margin-bottom:8px;flex-wrap:wrap;}
  .comment-author{font-family:'Raleway',sans-serif;font-size:12px;font-weight:700;letter-spacing:0.1em;text-transform:uppercase;color:var(--black);}
  .comment-date{font-family:'Lato',sans-serif;font-size:11px;color:var(--meta);}
  .comment-body{font-size:15px;line-height:1.75;color:var(--text);}
  .comment-reply{margin-left:32px;border-left:2px solid var(--border);padding-left:20px;}
  .yt-thumb-link{display:block;text-decoration:none;margin:28px 0}
  .yt-thumb-wrap{position:relative;width:100%;padding-top:56.25%;background:#000;border-radius:2px;overflow:hidden}
  .yt-thumb-wrap img{position:absolute;top:0;left:0;width:100%;height:100%;object-fit:cover;opacity:0.85;transition:opacity 0.2s}
  .yt-thumb-link:hover .yt-thumb-wrap img{opacity:1}
  .yt-play-btn{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);width:68px;height:48px}
  .yt-watch-label{font-family:'Raleway',sans-serif;font-size:10px;font-weight:700;letter-spacing:0.18em;text-transform:uppercase;color:var(--meta);margin-top:8px;text-align:center}
  </style>
</head>
<body><div class="site-wrapper">
<div class="top-rule"></div>
<header>
  <div class="site-header">
    <nav class="nav-left">
      <a href="the-brothers.html">The Brothers</a>
      <a href="index.html">The Road</a>
      <a href="#">Contact</a>
    </nav>
    <div class="logo-center">
      <a href="index.html"><img src="[LOGO_BASE64]" alt="The Bro'd Trip" style="max-width:240px;height:auto;display:block;margin:0 auto"></a>
    </div>
    <nav class="nav-right">
      <a href="in-the-news.html">In The News</a>
      <a href="sponsors.html">Sponsors</a>
      <a href="#">Store</a>
    </nav>
  </div>
  <nav class="nav-second"><a href="index.html">Through the Lens</a></nav>
  <hr class="header-rule">
</header>
<div class="site-body">
  <article class="article">
    <!-- OPTIONAL: series label e.g. "Pic(k) of the Week" -->
    <div class="post-series">SERIES LABEL (if applicable)</div>
    <h1 class="post-title">POST TITLE</h1>
    <div class="post-byline">By <span class="by-author">Adam Fricke</span> &bull; November 23, 2016</div>
    <div class="post-body">
      <p>Post content paragraphs go here.</p>
      <img src="[IMAGE_BASE64]" alt="Description" class="post-img">
      <p>More content.</p>
    </div>

    <!-- PIC-SPECS BLOCK — Only for Pic(k) of the Week posts -->
    <div class="pic-specs">
      <div class="pic-specs-title">Photo Specs</div>
      <div class="pic-specs-grid">
        <div><span class="spec-label">Focal Length</span><span class="spec-value">14mm</span></div>
        <div><span class="spec-label">Aperture</span><span class="spec-value">f/4.5</span></div>
        <div><span class="spec-label">Shutter Speed</span><span class="spec-value">1/125s</span></div>
        <div><span class="spec-label">ISO</span><span class="spec-value">6400</span></div>
        <div><span class="spec-label">Format</span><span class="spec-value">RAW</span></div>
        <div><span class="spec-label">Edited In</span><span class="spec-value">Lightroom CC</span></div>
      </div>
    </div>

    <!-- AUTHOR BIO — Adam Fricke -->
    <div class="author-bio">
      <img src="[ADAM_HEADSHOT_BASE64]" alt="Adam Fricke">
      <div class="author-bio-text">
        <div class="author-bio-name">Adam Fricke</div>
        <div class="author-bio-quote">Although my destination may be uncertain, my direction is Always Forward. Photography, Videography, Writing, Music, Surfing, Beer. Not necessarily in that order. Follow me @AdamFricke</div>
      </div>
    </div>

    <!-- AUTHOR BIO — Justin Fricke (use instead of Adam when Justin is author) -->
    <!--
    <div class="author-bio">
      <img src="[JUSTIN_HEADSHOT_BASE64]" alt="Justin Fricke">
      <div class="author-bio-text">
        <div class="author-bio-name">Justin Fricke</div>
        <div class="author-bio-quote">Justin's bio quote here.</div>
      </div>
    </div>
    -->

    <!-- COMMENTS SECTION — Only include if comments exist. -->
    <!-- HEADING COUNT = total number of all comments (top-level + replies combined) -->
    <!-- NESTING RULE: replies MUST be nested inside .comment-reply divs inside the parent .comment.
         Never place a reply as a sibling .comment — always nest it. Multi-level replies are nested
         further (a reply-to-a-reply goes inside another .comment-reply inside the first reply's .comment). -->
    <div class="comments-section">
      <div class="comments-heading">3 Comments</div>

      <!-- Top-level comment with one reply: -->
      <div class="comment">
        <div class="comment-meta">
          <span class="comment-author">Commenter Name</span>
          <span class="comment-date">Month DD, YYYY</span>
        </div>
        <div class="comment-body">Comment text here.</div>
        <div class="comment-reply">
          <div class="comment">
            <div class="comment-meta">
              <span class="comment-author">Reply Author</span>
              <span class="comment-date">Month DD, YYYY</span>
            </div>
            <div class="comment-body">Reply text here.</div>
            <!-- If there's a reply-to-the-reply, nest another .comment-reply here: -->
            <div class="comment-reply">
              <div class="comment">
                <div class="comment-meta">
                  <span class="comment-author">2nd-Level Reply Author</span>
                  <span class="comment-date">Month DD, YYYY</span>
                </div>
                <div class="comment-body">Reply-to-reply text.</div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Top-level comment with no reply: -->
      <div class="comment">
        <div class="comment-meta">
          <span class="comment-author">Another Commenter</span>
          <span class="comment-date">Month DD, YYYY</span>
        </div>
        <div class="comment-body">Another comment.</div>
      </div>
    </div>

    <!-- POST NAVIGATION -->
    <nav class="post-nav">
      <div>
        <span class="nav-label">&larr; Previous</span>
        <a href="previous-post.html" class="nav-title">Previous Post Title</a>
      </div>
      <div class="post-nav-next">
        <span class="nav-label">Next &rarr;</span>
        <a href="next-post.html" class="nav-title">Next Post Title</a>
      </div>
    </nav>
  </article>

  <!-- SIDEBAR — This is the authoritative sidebar. Do not change the structure. -->
  <aside class="sidebar">
    <div class="sw">
      <div class="sw-head">What Is The Bro&#8217;d Trip?</div>
      <div class="sw-body">
        <img src="[VAN_IMAGE_BASE64]" alt="The Bro'd Trip Van">
        <p>We&#8217;re two brothers fighting the pull of the post-college desk job. Traveling all 50 states by van, we&#8217;re seeking meaningful work that we love.</p>
        <a href="index.html" class="back-link">&#8592; Back to All Posts</a>
      </div>
    </div>
    <div class="sw">
      <div class="sw-head">Trip Stats</div>
      <div class="sw-body">
        <div class="stats-grid">
          <div class="stat-cell"><span class="stat-num">50</span><span class="stat-lbl">States</span></div>
          <div class="stat-cell"><span class="stat-num">366</span><span class="stat-lbl">Days</span></div>
          <div class="stat-cell"><span class="stat-num">241</span><span class="stat-lbl">Posts</span></div>
          <div class="stat-cell"><span class="stat-num">2</span><span class="stat-lbl">Brothers</span></div>
        </div>
      </div>
    </div>
    <div class="sw">
      <div class="sw-head">Recent Posts</div>
      <div class="sw-body">
        <ul>
          <li><a href="favorite-states.html">The Bro&#8217;d Trip&#8217;s Favorite States</a></li>
          <li><a href="rearview-mirror-brod-trip-2016.html">Rearview Mirror: The Bro&#8217;d Trip 2016</a></li>
          <li><a href="rearview-mirror-december.html">Rearview Mirror: December</a></li>
          <li><a href="alabama-outdoor-adventure.html">Alabama Outdoor Adventures</a></li>
          <li><a href="vlog-55-heading-home.html">Vlog_55 | Heading Home!</a></li>
        </ul>
      </div>
    </div>
    <div class="sw">
      <div class="sw-head">Fueled By:</div>
      <div class="sw-body">
        <div class="fueled-item">
          <span class="fueled-name">MERRELL</span>
        </div>
        <div class="fueled-item">
          <span class="fueled-name" style="color:var(--black)">EnerPlex</span>
          <span class="fueled-tagline">Always in Charge</span>
        </div>
      </div>
    </div>
  </aside>
</div>

<footer class="site-footer">
  <div>
    <div class="footer-brand">The Bro'd Trip</div>
    <div class="footer-sub">2 Brothers &middot; 4 Wheels &middot; 50 States &middot; 1 Year &middot; 2015&ndash;2017</div>
  </div>
  <div class="footer-note">
    Site rebuilt with love &amp; preserved for posterity.<br>
    Original posts: <a href="https://web.archive.org/web/20160101000000*/brod-trip.com" target="_blank">archive.org</a>
  </div>
</footer>
</div></body></html>
```

---

## 7. Sidebar Rules (CRITICAL — Match Original WordPress Blog)

The sidebar has exactly **4 widgets** in this order. Do not add, remove, or reorder them.

| Widget | Content |
|--------|---------|
| **What Is The Bro'd Trip?** | Van image (base64) + description paragraph + "← Back to All Posts" link |
| **Trip Stats** | 2×2 grid: 50 States / 366 Days / 241 Posts / 2 Brothers |
| **Recent Posts** | Static list of 5 most recent posts |
| **Fueled By:** | MERRELL (gold) + EnerPlex "Always in Charge" (black) |

The van image source file is `images/151024_VanPickup-042-300x200.jpg` (downloaded from the original WordPress blog's Wayback archive — `brod-trip.com/wp-content/uploads/2015/10/151024_VanPickup-042-300x200.jpg`). It is 300×200px, the two brothers standing in front of the white Sprinter van. Encode it fresh or copy its base64 from an existing correct post (e.g., `pick-of-the-week-54-always-summer.html`). Base64 prefix: `data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAeAB4AAD/7UrUUGhvd...` **Do NOT use Adam's or Justin's headshot base64 here, and do NOT use 3_Guys_1_Van.jpg.**

---

## 8. YouTube Embed Pattern

For vlog posts that have a YouTube video, use the thumbnail-link pattern (NOT an iframe):

```html
<a href="https://www.youtube.com/watch?v=VIDEO_ID" target="_blank" class="yt-thumb-link">
  <div class="yt-thumb-wrap">
    <img src="https://img.youtube.com/vi/VIDEO_ID/hqdefault.jpg" alt="VIDEO TITLE">
    <div class="yt-play-btn">
      <svg viewBox="0 0 68 48" xmlns="http://www.w3.org/2000/svg">
        <path d="M66.5 7.7c-.8-2.9-3-5.1-5.8-5.8C55.8 0 34 0 34 0S12.2 0 7.3 1.9C4.6 2.6 2.3 4.9 1.5 7.7 0 12.7 0 24 0 24s0 11.3 1.5 16.3c.8 2.9 3 5.1 5.8 5.8C12.2 48 34 48 34 48s21.8 0 26.7-1.9c2.8-.8 5-3 5.8-5.8C68 35.3 68 24 68 24s0-11.3-1.5-16.3z" fill="#ff0000"/>
        <path d="M27 34l18-10-18-10v20z" fill="#fff"/>
      </svg>
    </div>
  </div>
  <div class="yt-watch-label">Watch on YouTube &rarr;</div>
</a>
```

The YouTube thumbnail URL `https://img.youtube.com/vi/VIDEO_ID/hqdefault.jpg` works without any API key.

---

## 9. Image Encoding Workflow

```bash
# Encode any image file to base64 for embedding:
python3 -c "
import base64
with open('ImageFile.jpg', 'rb') as f:
    data = base64.b64encode(f.read()).decode()
print('data:image/jpeg;base64,' + data[:80] + '...')
"
```

- Use `data:image/jpeg;base64,...` for .jpg/.jpeg files
- Use `data:image/png;base64,...` for .png files
- If a file has the wrong extension (e.g., `.html` but is actually a JPEG), run `file ImageFile.html` to confirm, then encode as the correct MIME type
- Thumbnail for `postThumbs` can be the same base64 as the post image (full size is fine)

---

## 10. Author Bio Images

The headshot images are embedded base64 in the HTML. To reuse them across posts, copy the base64 `src` value from an existing correct post:

- **Adam Fricke headshot**: copy `src` from `<div class="author-bio"><img src="..."` in `unity-in-dc.html`
- **Justin Fricke headshot**: copy `src` from `<div class="author-bio"><img src="..."` in `pick-of-the-week-55-comfort-zone.html`

---

## 11. Logo and Van Image

- **Logo** (site header): copy from any existing post — `<img src="data:image/png;base64,..." alt="The Bro'd Trip"` in the `.logo-center` div
- **Van image** (sidebar "What Is The Bro'd Trip?"): source file is `images/151024_VanPickup-042-300x200.jpg`. Copy the base64 `src` from the first `.sw-body` `<img>` inside the sidebar of any post built after May 2026 (e.g., `unity-in-dc.html`). The base64 prefix is `data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAeAB4AAD/7UrUUGhvd...`

---

## 12. Post-Type Patterns

### Standard blog post
- `.post-series` div: omit (or use if part of a recurring series)
- `.pic-specs`: omit
- Author bio: include (Adam or Justin depending on author)

### Pic(k) of the Week
- `.post-series` div: `PIC(K) OF THE WEEK`
- `.pic-specs`: include with all 6 specs (focal length, aperture, shutter, ISO, format, edited in)
- Author bio: include (usually Adam)

### Vlog post
- `.post-series` div: `VLOG`
- YouTube embed: include using pattern in Section 8
- No `.pic-specs`
- Author bio: include

### Rearview Mirror (monthly recap)
- `.post-series` div: `REARVIEW MIRROR`
- No `.pic-specs`
- Author bio: include

---

## 13. HTML Entities Reference

| Character | Entity |
|-----------|--------|
| ' (curly right single quote / apostrophe) | `&#8217;` |
| ' (curly left single quote) | `&#8216;` |
| " (curly left double quote) | `&#8220;` |
| " (curly right double quote) | `&#8221;` |
| … (ellipsis) | `&#8230;` |
| — (em dash) | `&mdash;` |
| – (en dash) | `&ndash;` |
| · (middle dot) | `&middot;` |
| ← | `&larr;` |
| → | `&rarr;` |
| & | `&amp;` |

---

## 14. Wayback Machine CDX API

To find all archived URLs for the site:
```
https://web.archive.org/cdx/search/cdx?url=brod-trip.com/*&output=text&fl=original,timestamp&filter=statuscode:200
```

To check a specific post for its best snapshot:
```
https://web.archive.org/cdx/search/cdx?url=brod-trip.com/[slug]/&output=text&fl=timestamp,statuscode&filter=statuscode:200
```

Then access: `https://web.archive.org/web/[TIMESTAMP]/http://brod-trip.com/[slug]/`

---

## 16. Missing Photo Placeholder (REQUIRED)

Every post image that could not be recovered from the Wayback Machine archive **must** get a placeholder div — placed at the **exact position** in the post body where the original image appeared — showing the **exact original WordPress filename** so Adam can drop the file in later.

### How to find the exact filename

1. Load the Wayback Machine archive URL for the post (e.g. `https://web.archive.org/web/TIMESTAMP/http://brod-trip.com/post-slug/`)
2. Fetch the HTML and extract every `<img>` tag whose `src` contains `wp-content/uploads`
3. The filename is the **last path segment** of that URL:
   - URL: `https://web.archive.org/web/20170610045912im_/http://brod-trip.com/wp-content/uploads/2016/11/AK8A1424.jpg`
   - Filename: `AK8A1424.jpg`
4. Use that **exact filename** — never invent or derive a name from the post slug

If the Wayback archive is unavailable and the filename truly cannot be determined, use `[filename unknown — add when image is recovered]` as a temporary value. Never auto-generate a name.

### Identifying image positions

When fetching the Wayback HTML, note the text immediately before and after each `<img>` tag to determine which paragraph the placeholder should follow (or precede). Place the placeholder div at the matching position in the rebuilt HTML.

**CSS** (add to `<style>` block — already present in posts built after May 2026):
```css
.missing-photo{border:2px dashed var(--border);border-radius:2px;padding:48px 20px;text-align:center;margin:28px 0;background:#fafafa}
.missing-photo svg{margin-bottom:14px;opacity:0.4}
.missing-photo p{font-family:'Raleway',sans-serif;font-size:10px;font-weight:700;letter-spacing:0.2em;text-transform:uppercase;color:var(--meta);margin:0}
.missing-photo .missing-filename{margin-top:8px;font-family:'Lato',sans-serif;font-size:13px;font-weight:400;letter-spacing:0.05em;text-transform:none;color:var(--accent)}
```

**HTML** (replace `Expected_Filename.jpg` with the exact filename extracted from the Wayback URL):
```html
<div class="missing-photo">
  <svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="#ccc" stroke-width="1.5">
    <rect x="3" y="3" width="18" height="18" rx="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/>
  </svg>
  <p>Original photo unavailable &mdash; lost in the archive</p>
  <p class="missing-filename">Expected file: Expected_Filename.jpg</p>
</div>
```

**Example:** Wayback URL `...wp-content/uploads/2016/11/AK8A1424.jpg` → filename `AK8A1424.jpg`:
```html
<p class="missing-filename">Expected file: AK8A1424.jpg</p>
```

---

## 15. Reference Files

When in doubt about template/styling, read these reference files (they are confirmed correct):
- `pick-of-the-week-54-always-summer.html` — POTW post, Adam author, no comments
- `unity-in-dc.html` — standard post, Adam author, 1 comment
- `frickes-flights-dogfish-head-brewery.html` — standard post, Adam author, no comments
- `pick-of-the-week-55-comfort-zone.html` — POTW post, Justin author, no comments

---

## 17. Fast Build Script (Use This Every Session)

**Do not rewrite the template from scratch.** Extract it from the reference post and build new posts in Python. This avoids re-encoding the sidebar van/logo/sponsor images every time.

### Reference post template line ranges

Source file: `frickes-flights-dogfish-head-brewery.html`

| Section | Lines (1-indexed) | 0-indexed slice |
|---|---|---|
| CSS block (inside `<style>`) | 5–98 | `[4:98]` |
| Google Fonts `<link>` | 4 | `[3]` |
| Head close + header + `<div class="site-body">` | 99–121 | `[98:121]` |
| Sidebar `<aside>` | 226–270 | `[225:270]` |
| Footer + closing tags | 272–282 | `[271:282]` |

### Reusable Python build functions

Paste this at the top of every build script:

```python
import base64, re

BASE = '/Users/allyson/Desktop/Brod Trip Website'

with open(f'{BASE}/frickes-flights-dogfish-head-brewery.html', 'r', encoding='utf-8') as f:
    ref = f.readlines()

css_block    = ''.join(ref[4:98])   # ends at </style> (line 98 after yt CSS added)
fonts_line   = ref[3]
head_to_body = ''.join(ref[98:121])  # </head> through <div class="site-body">
sidebar      = ''.join(ref[225:270]) # <aside class="sidebar"> through </aside>
footer_block = ''.join(ref[271:282]) # <footer> through </div></body></html>

def ph(filename):
    return f'''      <div class="missing-photo">
        <svg width="48" height="48" viewBox="0 0 24 24" fill="none" stroke="#ccc" stroke-width="1.5">
          <rect x="3" y="3" width="18" height="18" rx="2"/><circle cx="8.5" cy="8.5" r="1.5"/><polyline points="21 15 16 10 5 21"/>
        </svg>
        <p>Original photo unavailable &mdash; lost in the archive</p>
        <p class="missing-filename">Expected file: {filename}</p>
      </div>'''

def img(rel_path, alt='', css_class='post-img'):
    # Use file path — images live in images/ subdir, served by GitHub Pages
    return f'      <img src="{rel_path}" alt="{alt}" class="{css_class}">'

# Load author bios — extract headshot img from known post, use REAL bio copy only:
import re as _re
with open(f'{BASE}/vlog-49-pulled-over-by-cops.html', 'r', encoding='utf-8') as _f:
    _justin_img = _re.search(r'(<img src="data:image/jpeg;base64,[^"]*"\s*alt="Justin Fricke">)', _f.read()).group(1)
# REAL Justin bio copy (from Wayback Machine — do NOT change or invent):
justin_bio = f'''<div class="author-bio">
      {_justin_img}
      <div class="author-bio-text">
        <div class="author-bio-name">Justin Fricke</div>
        <div class="author-bio-quote">Hi I&#8217;m Justin! I&#8217;m a 25 year old millennial that&#8217;s trading in his cool downtown office job that comes with all the benefits for a life on the road. I may not know my destination, but I&#8217;m strapped in and ready for the journey. Follow me @JustinLFricke</div>
      </div>
    </div>'''
# Adam bio — extract from 3-guys-1-van (headshot + real copy, no tags):
with open(f'{BASE}/3-guys-1-van.html', 'r', encoding='utf-8') as _f:
    adam_bio = _re.search(r'(<div class="author-bio">.*?</div>\s*</div>\s*</div>)', _f.read(), _re.DOTALL).group(1)

def build_post(title, title_tag, date, author, series, body_html,
               prev_href, prev_title, next_href, next_title, comments_html=''):
    # date format: "October 13, 2016"  author: "Justin" or "Adam"
    bio = justin_bio if author == 'Justin' else adam_bio
    return f'''<!DOCTYPE html>
<html lang="en">
<head>
{fonts_line}<title>{title_tag} | The Bro&#8217;d Trip</title>
{css_block}</head>
{head_to_body}  <article class="article">
    <div class="post-series">{series}</div>
    <h1 class="post-title">{title}</h1>
    <div class="post-byline">By <span class="by-author">{author} Fricke</span> &bull; {date}</div>
    <div class="post-body">
{body_html}
    </div>
    {bio}
    <nav class="post-nav">
      <div><span class="nav-label">&larr; Previous</span><a href="{prev_href}" class="nav-title">{prev_title}</a></div>
      <div class="post-nav-next"><span class="nav-label">Next &rarr;</span><a href="{next_href}" class="nav-title">{next_title}</a></div>
    </nav>
  </article>
{sidebar}
</div>
{footer_block}
</html>'''

def save(filename, html):
    with open(f'{BASE}/{filename}', 'w', encoding='utf-8') as f:
        f.write(html)
    print(f'{filename}: {len(html)//1024}KB')
```

### Fetching archive content

```bash
# Get all img filenames and paragraph context from a Wayback URL:
curl -s --max-time 90 "https://web.archive.org/web/TIMESTAMP/http://brod-trip.com/SLUG/" | python3 -c "
import sys, re
html = sys.stdin.read()
# Date
print('DATE:', re.search(r'datetime=\"(\d{4}-\d{2}-\d{2})', html).group(1) if re.search(r'datetime=\"(\d{4}-\d{2}-\d{2})', html) else 'not found')
# Author
a = re.search(r'entry-author-name[^>]*>([^<]+)<', html)
print('AUTHOR:', a.group(1) if a else 'not found')
# Images
for m in re.finditer(r'wp-content/uploads/\d+/\d+/([^\"\' &]+)', html):
    if not any(x in m.group(1) for x in ['300x', 'VanPickup','MRL-LOGO','Enerplex']):
        print('IMG:', m.group(1))
# Paragraphs + images
body = re.search(r'entry-content.*?itemprop=\"text\">(.*?)</div>\s*</div>\s*<footer', html, re.S)
if body:
    for p in re.findall(r'<p[^>]*>(.*?)</p>', body.group(1), re.S):
        txt = re.sub(r'<[^>]+>', '', p).strip()
        if txt and len(txt) > 20: print('P:', txt[:200])
        img = re.search(r'wp-content/uploads/\d+/\d+/([^\"\' &]+)', p)
        if img: print('  -> IMG:', img.group(1))
"
```

### Updating index.html

Images go in `images/` subdirectory (e.g. `images/Merrell_HQ.jpg`), not the site root.

Use regex to match keys — they use curly apostrophes (`’`) which won't match a plain `'`:

```python
with open(f'{BASE}/index.html', 'r', encoding='utf-8') as f:
    idx = f.read()

# builtPages — insert after last known entry (use re.search to find the anchor)
bp_anchor = re.search(r'(\"LAST KNOWN TITLE[^\"]*\": \"last-known-file\.html\",)', idx)
old = bp_anchor.group(1)
idx = idx.replace(old, old + '\n    "NEW TITLE": "new-file.html",', 1)

# postThumbs — same anchor pattern but match "data:image" instead of ".html"
pt_anchor = re.search(r'(\"LAST KNOWN TITLE[^\"]*\": \"data:image[^\"]+\",)', idx)
old = pt_anchor.group(1)
thumb_b64 = 'data:image/jpeg;base64,' + base64.b64encode(open(f'{BASE}/images/Photo.jpg','rb').read()).decode()
idx = idx.replace(old, old + f'\n    "NEW TITLE": "{thumb_b64}",', 1)

with open(f'{BASE}/index.html', 'w', encoding='utf-8') as f:
    f.write(idx)
```

**Title keys must exactly match the `posts[]` array** — copy them verbatim. Check with:
```bash
grep -o '"TITLE YOU WANT"' /Users/allyson/Desktop/Brod\ Trip\ Website/index.html | head -2
```

### Extracting and cleaning Wayback body content (REQUIRED)

**NEVER paste raw Wayback HTML into `body_html`.** Always run it through `clean_body()` first. WordPress Wayback archives contain leftover `</div>` tags, `<footer class="entry-footer">` category/tag sections, RDF comments, and Wayback-injected wrapper elements that will break the page layout if included.

```python
import re, os

IMAGES_DIR = f'{BASE}/images'
available_images = set(os.listdir(IMAGES_DIR))

def clean_body(html):
    """Extract and clean the post body from a Wayback HTML page."""
    m = re.search(r'<div[^>]*class="[^"]*entry-content[^"]*"[^>]*>(.*)', html, re.DOTALL)
    if not m: return ""
    c = m.group(1)

    # ── CRITICAL: Stop BEFORE WordPress footer/comments/author sections ──────
    # These patterns mark the END of the real post body. Anything after them
    # is WordPress boilerplate (tags, categories, share buttons) — NOT content.
    for pat in [
        r'<footer[^>]*class="[^"]*entry-footer[^"]*"',   # ← tags/categories footer
        r'</div>\s*</section>\s*<div[^>]*(?:id="comments"|class="[^"]*entry-comments)',
        r'<div[^>]*(?:id="comments"|class="[^"]*entry-comments)',
        r'<section[^>]*class="[^"]*entry-author',
        r'<div[^>]*class="[^"]*sharedaddy',
        r'</article>',
    ]:
        mm = re.search(pat, c, re.DOTALL)
        if mm:
            c = c[:mm.start()]
            break

    # ── Strip trailing orphan </div> tags from WordPress entry-content wrapper ─
    # After truncating before <footer class="entry-footer">, there may be leftover
    # </div> tags that were closing WordPress's inner wrapper divs. Remove them.
    c = re.sub(r'<!--.*?RDF.*?-->', '', c, flags=re.DOTALL)          # remove RDF comments (use .* not [^-]* — comments contain > chars)
    c = re.sub(r'(\s*(?:<!--[^>]*-->)?\s*</div>)+\s*$', '', c)     # strip trailing </div>s even with comments between them

    # ── Clean Wayback-injected URLs ──────────────────────────────────────────
    c = re.sub(r'https?://web\.archive\.org/web/\d+im_/', '', c)
    c = re.sub(r'https?://web\.archive\.org/web/\d+(?:if_)?/', '', c)
    c = re.sub(r'\s+srcset="[^"]*"', '', c)
    c = re.sub(r'\s+sizes="[^"]*"', '', c)
    c = re.sub(r'https?://brod-trip\.com/', '', c)
    c = re.sub(r'https?://[^/]+/wp-content/', 'wp-content/', c)

    # ── Remove iframes (YouTube — replaced separately with yt_embed()) ───────
    c = re.sub(r'<p>\s*<iframe[^>]*youtube[^>]*>.*?</iframe>\s*</p>',
               '<!--YT_PLACEHOLDER-->', c, flags=re.DOTALL)

    # ── Strip WordPress-only markup ──────────────────────────────────────────
    c = re.sub(r'\[/?caption[^\]]*\]', '', c)
    c = re.sub(r'\s+class="[^"]*(?:aligncenter|alignnone|wp-image)[^"]*"', '', c)
    c = re.sub(r'\s+width="\d+"', '', c)
    c = re.sub(r'\s+height="\d+"', '', c)
    c = re.sub(r'<a[^>]+href="[^"]*wp-content[^"]*"[^>]*>\s*(<img[^>]*>)\s*</a>',
               r'\1', c, flags=re.DOTALL)
    c = re.sub(r'<(?:header|div)[^>]*class="[^"]*entry-(?:header|content)[^"]*"[^>]*>', '', c)
    c = re.sub(r'</header>', '', c)
    c = re.sub(r'<p>\s*</p>', '', c)
    return c.strip()

def convert_images(body_html):
    """Replace wp-content img paths with local images/ paths, or placeholder divs."""
    def replace_img(m):
        filename = m.group(1).split('/')[-1]
        if filename in available_images:
            return f'<img src="images/{filename}" alt="">'
        return f'''<div class="missing-photo">
        <svg viewBox="0 0 60 60" xmlns="http://www.w3.org/2000/svg" width="48" height="48"><rect width="60" height="60" rx="6" fill="#e8e0d8"/><path d="M15 42l12-16 9 12 6-8 9 12H15z" fill="#c0b8b0"/><circle cx="42" cy="18" r="5" fill="#c0b8b0"/></svg>
        <p class="missing-label">{filename}</p>
      </div>'''
    return re.sub(r'<img[^>]+src="(wp-content/[^"]+)"[^>]*>', replace_img, body_html)

def get_comments(html):
    """Extract comments from Wayback HTML and return formatted HTML string."""
    m = re.search(r'(?:id="comments"|class="[^"]*entry-comments[^"]*")[^>]*>(.*)', html, re.DOTALL)
    if not m: return ''
    mm = re.search(r'<ol[^>]*class="[^"]*comment-list[^"]*"[^>]*>(.*?)</ol>', m.group(1), re.DOTALL)
    if not mm: return ''
    items = re.split(r'(?=<li[^>]*class="[^"]*comment)', mm.group(1))
    result = []
    for item in items:
        if not item.strip(): continue
        name = re.search(r'itemprop="name">(.*?)</span>', item)
        if not name:
            name = re.search(r'<b[^>]*class="[^"]*fn[^"]*">(.*?)</b>', item)
        txt = re.search(r'<div[^>]*class="[^"]*comment-content[^"]*"[^>]*>(.*?)</div>', item, re.DOTALL)
        if not txt:
            txt = re.search(r'<p[^>]*itemprop="text"[^>]*>(.*?)</p>', item, re.DOTALL)
        if name and txt:
            n = re.sub(r'<[^>]+>', '', name.group(1)).strip()
            t = re.sub(r'<[^>]+>', ' ', txt.group(1)).strip()
            result.append(f'      <div class="comment"><strong>{n}</strong><p>{t}</p></div>')
    return '\n'.join(result)
```

**Typical usage sequence:**
```python
raw_html = open('/tmp/postNNN.html', encoding='utf-8', errors='replace').read()
body     = clean_body(raw_html)
body     = body.replace('<!--YT_PLACEHOLDER-->', yt_embed('VIDEO_ID', 'images/thumb.jpg', 'Alt text'))
body     = convert_images(body)
comments = get_comments(raw_html)
html_out = build_post(..., body_html=body, comments_html=comments)
```

---

### MANDATORY post-build validation (run after every build)

**Always run this check** before considering a post done. A non-zero balance means stray WordPress `</div>` tags are in the article — the sidebar and footer will render broken.

```python
def validate_post(path):
    with open(path, encoding='utf-8') as f:
        html = f.read()
    start = html.find('<article class="article">')
    end   = html.find('</article>')
    section = html[start:end]
    opens  = section.count('<div')
    closes = section.count('</div>')
    balance = opens - closes
    status = '✓' if balance == 0 else f'✗ BROKEN (balance={balance})'
    print(f'{status}  {path.split("/")[-1]}')
    return balance == 0

# Example — validate all 9 posts at once:
import os
BASE = '/Users/allyson/Desktop/Brod Trip Website'
for f in ['pick-week-49-losing-control.html', 'enchanted-highway.html', ...]:
    validate_post(f'{BASE}/{f}')
```

**If balance ≠ 0:** the `clean_body()` function did not fully strip WordPress wrapper tags. Debug by printing the last 300 chars of the `clean_body()` output and removing any trailing `</div>`, `</section>`, or `</footer>` tags manually.

---

### Per-post checklist

- [ ] Fetch Wayback archive URL with curl
- [ ] Extract: date, author, series, all paragraph text, all image filenames + positions
- [ ] Identify provided photos (user supplies filename) vs missing (placeholder)
- [ ] Run `clean_body()` → `convert_images()` — never paste raw Wayback HTML directly
- [ ] Replace `<!--YT_PLACEHOLDER-->` with `yt_embed()` if vlog post
- [ ] Build HTML using `build_post()` — body_html is a single string of `<p>` tags + `ph()` / `embed_img()` calls
- [ ] Determine prev/next nav from the `posts[]` array date order
- [ ] `save('slug.html', html)`
- [ ] **Run `validate_post()` — confirm div balance = 0 before proceeding**
- [ ] Update `index.html` builtPages + postThumbs (no thumb entry if no photo recovered)
- [ ] Verify with `grep -n "Expected file:" slug.html`
