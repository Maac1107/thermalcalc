# thermalcalc
<!DOCTYPE html>
<html>
  <head>
    <style>
      table {
        width: 80%;
        margin: auto;
      }
      table,
      th,
      td {
        border: 1px solid black;
        border-collapse: collapse;
      }
      th,
      td {
        padding: 10px;
      }
      input[type="text"] {
        width: 100%;
        padding: 5px;
        box-sizing: border-box;
      }
    </style>
    <script>
      // Function to calculate delta
      function calculateDelta(length, plannedInstallationTemperature) {
        const delta = (11.7 * Math.pow(10, -6)) * length * (plannedInstallationTemperature - 20);
        return delta;
      }

      // Function to update delta and real-time temperature values in the table
      function updateValues(event) {
        // Get the length and planned installation temperature values from the inputs
        const length = Number(event.target.parentNode.parentNode.children[1].children[0].value);
        const plannedInstallationTemperature = Number(event.target.parentNode.parentNode.children[2].children[0].value);
                
        // Calculate delta using the formula
        const delta = calculateDelta(length, plannedInstallationTemperature);
        
        // Update the delta value in the table
        event.target.parentNode.parentNode.children[3].innerHTML = delta.toFixed(4);

        // Fetch temperature data from OpenWeather API
        const apiKey = "4028e66ce6ba44d5d9282143053e7984";
        const units = "metric";
        const url = `https://api.openweathermap.org/data/2.5/weather?q=Melbourne,au&appid=4028e66ce6ba44d5d9282143053e7984&units=metric&lang=en`;

        fetch(url)
          .then(response => response.json())
          .then(data => {
            // Get the temperature from the API response
            const temperature = data.main.temp;
            
            // Update the real-time temperature value in the table
            event.target.parentNode.parentNode.children[4].innerHTML = temperature.toFixed(1);
          })
          .catch(error => {
            console.error(error);
          });
      }
    </script>
  </head>
  <body>
    <table>
      <tr>
        <th>Identifier</th>
        <th>Length</th>
        <th>Planned Installation Temperature</th>
        <th>Delta</th>
        <th>Real-Time Temperature</th>
      </tr>
      <tr>
        <td><input type="text" oninput="updateValues(event)"></td>
        <td><input type="text" oninput="updateValues(event)"></td>
        <td><input type="text" oninput="updateValues(event)"></td>
        <td></td>
        <td></td>
      </tr>
      <tr>
        <td><input type="text" oninput="updateValues(event)"></td>
        <td><input type="text" oninput="updateValues(event)"></td>
        <td><input type="text" oninput="updateValues(event)"></td>
        <td></td>
        <td></td>
      </tr>
      <!-- Add more rows as needed -->
    </table>
  </body>
</html>
