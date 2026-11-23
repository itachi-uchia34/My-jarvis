# JARVIS app

A voice AI assistant: animated human-face-with-mask avatar, "Hey Jarvis" wake word, spoken replies,
timers, date/time, live weather (Open-Meteo, no key), optional web search for current facts, and a
"Briefing" box for facts you want it to know. Installable as an app (PWA). Zero npm dependencies.

## 1. Run it
1. Install Node 18+.
2. `cp .env.example .env` and put your Anthropic API key in it (console.anthropic.com).
3. `node server.js` then open http://localhost:3000 (use Chrome, Edge or Safari for voice).

## 2. Install it as an app
Open the page on your phone or desktop (must be HTTPS or localhost) and choose
"Install app" / "Add to Home Screen". It then opens full screen like a native app.

## 3. Put it online (so your phone can reach it)
Deploy this folder to any Node host (Render, Railway, Fly.io, a VPS). Set the same variables
from `.env` in the host's settings, start command `node server.js`.
Set `JARVIS_PIN` so strangers can't spend your API credit; the app asks for it once.

## 4. Optional: wrap as an Android/iOS app (Capacitor)
```
mkdir jarvis-mobile && cd jarvis-mobile && npm init -y
npm i @capacitor/core @capacitor/cli @capacitor/android
npx cap init Jarvis com.yourname.jarvis --web-dir=../public
```
In `capacitor.config.json` add `"server": { "url": "https://YOUR-DEPLOYED-URL" }`, then
`npx cap add android && npx cap open android` and build in Android Studio. Add the
RECORD_AUDIO permission to AndroidManifest.xml. Note: browser speech recognition is flaky inside
WebViews; for best results add the `@capacitor-community/speech-recognition` plugin.

## Matrix face
Header buttons: 📷 turns your camera into a live Matrix-code face (tap again to turn it off); 🖼 lets you
pick any photo to render as a Matrix-code face (it talks with the code flickering around the mouth).
Everything is processed on your device; photos are saved only in your browser.

## Loyalty (owner-only Jarvis)
Open the 📚 panel:
- **Owner name / Address me as:** Jarvis greets you by name and title.
- **Memory:** Jarvis saves lasting facts you tell it ("remember that I'm vegetarian") and uses them in later chats.
  See and delete them under "What Jarvis remembers about you", or say "forget ...". They are stored only in your browser.
- **Set PIN / Set passphrase:** locks the app on open, after idle time, and when you say "Jarvis, lock down".
  Unlock with the PIN, or by saying your passphrase. This is a convenience lock for the app on your device, not encryption:
  the passphrase checks the words you say, not whose voice it is.
- Server side, set `JARVIS_PIN` in `.env` to protect your API key from other people.

## Languages (English and Urdu / اردو)
Use the language menu under the orb. In Urdu mode: speech recognition listens in Urdu (ur-PK), Jarvis replies in
Urdu script (right-to-left, Nastaliq font where available), and the wake word works as "ہے جارویس" or "Jarvis".
Voice commands such as "لاک کریں" lock the app. Spoken Urdu replies need an Urdu text-to-speech voice installed on the
device (Android/Chrome and Windows usually have one; if none is found, replies still show as text).
Urdu speech recognition works in Chrome and Edge; Safari support varies.

## Do-everything skills, with a loyalty charter
Tools Jarvis can combine: timers, date/time, weather, web search, reading a web page, calculator,
notes and to-dos (stored on your device), opening links, and drafting an email / WhatsApp / SMS.
Safeguards built in:
- **Approval first.** Opening a link or drafting a message shows an Allow / Deny card (or say "yes" / "no"). Nothing is sent automatically, and Jarvis is told never to claim it is done before you approve.
- **Untrusted content.** Web pages and search results are treated as data, never as instructions, and fetching is blocked for private or internal addresses.
- **Your data stays yours.** The server stores nothing. Memory, notes, to-dos and the activity log live in each user's own browser, so one user's Jarvis can never see another's. The Activity log (📚 panel) lists what Jarvis did.
- **Honest limits.** It cannot buy things, log in to accounts or control other apps, and it declines harmful or illegal requests.
- **Shared server?** Set `JARVIS_PINS` with one PIN per user; requests are also rate limited (30 per minute per address).
The rules live in `SYSTEM` (server.js): edit them to change Jarvis's behaviour.

## Files
- `server.js`  : serves the app, calls Claude, runs tools (weather, time, timers, web search)
- `public/index.html` : the whole UI (face, voice, timers, themes, briefing)
- `public/manifest.json`, `public/sw.js`, icons : make it installable

## Customise
Personality and rules: `SYSTEM` in `server.js`. Model: `MODEL` in `.env`.
Add a skill: add an entry to `TOOLS` and a branch in `runTool`.
