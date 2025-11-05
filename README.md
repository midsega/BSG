# BSG
Browser Sound Generator
# Web Audio Sound Effect Generator

A browser-based playground for designing retro-style sound effects with the Web Audio API. Tweak oscillator, noise, envelope, filter, delay, and LFO parameters, randomize new presets, and capture recordings directly from your browser.

## Features

- **Interactive synthesizer** – Single-page UI with oscillator, noise, ADSR envelope, multimode filter, tempo-synced delay, and LFO controls.
- **Randomize & reset** – Quickly explore new sounds or return to the default preset.
- **Recording workflow** – Capture performances to WebM/Opus and download immediately.
- **Preset management** – Save or load JSON preset files.
- **Toolbox API** – Register reusable helper tools that interact with the audio graph (copy presets, panic stop, macro gestures, etc.).

## Getting Started

The project is a static site served from `index.html`. A small Vite setup is provided for local development.

```bash
npm install
npm run dev
```

Visit the printed URL (defaults to `http://localhost:5173`) to open the interface. Hot reloading is provided by Vite while editing the page.

### Production build

Generate an optimized build with:

```bash
npm run build
```

Artifacts will be written to `dist/`, which can be deployed to any static host. Preview the production build with:

```bash
npm run preview
```

## Toolbox API

Custom tools can be registered at runtime to extend the interface. Each tool appears in the **Tools** panel and runs custom logic when activated.

```js
registerSoundTool({
  id: 'my-tool',
  label: 'Do something',
  description: 'Runs a custom routine.',
  action: ({ context, params, log, stopAll }) => {
    log('Custom tool activated');
    // interact with the generator
    const voice = context.mkVoice({ ...params(), freq: 880 });
    setTimeout(() => voice.stop(), 500);
  }
});
```

Helpers provided to `action` include:

- `context`: access to the lazily-created `AudioContext`, voice helpers, and routing nodes.
- `params()`: function returning current UI parameters.
- `log(message)`: appends a message to the on-screen log.
- `stopAll()`: stops all active voices.

Use `unregisterSoundTool(id)` to remove a tool.

## Project Structure

```
.
├── README.md        # Project documentation
├── index.html       # Application UI and logic
└── package.json     # Dev tooling via Vite
```

## Browser Support

The generator targets modern browsers with Web Audio, MediaRecorder, and Clipboard API support (recent versions of Chrome, Edge, and Firefox). Safari is supported with user-gesture activation of the AudioContext.

## License

MIT