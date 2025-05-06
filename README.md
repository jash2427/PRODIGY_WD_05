# PRODIGY_WD_05
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Weather App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(#6dd5ed, #2193b0);
      color: #fff;
      text-align: center;
      padding: 30px;
    }

    h1 {
      margin-bottom: 20px;
    }

    input[type="text"] {
      padding: 10px;
      width: 250px;
      border-radius: 5px;
      border: none;
    }

    button {
      padding: 10px 15px;
      border: none;
      background: #fff;
      color: #2193b0;
      font-weight: bold;
      border-radius: 5px;
      cursor: pointer;
    }

    .weather-info {
      margin-top: 30px;
      background: rgba(255, 255, 255, 0.2);
      padding: 20px;
      border-radius: 10px;
      display: inline-block;
    }

    .label {
      font-weight: bold;
      margin-top: 10px;
    }
  </style>
</head>
<body>

  <h1>Weather App</h1>
  <input type="text" id="cityInput" placeholder="Enter city name">
  <button onclick="getWeather()">Search</button>

  <div class="weather-info" id="weatherDisplay" style="display:none;">
    <h2 id="location"></h2>
    <p><span class="label">Temperature:</span> <span id="temperature"></span>°C</p>
    <p><span class="label">Condition:</span> <span id="condition"></span></p>
  </div>

  <script>
    const apiKey = 'YOUR_API_KEY_HERE'; // Replace with your OpenWeatherMap API key

    function getWeather() {
      const city = document.getElementById("cityInput").value;
      if (!city) {
        alert("Please enter a city name.");
        return;
      }

      const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${apiKey}&units=metric`;

      fetch(url)
        .then(response => {
          if (!response.ok) throw new Error("City not found");
          return response.json();
        })
        .then(data => {
          document.getElementById("location").innerText = `${data.name}, ${data.sys.country}`;
          document.getElementById("temperature").innerText = data.main.temp;
          document.getElementById("condition").innerText = data.weather[0].description;
          document.getElementById("weatherDisplay").style.display = "block";
        })
        .catch(error => {
          alert("Error fetching weather data: " + error.message);
        });
    }

    // Optional: Auto detect user's location
    window.onload = function() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(pos => {
          const lat = pos.coords.latitude;
          const lon = pos.coords.longitude;
          fetch(`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${apiKey}&units=metric`)
            .then(response => response.json())
            .then(data => {
              document.getElementById("location").innerText = `${data.name}, ${data.sys.country}`;
              document.getElementById("temperature").innerText = data.main.temp;
              document.getElementById("condition").innerText = data.weather[0].description;
              document.getElementById("weatherDisplay").style.display = "block";
            });
        });
      }
    }
  </script>

</body>
</html>
