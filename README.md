# Word Hunt

A hands-off Twitch chat word guessing game for OBS. Viewers type guesses in chat whenever they like, and the first correct guess wins the round.

- Each word starts with one random letter showing.
- More letters can appear on a timer, but the last letter is never revealed.
- The winner goes on the on-screen leaderboard, and the next word starts a few seconds later.
- No commands, mods, or streamer involvement are needed once it's running.

## Contents

- [Requirements](#requirements)
- [Step 1: Save the file and set your channel](#step-1-save-the-file-and-set-your-channel)
- [Step 2: Add it to OBS](#step-2-add-it-to-obs)
- [Step 3: Set up your word list (optional)](#step-3-set-up-your-word-list-optional)
- [Step 4: Choose the look and game options](#step-4-choose-the-look-and-game-options)
- [Step 5: Connect Firebot (optional)](#step-5-connect-firebot-optional)
- [Troubleshooting](#troubleshooting)

## Requirements

- **OBS Studio**, or another streaming app that supports browser sources
- **The Word Hunt game file**, the `.html` file in this repository
- **A text editor** such as Notepad (Windows), TextEdit in plain text mode (Mac), or VS Code
- **A Google account** (optional), for your own live word list
- **Firebot** (optional), for chat announcements and currency

## Step 1: Save the file and set your channel

Download the game file and put it in a folder you won't move, such as `Documents/Stream Overlays/Word Hunt`. OBS loads it from that location every time.

1. Open the game file in your text editor (right-click it, then **Open with**). Don't double-click it, since that opens it in a web browser instead.
2. Find the **SETTINGS** section near the top of the script.
3. Change `CHANNEL` to your Twitch channel name, in lowercase, between the quotes:
   ```js
   const CHANNEL = "yourchannel";
   ```
4. Add any bot accounts to `IGNORE_USERS` so they can never win. Names go in lowercase, in quotes, separated by commas.
5. Save the file.

The other settings in that section are optional:

| Setting | What it does | Default |
| --- | --- | --- |
| `REVEAL_EVERY_SEC` | Starting seconds between letter reveals (the gear menu can change it later) | `45` |
| `PAUSE_BETWEEN_WORDS_SEC` | How long the answer stays up after a win | `8` |
| `LEADERBOARD_SIZE` | How many names the leaderboard shows | `5` |
| `WORDS_SHEET_URL` | Your Google Sheet link ([Step 3](#step-3-set-up-your-word-list-optional)) | empty |
| `SHEET_CHECK_EVERY_SEC` | How often the sheet is checked for changes | `60` |
| `FIREBOT_PRESET_LIST_ID` | Your Firebot preset list ID ([Step 5](#step-5-connect-firebot-optional)) | empty |

The game reads chat without logging in, so you never need to enter a password or token.

## Step 2: Add it to OBS

1. In OBS, click **+** under Sources and choose **Browser**. Name it "Word Hunt."
2. Check **Local file**, click **Browse**, and select the game file.
3. Set **Width** to `1000` and **Height** to `700`. This leaves room for the Large size, the Wide bar layout, and the gear menu. Empty space is transparent.
4. **Uncheck** "Shutdown source when not visible." If it stays checked, hiding the source stops the game and guesses made while it's hidden are missed.
5. Click **OK**, then drag and resize the game where you want it in your scene.

Within a few seconds the overlay shows a word with one letter. If it says "Connecting to chat" for longer than that, see [Troubleshooting](#troubleshooting).

> [!TIP]
> After editing the file later, open the source's Properties and click **Refresh cache of current page** so OBS loads your changes.

## Step 3: Set up your word list (optional)

Out of the box, the game uses its built-in list of about 100 everyday words. Making your own list in a Google Sheet lets you pick themed words, and you or a mod can change them from any device mid-stream without reloading anything.

### Create the sheet

1. Go to [sheets.google.com](https://sheets.google.com) and create a blank spreadsheet. Give it a name like "Word Hunt Words."
2. Type **Words** in cell A1 as a header. It's skipped by the game.
3. Starting in A2, type one word per cell going down **column A**. Leave the other columns empty.

### Publish it and connect it

1. Go to **File > Share > Publish to web**.
2. Pick your sheet tab, choose **Comma-separated values (.csv)**, and click **Publish**.
3. Copy the link it shows.
4. In the game file, paste that link between the quotes in `WORDS_SHEET_URL`, save, and refresh the browser source.

### Choosing words

- Only single words made of the letters A to Z are used. Phrases, hyphens, apostrophes, and numbers are skipped, because chat guesses are matched one word at a time.
- Words of 5 to 10 letters tend to play best. Very short words get guessed instantly.
- Avoid words people type in normal chat (like "hello" or "stream"), or someone may win by accident.
- A themed list works well, for example a holiday, a season, or a game you play on stream. Aim for at least 50 words so repeats are rare.
- If you later have a list saved as a `.csv` file, you can load it with **File > Import > Upload**, then choose **Replace current sheet**.

### How updates work

The game checks the sheet every 60 seconds and uses new words starting with the next round. Google can take a few minutes to publish your edits, so changes aren't always instant. If the sheet can't be reached, the game uses the last copy it loaded, or the built-in list if it has never loaded the sheet.

> [!IMPORTANT]
> Keep the published link private. Anyone with it can see every upcoming answer. To let a mod add words, share the sheet with them as an editor instead.

## Step 4: Choose the look and game options

All display and game options are set from a gear menu on the overlay, and every choice is saved between streams.

1. Make sure the Word Hunt source is **visible** (eye icon on). A hidden source shows nothing in Interact.
2. Right-click the source and choose **Interact**.
3. Move your mouse over the overlay. A gear appears in its top-right corner. Click it.
4. Tap the options you want; they change right away. Click **Done** when finished.

The gear only appears while your mouse is over the overlay in the Interact window, so viewers normally never see it.

| Option | Choices |
| --- | --- |
| Color | Lavender, Denim, Bubblegum, Forest, Midnight, Halloween |
| Size | Small, Medium, Large |
| Layout | Tall panel, or Wide bar with the leaderboard beside the word |
| Leaderboard | Show or Hide |
| Reveal letters over time | On, or Off (only the starting letter shows) |
| Seconds between reveals | 5 to 600; use − and + or type a number (shown only when reveals are on) |
| Scrambled letters hint | Show or Hide a row of the word's letters in mixed-up order |
| Reset leaderboard | Click twice to confirm |

The menu also shows which word list is in use, how many words it has, and when the sheet was last checked.

## Step 5: Connect Firebot (optional)

The game can only read chat, so [Firebot](https://firebot.app) handles anything that posts in chat or awards currency. Firebot must be running for this to work; the game itself works fine without it.

### Create the preset effect list

1. In Firebot, go to **Preset Effect Lists** and create a new one named "Word Hunt."
2. Under **Arguments**, click **+** three times and name them exactly `username`, `word`, and `wins`.
3. Add an effect such as **Chat** with a message like:
   ```
   $presetListArg[username] got it! The word was $presetListArg[word].
   ```
4. Leave Run Mode on **Sequential (All)** and click **Save**.
5. Reopen the list and expand **How to trigger from Stream Deck**. Copy only the ID at the end of the URL shown, after `/preset/`.
6. Paste that ID between the quotes in `FIREBOT_PRESET_LIST_ID`, save, and refresh the browser source.

### Keep a leaderboard in Firebot (optional)

1. Go to **Currency** and create one, such as "Word Hunt Wins."
2. Set the payout amounts to **0**, and set **Limit** to a large number like `100000`. A limit of 0 blocks every point.
3. In the Word Hunt preset list, add an **Update Currency** effect: choose the currency, Operation **Add**, Target **Single User**, Username `$presetListArg[username]`, Amount **1**.

The on-screen leaderboard and the Firebot currency count separately. They stay matched as long as Firebot is running whenever the game is, and nobody edits points by hand.

**To reset both:** use **Reset leaderboard** in the gear menu, then run a one-time Update Currency effect in Firebot set to **Set**, **All Viewers**, Amount **0**.

## Troubleshooting

| Problem | Fix |
| --- | --- |
| Says "Set CHANNEL at the top of this file" | `CHANNEL` is still the placeholder. Set it in [Step 1](#step-1-save-the-file-and-set-your-channel) and refresh the source. |
| Stuck on "Connecting to chat" | Check the channel name spelling (lowercase, no `#`) and your internet connection, then refresh the source. |
| Correct guesses don't count | Make sure the guesser isn't in `IGNORE_USERS`. The word must appear as its own word in the message. |
| Changes to the file don't show | Properties > **Refresh cache of current page**. |
| Can't find the gear | Turn the source's visibility on, open **Interact**, and hover over the overlay's top-right corner. |
| Bottom of the gear menu is cut off | Make the browser source at least 700 pixels tall. |
| Your sheet words aren't used | Check that the link is the **Publish to web** CSV link, not the normal sheet link. Wait a few minutes for Google to publish edits. |
| The gear menu says "last copy, sheet not reachable" | Google couldn't be reached; the game keeps using the last list it loaded and retries every 60 seconds. |
| Firebot effects never run | Firebot must be running. Check the preset ID has no extra spaces, then test the list with its play button. |
| Chat message works but currency doesn't | In the Currency settings, set **Limit** above 0. In the effect, check Amount is 1 and the right currency is picked. |
| A new word starts every time the source is shown again | Uncheck "Shutdown source when not visible" so the game keeps running while hidden. The leaderboard is saved either way. |
