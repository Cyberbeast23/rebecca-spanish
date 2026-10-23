# Rebecca's Spanish

Weekly beginner Spanish lessons for Rebecca (adult beginner), published with GitHub Pages:
https://cyberbeast23.github.io/rebecca-spanish/

## Layout

```
index.html                     hub — list of lessons, newest first
assets/player.js               shared audio player + record-and-compare (RebeccaSpanishAudio)
assets/autosave.js             autosaves form inputs to localStorage
used-words.txt                 every word/phrase already taught (one per line) — avoid repeats
lessons/YYYY-MM-DD/spanish.html   the weekly lesson
lessons/YYYY-MM-DD/audio/*.mp3    that lesson's audio clips
```

## Adding a weekly lesson

1. **Create** `lessons/YYYY-MM-DD/spanish.html` (date = publish day). Suggested structure:
   theme intro → vocabulary cards (8–12 new words, Spanish / English / pronunciation hint, example sentence)
   → short dialogue → flashcards / matching / multiple-choice / fill-in practice → mini cultural note → review of earlier words.
   Check `used-words.txt` first and don't re-teach words already listed (reviewing them is fine).
2. **Audio**: generate one mp3 per word (and per example sentence, `-ex` suffix) into `lessons/YYYY-MM-DD/audio/`,
   e.g. `01-hola-hello.mp3`, `01-hola-hello-ex.mp3`.
   - Preferred: Edge TTS — `edge-tts --voice es-MX-DaliaNeural --text "Hola" --write-media 01-hola-hello.mp3`
     (or `es-MX-JorgeNeural`, `es-ES-ElviraNeural`, `es-ES-AlvaroNeural`). Use a slower rate for beginners, e.g. `--rate=-15%`.
   - Fallback: gTTS — `gtts-cli -l es "Hola" -o 01-hola-hello.mp3`.
3. **Player**: each vocabulary card gets `data-audio="01-hola-hello.mp3"` and an empty `<div class="practice"></div>`. Load and wire:
   ```html
   <script src="../../assets/player.js"></script>
   <script>RebeccaSpanishAudio.wireLesson({ playAllBtn: "#playAll", stopBtn: "#stopAudio" });</script>
   ```
   Clips resolve relative to the page (`audio/`); override with `wireLesson({ base: "..." })` if needed.
4. **Autosave**: the very last line before `</body>` must be
   ```html
   <script src="../../assets/autosave.js"></script>
   ```
5. **Hub**: in `index.html`, between `<!-- LESSONS-START -->` and `<!-- LESSONS-END -->`, remove the `#no-lessons`
   placeholder (first time only), move the `latest` class/badge off the previous lesson, and add at the top:
   ```html
   <a class="lesson latest" href="lessons/YYYY-MM-DD/spanish.html"><strong>Week N: Theme <span class="badge">NEW</span></strong><span>Date · hola, adiós, gracias…</span></a>
   ```
6. **used-words.txt**: append the new words (one per line, `spanish — english`).
7. **Commit & push** as Cyberbeast23:
   ```
   git -c user.name=Cyberbeast23 -c user.email=Cyberbeast23@users.noreply.github.com commit -am "Add lesson YYYY-MM-DD: Theme"
   git push origin main
   ```
   Pages redeploys from `main` / root within a minute or two.
