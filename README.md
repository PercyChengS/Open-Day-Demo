# Open Day Demo - Cloud Architecture & Web Development

## Demo Timeline & Instructions

| Time | Instruction | Command |
|------|-------------|---------|
| Min 1 | Hook: Open with familiar app logos (WhatsApp etc.), ask what they have in common | / |
| Min 2 | What Is the Cloud?: Explain architecture (laptop -> server -> devices) and show AWS global map. | / |
| Min 3 | The Server Is Alive: Access Ubuntu server, check status, load default nginx page. | `1. sudo systemctl start nginx`<br/>`2. sudo systemctl status nginx`<br/>`3. Press 'q' to exit status view` |
| Min 4 | Launching the Photo Album: Present QR code and link for audience to scan and view on phones. | `1. cd /var/www/`<br/>`2. sudo git clone https://github.com/ngsanluk/bootstrap-album` |
| Min 5 | Change 1 (Change background): Navigate to css folder, download new background style, refresh website. | `1. cd /var/www/album1/css`<br/>`2. wget -O style.css https://raw.githubusercontent.com/ngsanluk/bootstrap-album/main/css/style.css` |
| Min 6 | Change 2 (Replace photos): Navigate to album1, replace existing images with new ones, refresh website. | `1. scp /Users/**User**/Downloads/images/* root@ur_ip:/var/www/album1/images/` |
| Min 7 | Change 3 (Add weather forecast): Open index.html, insert HKO public API code between &lt;body&gt; tags, save and exit. | `1. cd /var/www/album1`<br/>`2. nano index.html`<br/>`3. Paste the HKO weather code`<br/>`4. Ctrl+O, Enter to save`<br/>`5. Ctrl+X to exit`<br/>`6. Refresh website` |
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

## Min 7: HKO Weather Forecast API Code

Insert the following code between the `<body>` tags in `/var/www/album1/index.html`:

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

  .hko-weather-card:last-child {
    border-right: none;
  }

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

  .hko-temp,
  .hko-humidity {
    font-size: 21px;
    font-weight: 800;
    line-height: 1.15;
    white-space: nowrap;
    letter-spacing: -0.5px;
  }

  .hko-temp {
    margin-bottom: 3px;
  }

  .hko-humidity {
    margin-bottom: 10px;
  }

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

  .hko-umbrella {
    font-size: 25px;
    line-height: 1;
    flex-shrink: 0;
  }

  .hko-wind-text {
    display: inline-block;
    max-width: 88px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  .hko-weather-loading,
  .hko-weather-error {
    color: #ffffff;
    font-size: 18px;
    padding: 18px;
    text-align: center;
  }

  #hko-weather-widget::-webkit-scrollbar {
    height: 6px;
  }

  #hko-weather-widget::-webkit-scrollbar-thumb {
    background: rgba(255, 255, 255, 0.35);
    border-radius: 10px;
  }

  @media (max-width: 768px) {
    .hko-weather-row {
      min-width: 1350px;
    }

    .hko-weather-card {
      flex: 0 0 150px;
      width: 150px;
      padding: 6px 5px 9px;
    }

    .hko-date {
      font-size: 20px;
    }

    .hko-week {
      font-size: 17px;
      margin-bottom: 8px;
    }

    .hko-icon {
      width: 76px;
      height: 76px;
      margin-bottom: 7px;
    }

    .hko-temp,
    .hko-humidity {
      font-size: 18px;
      letter-spacing: -0.8px;
    }

    .hko-humidity {
      margin-bottom: 8px;
    }

    .hko-wind {
      font-size: 17px;
    }

    .hko-umbrella {
      font-size: 22px;
    }

    .hko-wind-text {
      max-width: 75px;
    }
  }
</style>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    loadHKOForecast();
  });

  function loadHKOForecast() {
    var container = document.getElementById("hko-weather-widget");

    if (!container) {
      console.error("HKO widget container not found.");
      return;
    }

    var apiUrl =
      "https://data.weather.gov.hk/weatherAPI/opendata/weather.php?dataType=fnd&lang=tc";

    fetch(apiUrl)
      .then(function (response) {
        if (!response.ok) {
          throw new Error("HKO API request failed. Status: " + response.status);
        }

        return response.json();
      })
      .then(function (data) {
        console.log("HKO API data:", data);

        var forecasts = data.weatherForecast;

        if (!forecasts || !forecasts.length) {
          throw new Error("No forecast data found.");
        }

        var html = '<div class="hko-weather-row">';

        forecasts.forEach(function (item) {
          var dateText = formatHKODate(item.forecastDate);
          var weekText = item.week || "";
          var iconUrl = getHKOIconUrl(item.ForecastIcon);

          var minTemp =
            item.forecastMintemp && item.forecastMintemp.value
              ? item.forecastMintemp.value
              : "";

          var maxTemp =
            item.forecastMaxtemp && item.forecastMaxtemp.value
              ? item.forecastMaxtemp.value
              : "";

          var minRH =
            item.forecastMinrh && item.forecastMinrh.value
              ? item.forecastMinrh.value
              : "";

          var maxRH =
            item.forecastMaxrh && item.forecastMaxrh.value
              ? item.forecastMaxrh.value
              : "";

          var windText = simplifyWind(item.forecastWind || "");

          html +=
            '<div class="hko-weather-card">' +
              '<div class="hko-date">' + dateText + '</div>' +
              '<div class="hko-week">(' + weekText + ')</div>' +

              '<img class="hko-icon" src="' + iconUrl + '" alt="' + escapeHTML(item.forecastWeather || "weather icon") + '">' +

              '<div class="hko-temp">' +
                minTemp + ' | ' + maxTemp + '°C' +
              '</div>' +

              '<div class="hko-humidity">' +
                minRH + ' - ' + maxRH + '%' +
              '</div>' +

              '<div class="hko-wind">' +
                '<span class="hko-umbrella">☂️</span>' +
                '<span class="hko-wind-text">' + escapeHTML(windText) + '</span>' +
              '</div>' +
            '</div>';
        });

        html += "</div>";

        container.innerHTML = html;
      })
      .catch(function (error) {
        console.error("HKO weather widget error:", error);

        container.innerHTML =
          '<div class="hko-weather-error">' +
            '未能載入香港天文台天氣預報。<br>' +
            '請檢查瀏覽器 Console 或網站是否阻擋外部 API。' +
          '</div>';
      });
  }

  function formatHKODate(dateString) {
    if (!dateString || dateString.length !== 8) {
      return "";
    }

    var month = Number(dateString.substring(4, 6));
    var day = Number(dateString.substring(6, 8));

    return month + "月" + day + "日";
  }

  function getHKOIconUrl(iconCode) {
    return "https://www.hko.gov.hk/images/HKOWxIconOutline/pic" + iconCode + ".png";
  }

  function simplifyWind(windText) {
    if (!windText) {
      return "";
    }

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
      .replace(/&/g, "&amp;")
      .replace(/</g, "&lt;")
      .replace(/>/g, "&gt;")
      .replace(/"/g, "&quot;")
      .replace(/'/g, "&#039;");
  }
</script>
```
