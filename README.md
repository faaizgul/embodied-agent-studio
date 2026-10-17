# Embodied Agent Studio

Give an AI agent a 3D body, a voice and a personality, in the browser, using free tools only.

Built for teaching design students how an embodied AI agent is put together. Every layer is visible and editable:

| Layer | What it does | Runs on |
| --- | --- | --- |
| 1. Body | A 3D avatar with blinking, idle movement, facial expressions and lip movement | three.js and three-vrm, in the browser |
| 2. Brain | A large language model that follows a character sheet you write | Google Gemini or Groq free tier (your own key), or rehearsal mode with no key |
| 3. Voice | Speech to text (ears) and text to speech (mouth) | The browser's built-in Web Speech API |
| 4. Stage | Save, share and present the agent; export a single-file demo | Any browser; free hosting such as GitHub Pages |

A "Behind the scenes" log shows each step of every turn: what the ears heard, what was sent to the brain, which expression the face chose, and which voice spoke.

## Quick start

1. Open the studio in **Chrome or Edge** on a laptop (other browsers cannot listen to the microphone).
2. Click **Start conversation**. The built-in character talks in rehearsal mode, with no account needed.
3. In **Brain**, choose Google Gemini and paste a free key from [Google AI Studio](https://aistudio.google.com/apikey). Click **Test the brain**.
4. Fill in the character sheet: job, audience, personality, knowledge and boundaries.
5. In **Body**, load your own avatar (see below), or keep the built-in character and change its colours.
6. In **Voice**, pick a voice and press the mic button on the stage to talk.
7. In **Stage**, export a standalone demo file to share.

## Making an avatar (free)

- **VRoid Studio** (Windows and macOS, free): design an anime-style character, then use *Export as VRM*. VRM avatars get full facial expressions, blinking and lip movement.
- **Any single-file `.glb`**: loads with best-effort expressions if the model has common blendshapes (ARKit or Oculus viseme names).

Check the licence of any avatar you did not make yourself before you show it publicly.

## Free AI keys

| Provider | Where to get a key | Default model in the studio |
| --- | --- | --- |
| Google Gemini | aistudio.google.com/apikey | `gemini-3.8-flash` |
| Groq | console.groq.com/keys | `llama-3.3-70b-versatile` |

Model names change often. If the studio says a model was not found, copy the current free model name from the provider's site into the **Model** box.

Keys are stored only in the browser you typed them into. They are never saved into character files or exported demos.

**Privacy:** free tiers may use what you send to improve their models. Do not put personal data into the character sheet or the conversation.

## Hosting your own copy (GitHub Pages)

1. Create a public GitHub repository and upload `index.html`.
2. In the repository, go to **Settings > Pages**, choose **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. After a minute the studio is live at `https://<your-username>.github.io/<repository-name>/`.

The same works for an exported demo file: rename it `index.html` and upload it to its own repository, or drag it onto Netlify Drop.

You can also open `index.html` directly from your computer in Chrome, though the browser may ask for microphone permission more often.

## How it works

- `index.html` is the whole app: no build step and no server.
- three.js `0.170.0` and `@pixiv/three-vrm` `3.4.5` load from the jsDelivr CDN.
- The brain uses the OpenAI-compatible chat completions format, so most providers work through **Other OpenAI-compatible service**.
- The model is asked to begin each reply with an emotion tag, such as `[happy]`. The studio turns that tag into a facial expression and removes it before speaking.

## Credits and licence

Created by Faaiz Gul for the "AI agents in XR" workshop at Srishti, Bangalore, October 2026.

Released under the MIT licence (see `LICENSE`). Uses [three.js](https://threejs.org) (MIT) and [three-vrm](https://github.com/pixiv/three-vrm) (MIT).
