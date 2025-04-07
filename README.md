#  Gensyn-ai-RL-Swarm Guide (Mac/Linux) 

</div>

---

## 🖥 Device/System Requirements

> ⚠️ **The Node won't work on low-spec devices. Attempting to run it may crash your system.**

### ✅ Recommended: Use a VPS or High-Spec Machine

Start by accessing your VPS:

```bash
ssh username@your_server_ip
```

---

##  Pre-Requirements

###  Install Python and Essential Tools

#### Linux / WSL:

```
sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof
```

#### macOS:

```
brew install python
```

Verify Python version:

```
python3 --version
```

---

###  Install Node.js, npm & Yarn

#### Linux / WSL:

```
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt update && sudo apt install -y nodejs
```

Install Yarn:

```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list > /dev/null
sudo apt update && sudo apt install -y yarn
```

#### macOS:

```
brew install node && corepack enable && npm install -g yarn
```

Check versions:

```
node -v
npm -v
yarn -v
```

---

<div align="center">

##  Start the Node (Linux / Mac)

</div>

### 1️⃣ Clone RL-SWARM Repository

```
git clone https://github.com/gensyn-ai/rl-swarm.git
```

### 2️⃣ Create a `screen` session (for VPS users)

```
screen -S gensyn
```

### 3️⃣ Navigate to Project Directory

```
cd rl-swarm
```

### 4️⃣ Setup Python Virtual Environment

```
python3 -m venv .venv
source .venv/bin/activate
```

### 5️⃣ Install Dependencies

```
cd modal-login
yarn install
yarn upgrade
yarn add next@latest viem@latest
```

### 6️⃣ Run the Swarm Node

```
cd ..
./run_rl_swarm.sh
```

- On prompt: `Would you like to connect to the Testnet? [Y/n]` → **Enter `Y`**
- Login via Web Popup (or open http://localhost:3000/ manually)
- Enter Email → OTP → Back to Terminal
- You'll receive an `ORG_ID`. **Save it!**
- Next prompt: `Push models to HuggingFace? [y/N]` → **Enter `N`**

🎉 You're all set! Logs will start generating shortly.

---

## 🧭 Manage Your `screen` Session

- Detach: `Ctrl + A`, then press `D`
- Reattach:

```
screen -r gensyn
```

---

<div align="center">

## 🛠️ FAQ & Troubleshooting

</div>

---

### 1️⃣ Access `http://localhost:3000/` on VPS

```
sudo apt install ufw -y
sudo ufw allow 3000/tcp
sudo ufw enable
```

Install and run `cloudflared`:

```
wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb
cloudflared --version
cloudflared tunnel --url http://localhost:3000
```

Access the public URL printed in terminal from your local browser.

---

### 2️⃣ Fix OOM Errors on macOS

Edit `.zshrc`:

```
nano ~/.zshrc
```

Add:

```
export PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.0
export PYTORCH_ENABLE_MPS_FALLBACK=1
```

Reload:

```
source ~/.zshrc
```

---

### 3️⃣ How to Get Node Name

Check the terminal log after login for your `ORG_ID`.

---

### 4️⃣ Backup `swarm.pem` for Future Login

From your local terminal:

```
scp USERNAME@YOUR_IP:~/rl-swarm/swarm.pem ~/swarm.pem
```

---

### 5️⃣ Start Node Next Day

```
cd rl-swarm
python3 -m venv .venv
source .venv/bin/activate
./run_rl_swarm.sh
```

---

### 6️⃣ Node Stuck? Getting GPU/RAM Errors?

This is often due to older hardware or low memory.

1. Stop Node: `Ctrl + C`
2. Reset shell config:

```
nano ~/.bashrc
```

Add:

```
export PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.0
```

Reload:

```
source ~/.bashrc
```

3. Edit Config File:

- **Linux/WSL:**

```
nano ~/rl-swarm/hivemind_exp/configs/gpu/grpo-qwen-2.5-0.5b-deepseek-r1.yaml
```

- **macOS:**

```
nano ~/rl-swarm/hivemind_exp/configs/mac/grpo-qwen-2.5-0.5b-deepseek-r1.yaml
```

Change:

```
vllm_gpu_memory_utilization: 0.2
```

To:

```
vllm_gpu_memory_utilization: 0.4
```

Save with `Ctrl + X`, then `Y` + `Enter`. Restart your terminal and run the node again.

---

### 📚 More Help

- 🔗 Official Docs: https://github.com/gensyn-ai/rl-swarm/tree/brian-address-cpu-only-crashes?tab=readme-ov-file#troubleshooting
