⛏️ CKPool Solo Mining Tracker
A real-time Bitcoin solo mining dashboard for solo.ckpool.org, built as a single self-contained HTML file. No server, no dependencies to install — just open in a browser.
Features
Live share difficulty tracking — polls the ckpool API every 2 seconds and displays the latest, session best, and all-time best share difficulty
Real-time hashrate — shows current (1m), session best, and 24h average hashrate
Share difficulty chart — accumulates share difficulty history from the start of each round; resets automatically when a new block is found
Session reset on new block — detects new blocks via mempool.space WebSocket (with REST fallback every 20 seconds); resets session best hashrate, session best share, and the chart on each new block
Latest block info — shows height, found time (with elapsed time), pool/miner name, and network difficulty, sourced from mempool.space
Multi-worker support — tracks multiple workers under the same address
Persistent all-time best — stores your all-time best share difficulty in localStorage; survives page reloads and browser restarts
Tab visibility handling — pauses polling when the tab is hidden, resumes and syncs when it comes back into focus
CORS proxy fallback chain — automatically rotates through multiple public CORS proxies (codetabs.com → allorigins.win → thingproxy) if one goes down
Usage
Download ckpool_tracker.html
Open it in any modern browser (Chrome, Firefox, Safari, Edge)
Enter your BTC wallet address and click Save Address
Data starts loading automatically
No installation, no build step, no API key required.
How It Works
Data Sources
Data
Source
Share difficulty, hashrate
solo.ckpool.org/users/{address} via CORS proxy
New block detection
mempool.space WebSocket (wss://mempool.space/api/v1/ws)
Latest block details
mempool.space/api/v1/blocks (REST fallback)
Share Difficulty Model
ckpool reports bestshare — the highest share difficulty submitted in the current session. This tracker stores it across sessions using localStorage to maintain an all-time best value.
Session Reset Logic
A new block is detected when the block height increases. On detection:
Session best hashrate → reset
Session best share difficulty → reset
Share difficulty chart → cleared and starts accumulating from zero
This gives you a clean per-round view of your mining progress.
Block Detection
The tracker listens to the mempool.space WebSocket for real-time block events. If the WebSocket connection drops, a REST poll every 20 seconds acts as a fallback to ensure the new block is never missed for long.
Share Difficulty Chart
The chart accumulates every data point from the moment a new block is found until the next one — showing the full history of a single mining round. Points are downsampled (every other point removed) if the count exceeds 2,000 to prevent memory growth during very long rounds.
Screenshot
(Add your own screenshot here)
Requirements
A modern web browser with JavaScript enabled
Internet access (to reach solo.ckpool.org and mempool.space)
An active worker connected to solo.ckpool.org
Notes
The CORS proxy exposes your wallet address to a third-party server. For privacy, consider hosting your own proxy.
All-time best share is stored in your browser's localStorage under the key ckpool_max_best_share. Clearing browser data will reset it.
This tracker is read-only. It does not submit shares, control your miner, or interact with your wallet in any way.
License
MIT — free to use, modify, and distribute.
Created by nomicode & Claude
