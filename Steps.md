Here’s your **updated and corrected setup guide** reflecting the latest steps you’ve taken on Kubuntu/Linux, including using `ollama serve` style GPU-backed local LLM hosting. I’ve kept it clean and step-by-step:

```markdown
# DeepSeek-Coder-7B-Instruct Setup on Kubuntu/Linux (Any Laptop)

This guide helps you run **DeepSeek-Coder-7B-Instruct v1.5** locally using `text-generation-webui`, accessible from other devices on your local Wi-Fi. GPU support is optional but recommended.

---

## 1️⃣ Prerequisites

- Kubuntu / Ubuntu laptop
- Python 3.12+
- Git installed
- Terminal familiarity
- Model in **safetensors format**:

```

~/models/deepseek-coder-7b-instruct-v1.5/

```

- Folder contents example:

```

config.json
generation_config.json
model-00001-of-00003.safetensors
model-00002-of-00003.safetensors
model-00003-of-00003.safetensors
tokenizer.json

````

- NVIDIA GPU recommended for faster inference (CUDA 13.0+ supported in your setup)

---

## 2️⃣ Clone Text Generation WebUI

```bash
git clone https://github.com/oobabooga/text-generation-webui
cd text-generation-webui
````

---

## 3️⃣ Create and activate Python virtual environment

```bash
python3 -m venv venv
source venv/bin/activate
```

> Your prompt should now show `(venv)`.

---

## 4️⃣ Install dependencies

```bash
pip install -r requirements/full
```

Verify:

```bash
python -c "import torch; import transformers; import yaml; print('Dependencies OK')"
```

---

## 5️⃣ Place your model in the WebUI directory

```bash
mkdir -p models
cp -r ~/models/deepseek-coder-7b-instruct-v1.5 models/
```

---

## 6️⃣ Start the server

```bash
python server.py \
  --model models/deepseek-coder-7b-instruct-v1.5 \
  --listen --host 0.0.0.0 --port 5000
```

* `--listen` → allows connections from other devices
* `--host 0.0.0.0` → binds to all interfaces
* `--port 5000` → can be changed

> On another device in the same Wi-Fi:

```
http://<YOUR_LAPTOP_IP>:5000
```

---

## 7️⃣ Alternative: Using Ollama-style GPU server (Linux)

If using **Ollama LLM server**:

```bash
# Start the server
ollama serve
```

* Logs will show GPU detected and server listening on `0.0.0.0:11434`.
* Test connectivity:

```bash
curl http://localhost:11434
```

* Stop the service if needed:

```bash
sudo systemctl stop ollama
sudo systemctl daemon-reload
```

> Ollama automatically detects GPUs and can handle VRAM-limited environments.

---

## 8️⃣ Test the model

From another device:

```bash
curl http://<YOUR_LAPTOP_IP>:5000/generate \
  -d '{"prompt":"Write a Python function to reverse a string"}'
```

---

## 9️⃣ Optional: Connect Aider for Claude Code–style workflow

1. Install:

```bash
pip install aider-chat
```

2. Initialize in a project folder:

```bash
cd ~/your-code-repo
aider init
aider --model http://<YOUR_LAPTOP_IP>:5000
```

* Now you can **edit, refactor, and generate code interactively**.

---

## 🔟 Tips

* Ensure **firewall allows port 5000** (or your chosen port)
* For GPU usage, install matching CUDA version
* For headless setups, use **`screen`** or **`tmux`**
* Ollama automatically restarts on crash (Linux: system service, Mac: toolbar app)

---

## References

* [Text Generation WebUI GitHub](https://github.com/oobabooga/text-generation-webui)
* [DeepSeek-Coder-7B on Hugging Face](https://huggingface.co/deepseek-ai/deepseek-coder-7b-instruct-v1.5)
* [Aider Chat](https://github.com/YourCode-AI/aider)

```

---

✅ This now reflects **both the standard WebUI setup and the Ollama GPU-backed server approach**, with proper Linux instructions for starting/stopping and testing the server.  

If you want, I can **merge Ollama into the WebUI workflow**, so it automatically uses GPU and listens for external connections without running `python server.py` manually. This is ideal for laptops with limited VRAM. Do you want me to do that?
```
