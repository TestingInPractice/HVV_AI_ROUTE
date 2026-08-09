---
title: "FLUX.2 klein 4B GGUF через Portainer (k5)"
description: Инструкция по поднятию unsloth/FLUX.2-klein-4B-GGUF в контейнере через Portainer.
tags:
  - flux
  - gguf
  - unsloth
  - portainer
---

# FLUX.2 klein 4B GGUF через Portainer (k5)

Поднимаю [unsloth/FLUX.2-klein-4B-GGUF](https://huggingface.co/unsloth/FLUX.2-klein-4B-GGUF) на хосте k5 в контейнере через Portainer. Квантованный GGUF оригинальной [FLUX.2-klein-4B](https://huggingface.co/black-forest-labs/FLUX.2-klein-4B) от Black Forest Labs. Методика Unsloth Dynamic 2.0, инструментарий ComfyUI-GGUF.

## Что фактически взяли

Модель та же (BFL FLUX.2 klein 4B), но не safetensors от BFL, а GGUF-квант от **leejet/FLUX.2-klein-4B-GGUF** (конвертер stable-diffusion.cpp):

- Диффузия: `flux-2-klein-4b-Q4_0.gguf` (2.29 ГБ) — FLUX.2 klein 4B, квант Q4_0
- Энкодер: **Qwen3-4B** — нужен FLUX.2 для текста
- VAE: `flux2-vae.safetensors` — взяли из [Comfy-Org/flux2-klein-4B](https://huggingface.co/Comfy-Org/flux2-klein-4B), т.к. BFL-репозиторий закрыт (401/gated)

Квант Q4_0 выбран для скорости/памяти на RTX 3070.

## Факты о модели

- 4B параметров, rectified flow transformer, текст → изображение + многореференсное редактирование.
- Лицензия Apache 2.0 — можно использовать коммерчески.
- ~13GB VRAM, работает на RTX 3090/4070.
- Официальный репозиторий: https://github.com/black-forest-labs/flux2

## Варианты запуска (из карточки модели)

### Unsloth Studio

```bash
# macOS, Linux, WSL
curl -fsSL https://unsloth.ai/install.sh | sh
unsloth studio -H 0.0.0.0 -p 8888
# открыть http://localhost:8888, найти unsloth/FLUX.2-klein-4B-GGUF
```

```powershell
# Windows
irm https://unsloth.ai/install.ps1 | iex
unsloth studio -H 0.0.0.0 -p 8888
```

Через браузер без установки: https://huggingface.co/spaces/unsloth/studio

### FastModel (Python)

```bash
pip install unsloth
```

```python
from unsloth import FastModel
model, tokenizer = FastModel.from_pretrained(
    model_name="unsloth/FLUX.2-klein-4B-GGUF",
    max_seq_length=2048,
)
```

### Diffusers 🧨 (Flux2KleinPipeline)

```bash
pip install -U diffusers
```

```python
import torch
from diffusers import Flux2KleinPipeline

device = "cuda"
dtype = torch.bfloat16

pipe = Flux2KleinPipeline.from_pretrained("black-forest-labs/FLUX.2-klein-4B", torch_dtype=dtype)
pipe.enable_model_cpu_offload()

prompt = "A cat holding a sign that says hello world"
image = pipe(
    prompt,
    height=1024,
    width=1024,
    guidance_scale=4.0,
    num_inference_steps=4,
    generator=torch.Generator(device=device).manual_seed(0)
).images[0]
image.save("flux-klein.png")
```

## Размеры квантов

| Квант | Размер |
|---|---|
| Q2_K | 1.83 GB |
| Q3_K_S | 2.1 GB |
| Q3_K_M | 2.12 GB |
| Q4_0 | 2.46 GB |
| Q4_K_S | 2.58 GB |
| Q4_K_M | 2.6 GB |
| Q4_1 | 2.69 GB |
| Q5_0 | 2.92 GB |
| Q5_K_S | 3.05 GB |
| Q5_K_M | 3.07 GB |
| Q5_1 | 3.15 GB |
| Q6_K | 3.41 GB |
| Q8_0 | 4.3 GB |
| BF16 / F16 | 7.75 GB |

## Ограничения модели

- Не источник фактов; рендер текста может искажаться.
- Может не совпадать с промптом, зависит от стиля промпта.
- Запрещено: незаконный контент, контент с несовершеннолетними, обманный/вредный контент, неконсенсуальные интимные изображения.

## Статус
- [x] инструкция записана
- [ ] запущено на k5 через Portainer
- [ ] проверено
