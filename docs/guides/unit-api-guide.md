---
title: Rei Unit - API Integration Guide
---

Rei Unit provides an OpenAI-compatible API, which means you can use it with **any tool that supports custom OpenAI endpoints**.

## Compatible Tools

Rei Unit works with any OpenAI-compatible tool, including:

- Cursor
- OpenAI Codex CLI
- Continue (VS Code)
- Aider
- Cline (VS Code)
- Open WebUI
- LM Studio
- And many more...

If a tool lets you set a custom OpenAI base URL, it works with Rei Unit.

## Connection Details

For any tool, you just need these three things:

| Setting              | Value                          |
| -------------------- | ------------------------------ |
| Base URL             | `https://coder.reilabs.org/v1` |
| API Key              | Your Rei Unit API key          |
| **Available Models** |
| Qwen 3 Coder         | `rei-qwen3-coder`              |
| Kimi K2.5            | `rei-coder-lite`               |
| GPT 5.4              | `rei-coder-pro`                |
| Claude Opus 4.6      | `rei-coder-max`                |

Credit consumption will vary based on each model from `rei-qwen3-coder` being the most cost effective to `rei-coder-max` being the most intensive. For in-depth pricing refer to OpenRouter for the most up to date prices.

---

## Setup Instructions

### Cursor IDE

1. Open **Settings** → **Models** → **API Keys**
2. Find **OpenAI API Key** section
3. Click **Override OpenAI Base URL**
4. Enter: `https://coder.reilabs.org/v1`
5. Enter your Rei API key
6. Add model: `rei-qwen3-coder`

### OpenAI Codex CLI

1. Create config file:

```bash
mkdir -p ~/.codex
nano ~/.codex/config.toml
```

2. Add configuration:

```toml
model = "rei-qwen3-coder"

[providers.rei]
name = "Rei Unit"
base_url = "https://coder.reilabs.org/v1"
wire_api = "chat"
env_key = "REI_API_KEY"

[providers.rei.models.rei-qwen3-coder]
```

3. Set your API key:

```bash
echo 'export REI_API_KEY="your-api-key-here"' >> ~/.zshrc
source ~/.zshrc
```

4. Run Codex:

```bash
codex
```

### Continue (VS Code)

1. Install Continue extension
2. Open Continue settings (`~/.continue/config.json`)
3. Add to models array:

```json
{
  "title": "Rei Qwen3 Coder",
  "provider": "openai",
  "model": "rei-qwen3-coder",
  "apiBase": "https://coder.reilabs.org/v1",
  "apiKey": "your-api-key-here"
}
```

### Aider

```bash
aider --openai-api-base https://coder.reilabs.org/v1 \
      --openai-api-key your-api-key-here \
      --model rei-qwen3-coder
```

Or set environment variables:

```bash
export OPENAI_API_BASE=https://coder.reilabs.org/v1
export OPENAI_API_KEY=your-api-key-here
aider --model rei-qwen3-coder
```

### Cline (VS Code)

1. Install Cline extension
2. Open Cline settings
3. Select **OpenAI Compatible** as provider
4. Enter Base URL: `https://coder.reilabs.org/v1`
5. Enter API Key
6. Enter Model: `rei-qwen3-coder`

### Open WebUI

1. Go to **Settings** → **Connections**
2. Add new OpenAI connection
3. URL: `https://coder.reilabs.org/v1`
4. API Key: Your Rei key
5. The model will auto-discover

---

## Troubleshooting

### Test the API directly

```bash
curl -X POST https://coder.reilabs.org/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{"model": "rei-qwen3-coder", "messages": [{"role": "user", "content": "Hello"}]}'
```

You should get a JSON response with the model's reply.

### Common Issues

- **"Invalid API key"** — Check your key is correct, no extra spaces
- **"Model not found"** — Use exactly `rei-qwen3-coder`
- **Connection timeout** — Check your internet connection
