# Phi-3 Mini Fine‑Tuning — Streamlit Demo

This repository contains a minimal Streamlit app (`app.py`) to chat with **microsoft/Phi-3-mini-4k-instruct** and your **fine‑tuned LoRA adapter** (or a merged model) locally.

> You can use the base model from Hugging Face **or** load a **local** base/merged model under `./models/...`. If you have LoRA adapters from your notebook, point the app to that adapter folder to use your fine‑tuned weights.

## Google Colab Notebook

You can run the fine-tuning and inference directly on Google Colab using the link below:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/12WSMqpwqf4W7ADmYl4K1aG7YJTszUrj-#scrollTo=yZoNVRqxMaSG)


##  Folder Structure

```
.
├── models/                      
├── Notebook/                     
├── app.py                      
├── requirements.txt
├── README.md
└── .gitignore
```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **(Optional) If downloading the base model from Hugging Face requires auth:**
   ```bash
   # create a token at https://huggingface.co/settings/tokens
   set HF_TOKEN=hf_xxx          # Windows (cmd)
   export HF_TOKEN=hf_xxx       # macOS/Linux
   ```

## Run the App

```bash
streamlit run app.py
```

Open the URL shown in your terminal (usually http://localhost:8501).

## 🔧 How to point the app to your weights

- **Base model from HF** (default): type `microsoft/Phi-3-mini-4k-instruct` in the sidebar.
- **Local merged model**: put it under `./models/phi3-mini-merged` and set the Base model to that path.
- **LoRA adapter**: put the adapter under `./models/phi3-mini-lora` and set _LoRA adapter path_ to that folder. The app will attach it on top of the base.

> The app attempts to load in **4‑bit** (bitsandbytes) by default to reduce VRAM. If `bitsandbytes` is not available or you are on CPU/Windows without proper CUDA, uncheck the *Load in 4-bit* option.

##  Tips

- If you face OOM, reduce **Max new tokens** or uncheck **4‑bit** and run on CPU (slower).
- You can change the **system prompt** by editing the first message in `st.session_state.messages`.
- If the app can't find your LoRA, make sure the folder contains typical PEFT files like `adapter_config.json` and `adapter_model.safetensors`.

Then in the app, set **Base model** to `models/phi3-mini-merged` and leave **LoRA adapter path** empty.

##  Troubleshooting

- **`bitsandbytes` errors on Windows**: try unchecking 4‑bit loading. If GPU is available, ensure proper CUDA/toolkit drivers.
- **`CUDA out of memory`**: reduce `max_new_tokens`, disable `do_sample` (temperature=0), or switch to CPU.
- **Slow generation**: run on a GPU, enable 4‑bit, and keep `max_new_tokens` modest (e.g., 128–256).

