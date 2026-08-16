# SIEGE Lab — siegelab.org

Static single-page site for the **Secure and Intelligent Edge Research Lab (SIEGE)**,
University of Maine, Electrical & Computer Engineering.

No build step, no dependencies. Plain HTML + CSS.

## Structure

```
index.html                  the entire site (markup + inline CSS)
assets/siege-logo.png       lab logo, for light backgrounds
assets/siege-logo-light.png same logo recolored for dark backgrounds. Only the
                            wordmark changes: everything right of x=383 has its
                            dark slate and black luminance-inverted to near-white,
                            with the mint kept as-is. The shield left of x=383 is
                            copied through unchanged, so it keeps its original
                            dark color. Used in the hero and footer (always dark),
                            and swapped into the header via <picture> under
                            prefers-color-scheme. Regenerate from the original if
                            the logo changes; x=383 is the midpoint of the empty
                            column gap (358–408) between shield and wordmark.
assets/favicon.png          shield mark, used as the browser-tab icon
assets/team/*.jpg           team portraits (640px, JPEG)
robots.txt                  crawler policy + sitemap pointer
sitemap.xml                 single-URL sitemap
.nojekyll                   tells GitHub Pages to serve files as-is
```

## Local preview

```bash
python3 -m http.server 8000
# open http://localhost:8000
```

## Editing

**Add a team member** — copy a `<article class="member">` block in the Team
section, drop a square portrait into `assets/team/`, and update the name, year,
and research interest.

**Add a publication** — copy an `<article class="pub">` block and place it in
the list so entries stay ordered **newest first**. Fields:

- `.pub__year` — year shown in the left rail
- `.pub__type` — `Journal`, `Conference`, or `Preprint`. Add the matching
  modifier for the badge color: `pub__type--journal` (green) or
  `pub__type--conference` (orange). Preprints use the base grey, no modifier.
- `.tag` — the project codename badge (optional; omit for papers with no codename)
- `.pub__venue` — venue in `<em>`, then the date after the `·` separator
- `.pub__authors` — wrap lab members in `<span class="me">` to bold them
- `.btn` — the paper link

Also bump the `28 publications` / `8 researchers` counts in the hero.

## Deployment

This repo is an organization Pages site, so pushing to `main` publishes to
<https://siege-research.github.io/>.

**Custom domain.** `siegelab.org` currently resolves to the old Google Sites
build. To move it here:

1. Add a `CNAME` file at the repo root containing `www.siegelab.org`.
2. Point the `www` DNS record at `siege-research.github.io`
   (it is currently a CNAME to `ghs.googlehosted.com`).
3. Redirect the apex `siegelab.org` to **`https://`**`www.siegelab.org` —
   the current registrar redirect targets plain `http://`, which costs every
   visitor an extra insecure hop.
4. Enable *Enforce HTTPS* in the repo's Pages settings.

`index.html` already sets `<link rel="canonical">` and `og:url` to
`https://www.siegelab.org/`. If you want to run the GitHub Pages URL as the
primary site instead, change both to `https://siege-research.github.io/` and
update `sitemap.xml` and `robots.txt` to match.

## Content provenance

Team photos, roles, and research interests come from the existing siegelab.org
Google Sites build.

The publication list is seeded from the PI's Google Scholar profile
([`zzXy8kgAAAAJ`](https://scholar.google.com/citations?hl=en&user=zzXy8kgAAAAJ&view_op=list_works&sortby=pubdate)),
sorted newest first. Every **title, venue, date, and author list** was then
verified against Crossref, arXiv, or the publisher DOI rather than taken from
either site as-is — the old Google Sites build listed project codenames with no
venue and no date at all.

Corrections carried over:

- The Efficient-AI paper linked through the UMaine library proxy
  (`ieeexplore-ieee-org.wv-o-ursus-proxy02.ursus.maine.edu`), which fails for
  anyone off campus. It now points at the public IEEE Xplore URL.
- Nick Millet's "Details" link was a placeholder (`http://add`), so no link is
  rendered on his card yet.

Deliberately **not** included from the Scholar profile, since this section
lists papers:

- Patents and patent applications — *Learning memory systems and methods*
  (US App. 19/037,772), US Patents 12,058,238 and 12,049,151.
- The NSF award record for *KIPPER* (Award 2350363).
- The standalone ILASH preprint (arXiv:2412.02116), superseded by the
  IEEE Access version already listed.
- *Hardware Aware Mitigation of Timing Side-Channel Vulnerabilities in Critical
  Infrastructure Software* (ORNL, 2026) — no public URL found, and it appears
  to be the ORNL report version of DISARM (same authors, same topic). Add it
  manually if it should stand as its own entry.
