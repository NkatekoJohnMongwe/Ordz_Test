# ORDZ SA website

A static, multi-page site for Ordz SA: HTML5, CSS3 and vanilla JavaScript. No build step, no framework.

```
ordz-sa/
├── index.html                 Home
├── sa-hip-hop-history.html    Interactive timeline, regions, sources
├── hip-hop-glossary.html      Searchable A–Z glossary (71 terms)
├── industry-directory.html    Filterable directory
├── classifieds.html           Filterable classifieds + "Post a classified" form
├── about.html
├── contact.html
├── css/style.css
├── js/script.js               All behaviour + Google Analytics event tracking
├── js/data-fallback.js        Copy of /data so pages also work when opened from disk
├── data/*.json                Glossary, directory, classifieds, artists, releases, stories
└── images/                    Logos (4 colourways), favicon, share image
```

## Deploy to GitHub Pages

1. Create a GitHub repository (e.g. `ordz-sa`) and upload everything in this folder to the repository root.
2. Go to **Settings → Pages**. Under **Build and deployment**, choose **Deploy from a branch**, pick `main` and `/ (root)`, then **Save**.
3. After a minute the site is live at `https://<your-username>.github.io/<repo>/`.
4. Custom domain (optional): in **Settings → Pages → Custom domain** enter your domain, then add the DNS records GitHub shows you. Keep the `.nojekyll` file.
5. Replace the placeholder domain `https://www.ordz.online` with your final domain in the `<link rel="canonical">` and Open Graph tags (search the HTML files for `www.ordz.online`).

To test locally with the JSON files loading normally: `python3 -m http.server` in this folder, then open http://localhost:8000.

## Google Analytics (GA4)

The Google tag with Measurement ID **G-ZP013XMK5E** is the first thing in the `<head>` of every page, and the same ID is in `CONFIG` at the top of `js/script.js`.

Events sent (all via `gtag('event', …)`):

| Event | When |
|---|---|
| `whatsapp_click` | Any link to the WhatsApp group |
| `navigation_click` | Header, mobile menu and footer links |
| `social_click` | Social / website / WhatsApp chips on listings, footer social links |
| `glossary_search` | Typing in the glossary search (debounced) |
| `directory_search`, `directory_filter` | Directory search and filters |
| `classified_open_form`, `classified_submit`, `classified_filter` | Classifieds |
| `contact_submit`, `enquiry_type_select` | Contact page |
| `artist_profile_click` | "Discover artist" buttons |
| `colourway_change`, `history_era_select` | Logo colourway picker, timeline tabs |

In GA4 go to **Admin → Events** to mark the important ones as key events.

## Forms

There is no server. The contact form and "Post a classified" form open the visitor's email app with the message filled in, addressed to `CONFIG.contactEmail` in `js/script.js` (currently the placeholder `hello@ordz.online`: change it).

For real submissions, create a free endpoint with Formspree, Basin or Getform and paste its URL into `CONFIG.formEndpoint`. The forms will then POST JSON instead.

## Replacing placeholder content

Everything marked "Sample listing" or "Placeholder" is dummy content that shows the layout. Edit the JSON files in `/data` (or point `loadData()` in `js/script.js` at an API) and the pages update with no HTML changes. Record shapes:

- `directory.json`: `name, category, city, province, description, services[], website, email, phone, instagram, facebook, tiktok, x, whatsapp`
- `classifieds.json`: `title, group, category, location, description, date, contact, whatsapp, email`
- `artists.json`: `name, location, bio, image, url, colour`
- `releases.json`: `artist, title, type, date, cover, url, colour`
- `stories.json`: `headline, category, date, description, url`
- `glossary.json`: `term, category, definition, example, southAfricanContext`

If you edit the JSON, also update `js/data-fallback.js` (or delete it and always serve over http).

Social links in the footer are placeholders (`#`): replace them with real profile URLs.

Future connections (WordPress, Supabase, Firebase, Airtable, Google Sheets, REST APIs): all rendering goes through `loadData(name)`, which returns `{ items: [...] }`. Swap its `fetch` URL for your API and keep the same field names.

## Brand colour code

The four logo colourways carry meaning across the site: red = brand and archive, blue = words and editorial, green = WhatsApp and community, orange = industry and directory. Visitors can switch the colourway from the home page hero; the choice is remembered.

## Content accuracy

The history timeline and glossary are starter copy. Check dates, discographies and attributions against the sources on the history page before launch. The glossary entry for **CAPI** is flagged for editorial confirmation.
