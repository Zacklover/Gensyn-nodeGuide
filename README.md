Gensyn Node Setup GuideWelcome to the official community guide for running a Gensyn node! Gensyn is a decentralized protocol that aggregates global compute resources into a unified network for machine learning workloads. By running a node, you contribute to collaborative AI training (like RL Swarm), earn testnet rewards, and help shape the future of open AI. This guide focuses on the RL Swarm application, the primary way to participate in the testnet as of December 2025.Why run a node?  Earn $GENS testnet tokens for compute contributions.  
Access leaderboards and track your performance.  
Join a permissionless network—no waitlist required.

 Disclaimer: This guide is for educational purposes. Always DYOR; running a node involves costs (e.g., VPS/GPU rental). Rewards are not guaranteed.PrerequisitesHardware RequirementsRecommended: GPU with at least 12GB VRAM (e.g., RTX 4090 or A100). Lower-end GPUs (12GB+) work but may limit performance.
CPU/RAM: 6+ cores, 16GB+ RAM.
Storage: 200GB+ SSD (for models and logs).
OS: Ubuntu 22.04 LTS (preferred for stability).
Internet: Stable connection with port 3000 open for tunneling.

SoftwareSSH client (e.g., Terminal on macOS/Linux, PuTTY on Windows).
Hugging Face account (for model access—create a token with "Write" permissions).

Cost EstimateVPS/Cloud GPU: $0.34/hour (e.g., RunPod RTX 4090). Run 12 hours/day to keep costs low ($120/month).

Setup OptionsLocal Machine: If you have a compatible GPU.
VPS (Recommended for beginners): Use providers like DigitalOcean, Contabo, or AWS.
Cloud GPU (Best performance): RunPod or Vast.ai for on-demand GPUs.

Step 1: Set Up Your EnvironmentOption 1: Local Setup (macOS/Linux)Open Terminal.
Update packages:  

sudo apt update

Option 2: VPS/Cloud SetupSign up for a provider (e.g., RunPod).
Create an instance:Ubuntu 22.04.
Add GPU if available.
Fund account (crypto/card accepted).

SSH into your server:  

ssh root@<YOUR_SERVER_IP>

(Password or key as per provider.)

Step 2: Install DependenciesRun these commands on your server/local machine:

# Update system
sudo apt update && sudo apt upgrade -y

# Install core packages
sudo apt install -y python3 python3-venv python3-pip curl wget screen git build-essential gcc g++ lsof nano unzip iproute2

# Install Node.js and Yarn
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs
npm install -g yarn

Verify:  

node -v  # Should show v22.x
yarn -v  # Should show latest

Optional: Install tunneling tool (e.g., ngrok for login access):  

# For ngrok (sign up at ngrok.com for free auth token)
wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
tar xvzf ngrok-v3-stable-linux-amd64.tgz
sudo mv ngrok /usr/local/bin/
ngrok authtoken <YOUR_NGROK_TOKEN>

Step 3: Clone and Set Up RL SwarmCreate a screen session (keeps your node running in background):  

screen -S gensyn

Clone the official repo:  

git clone https://github.com/gensyn-ai/rl-swarm.git
cd rl-swarm

Create a Python virtual environment:  

python3 -m venv .venv
source .venv/bin/activate

Install Python dependencies:  

pip install -r requirements.txt

Step 4: Configure and Run the NodeMake the run script executable:  

chmod +x run_rl_swarm.sh

Start the swarm:  

./run_rl_swarm.sh

Authentication:  The script will prompt login at http://localhost:3000.  
If on VPS/cloud, use tunneling:  

# In a new terminal (or detach screen with Ctrl+A+D, then screen -r gensyn)
ngrok http 3000

Copy the ngrok URL (e.g., https://abc123.ngrok.io) and open in browser.

Log in with GitHub/Hugging Face (first time setup).  
Paste your Hugging Face token when prompted.  
Confirm with 'Y' and Enter.

Wallet/Key Generation:  The node will generate a swarm.pem key (your identity—back it up!).  
Copy to local: scp root@<IP>:~/rl-swarm/swarm.pem ~/Downloads/  
Note your Node ID and Node Name (displayed in terminal, e.g., "whistling hulking armadillo"). Save these for dashboard tracking!

Detach screen: Ctrl+A+D. Reattach anytime: screen -r gensyn.

Your node is now running! It will connect to the swarm, download models (e.g., Qwen2.5-0.5B), and start contributing.Step 5: Monitoring and MaintenanceLogs: Check in screen session: tail -f logs/swarm.log.  
Dashboard: Visit Gensyn Testnet Dashboard and search by Node ID. Track wins, leaderboard position, and rewards.  
Updates: Pull latest repo changes: git pull origin main && pip install -r requirements.txt. Restart with ./run_rl_swarm.sh.  
Models: Edit config.yaml for custom models (e.g., add larger ones like 72B for better rewards).  
Stop Node: screen -r gensyn, then Ctrl+C. For cloud, shut down pod to save costs.

Leaderboard TipsRun consistently for higher scores.  
Use efficient models to avoid OOM errors.  
Join swarms like BlockAssist (Minecraft AI training—no node needed for playtesting).

TroubleshootingIssue
Solution
Batch size error
Edit config.yaml: Set batch_size: 1 or lower. Restart.
Tunneling fails
Try alternatives: npx localtunnel --port 3000 or Cloudflared. Ensure ports open.
Key generation fails
Delete .venv and reinstall. Run python generate_keys.py manually.
No rewards/leaderboard
Verify Node ID in dashboard. Wait 10-15 mins for sync. Check Discord for swarm status.
GPU not detected
Install CUDA: sudo apt install nvidia-cuda-toolkit. Run nvidia-smi.
Out of memory
Reduce model size in config or upgrade GPU.
Login loop
Clear browser cache. Use incognito. If stuck, Ctrl+A+D, setup ngrok, retry step 4.

Common Fixes Script: For quick error resolution, run this one-liner (from community):  

curl -sSL https://raw.githubusercontent.com/zunxbt/installation/main/node.sh | bash

Community ResourcesOfficial Docs: docs.gensyn.ai  
Testnet: gensyn.ai/testnet  
Discord: Join for support and updates.  
GitHub Repo: github.com/gensyn-ai/rl-swarm  
Twitter/X: Follow @gensyn_ai
 for announcements.

Contributing BackFound a bug? Improve this guide? Fork this repo and submit a PR!  Happy swarming!  Let's decentralize AI together. Questions? Ping us in Discord.Last updated: December 4, 2025. Gensyn Testnet Phase: RL Swarm v2 (GenRL backend with >100 environments).

