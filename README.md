# Zombie-night-
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Moonlight Survivor: Final Boss - Giselle ❤️</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; touch-action: none; }
        body, html { width: 100%; height: 100%; overflow: hidden; background: #000; position: fixed; font-family: -apple-system, sans-serif; }

        #sky { position: absolute; inset: 0; background: linear-gradient(to bottom, #0f0c29, #1a1a2e); z-index: -2; }
        #moon { position: absolute; top: 15%; left: 50%; transform: translateX(-50%); font-size: 80px; z-index: -1; filter: drop-shadow(0 0 20px #f1c40f); }

        /* POV Blaster */
        #blaster { position: absolute; bottom: 35%; left: 50%; transform: translateX(-50%); font-size: 80px; z-index: 100; transition: left 0.1s ease-out; }

        /* HUD */
        #hud { position: absolute; top: 10%; width: 100%; display: flex; flex-direction: column; align-items: center; z-index: 200; color: #fff; font-weight: bold; text-shadow: 2px 2px 4px #000; }

        .zombie { position: absolute; font-size: 55px; transform: translateX(-50%); z-index: 50; transition: color 0.3s; }
        .boss { font-size: 120px !important; filter: drop-shadow(0 0 20px red); }
        .bolt { position: absolute; width: 6px; height: 30px; background: #f1c40f; border-radius: 5px; box-shadow: 0 0 10px #f1c40f; z-index: 60; }
        .sparkle { position: absolute; font-size: 20px; z-index: 70; pointer-events: none; animation: burst 0.6s forwards; }
        @keyframes burst { to { transform: translate(var(--tx), var(--ty)) scale(0); opacity: 0; } }

        /* Final Scene: 3rd POV Sleeping */
        #sleep-scene {
            display: none; position: fixed; inset: 0; background: #1a1a2e;
            z-index: 2000; flex-direction: column; justify-content: center; align-items: center; text-align: center; color: white; padding: 20px;
        }
        .bed { font-size: 100px; margin-bottom: 20px; animation: breathe 3s infinite ease-in-out; }
        @keyframes breathe { 0%, 100% { transform: scale(1); } 50% { transform: scale(1.05); } }

        .overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.9); display: flex; flex-direction: column; justify-content: center; align-items: center; text-align: center; z-index: 1000; padding: 40px; color: white; }
        .btn-start { padding: 20px 60px; border-radius: 50px; background: #6c5ce7; color: white; border: none; font-size: 1.3rem; font-weight: bold; margin-top: 25px; }
    </style>
</head>
<body>

<div id="sky"></div>
<div id="moon">🌕</div>

<div id="game-stage">
    <div id="blaster">🔫</div>
    <div id="hud">
        <div id="wave-display">WAVE 1/10</div>
        <div id="score-display">Zombies: 3</div>
        <div id="lives" style="font-size: 24px; margin-top: 10px;">❤️❤️❤️</div>
    </div>
</div>

<div id="start-screen" class="overlay">
    <h1 style="color: #6c5ce7;">Moonlight Survivor 🧟‍♂️</h1>
    <p style="margin-top: 15px;">Defeat 10 waves to secure your dreams, Giselle!</p>
    <button class="btn-start" onclick="startGame()">START DEFENSE 🎶</button>
</div>

<div id="sleep-scene">
    <div class="bed">🛌💤</div>
    <h1 style="color: #f1c40f; font-size: 1.8rem; margin-bottom: 15px;">It was all a dream...</h1>
    <p style="line-height: 1.6; font-size: 1.1rem; padding: 0 20px;">
        I love you Giselle! My gorgeous, precious, smart, talented, sweet perfect baby!<br><br>
        Goodnight my love and sweet dreams! Remember I love you with all my heart. ❤️
    </p>
</div>

<script>
    let active = false, currentWave = 1, zombiesToSpawn = 0, zombiesActive = 0, lives = 3, bossHP = 15;
    const blaster = document.getElementById('blaster');
    const livesDisplay = document.getElementById('lives');
    const waveDisplay = document.getElementById('wave-display');
    const scoreDisplay = document.getElementById('score-display');

    function startGame() {
        document.getElementById('start-screen').style.display = 'none';
        active = true;
        startWave();
    }

    function startWave() {
        if (currentWave > 10) { endToSleep(); return; }
        waveDisplay.innerText = `WAVE ${currentWave}/10`;
        zombiesToSpawn = currentWave * 3;
        zombiesActive = zombiesToSpawn;
        
        if(currentWave === 10) {
            waveDisplay.innerText = "🚨 FINAL BOSS 🚨";
            zombiesToSpawn = 1; zombiesActive = 1;
            scoreDisplay.innerText = `Boss Health: ${bossHP}`;
        } else {
            scoreDisplay.innerText = `Zombies: ${zombiesActive}`;
        }
        spawnZombies();
    }

    document.addEventListener('touchstart', (e) => {
        if (!active) return;
        let x = e.touches[0].clientX;
        blaster.style.left = x + 'px';
        fireBolt(x);
    });

    function fireBolt(x) {
        const bolt = document.createElement('div');
        bolt.className = 'bolt';
        bolt.style.left = x + 'px';
        bolt.style.bottom = '35%';
        document.body.appendChild(bolt);

        let bY = 35;
        const fly = setInterval(() => {
            bY += 5;
            bolt.style.bottom = bY + '%';
            document.querySelectorAll('.zombie').forEach(z => {
                let bR = bolt.getBoundingClientRect();
                let zR = z.getBoundingClientRect();
                if (bR.left < zR.right && bR.right > zR.left && bR.top < zR.bottom && bR.bottom > zR.top) {
                    createSparkle(zR.left + (zR.width/2), zR.top + (zR.height/2));
                    bolt.remove(); clearInterval(fly);
                    
                    if (z.classList.contains('boss')) {
                        bossHP--;
                        scoreDisplay.innerText = `Boss Health: ${bossHP}`;
                        if (bossHP <= 0) { z.remove(); currentWave++; setTimeout(startWave, 1000); }
                    } else {
                        z.remove();
                        zombiesActive--;
                        scoreDisplay.innerText = `Zombies: ${zombiesActive}`;
                        if (zombiesActive <= 0) { currentWave++; setTimeout(startWave, 1000); }
                    }
                }
            });
            if (bY > 100) { clearInterval(fly); bolt.remove(); }
        }, 10);
    }

    function createSparkle(x, y) {
        for(let i=0; i<8; i++) {
            const s = document.createElement('div');
            s.className = 'sparkle'; s.innerHTML = '✨';
            s.style.left = x + 'px'; s.style.top = y + 'px';
            s.style.setProperty('--tx', (Math.random()*150-75)+'px');
            s.style.setProperty('--ty', (Math.random()*150-75)+'px');
            document.body.appendChild(s);
            setTimeout(() => s.remove(), 600);
        }
    }

    function spawnZombies() {
        if (!active || zombiesToSpawn <= 0) return;
        const z = document.createElement('div');
        z.className = 'zombie';
        z.innerHTML = currentWave === 10 ? '👹' : '🧟‍♂️';
        if(currentWave === 10) z.classList.add('boss');
        
        z.style.left = (Math.random() * 70 + 15) + 'vw';
        z.style.top = '-120px';
        document.body.appendChild(z);

        let y = -120;
        let speed = currentWave === 10 ? 1.5 : 2.5 + (currentWave * 0.3);
        
        const fall = setInterval(() => {
            if (!active) { clearInterval(fall); z.remove(); return; }
            y += speed; z.style.top = y + 'px';
            
            // Boss side-to-side movement
            if(currentWave === 10) {
                z.style.left = (50 + Math.sin(Date.now()/500) * 30) + 'vw';
            }

            if (y > window.innerHeight * 0.65) {
                lives--; updateLives(); z.remove();
                if (lives <= 0) { active = false; alert("The dream turned into a nightmare! Try again, Giselle."); location.reload(); }
                zombiesActive--;
                if (zombiesActive <= 0) { currentWave++; setTimeout(startWave, 1000); }
                clearInterval(fall);
            }
        }, 16);
        zombiesToSpawn--;
        if (currentWave < 10) setTimeout(spawnZombies, 1200 - (currentWave * 50));
    }

    function updateLives() { livesDisplay.innerText = "❤️".repeat(Math.max(0, lives)); }
    
    function endToSleep() {
        active = false;
        document.getElementById('game-stage').style.display = 'none';
        document.getElementById('sleep-scene').style.display = 'flex';
    }

    heroLane = 50;
</script>
</body>
</html>
