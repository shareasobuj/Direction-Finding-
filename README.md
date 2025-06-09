# Direction-Finding-

<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Compass Direction App</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background: #0a0a0a;
      color: #fff;
      font-family: Arial, sans-serif;
      text-align: center;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .compass {
      width: 200px;
      height: 200px;
      border: 8px solid #fff;
      border-radius: 50%;
      position: relative;
      margin-bottom: 20px;
      transform: rotate(0deg);
      transition: transform 0.2s ease-out;
    }

    .needle {
      width: 4px;
      height: 100px;
      background: red;
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateX(-50%);
      transform-origin: bottom;
    }

    #direction-text {
      font-size: 1.5em;
      margin-top: 10px;
    }

    button {
      padding: 10px 20px;
      font-size: 16px;
      background: #1e90ff;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    button:hover {
      background: #0d74d1;
    }
  </style>
</head>
<body>
  <h1>Direction Finder</h1>
  <div class="compass" id="compass">
    <div class="needle" id="needle"></div>
  </div>
  <div id="direction-text">Direction: --</div>
  <button onclick="requestAccess()">Enable Compass</button>

  <script>
    function getDirectionName(degree) {
      const directions = ['North', 'North-East', 'East', 'South-East', 'South', 'South-West', 'West', 'North-West'];
      const index = Math.round(degree / 45) % 8;
      return directions[index];
    }

    function requestAccess() {
      if (typeof DeviceOrientationEvent !== "undefined" && typeof DeviceOrientationEvent.requestPermission === "function") {
        // iOS devices
        DeviceOrientationEvent.requestPermission()
          .then(permissionState => {
            if (permissionState === "granted") {
              window.addEventListener("deviceorientation", handleOrientation);
            } else {
              alert("Permission denied.");
            }
          })
          .catch(console.error);
      } else {
        // Android or other
        window.addEventListener("deviceorientationabsolute", handleOrientation, true);
        window.addEventListener("deviceorientation", handleOrientation, true);
      }
    }

    function handleOrientation(event) {
      let alpha = event.alpha;

      if (typeof event.webkitCompassHeading !== "undefined") {
        alpha = event.webkitCompassHeading; // iOS
      }

      const compass = document.getElementById("compass");
      const needle = document.getElementById("needle");
      const directionText = document.getElementById("direction-text");

      if (alpha !== null) {
        compass.style.transform = `rotate(${-alpha}deg)`;
        directionText.innerText = `Direction: ${getDirectionName(alpha)} (${Math.round(alpha)}°)`;
      } else {
        directionText.innerText = "Direction: Not supported";
      }
    }
  </script>
</body>
</html>
