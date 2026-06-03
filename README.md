Berikut adalah kode HTML untuk game simulasi kucing interaktif seperti "Talking Angela", di mana kucing dapat bergerak, berbicara, dan memiliki kebutuhan sehari-hari seperti manusia.
```html
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Kucing Pintar - Virtual Pet Simulator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            min-height: 100vh;
            background: linear-gradient(145deg, #1a472a 0%, #2a5a3a 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Segoe UI', 'Quicksand', system-ui, -apple-system, 'Comic Neue', sans-serif;
            padding: 20px;
        }

        /* Container Utama */
        .game-container {
            max-width: 600px;
            width: 100%;
            background: #fdf8e7;
            border-radius: 72px;
            box-shadow: 0 30px 40px rgba(0, 0, 0, 0.4), inset 0 1px 4px rgba(255, 255, 255, 0.8);
            overflow: hidden;
            backdrop-filter: blur(2px);
            transition: all 0.2s ease;
        }

        /* Area kucing & animasi */
        .cat-area {
            background: #feeed6;
            padding: 30px 20px 20px;
            position: relative;
            border-bottom: 6px solid #ffcf9a;
            background-image: radial-gradient(circle at 10% 30%, rgba(255,215,170,0.4) 2%, transparent 2.5%);
            background-size: 28px 28px;
        }

        .cat-sprite {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            transition: transform 0.2s ease;
        }

        .cat-image {
            width: 200px;
            height: 200px;
            background: #f4b942;
            border-radius: 50%;
            position: relative;
            box-shadow: 0 12px 24px rgba(0, 0, 0, 0.2);
            transition: all 0.1s ease;
            cursor: pointer;
        }

        /* Wajah kucing (lucu) */
        .face {
            position: relative;
            width: 100%;
            height: 100%;
        }

        .ears {
            position: absolute;
            top: -20px;
            width: 100%;
            display: flex;
            justify-content: space-between;
            padding: 0 20px;
        }

        .ear {
            width: 45px;
            height: 45px;
            background: #f4b942;
            clip-path: polygon(20% 0%, 80% 0%, 100% 100%, 0% 100%);
            border-radius: 40% 40% 30% 30%;
        }

        .ear-left {
            transform: rotate(-15deg);
        }

        .ear-right {
            transform: rotate(15deg);
        }

        .eyes {
            position: absolute;
            top: 55px;
            width: 100%;
            display: flex;
            justify-content: space-evenly;
            padding: 0 35px;
        }

        .eye {
            width: 28px;
            height: 32px;
            background: white;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: inset 0 0 0 2px #7a4c1d;
        }

        .pupil {
            width: 14px;
            height: 18px;
            background: #1e2a1e;
            border-radius: 50%;
            transition: 0.05s linear;
        }

        .nose {
            position: absolute;
            top: 105px;
            left: 50%;
            transform: translateX(-50%);
            width: 22px;
            height: 18px;
            background: #ff8c8c;
            border-radius: 50% 50% 45% 45%;
        }

        .mouth {
            position: absolute;
            top: 130px;
            left: 50%;
            transform: translateX(-50%);
            width: 40px;
            height: 20px;
            border-bottom: 4px solid #8b5a2b;
            border-radius: 0 0 40px 40px;
        }

        /* status & balon ucapan */
        .speech-bubble {
            background: white;
            border-radius: 48px;
            padding: 12px 20px;
            max-width: 260px;
            margin: 15px auto 5px;
            text-align: center;
            font-weight: bold;
            color: #553b1f;
            font-size: 1.1rem;
            box-shadow: 0 8px 12px rgba(0,0,0,0.1);
            position: relative;
            transition: all 0.2s;
            border: 2px solid #ffcd94;
        }

        .speech-bubble::before {
            content: '';
            position: absolute;
            bottom: -12px;
            left: 30px;
            border-width: 12px 12px 0 0;
            border-style: solid;
            border-color: white transparent transparent transparent;
            filter: drop-shadow(2px 2px 0 #ffcd94);
        }

        /* panel statistik (hidup sehari-hari) */
        .stats-panel {
            background: #fff1e0;
            padding: 18px 20px;
            display: flex;
            justify-content: space-between;
            gap: 12px;
            flex-wrap: wrap;
            border-bottom: 2px solid #ffe0b5;
        }

        .stat {
            flex: 1;
            background: #ffffffcc;
            backdrop-filter: blur(4px);
            border-radius: 60px;
            padding: 6px 12px;
            text-align: center;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 8px;
            justify-content: center;
            box-shadow: inset 0 1px 2px #0001, 0 2px 5px rgba(0,0,0,0.05);
        }

        .stat span:first-child {
            font-size: 1.4rem;
        }

        .stat-value {
            font-size: 1.1rem;
            background: #2b2b2b20;
            padding: 4px 10px;
            border-radius: 40px;
            min-width: 55px;
        }

        /* Action buttons */
        .actions {
            padding: 20px 20px 30px;
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            justify-content: center;
            background: #f7ebd2;
        }

        .btn {
            background: #ffcd94;
            border: none;
            font-size: 1rem;
            font-weight: bold;
            padding: 12px 18px;
            border-radius: 60px;
            font-family: inherit;
            cursor: pointer;
            transition: 0.1s linear;
            box-shadow: 0 4px 0 #a56b2f;
            color: #3e2a1a;
            display: inline-flex;
            align-items: center;
            gap: 8px;
        }

        .btn:active {
            transform: translateY(2px);
            box-shadow: 0 1px 0 #a56b2f;
        }

        .btn-feed {
            background: #ffaa66;
            color: #412e1b;
        }

        .btn-play {
            background: #b3d9a0;
            box-shadow: 0 4px 0 #6f8f5a;
        }

        .btn-talk {
            background: #ffb7c5;
            box-shadow: 0 4px 0 #b25d6e;
        }

        /* animasi gerakan kucing */
        @keyframes wiggle {
            0% { transform: rotate(0deg);}
            25% { transform: rotate(8deg);}
            75% { transform: rotate(-8deg);}
            100% { transform: rotate(0deg);}
        }

        .cat-wiggle {
            animation: wiggle 0.25s ease-in-out;
        }

        .hungry-shake {
            animation: shake 0.4s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
        }

        @keyframes shake {
            0% { transform: translate(0,0);}
            25% { transform: translate(-4px, 0);}
            75% { transform: translate(4px, 0);}
            100% { transform: translate(0,0);}
        }

        .mood-badge {
            background: #ffffe0;
            border-radius: 100px;
            padding: 4px 12px;
            font-size: 0.75rem;
            font-weight: bold;
            text-align: center;
            margin-top: 6px;
            display: inline-block;
        }

        footer {
            text-align: center;
            font-size: 0.7rem;
            background: #e4d5bb;
            padding: 10px;
            color: #6b4c2c;
        }
    </style>
</head>
<body>

<div class="game-container">
    <div class="cat-area" id="catArea">
        <div class="cat-sprite" id="catSprite">
            <div class="cat-image" id="catImage">
                <div class="face">
                    <div class="ears">
                        <div class="ear ear-left"></div>
                        <div class="ear ear-right"></div>
                    </div>
                    <div class="eyes">
                        <div class="eye"><div class="pupil" id="pupilLeft"></div></div>
                        <div class="eye"><div class="pupil" id="pupilRight"></div></div>
                    </div>
                    <div class="nose"></div>
                    <div class="mouth"></div>
                </div>
            </div>
            <div class="speech-bubble" id="speechBubble">Meow! Aku kucing pintar~</div>
            <div class="mood-badge" id="moodText">😺 Ceria</div>
        </div>
    </div>

    <div class="stats-panel">
        <div class="stat"><span>🍗</span> <span class="stat-value" id="hungerStat">70</span> <span>Lapar</span></div>
        <div class="stat"><span>💧</span> <span class="stat-value" id="thirstStat">70</span> <span>Haus</span></div>
        <div class="stat"><span>⚡</span> <span class="stat-value" id="energyStat">80</span> <span>Energi</span></div>
        <div class="stat"><span>😊</span> <span class="stat-value" id="happyStat">75</span> <span>Kebahagiaan</span></div>
    </div>

    <div class="actions">
        <button class="btn btn-feed" id="feedBtn">🍖 Beri Makan</button>
        <button class="btn btn-feed" id="waterBtn">💧 Beri Minum</button>
        <button class="btn btn-play" id="playBtn">🧸 Bermain</button>
        <button class="btn btn-talk" id="talkBtn">💬 Ajak Bicara</button>
        <button class="btn" id="sleepBtn">😴 Tidur Siang</button>
    </div>
    <footer>✨ Sentuh tubuh kucing untuk diajak bicara! Kelola kebutuhannya sehari-hari ✨</footer>
</div>

<script>
    // Data kondisi kucing (seperti manusia + kebutuhan)
    let catState = {
        hunger: 70,      // 0 = sangat lapar, 100 = kenyang
        thirst: 70,
        energy: 80,
        happiness: 75,
        isSleeping: false,
        lastUpdate: Date.now()
    };

    // Daftar respon bicara (life-like)
    const talkResponses = [
        "Hai! Namaku Momo, bagaimana harimu?", "Aku suka ikan dan susu! 😻", "Hari ini cerah, ayo main yuk!",
        "Perutku keroncongan... mungkin lapar?", "Kamu sahabat terbaikku!❤️", "Meong~ aku bisa bicara loh!",
        "Jangan lupa minum, ya!", "Aku ingin tidur siang sebentar...", "Wah kamu baik sekali.",
        "Ceritakan tentang dirimu dong!", "Aku kucing modern, bisa ngobrol hehe", "Ayo bermain bola benang!",
        "Nyanyikan aku lagu? Meow meow~"
    ];
    
    let currentMoodLabel = "😺 Ceria";
    let speechTimeout = null;
    let gameLoopInterval = null;
    let energyTimer = null;

    // DOM elements
    const hungerSpan = document.getElementById('hungerStat');
    const thirstSpan = document.getElementById('thirstStat');
    const energySpan = document.getElementById('energyStat');
    const happySpan = document.getElementById('happyStat');
    const speechBubble = document.getElementById('speechBubble');
    const moodTextSpan = document.getElementById('moodText');
    const catSpriteDiv = document.getElementById('catSprite');
    const catImageDiv = document.getElementById('catImage');
    const pupilLeft = document.getElementById('pupilLeft');
    const pupilRight = document.getElementById('pupilRight');

    // update tampilan statistik & mood berdasarkan state
    function updateUI() {
        hungerSpan.innerText = Math.floor(catState.hunger);
        thirstSpan.innerText = Math.floor(catState.thirst);
        energySpan.innerText = Math.floor(catState.energy);
        happySpan.innerText = Math.floor(catState.happiness);
        
        // update Mood text berdasarkan parameter
        let moodMsg = "";
        if (catState.hunger < 25) moodMsg = "🍽️ Sangat Lapar!";
        else if (catState.thirst < 20) moodMsg = "💧 Kehausan!";
        else if (catState.energy < 20) moodMsg = "😴 Mengantuk & Lesu";
        else if (catState.happiness < 30) moodMsg = "😢 Sedih, ajak main!";
        else if (catState.hunger > 80 && catState.happiness > 70) moodMsg = "😻 Sangat Bahagia!";
        else if (catState.energy > 70 && catState.happiness > 60) moodMsg = "⚡ Aktif & Ceria";
        else moodMsg = "🐱 Sehari-hari santai";
        
        if (catState.isSleeping) moodMsg = "💤 Tidur nyenyak... zzz";
        currentMoodLabel = moodMsg;
        moodTextSpan.innerText = currentMoodLabel;
        
        // update wajah pupil mengikuti 'mouse' sederhana (efek hidup)
        // tambah efek jika lapar atau senang
        if (catState.hunger < 20) {
            catImageDiv.classList.add('hungry-shake');
            setTimeout(() => catImageDiv.classList.remove('hungry-shake'), 500);
        }
    }
    
    // Set percakapan dengan random
    function speak(message, isAuto = false) {
        if (speechTimeout) clearTimeout(speechTimeout);
        speechBubble.innerText = message;
        // tambahkan efek suara imajinasi (senyum)
        if (!isAuto) {
            // sedikit gerakan kucing ketika bicara
            catSpriteDiv.classList.add('cat-wiggle');
            setTimeout(() => catSpriteDiv.classList.remove('cat-wiggle'), 300);
        }
        // auto hilang setelah 3 detik kecuali ada pesan baru
        speechTimeout = setTimeout(() => {
            if (!catState.isSleeping) {
                let randomGreet = ["Meow~", "Aku disini loh!", "Senang bersama kamu!", "purrr..."];
                speechBubble.innerText = randomGreet[Math.floor(Math.random() * randomGreet.length)];
            } else {
                speechBubble.innerText = "Zzz... mimpi indah... 😴";
            }
        }, 3500);
    }
    
    // Ajak bicara interaktif berdasarkan state
    function talkToCat() {
        if (catState.isSleeping) {
            speak("Hush... aku sedang tidur. Bangunkan aku nanti ya 😴");
            return;
        }
        let customMsg = "";
        if (catState.hunger < 25) customMsg = "Aku sangat lapar! Tolong beri aku makanan dong 🥺";
        else if (catState.thirst < 20) customMsg = "Haus... minum dulu yuk 💧";
        else if (catState.energy < 20) customMsg = "Aku ngantuk sekali... boleh tidur sebentar?";
        else if (catState.happiness < 35) customMsg = "Aku merasa kurang bahagia, ayo bermain bersamaku!";
        else {
            let randomIndex = Math.floor(Math.random() * talkResponses.length);
            customMsg = talkResponses[randomIndex];
        }
        // meningkatkan sedikit happiness ketika diajak bicara (interaksi sosial)
        if (!catState.isSleeping) {
            catState.happiness = Math.min(100, catState.happiness + 8);
            catState.energy = Math.max(0, catState.energy - 2);
            speak(customMsg);
            updateUI();
            checkNeedsAndReact();
        } else {
            speak(customMsg);
        }
    }
    
    // makan
    function feed() {
        if (catState.isSleeping) { speak("Zzz... tidak bisa makan sambil tidur.."); return; }
        let increase = 28;
        catState.hunger = Math.min(100, catState.hunger + increase);
        catState.happiness = Math.min(100, catState.happiness + 5);
        speak("Nyam nyam... enaknya! Terima kasih! 🍗", false);
        updateUI();
        animateHappy();
        checkNeedsAndReact();
    }
    
    // minum
    function giveWater() {
        if (catState.isSleeping) { speak("Aku mimpi minum susu... hehe"); return; }
        catState.thirst = Math.min(100, catState.thirst + 30);
        catState.happiness = Math.min(100, catState.happiness + 3);
        speak("Segarrr... terima kasih 💧😸", false);
        updateUI();
        animateHappy();
        checkNeedsAndReact();
    }
    
    // bermain
    function play() {
        if (catState.isSleeping) { speak("Tidur dulu... nanti main yaa."); return; }
        if (catState.energy < 15) {
            speak("Aku terlalu lelah untuk bermain... ajak aku tidur dulu ya 😿");
            return;
        }
        catState.happiness = Math.min(100, catState.happiness + 20);
        catState.energy = Math.max(0, catState.energy - 15);
        catState.hunger = Math.max(0, catState.hunger - 6);
        catState.thirst = Math.max(0, catState.thirst - 5);
        speak("Hore! Aku senang sekali bermain bola benang! 🧶🎉", false);
        updateUI();
        // gerakan excited
        catSpriteDiv.classList.add('cat-wiggle');
        setTimeout(() => catSpriteDiv.classList.remove('cat-wiggle'), 400);
        checkNeedsAndReact();
    }
    
    // tidur siang (meningkatkan energi, happiness sedikit)
    function sleepNap() {
        if (catState.isSleeping) {
            speak("Aku masih tidur... jangan ganggu 😴");
            return;
        }
        catState.isSleeping = true;
        speak("Zzz.. aku tidur siang dulu ya... bangunkan dalam 5 detik... 💤");
        // efek mengurangi kelaparan dan haus lebih lambat saat tidur? tapi kita tetap jalankan tick ringan
        // kita set timer bangun otomatis setelah 5 detik
        if (window.sleepTimer) clearTimeout(window.sleepTimer);
        window.sleepTimer = setTimeout(() => {
            if (catState.isSleeping) {
                catState.isSleeping = false;
                catState.energy = Math.min(100, catState.energy + 35);
                catState.happiness = Math.min(100, catState.happiness + 8);
                updateUI();
                speak("Aku bangun! Segar bugar! ☀️ Meong~");
                checkNeedsAndReact();
            }
        }, 5000);
        updateUI();
        // jangan lupa stat turun dikit saat tidur? justru energi naik, haus/lapar naik sedikit realistis
        // tapi saat tidur tidak update penurunan berat dari loop, biar fair
    }
    
    function animateHappy() {
        catImageDiv.style.transform = "scale(1.02)";
        setTimeout(() => catImageDiv.style.transform = "", 180);
    }
    
    // pengecekan jika kondisi darurat: lapar/haus parah, maka kucing akan protes otomatis
    function checkNeedsAndReact() {
        if (catState.isSleeping) return;
        if (catState.hunger < 15) {
            speak("Aku kelaparan! Beri makan segera!! 🆘", true);
            catImageDiv.classList.add('hungry-shake');
            setTimeout(() => catImageDiv.classList.remove('hungry-shake'), 600);
            catState.happiness = Math.max(0, catState.happiness - 3);
        } else if (catState.thirst < 15) {
            speak("Haus... Haus.. tolong minumkan aku 😭💧", true);
            catState.happiness = Math.max(0, catState.happiness - 2);
        } else if (catState.energy < 15 && !catState.isSleeping) {
            speak("Aku mengantuk sekali... izinkan tidur sebentar ya...", true);
        } else if (catState.happiness < 20) {
            speak("Aku sedih... ingin bermain atau diajak bicara... 😢", true);
        } else if (catState.hunger > 30 && catState.happiness > 60 && Math.random() < 0.2) {
            // kadang ngobrol random hidup
            let dailyRemarks = ["Aku suka lihat langit!", "Hari ini aku merasa baik~", "Kamu hebat merawatku!"];
            speak(dailyRemarks[Math.floor(Math.random() * dailyRemarks.length)], true);
        }
        updateUI();
    }
    
    // Simulasi penurunan statistik setiap detik (hidup sehari-hari)
    function gameTick() {
        if (catState.isSleeping) {
            // selama tidur: hanya turun sangat kecil hunger/thirst, energi naik perlahan hingga batas max di timer bangun sudah atur, tapi tetap ada efek tidak ekstrim
            catState.hunger = Math.max(0, catState.hunger - 0.3);
            catState.thirst = Math.max(0, catState.thirst - 0.4);
            // happiness stabil dikit
            catState.happiness = Math.min(100, catState.happiness + 0.2);
            updateUI();
            return;
        }
        // penurunan normal seperti manusia
        let hungerDrop = 0.9;
        let thirstDrop = 1.0;
        let energyDrop = 0.7;   // berkurang karena aktivitas dasar
        let happinessDrop = 0.2;
        
        // jika lapar parah, happiness turun lebih cepat
        if (catState.hunger < 25) happinessDrop += 0.7;
        if (catState.thirst < 25) happinessDrop += 0.6;
        if (catState.energy < 20) happinessDrop += 0.5;
        
        catState.hunger = Math.max(0, catState.hunger - hungerDrop);
        catState.thirst = Math.max(0, catState.thirst - thirstDrop);
        catState.energy = Math.max(0, catState.energy - energyDrop);
        catState.happiness = Math.max(0, catState.happiness - happinessDrop);
        
        // jika energy sangat rendah, secara natural bisa panggil tidur? tapi biar user pilih tidur, hanya peringatan
        if (catState.energy < 5 && !catState.isSleeping) {
            speak("Aku sangat lelah... mungkin saatnya tidur siang (klik tidur)", true);
        }
        
        // batas maksimum
        catState.happiness = Math.min(100, catState.happiness);
        catState.energy = Math.min(100, catState.energy);
        
        updateUI();
        // cek reaksi karena kebutuhan kritis
        if (Math.random() < 0.1) checkNeedsAndReact();
        // update wajah pupil bergerak mengikuti cursor lembut (efek hidup)
    }
    
    // gerakan pupil mengikuti mouse dalam area cat (hidup)
    function handleMousemove(e) {
        const rect = catImageDiv.getBoundingClientRect();
        const centerX = rect.left + rect.width / 2;
        const centerY = rect.top + rect.height / 2;
        let mouseX = e.clientX;
        let mouseY = e.clientY;
        let deltaX = (mouseX - centerX) / (rect.width / 2);
        let deltaY = (mouseY - centerY) / (rect.height / 2);
        deltaX = Math.min(0.5, Math.max(-0.5, deltaX));
        deltaY = Math.min(0.4, Math.max(-0.4, deltaY));
        const shiftX = deltaX * 8;
        const shiftY = deltaY * 6;
        pupilLeft.style.transform = `translate(${shiftX}px, ${shiftY}px)`;
        pupilRight.style.transform = `translate(${shiftX}px, ${shiftY}px)`;
    }
    
    // Reset animasi tidur jika diperlukan
    function cancelSleepIfNeeded() {
        if (catState.isSleeping && window.sleepTimer) {
            clearTimeout(window.sleepTimer);
            catState.isSleeping = false;
            speak("Oh, kamu membangunkanku! ada apa? 😺");
            updateUI();
        }
    }
    
    // event listeners
    function init() {
        updateUI();
        gameLoopInterval = setInterval(() => gameTick(), 1000);
        
        document.getElementById('feedBtn').addEventListener('click', () => { cancelSleepIfNeeded(); feed(); });
        document.getElementById('waterBtn').addEventListener('click', () => { cancelSleepIfNeeded(); giveWater(); });
        document.getElementById('playBtn').addEventListener('click', () => { cancelSleepIfNeeded(); play(); });
        document.getElementById('talkBtn').addEventListener('click', () => { cancelSleepIfNeeded(); talkToCat(); });
        document.getElementById('sleepBtn').addEventListener('click', () => { sleepNap(); });
        
        // klik pada gambar kucing (ajak bicara langsung)
        catImageDiv.addEventListener('click', (e) => { 
            e.stopPropagation();
            cancelSleepIfNeeded();
            talkToCat(); 
        });
        
        // gerakan pupil mengikuti cursor (efek hidup)
        document.addEventListener('mousemove', handleMousemove);
        
        // tambahkan pesan awal setelah 1 detik
        setTimeout(() => {
            speak("Halo! Aku Momo, kucing pintar. Ayo rawat aku setiap hari! 🐾", false);
        }, 800);
    }
    
    init();
</script>
</body>
</html>
```
