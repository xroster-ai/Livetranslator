# 🌐 Aurora — Live Translator

A modern, futuristic **real-time two-way voice translator** that runs entirely in your browser, powered by the [OpenAI Realtime API](https://developers.openai.com/api/docs/guides/realtime) over WebRTC.

Point it at a conversation between two people speaking different languages — say **English ⇄ Chinese** — press **Start**, and it interprets both directions live, speaking the translation aloud and showing the transcript.

## ✨ Features

- **Two-way, hands-free translation.** One person speaks Language A, the other Language B. The interpreter automatically detects which language it hears and speaks the other one back — no buttons to toggle mid-conversation.
- **Bring your own API key.** Enter your OpenAI key once; it's stored only in your browser's `localStorage` and sent directly to OpenAI — never to any third-party server.
- **Any language pair.** Pick from a big preset list or type your own (English, Chinese, Spanish, Japanese, Arabic, …). One click to swap directions.
- **Switch models for cost control.** Choose the cheap `gpt-realtime-mini` (~$0.016/min) for everyday use, or a full model for maximum quality. Legacy `gpt-4o-realtime-preview` models are available as a fallback.
- **Voice selection with live preview.** Pick from 10 voices (including the new **Marin** and **Cedar**) and hit **▶ Preview** to hear a real sample before you start.
- **Tunable response delay.** A slider controls how long a speaker's pause must be before the interpreter jumps in (server-side voice-activity detection).
- **Futuristic UI.** Animated aurora orb, live mic level meter, glassmorphism panels, and a running conversation transcript (what was *heard* vs. the *translation*).
- **Keyboard shortcut.** Press <kbd>Space</kbd> to start/stop.
- **Installable PWA.** Add it to your home screen / desktop and launch it full-screen like a native app; the interface works offline (the OpenAI API itself still needs a connection).

## 🚀 Use it live

Once GitHub Pages is enabled (see **Deployment** below), the app is served at:

**https://xroster-ai.github.io/Livetranslator/**

Then:
1. Open **⚙️ Settings**, paste your OpenAI API key, and click **Save**.
2. Set **Language A** and **Language B**.
3. (Optional) Pick a model, a voice (Preview it), and the response delay.
4. Press **▶ Start listening** and allow microphone access.
5. Speak in either language — the translation is spoken back and transcribed.

> Your API key needs **Realtime API access**. Grab a key at <https://platform.openai.com/api-keys>.

## 🔒 Privacy & security

- The app is 100% static — there is **no backend**. Your key lives in your browser and is used only to open a direct WebRTC/HTTPS connection to `api.openai.com`.
- Because the key is used client-side, only run this with **your own** key on a device you trust. For a shared/public deployment you'd normally mint short-lived *ephemeral* tokens from a small server; that's intentionally omitted here to keep it a zero-backend, personal-use tool.

## 🧠 How it works

1. The browser captures your mic with `getUserMedia` and opens an `RTCPeerConnection`.
2. It creates an `oai-events` data channel and exchanges an SDP offer/answer with the Realtime API (`/v1/realtime/calls` for GA models, `/v1/realtime` for preview models).
3. A `session.update` event configures the interpreter instructions, voice, transcription, and turn-detection delay. Field shapes differ between the GA (`session.audio.*`) and preview (flat) generations — the app sends the right one per selected model.
4. Server-side VAD decides when a speaker has finished; the model responds with translated **audio** (played back) and a **text transcript** (shown on screen).

## 🛠️ Run locally

Because `getUserMedia` requires a secure context, serve it over `localhost`:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## 📦 Deployment (enable GitHub Pages — one time)

The whole app is a single static `index.html`, so GitHub Pages can serve it directly from this branch:

1. Go to **Settings → Pages** in this repository.
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Set **Branch** to `main` and folder to **`/ (root)`**, then **Save**.
4. Wait ~1 minute, then open **https://xroster-ai.github.io/Livetranslator/**.

### Install as an app (PWA)

Once the site is live over HTTPS, it's a full Progressive Web App:

- **Desktop (Chrome/Edge):** click the **⬇ Install app** button in the header, or the install icon in the address bar.
- **Android:** browser menu → *Add to Home screen* / *Install app*.
- **iOS (Safari):** Share → *Add to Home Screen*.

It then launches full-screen with its own icon, and the UI shell is cached for offline use via `sw.js` + `manifest.webmanifest`.

---

Built with the OpenAI Realtime API. Not affiliated with OpenAI.
