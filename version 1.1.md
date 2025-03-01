/* version 1.1*/
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Breathing Exercise</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
            width: 100vw;
            height: 100vh;
            overflow: hidden;
            background-image: url('https://viteeevec.github.io/breath/galaxy-unsplash.jpg');
            background-position: center;
            background-repeat: no-repeat;
            background-attachment: fixed;
            background-size: cover;
            /*color: white;*/
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }

/*         body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
            width: 100vw;
            height: 100vh;
            overflow: hidden;            
            background-size: cover;
            color: white;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: url('https://viteeevec.github.io/breath/galaxy-unsplash.jpg') no-repeat center center fixed;     
        } */
        
        
        .menu {
            position: absolute;
            top: 10px;
            left: 10px;
            background: rgba(0, 0, 0, 0.6);
            padding: 10px;
            border-radius: 8px;
        }
        .container {
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background: transparent;
        }
        .circle {
            width: 20vw;
            height: 20vw;
            max-width: 150px;
            max-height: 150px;
            border-radius: 50%;
            background-color: lightblue;
            transition: transform linear;
        }
        .breath-text {
            font-size: 5vw;
            margin-top: 50px;
        }
        .controls {
            margin-top: 20px;
        }
        .settings {
            display: none;
            position: absolute;
            top: 50px;
            left: 10px;
            background: rgba(0, 0, 0, 0.6);
            padding: 15px;
            border-radius: 8px;
        }
    </style>
</head>
<body>
    <div class="menu">
        <button onclick="toggleSettings()">Settings</button>
    </div>
    <div class="settings" id="settings">
        <label>Inhale (seconds): <input type="number" id="inhale" value="4"></label><br>
        <label>Hold (seconds): <input type="number" id="hold" value="4"></label><br>
        <label>Exhale (seconds): <input type="number" id="exhale" value="4"></label><br>
        <label>Relax (seconds): <input type="number" id="relax" value="4"></label><br>
        <label>Cycles: <input type="number" id="cycles" value="5"></label><br>
        <label>Inhale Color: <input type="color" id="inhaleColor" value="#00ff00"></label><br>
        <label>Hold Color: <input type="color" id="holdColor" value="#ffff00"></label><br>
        <label>Exhale Color: <input type="color" id="exhaleColor" value="#ff0000"></label><br>
        <label>Relax Color: <input type="color" id="relaxColor" value="#0000ff"></label><br><br>
    </div>
    <div class="container">
        <div class="circle" id="circle"></div>
        <div class="breath-text" id="breath-text">Ready?</div>
        <div class="controls">
            <button onclick="startBreathing()">Start</button>
            <button onclick="stopBreathing()">Stop/Reset</button>
        </div>
    </div>

    <script>
        let breathingIntervals = [];
        let stopRequested = false;

        function toggleSettings() {
            let settings = document.getElementById("settings");
            settings.style.display = settings.style.display === "none" ? "block" : "none";
        }

        function startBreathing() {
            stopRequested = false;
            clearAllIntervals();

            let inhaleTime = parseInt(document.getElementById('inhale').value) * 1000;
            let holdTime = parseInt(document.getElementById('hold').value) * 1000;
            let exhaleTime = parseInt(document.getElementById('exhale').value) * 1000;
            let relaxTime = parseInt(document.getElementById('relax').value) * 1000;
            let cycles = parseInt(document.getElementById('cycles').value);
            let count = 0;

            function breatheCycle() {
                if (count >= cycles || stopRequested) {
                    document.getElementById("breath-text").textContent = stopRequested ? "Ready?" : "Session Complete";
                    return;
                }
                count++;
                document.getElementById("breath-text").textContent = "Inhale...";
                document.getElementById("circle").style.backgroundColor = document.getElementById('inhaleColor').value;
                document.getElementById("circle").style.transitionDuration = inhaleTime + 'ms';
                document.getElementById("circle").style.transform = "scale(1.5)";
                breathingIntervals.push(setTimeout(() => {
                    document.getElementById("breath-text").textContent = "Hold...";
                    document.getElementById("circle").style.backgroundColor = document.getElementById('holdColor').value;
                    breathingIntervals.push(setTimeout(() => {
                        document.getElementById("breath-text").textContent = "Exhale...";
                        document.getElementById("circle").style.transitionDuration = exhaleTime + 'ms';
                        document.getElementById("circle").style.backgroundColor = document.getElementById('exhaleColor').value;
                        document.getElementById("circle").style.transform = "scale(1)";
                        breathingIntervals.push(setTimeout(() => {
                            document.getElementById("breath-text").textContent = "Relax...";
                            document.getElementById("circle").style.backgroundColor = document.getElementById('relaxColor').value;
                            breathingIntervals.push(setTimeout(breatheCycle, relaxTime));
                        }, exhaleTime));
                    }, holdTime));
                }, inhaleTime));
            }
            breatheCycle();
        }

        function stopBreathing() {
            stopRequested = true;
            clearAllIntervals();
            document.getElementById("breath-text").textContent = "Ready?";
            document.getElementById("circle").style.transform = "scale(1)";
            document.getElementById("circle").style.backgroundColor = "lightblue";
        }

        function clearAllIntervals() {
            while (breathingIntervals.length) {
                clearTimeout(breathingIntervals.pop());
            }
        }
    </script>
</body>
</html>
