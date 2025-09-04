<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Fractured States RPG</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #1e1e1e;
      color: #f0f0f0;
      padding: 20px;
    }
    button {
      padding: 10px;
      margin: 5px;
      background-color: #444;
      color: white;
      border: none;
      cursor: pointer;
      width: 100%;
      text-align: left;
    }
    button:hover {
      background-color: #666;
    }
    #log {
      margin-top: 20px;
      white-space: pre-line;
      background-color: #2c2c2c;
      padding: 10px;
    }
    .section {
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <h1>🛠️ Fractured States</h1>

  <div class="section">
    <h2>🗣️ Dialogue</h2>
    <div id="dialogue">Maya: "You really came back? After all these years?"</div>
    <button onclick="respond(1)">"I had nowhere else to go."</button>
    <button onclick="respond(2)">"I wanted to make things right."</button>
    <button onclick="respond(3)">"None of your business."</button>
  </div>

  <div class="section">
    <h2>📦 Resources</h2>
    <p>Wood: <span id="wood">0</span> | Metal: <span id="metal">0</span></p>
    <button onclick="gather('wood')">Gather Wood</button>
    <button onclick="gather('metal')">Scavenge Metal</button>
  </div>

  <div class="section">
    <h2>🏗️ Build Structures</h2>
    <button onclick="build('shelter')">Build Shelter (5 wood)</button>
    <button onclick="build('clinic')">Build Clinic (3 wood, 3 metal)</button>
    <button onclick="build('workshop')">Build Workshop (2 wood, 5 metal)</button>
  </div>

  <div class="section">
    <h2>📊 Reputation</h2>
    <p>Reputation Score: <span id="rep">0</span></p>
  </div>

  <div id="log"></div>

  <script>
    let resources = { wood: 0, metal: 0 };
    let reputation = 0;

    function updateDisplay() {
      document.getElementById('wood').innerText = resources.wood;
      document.getElementById('metal').innerText = resources.metal;
      document.getElementById('rep').innerText = reputation;
    }

    function respond(choice) {
      const dialogue = document.getElementById('dialogue');
      document.querySelectorAll('button').forEach(btn => {
        if (btn.parentElement.className === 'section' && btn.innerText.includes('"')) {
          btn.style.display = 'none';
        }
      });

      switch(choice) {
        case 1:
          dialogue.innerText = 'Maya: "That’s rough. I figured you might be lost."';
          reputation += 1;
          break;
        case 2:
          dialogue.innerText = 'Maya: "Still trying to be the hero, huh? I respect that."';
          reputation += 3;
          break;
        case 3:
          dialogue.innerText = 'Maya: "Same old Jalen. Closed off and cold."';
          reputation -= 2;
          break;
      }
      updateDisplay();
    }

    function gather(type) {
      resources[type]++;
      updateDisplay();
      logMessage(`You gathered 1 ${type}.`);
    }

    function build(structure) {
      const costs = {
        shelter: { wood: 5, metal: 0 },
        clinic: { wood: 3, metal: 3 },
        workshop: { wood: 2, metal: 5 }
      };

      const cost = costs[structure];
      if (resources.wood >= cost.wood && resources.metal >= cost.metal) {
        resources.wood -= cost.wood;
        resources.metal -= cost.metal;
        reputation += 2;
        updateDisplay();
        logMessage(`✅ You built a ${structure}. Reputation increased.`);
      } else {
        logMessage(`❌ Not enough resources to build ${structure}.`);
      }
    }

    function logMessage(msg) {
      document.getElementById('log').innerText += msg + '\n';
    }
  </script>
</body>
</html>
