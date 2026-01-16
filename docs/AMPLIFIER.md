# Amplifier Setup (OpenAI-Compatible Wrapper)

This repo exposes an OpenAI-compatible API at `http://localhost:8000` including
`/v1/responses`. Microsoft Amplifier expects the Responses API, so it should work
once the base URL points to this server.

## 1) Start the wrapper

```bash
poetry install
poetry run uvicorn src.main:app --reload --port 8000
```

If you want API key protection, set `API_KEY` before starting the server:

```bash
export API_KEY=your-wrapper-key
```

## 2) Initialize Amplifier with the wrapper URL

During `amplifier init`, set the **API base URL** to:

```
http://localhost:8000
```

If you already initialized, re-run `amplifier init` or update the saved config
to point to the same base URL.

## 3) Select the provider/model

Amplifier expects OpenAI-style endpoints, so use the OpenAI provider:

```bash
amplifier provider use openai --model claude-sonnet-4-5-20250929
```

Other Claude models from this wrapper can be used as well (see `/v1/models`).

## 4) (Optional) Set client auth for Amplifier

If the wrapper has `API_KEY` enabled, set an OpenAI-style API key in your shell
before running Amplifier:

```bash
export OPENAI_API_KEY=your-wrapper-key
```

Amplifier will send `Authorization: Bearer $OPENAI_API_KEY` to the wrapper.

## 5) Quick check

```bash
curl -X POST http://localhost:8000/v1/responses \
  -H "Content-Type: application/json" \
  -d '{"model":"claude-sonnet-4-5-20250929","input":"Hello!"}'
```

You should get a JSON response with `"object": "response"` and output text.
