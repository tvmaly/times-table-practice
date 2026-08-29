# Block Blaster Multiplication

Block Blaster Multiplication is a fast, touch-friendly multiplication practice game for kids who would rather blast targets than stare at worksheets. It is built for short homeschool or after-school practice sessions, especially on iPad Safari, and covers facts from `0 x 0` through `12 x 12`.

Kids answer multiplication facts with a large keypad, and every correct answer fires a shot at blocky arcade targets. The game quietly adapts to weak facts, brings missed questions back for review, and rewards faster recall with bonus damage without showing stressful countdown timers.

Parents get a built-in progress dashboard with accuracy, response-time trends, weak facts, mastery states, and session history. Progress stays private in the current browser only: there are no accounts, ads, analytics, backend services, or cross-device syncing.

## Quick start

Open the published GitHub Pages URL:

https://tvmaly.github.io/times-table-practice/

Or run it locally:

```sh
python3 -m http.server 4173
```

Then open `http://127.0.0.1:4173/` in a browser.

## How to play

1. On first launch, enter an optional callsign and tap **Start Recon Mission**.
2. Read the multiplication prompt, type the answer on the keypad, and tap **FIRE**.
3. Correct answers blast blocks. Faster correct answers cause extra damage.
4. If an answer is missed, re-enter the shown correction once to fire a small training shot.
5. Play 5-minute Quick Missions or 10-minute Full Missions to earn coins, unlock maps, and buy stronger blasters.
6. Open **Progress** anytime to review mastery, weak facts, and session history.

## Local testing

Run a simple local web server from the repository root:

```sh
python3 -m http.server 4173
```

Then open:

- Game: `http://127.0.0.1:4173/`
- Built-in tests: `http://127.0.0.1:4173/?test=1`

The game stores progress only in the current browser with `localStorage`. Clearing Safari website data, changing browsers, using private browsing, or changing devices can remove locally stored progress.

## GitHub Pages publishing

Published site URL:

https://tvmaly.github.io/times-table-practice/

1. Create a GitHub repository.
2. Put `index.html` and `README.md` in the repository root.
3. Push or upload the files to the `main` branch.
4. Open **Settings -> Pages**.
5. Under **Build and deployment**, select **Deploy from a branch**.
6. Select `main` and `/ (root)`, then save.
7. Open the published Pages URL after GitHub finishes deployment.
