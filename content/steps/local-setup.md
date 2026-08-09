---
title: "Запуск стека локально"
description: Краткий чек-лист: Docker, Portainer, OpenCode, FLUX.2 klein 4B GGUF.
tags:
  - docker
  - portainer
  - opencode
  - flux
---

# Запуск стека локально

## 1. Docker Desktop

- Скачать и установить: https://www.docker.com/products/docker-desktop/
- Запустить, дождаться статуса "Engine running".

## 2. Portainer

```bash
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/portainer_data \
  portainer/portainer-ce:latest
```

- UI: https://localhost:9443 (задать admin-пароль при первом входе).

## 3. OpenCode

```bash
# Node.js → npm (Windows)
npm i -g opencode-ai
# или
scoop install opencode
choco install opencode
```

- Проверка: `opencode --version`

## 4. FLUX.2 klein 4B GGUF

Скачать 3 файла:

- Диффузия (Q4_0, 2.29 GB): https://huggingface.co/leejet/FLUX.2-klein-4B-GGUF
- Текст-энкодер Qwen3-4B: https://huggingface.co/unsloth/Qwen3-4B-GGUF
- VAE `flux2_ae.safetensors`: https://huggingface.co/Comfy-Org/flux2-klein-4B

## 5. Запуск (контейнер stable-diffusion.cpp)

Образ: `ghcr.io/leejet/stable-diffusion.cpp:master`. Модели положить в одну папку (например `./models`) и смонтировать:

```bash
docker run -d --name sd-server --init --restart=always \
  -v ./models:/models -p 1234:1234 \
  --entrypoint /sd-server \
  ghcr.io/leejet/stable-diffusion.cpp:master \
  -m /models/flux-2-klein-4b-Q4_0.gguf \
  --vae /models/flux2_ae.safetensors \
  --llm /models/qwen3-4b.gguf \
  --host 0.0.0.0 --port 1234
```

- Ту же команду можно развернуть в Portainer (Add container → advanced mode) с маппингом порта `1234`.
- Генерация: `POST http://localhost:1234/v1/images/generations` (OpenAI-совместимо).
- Не хватает VRAM на 3070 → добавить `--offload-to-cpu`.
