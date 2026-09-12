# Open Day Demo - Cloud Architecture & Web Development

**Remanber to change the ip to your instant ip**
## Check List / Reminders

### Demo Computer (PC 2):
1. Terminal 
2. Open GitHub (https://github.com/PercyChengS/Open-Day-Demo/tree/main) or the html notes downloaded(readme-viewer.html) ready to copy the code
3. Login:
   - 3.1. `ssh root@ur_ip`
   - 3.2. Password: `S33D!Eatt0Fun`
4. Restore the demo:  (Do Once After Each Demo!!)
   ```bash
   cd /var/www && rm -rf album1 && sudo rm -rf /tmp/open-day-demo && sudo git clone https://github.com/PercyChengS/Open-Day-Demo.git /tmp/open-day-demo && sudo cp -r /tmp/open-day-demo/album1/. /var/www/album1 && sudo rm -rf /tmp/open-day-demo && sudo systemctl stop nginx
   ```

### PPT Computer (PC 1):
1. Open the PPT presentation
2. Open the demo website in **chorme browser: http://ur_ip (Remember to change to your ip)
3. Everytime refresh the website perform a **Hard Refresh** (`Ctrl + Shift + R` or Mac: `Cmd + Shift + R`) to force fresh files. Alternatively, open Developer Tools (`F12`), check **Disable cache** in the **Network** tab, and keep the panel open.



---

## Demo Timeline & Instructions

| Time | Instruction | Command |
|------|-------------|---------|
| Min 1 | Hook: Open with familiar app logos (WhatsApp etc.), ask what they have in common | / |
| Min 2 | What Is the Cloud?: Explain architecture (laptop -> server -> devices) and show AWS global map. | / |
| Min 3 | The Server Is Alive: Access Ubuntu server, check status, load default nginx page. | 1. `sudo systemctl start nginx && sudo systemctl status nginx`<br/>2. `Press 'q' to exit status view` |
| Min 3 (con't) - Min 4 | Prepare directory and Launch Photo Album: Present QR code and link for audience to scan and view on phones. | 1. `cd /var/www/ && sudo rm -rf album1 && sudo git clone --no-checkout --depth 1 --filter=blob:none https://github.com/PercyChengS/Open-Day-Demo album1_tmp && cd album1_tmp && sudo git sparse-checkout set bootstrap-album && sudo git checkout main && sudo mv bootstrap-album ../album1 && cd .. && sudo rm -rf album1_tmp` |
| Min 5 | Change 1 (Change background): Navigate to css folder, download new background style, refresh website. | 1. `cd /var/www/album1/css && wget -O style.css https://raw.githubusercontent.com/PercyChengS/Open-Day-Demo/main/style.css` |
| Min 6 | Change 2 (Replace photos, add music): Download new images from GitHub and overwrite index.html with the music version, then refresh website. | 1. `sudo git clone https://github.com/PercyChengS/Open-Day-Demo.git /tmp/demo-assets && sudo cp -r /tmp/demo-assets/SupportDoc/* /var/www/album1/images/ && sudo rm -rf /tmp/demo-assets && cd /var/www/album1 && sudo wget -O index.html https://raw.githubusercontent.com/PercyChengS/Open-Day-Demo/main/SupportDoc/index_v2_Photo_music.html` |
| Min 7 | Change 3 (Add weather forecast): Download and overwrite index.html with HKO public API version, then refresh website. | 1. `cd /var/www/album1 && sudo wget -O index.html https://raw.githubusercontent.com/PercyChengS/Open-Day-Demo/main/SupportDoc/index_v3_weather.html` |
| Min 8 | Change 4 (Add live chat): Download and overwrite index.html with Firebase chat version, then refresh website. | 1. `cd /var/www/album1 && sudo wget -O index.html https://raw.githubusercontent.com/PercyChengS/Open-Day-Demo/main/SupportDoc/index_v4_chat.html` |
| Min 9 | What Just Happened: Diagram showing flow from Laptop -> Cloud -> Phone. | / |
| Min 10 | Why This Matters: Career relevance and student's before-and-after learning transformation. | / |
| Min 11 | Closing: QR code still live, final concluding statement. | / |
---

## Overview
This demo showcases fundamental cloud architecture and web development concepts through an interactive photo album application. Participants will learn how web servers work and how changes made on a server instantly update across all connected devices.

## Key Concepts Covered
- **Cloud Architecture**: Understanding the relationship between clients (laptops/phones) and servers
- **Web Server Management**: Starting and managing nginx web server
- **Version Control**: Cloning repositories with git
- **File Management**: Using command-line tools to modify and deploy web content
- **Dynamic Web Content**: Integrating APIs and updating HTML content

---

### Fix: Disable Browser Cache So Audience Sees Updates Without Manual Refresh

Applies the pre-built nginx config (`nginx-default-album1.conf`) from the repo, which sets `Cache-Control: no-store, no-cache` on `index.html`/`css`/`js` so phones always fetch the latest version after each `wget -O` step.

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.bak && \
sudo wget -O /etc/nginx/sites-available/default https://raw.githubusercontent.com/PercyChengS/Open-Day-Demo/main/nginx-default-album1.conf && \
sudo nginx -t && sudo systemctl reload nginx
```

- Backs up the existing config to `default.bak` first.
- Downloads the no-cache config directly from GitHub raw and overwrites `/etc/nginx/sites-available/default`.
- `nginx -t` validates syntax before `reload`, so a bad download won't take nginx down.
- Run this once before Min 3, so all later Change steps (Min 5–8) show up instantly on audience phones.

---

