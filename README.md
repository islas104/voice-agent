# 🎙️ voice-agent

Real-time voice and multimodal AI agent R&D by [islas104](https://github.com/islas104).

This repo is built on an import of [Pipecat](https://github.com/pipecat-ai/pipecat) (BSD 2-Clause — see [LICENSE](LICENSE)), an open-source Python framework for real-time voice and multimodal conversational agents. We vendor the full framework here so we can modify it freely: services, transports, pipeline internals — everything is editable in-tree.

## What's in here

- `src/pipecat/` — the framework source (our working copy; edit freely)
  - `pipeline/` — the core pipeline/frame processing engine
  - `services/` — AI service integrations (STT, LLM, TTS, and more)
  - `transports/` — audio/video transports (WebRTC, WebSockets, Daily, local audio)
  - `audio/` — VAD, turn-taking, filters, resamplers
- `examples/` — runnable demos, including `examples/foundational/` (numbered, incremental)
- `tests/` — unit tests
- `docs/` — architecture docs

## Local setup

Requires Python 3.11+ and [uv](https://docs.astral.sh/uv/).

```bash
git clone git@github.com:islas104/voice-agent.git
cd voice-agent
uv sync                     # creates .venv, installs pipecat (editable) + dev tools
```

Service integrations are optional extras — install only what you need:

```bash
uv sync --extra openai --extra deepgram --extra cartesia --extra silero
```

Copy `env.example` to `.env` and fill in API keys for whichever services you use.

### Sanity check

```bash
uv run python -c "import pipecat; print('ok')"
uv run pytest tests/ -x -q      # unit tests
```

### Run an example

The foundational examples are numbered and build on each other:

```bash
uv sync --extra webrtc --extra silero --extra openai --extra cartesia --extra deepgram
uv run python examples/foundational/07-interruptible.py
```

Most open a local WebRTC page at http://localhost:7860 to talk to the bot.

## Dev workflow

- Format/lint: `uv run ruff format && uv run ruff check`
- Type check: `uv run pyright`
- Tests: `uv run pytest tests/`
- The framework installs editable, so changes under `src/pipecat/` take effect immediately.

## Upstream

The `upstream` remote points at `pipecat-ai/pipecat` for reference. Our history is independent of theirs, so pulling upstream improvements is done by cherry-pick or manual diff, not merge.

## License

BSD 2-Clause. Original framework copyright Daily (pipecat-ai); see [LICENSE](LICENSE). Modifications in this repo © islas104.
