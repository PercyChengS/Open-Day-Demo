# Open Day Demo - Cloud Architecture & Web Development

## Check List / Reminders (Do Once After Each Demo!!)

### Demo Computer (PC 2):
1. Terminal (Open two tabs)
2. Open GitHub (https://github.com/PercyChengS/Open-Day-Demo/tree/main) ready to copy the code
3. Login:
   - 3.1. `ssh root@47.81.211.14`
   - 3.2. Password: `S33D!Eatt0Fun`
4. Restore the demo:
   ```bash
   cd /var/www && rm -r album1 && sudo git clone https://github.com/PercyChengS/Open-Day-Demo.git /tmp/open-day-demo && sudo cp -r /tmp/open-day-demo/album1/. /var/www/album1 && sudo rm -rf /tmp/open-day-demo && sudo systemctl stop nginx
   ```
5. Ensure the local computer `$env:USERPROFILE/Downloads/SupportDoc` has the demo photos installed from the GitHub repository. Download from https://github.com/PercyChengS/Open-Day-Demo/tree/main

### PPT Computer (PC 1):
1. Open the PPT presentation
2. Open the demo website: http://47.81.211.14
3. In Chrome, press `Ctrl + Shift + I` (Mac: `Cmd + Option + I`) to open Developer Tools. Switch to the Network tab and check Disable cache. As long as this panel is open, refreshing the page guarantees that you will fetch the latest files from the server.

---

## Demo Timeline & Instructions

| Time | Instruction | Command |
|------|-------------|---------|
| Min 1 | Hook: Open with familiar app logos (WhatsApp etc.), ask what they have in common | / |
| Min 2 | What Is the Cloud?: Explain architecture (laptop -> server -> devices) and show AWS global map. | / |
| Min 3 | The Server Is Alive: Access Ubuntu server, check status, load default nginx page. | 1. `sudo systemctl start nginx && sudo systemctl status nginx`<br/>2. `Press 'q' to exit status view` |
| Min 3 (con't) | Prepare directory for deployment. | 1. `cd /var/www/ && rm -r album1` |
| Min 4 | Launching the Photo Album: Present QR code and link for audience to scan and view on phones. | 1. `cd /var/www/ && sudo git clone https://github.com/ngsanluk/bootstrap-album /var/www/album1` |
| Min 5 | Change 1 (Change background): Navigate to css folder, download new background style, refresh website. | 1. `cd /var/www/album1/css && wget -O style.css https://raw.githubusercontent.com/SEEDWanda/CCDemo/main/style.css` |
| Min 6 | Change 2 (Replace photos, add music): Navigate to album1, replace existing images with new ones, refresh website. | **Action: run the command on !!LOCAL!! (Depending on your OS)**<br/>- For Windows (PowerShell):<br/>1. `scp $env:USERPROFILE\Downloads\SupportDoc\* root@47.81.211.14:/var/www/album1/images/`<br/>- For Mac / Linux:<br/>1. `scp ~/Downloads/SupportDoc/* root@47.81.211.14:/var/www/album1/images/`<br/>2. Password:`S33D!Eatt0Fun`<br/><strong>Run On Server</strong><br/>3. `cd /var/www/album1 && rm -r index.html && nano index.html`<br/>4. [Replace the whole code from GitHub ↓](#min-6-replace-photos--add-music)<br/>5. `Press Control+X, Y, Enter` |
| Min 7 | Change 3 (Add weather forecast): Open index.html, insert HKO public API code, save and exit. | 1. `cd /var/www/album1 && nano index.html`<br/>2. [Insert the code after `<!-- MUSIC PART END -->` from GitHub ↓](#min-7-hko-weather-forecast-api-code)<br/>3. `Press Control+X, Y, Enter` |
| Min 8 | Change 4 (Add live chat): Open index.html, insert Firebase chat code, save and exit. | 1. `cd /var/www/album1 && nano index.html`<br/>2. [Insert the code after `<!-- MUSIC PART END -->` from GitHub ↓](#min-8-live-chat)<br/>3. `Press Control+X, Y, Enter` |
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

## How to Customize

---

### Min 6: Replace Photos & Add Music
Replace the whole code with the following in `/var/www/album1/index.html`
[↑ Back to Timeline](#demo-timeline--instructions)
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Bootstrap Album Demo</title>
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65"
      crossorigin="anonymous"
    />
    <link rel="stylesheet" href="./css/style.css" />
    <style>
      /* QR Toggle Wrapper */
      #qr-toggle-wrap {
        position: fixed;
        top: 16px;
        right: 16px;
        z-index: 99999;
        display: flex;
        flex-direction: column;
        align-items: flex-end;
      }

      /* Toggle Button */
      #qr-btn {
        background: #fff;
        border: none;
        border-radius: 50%;
        width: 48px;
        height: 48px;
        cursor: pointer;
        box-shadow: 0 2px 10px rgba(0,0,0,0.28), 0 1px 3px rgba(0,0,0,0.14);
        font-size: 1.4rem;
        display: flex;
        align-items: center;
        justify-content: center;
        transition: transform 0.18s ease, box-shadow 0.18s ease;
        -webkit-tap-highlight-color: transparent;
      }

      #qr-btn:active {
        transform: scale(0.92);
        box-shadow: 0 1px 4px rgba(0,0,0,0.18);
      }

      /* QR Code Panel */
      #qrcode-corner {
        display: none;
        margin-top: 10px;
        width: 150px;
        height: 150px;
        border-radius: 12px;
        box-shadow: 0 4px 18px rgba(0,0,0,0.28), 0 1.5px 4px rgba(0,0,0,0.14);
        background: #fff;
        padding: 6px;
        object-fit: contain;
        /* Animate open/close */
        animation: qr-fadein 0.18s ease;
      }

      @keyframes qr-fadein {
        from { opacity: 0; transform: translateY(-8px) scale(0.95); }
        to   { opacity: 1; transform: translateY(0) scale(1); }
      }

      /* On larger screens, show QR by default */
      @media (min-width: 768px) {
        #qrcode-corner {
          display: block;
        }
        #qr-btn {
          display: none;
        }
      }
    </style>
  </head>
  <body>

    <!-- QR Code: Toggle on mobile, always visible on desktop -->
    <div id="qr-toggle-wrap">
      <!-- Toggle button (mobile only) -->
      <button id="qr-btn" onclick="toggleQR()" aria-label="顯示 QR Code" title="Show QR Code">
        📷
      </button>
      <!-- QR Code image -->
      <img
        id="qrcode-corner"
        src="./images/qrcode.png"
        alt="QR Code"
        width="150"
        height="150"
      />
    </div>

    <h1>Photo Album Demo</h1>

    <!-- ===== MUSIC PART START ===== -->
    <audio id="bgMusic" loop muted autoplay>
      <source src="./images/demo.mp3" type="audio/mpeg" />
    </audio>

    <div class="text-center my-2" style="position: relative; z-index: 99999;">
      <button id="musicToggle" onclick="toggleMusic()" style="
        position: relative;
        z-index: 99999;
        background: rgba(0, 0, 0, 0.65);
        color: #ffffff;
        border: 2px solid #ffffff;
        border-radius: 30px;
        padding: 10px 28px;
        font-size: 1.1rem;
        font-weight: bold;
        cursor: pointer;
        letter-spacing: 1px;
        box-shadow: 0 0 12px rgba(255,255,255,0.5), 0 0 24px rgba(255,255,255,0.2);
        backdrop-filter: blur(4px);
        -webkit-backdrop-filter: blur(4px);
        transition: all 0.2s ease;
      ">
        🔊 Music ON
      </button>
    </div>
    <!-- ===== MUSIC PART END ===== -->

    <hr />

    <div class="container text-center">
      <div class="row justify-content-md-center">
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo01.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo02.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo03.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo04.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo05.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo06.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo07.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo08.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo09.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo10.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo11.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo12.png" alt="photo" />
        </div>
      </div>
    </div>

    <hr />

    <script
      src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js"
      integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4"
      crossorigin="anonymous"
    ></script>

    <!-- ===== QR TOGGLE JS ===== -->
    <script>
      let qrOpen = false;

      function toggleQR() {
        const qr  = document.getElementById('qrcode-corner');
        const btn = document.getElementById('qr-btn');
        qrOpen = !qrOpen;

        if (qrOpen) {
          qr.style.display = 'block';
          // Re-trigger animation
          qr.style.animation = 'none';
          qr.offsetHeight; // reflow
          qr.style.animation = '';
          btn.textContent = '✕';
          btn.setAttribute('aria-label', '關閉 QR Code');
        } else {
          qr.style.display = 'none';
          btn.textContent = '📷';
          btn.setAttribute('aria-label', '顯示 QR Code');
        }
      }

      // Close QR panel if user resizes to desktop (≥768px)
      window.addEventListener('resize', function () {
        const qr  = document.getElementById('qrcode-corner');
        const btn = document.getElementById('qr-btn');
        if (window.innerWidth >= 768) {
          qr.style.display = 'block';
          btn.style.display = 'none';
          qrOpen = true;
        } else {
          btn.style.display = 'flex';
          if (!qrOpen) qr.style.display = 'none';
        }
      });
    </script>

    <!-- ===== MUSIC JS START ===== -->
    <script>
      const audio = document.getElementById('bgMusic');
      const btn = document.getElementById('musicToggle');
      let isPlaying = false;

      // 第一次任何點擊 → 解除靜音並自動播放（視為默認 ON）
      document.addEventListener('click', function startOnInteraction() {
        audio.muted = false;
        audio.play().then(() => {
          isPlaying = true;
          btn.textContent = '🔊 Music ON';
        }).catch(() => {});
        document.removeEventListener('click', startOnInteraction);
      }, { once: true });

      function toggleMusic() {
        if (isPlaying) {
          audio.pause();
          isPlaying = false;
          btn.textContent = '🔇 Music OFF';
        } else {
          audio.muted = false;
          audio.play().then(() => {
            isPlaying = true;
            btn.textContent = '🔊 Music ON';
          });
        }
      }
    </script>
    <!-- ===== MUSIC JS END ===== -->

  </body>
</html>
```


---

### Min 7: HKO Weather Forecast API Code

Add the following code after `<!-- ===== MUSIC PART END ===== -->`
[↑ Back to Timeline](#demo-timeline--instructions)
```html
    <!-- HKO 9-Day Weather Forecast Widget -->
    <div id="hko-weather-widget">
      <p id="hko-swipe-hint">
        ← 左右拖動以查看 9 天預報 &nbsp;/&nbsp; Swipe left-right to view 9-day forecast →
      </p>
      <div class="hko-weather-loading">載入天氣預報中...</div>
    </div>

    <style>
      #hko-weather-widget {
        width: 100%;
        overflow-x: auto;
        -webkit-overflow-scrolling: touch;
        background: #174f8f;
        font-family: Arial, "Microsoft JhengHei", "PingFang HK", sans-serif;
        position: relative;
        z-index: 9999;
        padding-bottom: 12px;
      }
      #hko-swipe-hint {
        text-align: center;
        color: rgba(255, 255, 255, 0.75);
        font-size: 14px;
        margin: 6px 0 4px 0;
        padding: 0;
        font-family: Arial, "Microsoft JhengHei", "PingFang HK", sans-serif;
        letter-spacing: 0.5px;
        position: sticky;
        left: 0;
      }
      .hko-weather-row {
        display: flex;
        min-width: 1500px;
        background: #174f8f;
      }
      .hko-weather-card {
        flex: 0 0 166px;
        width: 166px;
        background: linear-gradient(180deg, #1e5a9e 0%, #174f8f 100%);
        color: #ffffff;
        text-align: center;
        padding: 7px 6px 10px;
        border-right: 5px solid rgba(255, 255, 255, 0.22);
        box-sizing: border-box;
        overflow: hidden;
      }
      .hko-weather-card:last-child { border-right: none; }
      .hko-date {
        font-size: 23px;
        font-weight: 700;
        line-height: 1.15;
        margin-bottom: 3px;
        white-space: nowrap;
      }
      .hko-week {
        font-size: 20px;
        font-weight: 700;
        line-height: 1.15;
        margin-bottom: 10px;
        white-space: nowrap;
      }
      .hko-icon {
        width: 84px;
        height: 84px;
        object-fit: contain;
        margin-bottom: 8px;
      }
      .hko-temp, .hko-humidity {
        font-size: 21px;
        font-weight: 800;
        line-height: 1.15;
        white-space: nowrap;
        letter-spacing: -0.5px;
      }
      .hko-temp { margin-bottom: 3px; }
      .hko-humidity { margin-bottom: 10px; }
      .hko-wind {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 3px;
        font-size: 20px;
        font-weight: 700;
        line-height: 1.1;
        white-space: nowrap;
        overflow: hidden;
      }
      .hko-umbrella { font-size: 25px; line-height: 1; flex-shrink: 0; }
      .hko-wind-text {
        display: inline-block;
        max-width: 88px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
      }
      .hko-weather-loading, .hko-weather-error {
        color: #ffffff;
        font-size: 18px;
        padding: 18px;
        text-align: center;
      }
      #hko-weather-widget::-webkit-scrollbar { height: 16px; }
      #hko-weather-widget::-webkit-scrollbar-track {
        background: rgba(255, 255, 255, 0.10);
        border-radius: 10px;
      }
      #hko-weather-widget::-webkit-scrollbar-thumb {
        background: rgba(255, 255, 255, 0.70);
        border-radius: 10px;
        border: 3px solid #174f8f;
      }
      #hko-weather-widget::-webkit-scrollbar-thumb:hover {
        background: rgba(255, 255, 255, 0.95);
      }
      @media (max-width: 768px) {
        .hko-weather-row { min-width: 1350px; }
        .hko-weather-card { flex: 0 0 150px; width: 150px; padding: 6px 5px 9px; }
        .hko-date { font-size: 20px; }
        .hko-week { font-size: 17px; margin-bottom: 8px; }
        .hko-icon { width: 76px; height: 76px; margin-bottom: 7px; }
        .hko-temp, .hko-humidity { font-size: 18px; letter-spacing: -0.8px; }
        .hko-humidity { margin-bottom: 8px; }
        .hko-wind { font-size: 17px; }
        .hko-umbrella { font-size: 22px; }
        .hko-wind-text { max-width: 75px; }
      }

      /* ===== MUSIC BUTTON HOVER ===== */
      #musicToggle:hover {
        background: rgba(0, 0, 0, 0.85) !important;
        box-shadow: 0 0 20px rgba(255,255,255,0.8), 0 0 40px rgba(255,255,255,0.4) !important;
        transform: scale(1.05);
      }
      #musicToggle:active {
        transform: scale(0.97);
      }
    </style>

    <script>
      document.addEventListener("DOMContentLoaded", function () {
        loadHKOForecast();
      });
      function loadHKOForecast() {
        var container = document.getElementById("hko-weather-widget");
        if (!container) { console.error("HKO widget container not found."); return; }
        var apiUrl = "https://data.weather.gov.hk/weatherAPI/opendata/weather.php?dataType=fnd&lang=tc";
        fetch(apiUrl)
          .then(function (response) {
            if (!response.ok) throw new Error("HKO API request failed. Status: " + response.status);
            return response.json();
          })
          .then(function (data) {
            var forecasts = data.weatherForecast;
            if (!forecasts || !forecasts.length) throw new Error("No forecast data found.");
            var html = '<div class="hko-weather-row">';
            forecasts.forEach(function (item) {
              var dateText = formatHKODate(item.forecastDate);
              var weekText = item.week || "";
              var iconUrl  = getHKOIconUrl(item.ForecastIcon);
              var minTemp  = item.forecastMintemp && item.forecastMintemp.value ? item.forecastMintemp.value : "";
              var maxTemp  = item.forecastMaxtemp && item.forecastMaxtemp.value ? item.forecastMaxtemp.value : "";
              var minRH    = item.forecastMinrh   && item.forecastMinrh.value   ? item.forecastMinrh.value   : "";
              var maxRH    = item.forecastMaxrh   && item.forecastMaxrh.value   ? item.forecastMaxrh.value   : "";
              var windText = simplifyWind(item.forecastWind || "");
              html +=
                '<div class="hko-weather-card">' +
                  '<div class="hko-date">'  + dateText + '</div>' +
                  '<div class="hko-week">(' + weekText + ')</div>' +
                  '<img class="hko-icon" src="' + iconUrl + '" alt="' + escapeHTML(item.forecastWeather || "weather icon") + '">' +
                  '<div class="hko-temp">'     + minTemp + ' | ' + maxTemp + '°C</div>' +
                  '<div class="hko-humidity">' + minRH   + ' - ' + maxRH   + '%</div>'  +
                  '<div class="hko-wind">' +
                    '<span class="hko-umbrella">☂️</span>' +
                    '<span class="hko-wind-text">' + escapeHTML(windText) + '</span>' +
                  '</div>' +
                '</div>';
            });
            html += "</div>";
            var loadingDiv = container.querySelector(".hko-weather-loading");
            if (loadingDiv) { loadingDiv.outerHTML = html; }
            else { container.insertAdjacentHTML("beforeend", html); }
          })
          .catch(function (error) {
            console.error("HKO weather widget error:", error);
            var loadingDiv = container.querySelector(".hko-weather-loading");
            var errHTML = '<div class="hko-weather-error">未能載入香港天文台天氣預報。<br>請檢查瀏覽器 Console 或網站是否阻擋外部 API。</div>';
            if (loadingDiv) { loadingDiv.outerHTML = errHTML; }
            else { container.insertAdjacentHTML("beforeend", errHTML); }
          });
      }
      function formatHKODate(dateString) {
        if (!dateString || dateString.length !== 8) return "";
        var month = Number(dateString.substring(4, 6));
        var day   = Number(dateString.substring(6, 8));
        return month + "月" + day + "日";
      }
      function getHKOIconUrl(iconCode) {
        return "https://www.hko.gov.hk/images/HKOWxIconOutline/pic" + iconCode + ".png";
      }
      function simplifyWind(windText) {
        if (!windText) return "";
        if (windText.indexOf("微風") !== -1) return "低";
        if (windText.indexOf("和緩") !== -1) return "低";
        if (windText.indexOf("清勁") !== -1) return "中";
        if (windText.indexOf("強風") !== -1) return "中高";
        if (windText.indexOf("烈風") !== -1) return "高";
        if (windText.indexOf("暴風") !== -1) return "高";
        if (windText.indexOf("颶風") !== -1) return "高";
        return "中";
      }
      function escapeHTML(text) {
        return String(text)
          .replace(/&/g,  "&amp;")
          .replace(/</g,  "&lt;")
          .replace(/>/g,  "&gt;")
          .replace(/"/g,  "&quot;")
          .replace(/'/g,  "&#039;");
      }
    </script>

    <hr />

    <div class="container text-center">
      <div class="row justify-content-md-center">
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo01.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo02.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo03.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo04.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo05.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo06.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo07.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo08.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo09.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo10.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo11.png" alt="photo" />
        </div>
        <div class="col-12 col-md-6 col-lg-4 col-xl-3">
          <img class="img-fluid" src="./images/photo12.png" alt="photo" />
        </div>
      </div>
    </div>

    <hr />

    <script
      src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js"
      integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4"
      crossorigin="anonymous"
    ></script>
```
### Min 8: Live Chat
Insert behind <!-- ===== MUSIC PART END ===== -->
[↑ Back to Timeline](#demo-timeline--instructions)
```html
<!-- ===== FIREBASE CHAT START ===== -->
<div id="chatContainer" style="
  position: fixed;
  bottom: 20px;
  right: 20px;
  width: 320px;
  background: rgba(0,0,0,0.75);
  border: 1px solid rgba(255,255,255,0.3);
  border-radius: 16px;
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  z-index: 99999;
  font-family: Arial, sans-serif;
  overflow: hidden;
">
  <!-- Header -->
  <div style="
    background: rgba(255,255,255,0.1);
    padding: 10px 16px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  ">
    <span style="color:#fff; font-weight:bold; font-size:0.95rem;">💬 Live Chat</span>
    <div style="display:flex; align-items:center; gap:8px;">
      <span onclick="clearChat()" style="
        color: rgba(255,255,255,0.6);
        font-size: 0.75rem;
        cursor: pointer;
      " title="Clear all messages">🗑️</span>
      <span id="chatToggleIcon" onclick="toggleChat()" style="
        color:#fff;
        font-size:0.85rem;
        cursor:pointer;
      ">▼</span>
    </div>
  </div>

  <!-- Chat Body -->
  <div id="chatBody">
    <div id="chatBox" style="
      height: 220px;
      overflow-y: auto;
      padding: 10px 14px;
      display: flex;
      flex-direction: column;
      gap: 6px;
    "></div>

    <!-- Input -->
    <div style="
      display: flex;
      gap: 6px;
      padding: 8px 10px;
      border-top: 1px solid rgba(255,255,255,0.15);
    ">
      <input id="nameInput" placeholder="Name" style="
        width: 80px;
        padding: 6px 8px;
        border-radius: 8px;
        border: none;
        background: rgba(255,255,255,0.15);
        color: #fff;
        font-size: 0.8rem;
        outline: none;
      " />
      <input id="msgInput" placeholder="Message..." style="
        flex: 1;
        padding: 6px 8px;
        border-radius: 8px;
        border: none;
        background: rgba(255,255,255,0.15);
        color: #fff;
        font-size: 0.8rem;
        outline: none;
      " onkeydown="if(event.key==='Enter') sendMsg()" />
      <button onclick="sendMsg()" style="
        padding: 6px 12px;
        border-radius: 8px;
        border: none;
        background: #4d96ff;
        color: #fff;
        font-weight: bold;
        cursor: pointer;
        font-size: 0.8rem;
      ">Send</button>
    </div>
  </div>
</div>

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
  import { getDatabase, ref, push, onValue, query, limitToLast }
    from "https://www.gstatic.com/firebasejs/10.12.0/firebase-database.js";

  // ⚠️ 替換成你自己的 Firebase databaseURL
  const app = initializeApp({
    databaseURL: "https://openday-ed369-default-rtdb.asia-southeast1.firebasedatabase.app/"
  });

  const db = getDatabase(app);
  const chatRef = ref(db, 'messages');
  const recentMessages = query(chatRef, limitToLast(200));

  // ✅ 記錄本頁面載入的時間，只顯示載入後的新訊息
  const sessionStart = Date.now();

  // ✅ 只顯示最近 24 小時 且 本次開啟頁面之後 的訊息
  const twentyFourHoursAgo = Date.now() - 24 * 60 * 60 * 1000;

  // ✅ 本地清除標記（按 🗑️ 後，清除本地顯示）
  let localClearedAt = null;

  onValue(recentMessages, (snapshot) => {
    const box = document.getElementById('chatBox');
    box.innerHTML = '';
    snapshot.forEach(child => {
      const d = child.val();

      // 跳過 24 小時前的訊息
      if (d.time < twentyFourHoursAgo) return;

      // 跳過本次開頁面之前的訊息（本地 session 過濾）
      if (d.time < sessionStart) return;

      // 跳過本地清除後的訊息
      if (localClearedAt && d.time < localClearedAt) return;

      const div = document.createElement('div');
      div.style.cssText = `
        background: rgba(255,255,255,0.1);
        border-radius: 8px;
        padding: 5px 10px;
        color: #fff;
        font-size: 0.82rem;
        word-break: break-word;
      `;
      div.innerHTML = `<b style="color:#4d96ff">${escapeHTML(d.name || 'Anonymous')}</b>: ${escapeHTML(d.text)}`;
      box.appendChild(div);
    });
    box.scrollTop = box.scrollHeight;
  });

  window.sendMsg = function() {
    const name = document.getElementById('nameInput').value.trim() || 'Anonymous';
    const text = document.getElementById('msgInput').value.trim();
    if (!text) return;
    push(chatRef, { name, text, time: Date.now() });
    document.getElementById('msgInput').value = '';
  };

  // ✅ 只清除本地顯示，不動 Firebase 資料
  window.clearChat = function() {
    localClearedAt = Date.now();
    document.getElementById('chatBox').innerHTML = '';
  };

  function escapeHTML(str) {
    return String(str)
      .replace(/&/g,"&amp;")
      .replace(/</g,"&lt;")
      .replace(/>/g,"&gt;");
  }
</script>
<script>
  function toggleChat() {
    const body = document.getElementById('chatBody');
    const icon = document.getElementById('chatToggleIcon');
    if (body.style.display === 'none') {
      body.style.display = 'block';
      icon.textContent = '▼';
    } else {
      body.style.display = 'none';
      icon.textContent = '▲';
    }
  }
</script>
<!-- ===== FIREBASE CHAT END ===== -->
```


