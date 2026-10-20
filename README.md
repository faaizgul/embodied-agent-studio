# Embodied Agent Studio

Give an AI agent a 3D body, a voice and a personality, in the browser, using free tools only.

**Open it:** https://faaizgul.github.io/embodied-agent-studio/

Built for teaching design students how an embodied AI agent is put together. Every layer is visible and editable:

| Layer | What it does | Runs on |
| --- | --- | --- |
| 1. Body | A 3D avatar with blinking, idle movement, facial expressions, gestures and lip movement | three.js and three-vrm, in the browser |
| 2. Brain | A large language model that follows a character sheet you write | Google Gemini or Groq free tier (your own key), Claude or OpenAI (paid key), or rehearsal mode with no key |
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

## Body behaviour: gestures and emotions

The agent's body moves with what it says. The stage shows two live labels next to the status: **Feeling** (the emotion the brain chose for the face) and **Gesture** (what the hands are doing).

Gestures: wave, namaste, point, open palms, shrug, hand to chin, nod and counting on fingers (1 to 5). Try each one from the Body tab.

Switch each behaviour on or off in **Body > Body behaviour** to compare how alive, or how uncanny, the agent feels:

| Behaviour | What it does |
| --- | --- |
| Idle movement | Breathing, small sways and glances. Blinking always stays on. |
| Gestures with emotions and words | A hand to the chin while thinking; open palms when happy; a shrug when concerned; pointing when the reply gives directions; counting when it mentions a number from 2 to 5. |
| Talking hands | Small hand movements that follow the rhythm of the voice. |
| Let the brain choose gestures | The model is told which gestures its body can make and adds a tag such as `[point]` or `[count:3]` when a person would gesture. Off by default. |

You can also choose the greeting (wave, namaste, nod or none) and how big the gestures are.

The agent is always told what body it has and what that body can do, so its words and its body agree.

**Which avatars can gesture.** The mascot, the robot and every VRM avatar can. A `.glb` can if it has a skeleton with standard bone names (Mixamo, Ready Player Me, VRoid, Rigify or MakeHuman); fingers work when the finger bones are there too. A model with no skeleton cannot move its arms, and the Body tab says so. The robot uses its own animations to wave and nod, and its three-fingered hands cannot point or count.

**For the curious:** open the browser console and try `studio.gesture('namaste')`, `studio.gesture('count', 4)` or `studio.emotion('surprised')`.

## Voice

**Listening modes**

| Mode | How it works | Best for |
| --- | --- | --- |
| Press to talk | The microphone is on only while you press it | A busy classroom; most user control |
| Hands-free turns | After each reply the agent listens again | One-to-one conversations |
| Always listening | The microphone stays on; the agent answers when it hears its name | Kiosks and demos with headphones |

The agent pauses its ears while it speaks, so it does not hear itself. Press the mic while it is talking to interrupt it.

**When it decides you have finished.** The studio does not wait for the browser to decide that a sentence is over, which can take several seconds. When your words stop changing for a set pause (0.6, 1 or 1.6 seconds, in the Voice tab), the turn ends and the agent replies. Shorter feels snappier but may cut people off.

**Why a reply takes time.** Each turn has three waits: the pause before your turn ends, the brain thinking, and the voice starting. The *Behind the scenes* tab shows all three for every reply. For the quickest conversation, keep a "lite" Gemini model or Groq for the brain, and use a browser voice. Bigger "thinking" models such as `gemini-3.8-flash` can take 5 to 30 seconds per reply on the free tier, which feels broken in a spoken conversation.

**Voices**

- **Browser voice** (free, instant): in **Microsoft Edge**, choose a voice marked *natural*, such as an "Online (Natural)" English (India) voice. Chrome on Windows only offers older, more robotic voices.
- **Gemini voice** (free tier, uses your Gemini key): very natural, with a written "voice direction" for accent, pace and tone. The agent's emotion for each reply is added automatically. It adds about 4 to 6 seconds before each reply, and free limits are tight. When the limit is reached, the studio falls back to the browser voice for a minute.

## Free AI keys

| Provider | Where to get a key | Default model in the studio |
| --- | --- | --- |
| Google Gemini | aistudio.google.com/apikey | `gemini-3.5-flash-lite` (brain, answers in about a second), `gemini-3.8-flash-tts` (voice) |
| Groq | console.groq.com/keys | `llama-3.3-70b-versatile` (usually the quickest) |
| Anthropic Claude (paid) | console.anthropic.com, with prepaid credits | `claude-haiku-5-5` |
| OpenAI (paid) | platform.openai.com, with prepaid credits | `gpt-6-luna` |

Claude and OpenAI keys are paid API keys. A Claude Pro or Max plan, or a ChatGPT Go, Plus or Pro plan, does not include API use: API keys are billed separately, by use.

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
