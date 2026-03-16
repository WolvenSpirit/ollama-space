# ollama-space
# 1. Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# 2. Start the server and allow outside connections (your local VS Code)
# The OLLAMA_HOST=0.0.0.0 is critical so the port-forwarding works
OLLAMA_HOST=0.0.0.0 ollama serve &

# 3. Pull the 7B coder model
ollama pull qwen2.5-coder:7b

.
​In the Codespace terminal, go to the Ports tab.
​Find port 11434.
​Right-click it and change Port Visibility to Public.
​Copy the "Forwarded Address" (it will look like https://...-11434.app.github.dev).
​In your local Continue config.json, use that URL:
```json

{
      "models": [
          {
                "title": "Cloud Qwen (7B)",
                      "provider": "ollama",
                            "model": "qwen2.5-coder:7b",
                                  "apiBase": "https://symmetrical-succotash-g5w95rpxq653vwjx-11434.app.github.dev"
                                      }
                                        ]
                                        }

}
```

Devcontainer 
```
{
  "name": "llm",
  "image": "mcr.microsoft.com/devcontainers/go:1-1.22-bookworm",
  "customizations": {
    "vscode": {
      "extensions": [
        "golang.Go",
        "continue.continue"
      ]
    }
  },
  "postCreateCommand": "curl -fsSL https://ollama.com/install.sh | sh && OLLAMA_HOST=0.0.0.0 ollama serve > ollama.log 2>&1 &",
  "forwardPorts": [
    11434,
    8080
  ],
  "portsAttributes": {
    "11434": {
      "label": "Ollama API",
      "visibility": "public"
    },
    "8080": {
      "label": "Go Web Server",
      "onAutoForward": "openPreview"
    }
  }
}

```