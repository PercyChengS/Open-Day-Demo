# Open Day Demo - Cloud Architecture & Web Development

## Demo Timeline & Instructions

| Time | Instruction | Command |
|------|-------------|---------|
| Min 1 | Hook: Open with familiar app logos (WhatsApp etc.), ask what they have in common | / |
| Min 2 | What Is the Cloud?: Explain architecture (laptop -> server -> devices) and show AWS global map. | / |
| Min 3 | The Server Is Alive: Access Ubuntu server, check status, load default nginx page. | `1. sudo systemctl start nginx`<br/>`2. sudo systemctl status nginx`<br/>`3. Press 'q' to exit status view` |
| Min 4 | Launching the Photo Album: Present QR code and link for audience to scan and view on phones. | `1. cd /var/www/`<br/>`2. sudo git clone https://github.com/ngsanluk/bootstrap-album` |
| Min 5 | Change 1 (Change background): Navigate to css folder, download new background style, refresh website. | `1. cd /var/www/album1/css`<br/>`2. wget -O style.css https://raw.githubusercontent.com/SEEDWanda/CCDemo/main/style.css` |
| Min 6 | Change 2 (Replace photos): Navigate to album1, replace existing images with new ones, refresh website. | `1. scp /Users/**User**/Downloads/images/* root@ur_ip:/var/www/album1/images/` |
| Min 7 | Change 3 (Add weather forecast): Open index.html, insert HKO public API code between <body> tags, save and exit. | `1. cd /var/www/album1`<br/>`2. nano index.html`<br/>`3. Insert the code between <body> and </body>`<br/>`"**Code"`<br/>`4. Press Control+X, Y, Enter` |
| Min 8 | What Just Happened: Diagram showing flow from Laptop -> Cloud -> Phone. | / |
| Min 9 | Why This Matters: Career relevance and student's before-and-after learning transformation. | / |
| Min 10 | Closing: QR code still live, final concluding statement. | / |

---

## Overview
This demo showcases fundamental cloud architecture and web development concepts through an interactive photo album application. Participants will learn how web servers work and how changes made on a server can be instantly reflected across devices.

## Key Concepts Covered
- **Cloud Architecture**: Understanding the relationship between clients (laptops/phones) and servers
- **Web Server Management**: Starting and managing nginx web server
- **Version Control**: Cloning repositories with git
- **File Management**: Using command-line tools to modify and deploy web content
- **Dynamic Web Content**: Integrating APIs and updating HTML content
