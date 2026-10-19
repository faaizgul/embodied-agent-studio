# Embodied Agent Studio

Give an AI agent a 3D body, a voice and a personality, in the browser, using free tools only.

**Open it:** https://faaizgul.github.io/embodied-agent-studio/

Built for teaching design students how an embodied AI agent is put together. Every layer is visible and editable:

| Layer | What it does | Runs on |
| --- | --- | --- |
| 1. Body | A 3D avatar with blinking, idle movement, facial expressions, gestures and lip movement | three.js and three-vrm, in the browser |
| 2. Brain | A large language model that follows a character sheet you write | Google Gemini or Groq free tier (your own key), or rehearsal mode with no key |
| 3. Voice | Ears (speech to text) and a mouth (text to speech), with three listening modes | The browser's Web Speech API, or the Gemini voice |
| 4. Stage | Save, share, embed, and bring the agent into the room on a phone | Any browser; free hosting such as GitHub Pages |

A **Behind the scenes** tab logs each step of every turn: what the ears heard, what was sent to the brain, which expression the face chose, and which voice spoke.

## Quick start

1. Open the studio in **Chrome or Edge** on a laptop. Other browsers cannot listen to the microphone.
2. Click **Start conversation**. The agent talks in rehearsal mode, with no account needed.
3. In **Brain**, choose Google Gemini and paste a free key from [Google AI Studio](https://aistudio.google.com/apikey). Click **Test the brain**.
4. Fill in the character sheet: name, job, audience, personality, knowledge and boundaries.
5. In **Body**, pick the robot or the mascot, or load your own avatar.
6. In **Voice**, choose how the agent listens and which voice it uses.
7. In **Stage**, save your project and export a demo to share.

## Body

Three starting points sit at different places on Masahiro Mori's uncanny valley curve:

- **Robot**: clearly non-human, with expressions and gestures (waves hello, nods, shakes its head). Model "RobotExpressive" by Tomás Laulhé, CC0, from the three.js project.
- **Mascot**: stylised and human-like, built from simple shapes. Change its outfit, skin, hair and eye colours.
- **Your avatar**: any `.vrm` or single-file `.glb`.

Where to get your own avatar for free:

- **VRoid Studio**: design an anime-style character and use *Export as VRM*. VRM avatars get full expressions, blinking and lip movement.
- **VRoid Hub**: ready-made VRM avatars. Check each one's licence.
- **MetaPerson Creator**: a realistic avatar from a selfie (first avatar free).
- **MakeHuman for Blender (MPFB)**: realistic humans, with a viseme pack for lip movement. Export as `.glb`.
- **Your own Blender model**: convert it with the VRM Add-on for Blender, or export a `.glb` with ARKit-style shape keys (`jawOpen`, `eyeBlinkLeft`, `mouthSmileLeft`).
- **Free character packs** (Quaternius, Kenney, KayKit, 100 Avatars): most have no face shapes, so the mouth will not move.

## Voice

**Listening modes**

| Mode | How it works | Best for |
| --- | --- | --- |
| Press to talk | The microphone is on only while you press it | A busy classroom; most user control |
| Hands-free turns | After each reply the agent listens again | One-to-one conversations |
| Always listening | The microphone stays on; the agent answers when it hears its name | Kiosks and demos with headphones |

The agent pauses its ears while it speaks, so it does not hear itself. Press the mic while it is talking to interrupt it.

**Voices**

- **Browser voice** (free, instant): in **Microsoft Edge**, choose a voice marked *natural*, such as an "Online (Natural)" English (India) voice. Chrome on Windows only offers older, more robotic voices.
- **Gemini voice** (free tier, uses your Gemini key): very natural, with a written "voice direction" for accent, pace and tone. The agent's emotion for each reply is added automatically. It adds about 4 to 6 seconds before each reply, and free limits are tight. When the limit is reached, the studio falls back to the browser voice for a minute.

## Free AI keys

| Provider | Where to get a key | Default model in the studio |
| --- | --- | --- |
| Google Gemini | aistudio.google.com/apikey | `gemini-3.8-flash` (brain), `gemini-3.8-flash-tts` (voice) |
| Groq | console.groq.com/keys | `llama-3.3-70b-versatile` |

Model names change often. If the studio says a model was not found, copy the current free model name from the provider's site into the **Model** box.

Keys are stored only in the browser you typed them into. They are never saved into project files or exported demos.

**Privacy:** free tiers may use what you send to improve their models. Do not put personal data into the character sheet or the conversation.

## Saving and sharing (student guide)

**Save project** downloads one `.json` file with your character sheet, voice and listening settings, camera, and your avatar file. Use **Open project** on any computer to carry on where you left off.

**Export standalone demo** downloads one `.html` file with the whole studio, your character and your avatar. It opens straight in presentation view. Visitors press **Add AI key** to paste their own free key, or use the script (rehearsal mode). It needs an internet connection.

To share the demo as a link with GitHub Pages:

1. Rename the demo file to `index.html`.
2. Create a free public GitHub repository and upload the file (*Add file*, *Upload files*). The GitHub website accepts files up to 25 MB.
3. Open *Settings*, then *Pages*. Choose *Deploy from a branch*, branch `main`, folder `/ (root)`, and click *Save*.
4. After a minute or two the link appears at the top of the Pages settings: `https://<your-username>.github.io/<repository-name>/`.

If the demo is over 25 MB (large VRoid avatars can be), drag it onto [Netlify Drop](https://app.netlify.com/drop) instead, or export the avatar from VRoid Studio with smaller textures.

## Putting the agent on a website

Host your demo, then paste this into any web page, replacing the address with your demo's link. `?embed` hides the studio controls.

```html
<iframe src="https://your-name.github.io/your-agent/?embed" width="420" height="640"
  style="border:0;border-radius:12px" allow="microphone; camera; autoplay; xr-spatial-tracking"
  title="My agent"></iframe>
```

## Phones and AR

- **Camera view** (any phone or laptop): the agent appears over the live camera. It does not stick to the floor.
- **View in AR** (Android phones with Chrome and Google Play Services for AR): open the hosted link on the phone, press *View in AR*, move the phone slowly to find the floor, and tap to place the agent. Tap again to move it. Choose *Life size* or *Tabletop* in the Stage tab.
- iPhones do not support web AR in Safari yet, so use camera view there.
- Headsets such as Meta Quest support WebXR too. That is planned for the next workshop.

## How it works

- `index.html` is the whole app: no build step and no server.
- three.js `0.170.0` and `@pixiv/three-vrm` `3.4.5` load from the jsDelivr CDN.
- The brain uses the OpenAI-compatible chat completions format, so most providers work through **Other OpenAI-compatible service**.
- The model is asked to begin each reply with an emotion tag, such as `[happy]`. The studio turns that tag into a facial expression, a gesture and a tone of voice, and removes it before speaking.
- With the Gemini voice, the mouth follows the loudness of the audio. With browser voices, it follows the rhythm of the words.

## Credits and licence

Created by Faaiz Gul for the "AI agents in XR" workshop at Srishti, Bangalore, October 2026.

Released under the MIT licence (see `LICENSE`). Uses [three.js](https://threejs.org) (MIT), [three-vrm](https://github.com/pixiv/three-vrm) (MIT), and the RobotExpressive model by Tomás Laulhé (CC0).

Uncanny valley: Mori, M. (1970). Bukimi no tani [The uncanny valley]. *Energy, 7*(4), 33–35. English translation: Mori, M., MacDorman, K. F., & Kageki, N. (2012). The uncanny valley [From the field]. *IEEE Robotics & Automation Magazine, 19*(2), 98–100. https://doi.org/10.1109/MRA.2012.2192811
