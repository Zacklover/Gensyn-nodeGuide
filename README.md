# Gensyn-nodeGuide
The cleanest, guide to run  Gensyn RL-Swarm +  on Mac/Linux 
Custom Gensyn RL-Swarm Node Installation Guide (Mac/Linux Edition)Hey swarm! Building on that upgrade guide we whipped up, here's the full fresh install for Gensyn's RL-Swarm node—straight from the official community repo,  for zero headaches. This gets you contributing compute to decentralized AI in ~15-30 mins. We're talking earning testnet points, training models on your idle GPU/CPU, and joining the hive before mainnet drops.Target Setup: Mac (local) or Linux VPS (recommended for 24/7 uptime). Windows? Use WSL2 and treat it like Linux.Why Bother? Gensyn turns your rig into a verifiable ML supercomputer. No Big Tech middlemen—just stake, swarm, and stack rewards.Current Version: CodeZero release (as of Dec 2025). Always peek at the repo for patches.Device/System RequirementsHardware: GPU/CPU with ≥8GB RAM (16GB+ for stability). Idle laptop works, but VPS crushes it.
Access: SSH for VPS (ssh username@ip).
Internet: Stable connection; open ports 3000/5000 if firewalled.

Pre-Requirements: Tools SetupFire these in your terminal. We'll check versions after.For Linux/WSL (Ubuntu/Debian):bash

sudo apt update && sudo apt install -y python3 python3-venv python3-pip curl wget screen git lsof

For Mac:bash

# Install Homebrew first if missing: /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install python

Verify Python (needs 3.10+):bash

python3 --version

Install Node.js (v20.x), npm, & yarn:Linux/WSL:bash

curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt update && sudo apt install -y nodejs
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list > /dev/null
sudo apt update && sudo apt install -y yarn

Mac:bash

brew install node && corepack enable && npm install -g yarn

Check 'em:bash

node -v  # Should be ~20.x
npm -v   # Any recent
yarn -v  # Any recent

Step-by-Step: Install & Launch the NodeDo this in a fresh terminal (or VPS SSH).Start a Screen Session (VPS only—keeps it running detached):bash

screen -S gensyn

Detach later: Ctrl+A then D. Reattach: screen -r gensyn.
Clone the RL-Swarm Repo:bash

git clone https://github.com/gensyn-ai/rl-swarm.git

Hop into the Folder:bash

cd rl-swarm

Create & Activate Virtual Environment:bash

python3 -m venv .venv
source .venv/bin/activate  # Your prompt changes—means it's live

Launch the Swarm Node! bash

./run_rl_swarm.sh

Handle the Prompts (Key Choices):Login Prompt: It'll say hit http://localhost:3000/. On local Mac? Open in browser. On VPS? See troubleshooting below for tunneling.
Push to Hugging Face? Type N (No—keeps it simple).
Model Name: Hit Enter for default (safe bet).
AI Prediction Market? Type Y (Yes—unlocks earnings).

Pro Model Picks (Especially for Mac—avoid custom to dodge terminations):Gensyn/Qwen2.5-0.5B-Instruct
Qwen/Qwen3-0.6B
nvidia/AceInstruct-1.5B
dnotitia/Smoothie-Qwen3-1.7B
Gensyn/Qwen2.5-1.5B-Instruct

Watch the logs scroll—your node is syncing and swarming. It generates files like swarm.pem for your ID.

Post-Install: Wallet & Dashboard SetupHead to Gensyn Dashboard—connect your EOA wallet (MetaMask/Ethers on Sepolia testnet).
Copy your swarm.pem to local if on VPS: scp username@your_ip:~/rl-swarm/swarm.pem ~/swarm.pem.
Submit your Node ID there to claim points.

Troubleshooting: Common Hiccups FixedIssue
Quick Fix
Can't access localhost:3000 on VPS
1. Allow port: sudo apt install ufw -y && sudo ufw allow 22 && sudo ufw allow 3000/tcp && sudo ufw enable.
2. Install Cloudflare Tunnel: wget -q https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && sudo dpkg -i cloudflared-linux-amd64.deb.
3. Run: cloudflared tunnel --url http://localhost:3000 (grab the link it spits out).
"Terminated" Error
In rl-swarm dir: deactivate && rm -rf .venv && python3 -m venv .venv && source .venv/bin/activate && ./run_rl_swarm.sh. Restart 1-2x or wait 5 mins.
Out of Memory (OOM)
Bump to ≥16GB RAM VPS or add swap: sudo fallocate -l 4G /swapfile && sudo chmod 600 /swapfile && sudo mkswap /swapfile && sudo swapon /swapfile.
Node Not Submitting Work
Check ports open (lsof -i :3000), wallet connected, and logs: tail -f ~/.gensyn/logs/*.log.
Daily Restart (Local PC)
cd rl-swarm && python3 -m venv .venv && source .venv/bin/activate && ./run_rl_swarm.sh.

Optional: Level Up with Telegram Bot & RolesWant auto-notifications and swarm roles? (Takes ~10 mins extra.)Install Go:Linux: cd ~ && wget https://go.dev/dl/go1.24.0.linux-amd64.tar.gz && sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.24.0.linux-amd64.tar.gz && echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc && echo 'export GOPATH=$HOME/go' >> ~/.bashrc && echo 'export PATH=$PATH:$GOPATH/bin' >> ~/.bashrc && source ~/.bashrc && go version.
Mac: brew install go.

Set Up Telegram Bot:Chat @BotFather → /newbot → Grab token (e.g., 84100000:AAGITsRXgxxxxNuP56-xxxxxxxxxxxxxxx).
Message your bot, then hit https://api.telegram.org/botYOUR_TOKEN/getUpdates → Copy your chat id.

Install & Config gswarm:bash

go install github.com/Deep-Commit/gswarm/cmd/gswarm@latest
gswarm --version  # Verify
gswarm  # Run: Enter token, chat ID, EOA address from dashboard

Link for Roles:In Gensyn Discord (swarm-link channel): /link-telegram → Get code.
To your bot: /verify {code}.
Boom—roles auto-assigned for exclusive channels/updates.

Next MovesMonitor: Tail logs and check dashboard for points.
Scale: Run multiple nodes? VPS fleet it.
Upgrade Later: Use our previous guide—git pull and venv refresh.

