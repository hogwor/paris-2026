# ⚜️ Paris '26 — Saints in Paris (No Steelers Allowed)

An interactive, phone-friendly trip guide for our Paris weekend around the
**NFL Paris Game** (New Orleans Saints vs. Pittsburgh Steelers, Stade de France,
**Sunday, October 25, 2026, 2:30 PM**). Who Dat.

House colors are **Saints black & old gold**, with **Browns brown & orange**
accents throughout (Dawg Pound represent 🐶). There's a rotating *Steelers
slander* box for the brother-in-law, a *Dawg Pound dispatch* box mixing Browns
hype and self-aware pain, and an allegiance strip repping the Saints, the Browns,
and one crossed-out Steelers badge. All in good fun. Mostly.

Built as a single static web page — no build step, no dependencies — so it's
easy to host on **GitHub Pages** and share one link with the whole group.

## What's inside

- **Itinerary** — Friday→Monday plan with tap-to-pick options per day
- **Train** — Kaiserslautern → Paris (Gare de l'Est) high-speed details + booking links
- **Where to Stay** — Airbnb shortlist (3–4 BR for 8), all near the RER B for game day
- **Booking Tracker** — checklist with a progress bar
- **Group Vote** — tap what you want to do; tallies rank the options
- **Packing & To-Do** — check-off list with progress
- **Live countdown** to kickoff

All picks, checkboxes, and votes are saved **on each person's own device**
(browser `localStorage`). There's no server, so nothing syncs between phones —
share the final calls in the group chat.

## How to publish it (GitHub Pages)

1. Create a new GitHub repo (e.g. `paris-2026`) and push these files.
   ```bash
   git init
   git add .
   git commit -m "Paris 2026 trip app"
   git branch -M main
   git remote add origin https://github.com/<you>/paris-2026.git
   git push -u origin main
   ```
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment → Source**, choose **GitHub Actions**.
   (The included workflow in `.github/workflows/deploy.yml` handles the rest.)
4. Wait ~1 minute. Your link will be:
   `https://<you>.github.io/paris-2026/`
5. Share that link with the group. 🎉

> Prefer the simplest route? You can instead pick **Deploy from a branch →
> `main` / root** in Settings → Pages and skip the Actions workflow entirely —
> since this is just `index.html`, that works too.

## Editing

Everything is in **`index.html`** — content, styles, and script in one file.
- Itinerary options, checklist, packing, and vote items are plain arrays/markup
  near the top of the `<script>` block and in the itinerary section — easy to tweak.
- Change the trip dates or kickoff time in the `countdown()` function.

## Local preview

Just open `index.html` in any browser, or run a tiny local server:
```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

---
*Cleveland Crawfish travel dept · ⚜️ Who Dat · 🐶 Go Browns · 🚫 No Steelers.*
