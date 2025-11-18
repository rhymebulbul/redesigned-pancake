```markdown
# DeepSeek-Coder-7B-Instruct Setup on Kubuntu (Any Laptop)

This guide will help you run the **DeepSeek-Coder-7B-Instruct v1.5** model locally using `text-generation-webui`, accessible from other devices on your local Wi-Fi.

---

## 1️⃣ Prerequisites

- Ubuntu / Kubuntu laptop
- Python 3.12+ installed
- Git installed
- Basic terminal familiarity
- Your DeepSeek-Coder-7B-Instruct model folder in **safetensors format**:

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

---

## 2️⃣ Clone the Text Generation WebUI

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

> After this, your prompt should show `(venv)`.

---

## 4️⃣ Install Python dependencies

Install **full dependencies** (recommended for CPU/GPU usage):

```bash
pip install -r requirements/full
```

Verify key packages:

```bash
python -c "import yaml; import torch; import transformers; print('All dependencies installed successfully')"
```

---

## 5️⃣ Place your DeepSeek model

Copy or move your model folder into the `text-generation-webui/models` directory:

```bash
mkdir -p models
cp -r ~/models/deepseek-coder-7b-instruct-v1.5 models/
```

---

## 6️⃣ Start the server (accessible over local network)

```bash
python server.py --model models/deepseek-coder-7b-instruct-v1.5 --listen --host 0.0.0.0 --port 5000
```

* `--listen` allows external connections
* `--host 0.0.0.0` binds to all interfaces
* `--port 5000` (can change if needed)

> Your laptop’s local IP (e.g., `192.168.1.42`) + port 5000 will be used by other devices:

```
http://192.168.1.42:5000
```

---

## 7️⃣ Test the model

From another device on the same Wi-Fi:

```bash
curl http://<YOUR_LAPTOP_IP>:5000/generate \
  -d '{"prompt":"Write a Python function to reverse a string"}'
```

---

## 8️⃣ Optional: Connect Aider for Claude Code–style workflow

1. Install Aider:

```bash
pip install aider-chat
```

2. Initialize in a project folder:

```bash
cd ~/your-code-repo
aider init
aider --model http://<YOUR_LAPTOP_IP>:5000
```

* Now you can **edit, refactor, and generate code interactively** from the terminal.

---

## 9️⃣ Tips

* Ensure your firewall allows **port 5000**.
* For GPU usage, install the appropriate CUDA version if using PyTorch GPU builds.
* For headless laptops, consider using **`screen`** or **`tmux`** to keep the server running.

---

## References

* [Text Generation WebUI GitHub](https://github.com/oobabooga/text-generation-webui)
* [DeepSeek-Coder-7B on Hugging Face](https://huggingface.co/deepseek-ai/deepseek-coder-7b-instruct-v1.5)
* [Aider Chat](https://github.com/YourCode-AI/aider)



```
