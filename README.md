<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>My Weather App</title>
</head>
<body>

  <h1>Weather Forecast for San Francisco</h1>
  <div id="forecast"></div>

  <script>
    const weatherApiUrl = 'https://api.weather.gov/gridpoints/MTR/84,105/forecast';

    fetch(weatherApiUrl)
      .then(response => {
        if (!response.ok) {
          throw new Error('There was a problem with the network response');
        }
        return response.json();
      })
      .then(data => {
        const forecastDiv = document.getElementById('forecast');
        const periods = data.properties.periods;

        const today = new Date();
        const maxDays = new Date();
        maxDays.setDate(today.getDate() + 3);

        let forecastHTML = '';

        periods.forEach(period => {
          const startTime = new Date(period.startTime);
          if (startTime <= maxDays) {
            const formattedDate = startTime.toLocaleDateString('en-US', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
            const formattedTime = startTime.toLocaleTimeString('en-US', { hour: 'numeric', minute: 'numeric', hour12: true });
            forecastHTML += `<h3>${period.name}</h3><p>Date: ${formattedDate}</p><p>Time: ${formattedTime}</p><p>${period.detailedForecast}</p><hr>`;
          }
        });

        forecastDiv.innerHTML = forecastHTML;
      })
      .catch(error => {
        const forecastDiv = document.getElementById('forecast');
        forecastDiv.innerHTML = `<p>There was a problem with the fetching command: ${error}</p>`;
      });
  </script>
</body>
</html>
