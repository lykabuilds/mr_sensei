# Mr Sensei 先生

**A Japanese study game. Your score is your climb up Mt Fuji.**

Mr Sensei turns kana and vocabulary drills into a fast quiz game. You answer by picking one of four colored tiles. Correct answers earn points, and those points carry you up Mt Fuji's ten stations, from 1合目 (1st station) to 山頂 (the summit). Each new station unlocks a fun fact about Japan, and every question ends with a short fact about the character or word you just answered.

▶ **Play it:** `https://<your-username>.github.io/mr-sensei/` *(the link works once GitHub Pages is on, see below)*

---

## Why I built it

I started Japanese classes (JLPT N5 level, *Minna no Nihongo*) and my study routine was passive: reading back over handwritten notes and screenshots. Active recall works better than re-reading, so I turned my class notes into a game I actually want to open every day.

The game is also where I try out a learning workflow: **class materials → structured lesson data → practice → measured results**.

## Features

| Feature | What it does |
|---|---|
| **Multi-select levels and lessons** | Choose かな, N5 and/or N4, then any mix of lessons (hiragana rows, dakuten ゛, handakuten ゜, combos like きゃ, katakana, class vocab, numbers, counters, greetings, verbs, adjectives) |
| **Four-answer quiz** | Colored answer tiles, a countdown timer, more points for faster answers, and streak bonuses |
| **Mt Fuji climb** | Points move you up 10 stations. Stations where you made a mistake turn red, and the station bars shake when you answer wrong |
| **Fun facts** | 豆知識 after every question (kanji origins, look-alike kana, pitch-accent pairs like あめ rain/candy) and 30 Japan facts unlocked by climbing |
| **Fact collection** | Facts you've seen are saved, and each game shows ones you haven't seen yet first |
| **Results and review** | Where you stopped on the mountain, how many stations were left, a correct/mistake strip, accuracy, best streak, and a list of what you missed |
| **Replay misses** | Drill only the items you got wrong |
| **Word library 単語帳** | Every word and kana you've practiced, with your accuracy (e.g. 75%, 3 of 4 correct) and a status: Need work, Learning or Mastered. Filter, search, sort weakest first, and practice your 20 weakest items |
| **Player profile** | Optional nickname, an uploaded photo (cropped and shrunk to 96×96 in the browser, shown only after the player agrees) or one of 12 kanji avatars (山 桜 猫 狐 鯉 月 雪 竹 龍 侍 茶 星). No sign-up: each browser gets a random anonymous player ID |
| **Leaderboard 番付** | Best 10-question game per player over the last 30 days. Shows your own best games until tracking is connected |
| **Stats export** | Copy your per-word stats as CSV to analyze in Excel or Power BI |
| **Pronunciation** | A "Hear it" button uses the browser's built-in Japanese voice (if the device has one) |
| **Settings** | Question direction (Japanese → answer, answer → Japanese, or mixed), number of questions, timer (20s, 10s, 5s or off), romaji hints on or off |

Keyboard: press **1–4** to answer and **Enter** for the next question.

## How it's built

- **One file, no build step.** Plain HTML, CSS and JavaScript in `index.html`.
- **No server and no accounts.** Best scores, collected facts and per-word stats are saved in the browser with `localStorage`.
- **Lesson content lives in a `DECKS` array**, so adding a lesson means adding one entry (see below).
- **The Mt Fuji scene is drawn in SVG**, not a photo, so it can show your progress on the trail.
- **Fonts:** Dela Gothic One, M PLUS Rounded 1c and DM Mono from Google Fonts.

## Anonymous tracking (optional)

Connect the game to a Google Sheet with a small Apps Script to get a global leaderboard and a per-answer log for Power BI. No email, real name or IP address is collected, only a random player ID, the optional nickname and photo, and answer results. Photos can be hidden by the admin from the sheet or removed by the player at any time. Setup takes about 10 minutes: see [SETUP-TRACKING.md](SETUP-TRACKING.md).

## Run it locally

Download the repo and open `index.html` in any modern browser. That's it.

## Put it online with GitHub Pages

1. Push this folder to a GitHub repository (for example `mr-sensei`).
2. In the repository, go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to *Deploy from a branch*. Choose the `main` branch and the `/ (root)` folder, then click **Save**.
4. After a minute or two the game is live at `https://<your-username>.github.io/mr-sensei/`.

## Add a new lesson

Find the `DECKS` array in `index.html` and add an entry:

```js
{id:'n5-l2', level:'n5', name:'Lesson 2 vocab', items:[
  w('ほん','hon','book'),
  w('じしょ','jisho','dictionary'),
]},
```

- `k('か','ka')` adds a kana item. `w('ねこ','neko','cat')` adds a word item.
- `level` is `'kana'`, `'n5'` or `'n4'`.
- To give a word its own fun fact, add it to `WORD_FACT`. Words without one get an automatic breakdown of their sounds.

## Roadmap

- [ ] **Daily Climb**: the same 7 questions for everyone each day, picked by the date
- [x] **Per-word stats export** (CSV)
- [x] **Anonymous tracking** to Google Sheets (Apps Script) with a global leaderboard
- [ ] **Power BI dashboard** of accuracy over time and most-missed kana, built from the exported stats
- [ ] **Lesson pipeline**: class screenshots → AI transcription → new lesson entries automatically
- [ ] More *Minna no Nihongo* lessons as my class covers them

## Screenshots

*Add screenshots here: `docs/setup.png`, `docs/play.png`, `docs/results.png`.*

<!-- ![Setup](docs/setup.png) ![Play](docs/play.png) ![Results](docs/results.png) -->

## Credits

- Designed and directed by **Lyka Marie Ganotisi**. Built with the help of Claude (Anthropic) as an AI coding partner.
- Vocabulary follows my JLPT N5 class notes, based on the *Minna no Nihongo* curriculum. This project is not affiliated with its publisher.
- Fun facts were checked for general accuracy, but please open an issue if you spot a mistake.

## License

[MIT](LICENSE)
