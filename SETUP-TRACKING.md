# Turn on tracking and the global leaderboard

Out of the box, Mr Sensei keeps everything in the player's browser. These steps connect it to a Google Sheet so you get:

- a **global leaderboard** (best 10-question game per player, last 30 days)
- an **`answers` sheet** with one row per question answered, ready for Power BI
- a **`games` sheet** with one row per finished game
- a **`players` sheet** with each player's latest nickname, kanji avatar and optional photo

Time needed: about 10 minutes. Cost: free.

## 1. Create the Google Sheet

1. Go to [sheets.new](https://sheets.new) and name the sheet `Mr Sensei data`.
2. Open **Extensions → Apps Script**.
3. Delete the sample code, then paste in everything from [`apps-script/Code.gs`](apps-script/Code.gs).
4. Click **Save** (disk icon).

## 2. Deploy it as a web app

1. Click **Deploy → New deployment**.
2. Click the gear next to **Select type** and choose **Web app**.
3. Set:
   - **Description:** `Mr Sensei v1`
   - **Execute as:** *Me*
   - **Who has access:** *Anyone*
4. Click **Deploy**, then **Authorize access** and allow it. Google may show "Google hasn't verified this app". That's expected for your own script: click **Advanced → Go to (project name)**.
5. Copy the **Web app URL**. It ends in `/exec`.

Quick test: open the URL in your browser. You should see `{"ok":true,"service":"mr-sensei"}`.

## 3. Connect the game

1. Open `index.html` and search for `const TRACKING_URL=''`.
2. Paste your URL between the quotes:
   ```js
   const TRACKING_URL='https://script.google.com/macros/s/XXXX/exec';
   ```
3. Commit and push. GitHub Pages updates in a minute or two.
4. Play one full 10-question game. The `games` and `answers` sheets appear automatically, and you show up on the leaderboard.

## 4. Updating the script later

If you change `Code.gs`, use **Deploy → Manage deployments → Edit (pencil) → Version: New version → Deploy**. This keeps the same URL. A *New deployment* would give you a new URL that you'd have to paste into the game again.

## What gets collected

| Collected | Not collected |
|---|---|
| Random player ID (e.g. `p_8f3a91c2d4e7`), created in the browser | Email, real name, Google account |
| Optional nickname and avatar number | IP address or location |
| Optional profile photo (96×96, only after the player ticks the consent box) | Original full-size photos |
| Each answer: lesson, kana/word, right or wrong, response time | Device or browser details |
| Each game: score, station reached, settings | |

The game shows players this in **Profile → Privacy**.

## Moderating photos and nicknames

Uploaded photos appear on the public leaderboard, so check the `players` sheet now and then.

| To do this | In the `players` sheet |
|---|---|
| Hide an inappropriate photo or nickname | Put `1` in the **hidden** column. They show as *Guest* with their kanji avatar, even if they update their profile |
| Undo | Change **hidden** back to `0` |
| Delete a player's data on request | Delete their rows in `players`, `games` and `answers` (filter by **player_id**) |

Players can also remove their own photo: **Profile → Remove photo → Save profile** deletes it from the sheet.

Tip: a photo cell holds the picture as text (`data:image/webp;base64,…`). To view one, paste the cell into your browser's address bar.

## Built-in protections

- **Spam limit:** 30 games and 10 profile updates per player ID per hour
- **Photo checks:** only small WebP, JPEG or PNG images are accepted, and only with consent
- **Size limit:** oversized posts are rejected
- **Clean data:** nicknames are limited to letters, numbers, spaces, `_` and `-`; numbers are clamped to sane ranges
- **No formula injection:** text starting with `=`, `+`, `-` or `@` is stored as plain text
- **No duplicates:** if a send is retried, the same game isn't saved twice
- **Private IDs:** the leaderboard never sends player IDs back to the browser

These stop casual abuse, not a determined attacker. Anyone could still post a fake high score, because the game runs in the browser. For a learning project that's an acceptable trade-off. If someone cheats, delete their row in the `games` sheet.

## Power BI

In Power BI Desktop: **Get data → Web**, then use the sheet's CSV export link, or download the sheet as .xlsx. Useful visuals:

- Accuracy by lesson (`answers`: average of `correct`, by `lesson`)
- Most-missed kana (`answers`: count where `correct` = 0, by `japanese`)
- Average response time by kana
- Active players per week (`games`: distinct `player_id` by week)
