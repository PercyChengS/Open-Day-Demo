# Open Day Demo - Cloud Architecture & Web Development

## Check List / Reminders

### Demo Computer (PC 2):
1. Terminal (Open two tabs)
2. Open GitHub (https://github.com/PercyChengS/Open-Day-Demo/tree/main) ready to copy the code
3. Login:
   - 3.1. `ssh root@47.81.211.14`
   - 3.2. Password: `S33D!Eatt0Fun`
4. Restore the demo:
   - 4.1. `cd /var/www`
   - 4.2. Remove the previous album: `rm -r album1`
   - 4.3. Download nginx template from git:
     ```bash
     sudo git clone https://github.com/PercyChengS/Open-Day-Demo.git /tmp/open-day-demo && sudo cp -r /tmp/open-day-demo/album1/. /var/www/album1 && sudo rm -rf /tmp/open-day-demo
     ```
   - 4.4. Close the nginx: `sudo systemctl stop nginx`
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
| Min 3 | The Server Is Alive: Access Ubuntu server, check status, load default nginx page. | 1. `sudo systemctl start nginx`<br/>2. `sudo systemctl status nginx`<br/>3. `Press 'q' to exit status view` |
| Min 3 (con't) | Prepare directory for deployment. | 1. `cd /var/www/`<br/>2. `rm -r album1` |
| Min 4 | Launching the Photo Album: Present QR code and link for audience to scan and view on phones. | 1. `cd /var/www/`<br/>2. `sudo git clone https://github.com/ngsanluk/bootstrap-album /var/www/album1` |
| Min 5 | Change 1 (Change background): Navigate to css folder, download new background style, refresh website. | 1. `cd /var/www/album1/css`<br/>2. `wget -O style.css https://raw.githubusercontent.com/SEEDWanda/CCDemo/main/style.css` |
| Min 6 | Change 2 (Replace photos, add music): Navigate to album1, replace existing images with new ones, refresh website. | **Action: run the command on !!LOCAL!! (Depending on your OS)**<br/>- For Windows (PowerShell):<br/>1. `scp $env:USERPROFILE\Downloads\SupportDoc\* root@47.81.211.14:/var/www/album1/images/`<br/>- For Mac / Linux:<br/>1. `scp ~/Downloads/SupportDoc/* root@47.81.211.14:/var/www/album1/images/`<br/>2. `Password: S33D!Eatt0Fun`<br/>3. `rm -r index.html`<br/>4. `nano index.html`<br/>5. [Replace the whole code from GitHub ↓](#min-6-replace-photos--add-music)<br/>6. `Press Control+X, Y, Enter` |
| Min 7 | Change 3 (Add weather forecast): Open index.html, insert HKO public API code between &lt;body&gt; tags, save and exit. | 1. `cd /var/www/album1`<br/>2. `nano index.html`<br/>3. [Insert the code after `<!-- MUSIC PART END -->` from GitHub ↓](#min-7-hko-weather-forecast-api-code)<br/>4. `Press Control+X, Y, Enter` |
| Min 8 | What Just Happened: Diagram showing flow from Laptop -> Cloud -> Phone. | / |
| Min 9 | Why This Matters: Career relevance and student's before-and-after learning transformation. | / |
| Min 10 | Closing: QR code still live, final concluding statement. | / |

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
  </head>
  <body>

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
        🔇 Music OFF
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

    <!-- ===== MUSIC JS START ===== -->
    <script>
      const audio = document.getElementById('bgMusic');
      const btn = document.getElementById('musicToggle');
      let isPlaying = false;

      document.addEventListener('click', function startOnInteraction() {
        if (!isPlaying) {
          audio.muted = false;
          audio.play().then(() => {
            isPlaying = true;
            btn.textContent = '🔊 Music ON';
          }).catch(() => {});
        }
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

[↑ Back to Timeline](#demo-timeline--instructions)

---

### Min 7: HKO Weather Forecast API Code

Add the following code after `<!-- ===== MUSIC PART END ===== -->`

```html
<!-- HKO 9-Day Weather Forecast Widget -->
<div id="hko-weather-widget">
  <div class="hko-weather-loading">載入天氣預報中...</div>
</div>
<style>
  #hko-weather-widget {
    width: 100%;
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
    background: #174f8f;
    font-family: Arial, "Microsoft JhengHei", "PingFang HK", sans-serif;
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
  .hko-date { font-size: 23px; font-weight: 700; }
  .hko-week { font-size: 20px; font-weight: 700; margin-bottom: 10px; }
  .hko-icon { width: 84px; height: 84px; object-fit: contain; margin-bottom: 8px; }
  .hko-temp, .hko-humidity { font-size: 21px; font-weight: 800; }
  .hko-wind { display: flex; align-items: center; justify-content: center; gap: 3px; }
  .hko-umbrella { font-size: 25px; }
  .hko-wind-text { max-width: 88px; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
  .hko-weather-loading, .hko-weather-error { color: #ffffff; font-size: 18px; padding: 18px; text-align: center; }
</style>
<script>
  document.addEventListener("DOMContentLoaded", function () {
    loadHKOForecast();
  });

  function loadHKOForecast() {
    var container = document.getElementById("hko-weather-widget");
    var apiUrl = "https://data.weather.gov.hk/weatherAPI/opendata/weather.php?dataType=fnd&lang=tc";
    fetch(apiUrl)
      .then(function (response) { return response.json(); })
      .then(function (data) {
        var forecasts = data.weatherForecast;
        var html = '<div class="hko-weather-row">';
        forecasts.forEach(function (item) {
          var dateText = formatHKODate(item.forecastDate);
          var weekText = item.week || "";
          var iconUrl = getHKOIconUrl(item.ForecastIcon);
          var minTemp = item.forecastMintemp?.value || "";
          var maxTemp = item.forecastMaxtemp?.value || "";
          var minRH = item.forecastMinrh?.value || "";
          var maxRH = item.forecastMaxrh?.value || "";
          var windText = simplifyWind(item.forecastWind || "");
          html +=
            '<div class="hko-weather-card">' +
              '<div class="hko-date">' + dateText + '</div>' +
              '<div class="hko-week">(' + weekText + ')</div>' +
              '<img class="hko-icon" src="' + iconUrl + '" alt="weather">' +
              '<div class="hko-temp">' + minTemp + ' | ' + maxTemp + '°C</div>' +
              '<div class="hko-humidity">' + minRH + ' - ' + maxRH + '%</div>' +
              '<div class="hko-wind"><span class="hko-umbrella">☂️</span>' +
              '<span class="hko-wind-text">' + escapeHTML(windText) + '</span></div>' +
            '</div>';
        });
        html += "</div>";
        container.innerHTML = html;
      })
      .catch(function () {
        container.innerHTML = '<div class="hko-weather-error">未能載入香港天文台天氣預報。</div>';
      });
  }

  function formatHKODate(d) {
    return Number(d.substring(4,6)) + "月" + Number(d.substring(6,8)) + "日";
  }
  function getHKOIconUrl(iconCode) {
    return "https://www.hko.gov.hk/images/HKOWxIconOutline/pic" + iconCode + ".png";
  }
  function simplifyWind(w) {
    if (w.indexOf("微風") !== -1 || w.indexOf("和緩") !== -1) return "低";
    if (w.indexOf("清勁") !== -1) return "中";
    if (w.indexOf("強風") !== -1) return "中高";
    if (w.indexOf("烈風") !== -1 || w.indexOf("暴風") !== -1 || w.indexOf("颶風") !== -1) return "高";
    return "中";
  }
  function escapeHTML(text) {
    return String(text).replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;");
  }
</script>
```

[↑ Back to Timeline](#demo-timeline--instructions)
