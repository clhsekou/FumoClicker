<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fumo Clicker</title>
    <style>
        /* General Body and Background */
        body {
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: black;
            font-family: sans-serif;
            overflow: hidden; /* Prevent scrollbars */
        }

        /* --- BACKGROUND STYLES --- */
        #bg-video-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            background-size: cover;
            background-position: center center;
            transition: background-image 0.5s ease-in-out, filter 0.5s ease;
            filter: blur(5px); /* Start blurred by default */
        }

        #bg-video-container.unblurred {
            filter: none !important;
        }

        #bg-video {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: opacity 0.5s ease-in-out;
            opacity: 1;
        }

        #bg-video-container.bg-active-image #bg-video {
           opacity: 0;
        }
        
        /* Typography */
        h1, h2, p {
            color: white;
            text-align: center;
            text-shadow: 2px 2px 8px black, 0 0 10px #333;
        }

        #counter {
            font-size: 2.5em;
            font-weight: bold;
            color: white;
            text-shadow: 2px 2px 8px black, 0 0 10px #333;
        }

        /* Panels and Containers */
        .panel {
            border-radius: 12px;
            box-shadow: 0 0 12px #222;
            padding: 16px;
            margin: 12px 0;
            background: rgba(0, 0, 0, 0.5);
            width: 300px;
        }

        /* Buttons */
        button {
            background: none;
            border: none;
            outline: none;
            color: white;
            cursor: pointer;
        }

        #clickButton {
            padding: 0;
            border-radius: 50%;
            transition: transform 0.1s ease;
        }

        #clickButton:active {
            transform: scale(0.95);
        }

        #FumoImage {
            width: 200px;
            height: 200px;
        }

        .bounce {
            animation: bounceClick 0.15s ease;
        }

        @keyframes bounceClick {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(0.9); }
        }

        #fullscreenBtn {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: rgba(0,0,0,0.5);
            padding: 10px;
            border-radius: 50%;
            z-index: 100;
            transition: opacity 0.3s ease;
        }
        #fullscreenBtn:hover {
            background: rgba(0,0,0,0.8);
        }
        
        /* Hide fullscreen button when fullscreen is active */
        body.fullscreen-active #fullscreenBtn {
            opacity: 0;
            pointer-events: none;
        }


        /* Shop Styling */
        #shop button {
            width: 100%;
            background: linear-gradient(90deg, #222 0%, #444 100%);
            color: #ffd700;
            font-weight: bold;
            border: 2px solid #ffd700;
            border-radius: 8px;
            padding: 12px 18px;
            margin: 8px 0;
            font-size: 1em;
            box-shadow: 0 2px 8px #222;
            transition: background 0.2s, color 0.2s, border-color 0.2s;
        }

        #shop button:hover {
            background: linear-gradient(90deg, #ffd700 0%, #444 100%);
            color: #222;
            border-color: #fff700;
        }

        /* Stats Section */
        #stats p:nth-child(2) {
            color: #ffd700;
            font-weight: bold;
            font-size: 1.2em;
        }

        /* Popup Message */
        #popup {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0.5);
            background: rgba(0, 0, 0, 0.85);
            color: white;
            padding: 20px 30px;
            border-radius: 10px;
            font-size: 20px;
            z-index: 1000;
            pointer-events: none;
            opacity: 0;
            transition: opacity 0.5s ease, transform 0.5s ease;
        }

        #popup.show {
            display: block;
            opacity: 1;
            transform: translate(-50%, -50%) scale(1);
        }

        /* Admin Panel */
        #adminPanel {
            position: fixed;
            top: 50%;
            transform: translateY(-50%);
            right: -320px;
            width: 280px;
            background: rgba(0,0,0,0.95);
            color: white;
            padding: 20px;
            box-shadow: -2px 0 10px rgba(0,0,0,0.5);
            transition: right 0.4s ease;
            z-index: 999;
            border-radius: 12px 0 0 12px;
        }

        #adminPanel.open {
            right: 0;
        }

        #adminPanel button {
            width: 100%;
            margin-top: 10px;
            padding: 10px;
            background: rgba(255, 255, 255, 0.1);
            color: white;
            border: 1px solid white;
            border-radius: 8px;
        }
         #adminPanel button:hover {
            background: rgba(255,255,255,0.2);
        }

        /* SONG SELECTOR STYLES */
        #sidebar {
            position: fixed;
            top: 0;
            left: 0;
            width: 280px;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(8px);
            transform: translateX(-100%);
            transition: transform 0.4s ease;
            z-index: 500;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            align-content: flex-start;
        }
        #sidebar.open {
            transform: translateX(0);
        }
        #songGrid {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 20px;
            padding: 60px 10px 20px 10px;
        }
        #toggle {
            position: fixed;
            top: 20px;
            left: 20px;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 10px 15px;
            border-radius: 8px;
            cursor: pointer;
            z-index: 501;
            transition: left 0.4s ease;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        #toggle.open {
            left: 300px;
        }
        .song-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 8px;
            cursor: pointer;
            background: transparent;
            padding: 5px;
            border: 2px solid transparent;
            border-radius: 8px;
        }
        .song-item .img-container {
            perspective: 500px;
        }
        .song-item img {
            width: 80px;
            height: 80px;
            object-fit: cover;
            border-radius: 8px;
            transition: transform 0.3s ease;
        }
        .song-item:hover img {
            transform: scale(1.1) rotateX(-25deg);
        }
        .song-item .title {
            color: white;
            font-size: 0.8em;
            text-align: center;
            max-width: 90px;
        }
        .song-item.selected {
             border-color: #ffd700;
        }
        
        /* AUDIO CONTROLS (for songs) */
        #audioControls {
            position: sticky;
            bottom: 0;
            left: 0;
            width: 100%;
            padding: 15px;
            background: rgba(0, 0, 0, 0.8);
            box-sizing: border-box;
            margin-top: auto;
        }
        #audioControls.hidden {
            display: none;
        }
        #nowPlayingTitle {
            display: block;
            text-align: center;
            font-size: 0.9em;
            font-weight: bold;
            margin-bottom: 10px;
            color: #ffd700;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        /* SHARED STYLES for progress bars and time */
        .progress-container {
            width: 100%;
            height: 8px;
            background-color: #444;
            border-radius: 4px;
            cursor: pointer;
        }
        .progress-bar {
            width: 0%;
            height: 100%;
            background-color: #ffd700;
            border-radius: 4px;
        }
        .time-container {
            font-size: 0.8em;
            color: #ccc;
            display: flex;
            justify-content: space-between;
            margin-top: 8px;
        }
        
        /* --- VIDEO CONTROLS STYLES --- */
        #videoControls {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            width: 350px;
            max-width: 90%;
            background: rgba(0,0,0,0.5);
            backdrop-filter: blur(5px);
            border-radius: 10px;
            padding: 8px 15px;
            display: flex;
            align-items: center;
            gap: 10px; /* Adjusted gap */
            z-index: 100;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.3s ease;
        }

        #bg-video-container:fullscreen #videoControls {
            opacity: 1;
            pointer-events: auto;
        }

        #playPauseBtn, #muteBtn {
            padding: 5px;
        }
        #videoTime {
            font-size: 0.8em;
            color: #ccc;
            white-space: nowrap;
            margin-left: 5px;
        }
        #videoProgressContainer {
            flex-grow: 1;
        }
        #videoProgressBar {
            background-color: #fff;
        }

    </style>
</head>
<body>
    <!-- Background Container -->
    <div id="bg-video-container">
        <video id="bg-video" autoplay loop muted playsinline>
            <source src="https://files.catbox.moe/ypvzka.mp4" type="video/mp4">
        </video>
        
        <!-- VIDEO CONTROLS ARE INSIDE THE CONTAINER -->
        <div id="videoControls">
            <button id="playPauseBtn" aria-label="Pause video"></button>
            <button id="muteBtn" aria-label="Unmute video"></button>
            <div class="progress-container" id="videoProgressContainer">
                <div class="progress-bar" id="videoProgressBar"></div>
            </div>
             <div id="videoTime" class="time-container">
                 <span id="videoCurrentTime">00:00</span>&nbsp;/&nbsp;<span id="videoTotalDuration">00:00</span>
            </div>
        </div>
    </div>

    <!-- Song Sidebar -->
    <div id="sidebar">
        <div id="songGrid">
            <!-- Song 1 -->
            <div class="song-item" data-file="https://file.garden/aJiJJr95R1hAB59P/Natsusan%20-%20Regression%20.mp3" data-title="Regression - Natsu" tabindex="0" role="button">
                <div class="img-container">
                    <img src="https://file.garden/aJiJJr95R1hAB59P/15daf2aa87cca76df456c0e73682149b.jpg" alt="Regression - Natsu cover" />
                </div>
                <span class="title">Regression - Natsu</span>
            </div>
            <!-- Song 2 (Moved to be inside the grid) -->
            <div class="song-item" data-file="https://file.garden/aJiJJr95R1hAB59P/Bad%20Apple__%20.mp3" data-title="Bad Apple - Alstroemeria Records" tabindex="0" role="button">
                <div class="img-container">
                    <img src="https://file.garden/aJiJJr95R1hAB59P/Bad%20Apple%20Wallpaper%20High%20Resolution.jpg" alt="Bad Apple - Alstroemeria Records cover" />
                </div>
                <span class="title">Bad Apple - Alstroemeria Records</span>
            </div>
        </div>
        <div id="audioControls" class="hidden">
            <span id="nowPlayingTitle"></span>
            <div class="progress-container" id="songProgressContainer">
                <div class="progress-bar" id="songProgressBar"></div>
            </div>
            <div class="time-container">
                <span id="songCurrentTime">00:00</span>
                <span id="songTotalDuration">00:00</span>
            </div>
        </div>
    </div>


    <!-- Sidebar Toggle Button -->
    <div id="toggle" aria-label="Toggle song sidebar" role="button" tabindex="0">
        <span id="toggleArrow">&gt;</span> <span id="toggleLabel">Songs</span>
    </div>

    <!-- Main Content -->
    <h1>Fumo Clicker</h1>
    <p>Click the Fumo to get more Fumos!</p>
    <span id="counter">0</span>

    <button id="clickButton">
        <img id="FumoImage" src="https://files.catbox.moe/xg7esw.webp" alt="Clickable Fumo">
    </button>

    <div id="stats" class="panel">
        <p>Click Power: <span id="clickPower">1</span></p>
        <p>Auto Clickers: <span id="autoClickerPower">0</span> /sec</p>
    </div>

    <div id="shop" class="panel">
        <h2>Shop</h2>
        <button id="buyUpgradeBtn">Upgrade Click (+1) - Cost: <span id="upgradeCost">10</span></button>
        <button id="buyAutoClickerBtn">Auto Clicker (+1/sec) - Cost: <span id="autoClickerCost">50</span></button>
        <button id="adminLoginBtn">Admin Login</button>
    </div>

    <!-- Hidden Elements -->
    <audio id="audio-player" style="display:none;"></audio>
    <div id="popup"></div>
    <button id="fullscreenBtn" aria-label="Toggle fullscreen mode">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="white" aria-hidden="true" focusable="false">
            <path d="M7 14H5v5h5v-2H7v-3zm0-4V7h3V5H5v5h2zm10 7h-3v2h5v-5h-2v3zm-3-12v2h3v3h2V5h-5z"/>
        </svg>
    </button>
    
    <!-- Admin Panel -->
    <div id="adminPanel">
        <h2>Admin Panel</h2>
        <button id="give1kFumos">+1,000 Fumos</button>
        <button id="give10kFumos">+10,000 Fumos</button>
        <button id="resetStatsBtn">Reset Stats</button>
        <button id="closeAdminBtn">Close</button>
    </div>

    <script>
        // --- STATE VARIABLES ---
        let count = 0, clickPower = 1, autoClickerPower = 0;
        let upgradeCost = 10, autoClickerCost = 50;
        let autoClickerInterval = null;

        // --- DOM ELEMENTS ---
        const counterEl = document.getElementById('counter');
        const clickButton = document.getElementById('clickButton');
        const fumoImage = document.getElementById('FumoImage');
        const clickPowerEl = document.getElementById('clickPower');
        const autoClickerPowerEl = document.getElementById('autoClickerPower');
        const upgradeCostEl = document.getElementById('upgradeCost');
        const autoClickerCostEl = document.getElementById('autoClickerCost');
        const buyUpgradeBtn = document.getElementById('buyUpgradeBtn');
        const buyAutoClickerBtn = document.getElementById('buyAutoClickerBtn');
        const popup = document.getElementById('popup');
        const fullscreenBtn = document.getElementById('fullscreenBtn');
        
        const bgVideoContainer = document.getElementById('bg-video-container');
        const bgVideo = document.getElementById('bg-video');
        const toggleBtn = document.getElementById('toggle');
        const sidebar = document.getElementById('sidebar');
        const songItems = document.querySelectorAll('.song-item');
        const audioPlayer = document.getElementById('audio-player');
        
        // SONG Audio Controls
        const audioControls = document.getElementById('audioControls');
        const nowPlayingTitle = document.getElementById('nowPlayingTitle');
        const songProgressContainer = document.getElementById('songProgressContainer');
        const songProgressBar = document.getElementById('songProgressBar');
        const songCurrentTimeEl = document.getElementById('songCurrentTime');
        const songTotalDurationEl = document.getElementById('songTotalDuration');

        // VIDEO Controls
        const videoControls = document.getElementById('videoControls');
        const playPauseBtn = document.getElementById('playPauseBtn');
        const muteBtn = document.getElementById('muteBtn'); // New Mute Button
        const videoProgressContainer = document.getElementById('videoProgressContainer');
        const videoProgressBar = document.getElementById('videoProgressBar');
        const videoCurrentTimeEl = document.getElementById('videoCurrentTime');
        const videoTotalDurationEl = document.getElementById('videoTotalDuration');
        
        // Player Icons
        const pauseIcon = `<svg width="24" height="24" viewBox="0 0 24 24" fill="white"><path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"/></svg>`;
        const playIcon = `<svg width="24" height="24" viewBox="0 0 24 24" fill="white"><path d="M8 5v14l11-7z"/></svg>`;
        const volumeIcon = `<svg width="24" height="24" viewBox="0 0 24 24" fill="white"><path d="M3 9v6h4l5 5V4L7 9H3zm13.5 3c0-1.77-1.02-3.29-2.5-4.03v8.05c1.48-.73 2.5-2.25 2.5-4.02zM14 3.23v2.06c2.89.86 5 3.54 5 6.71s-2.11 5.85-5 6.71v2.06c4.01-.91 7-4.49 7-8.77s-2.99-7.86-7-8.77z"/></svg>`;
        const muteIcon = `<svg width="24" height="24" viewBox="0 0 24 24" fill="white"><path d="M16.5 12c0-1.77-1.02-3.29-2.5-4.03v2.21l2.45 2.45c.03-.2.05-.41.05-.63zm2.5 0c0 .94-.2 1.82-.54 2.64l1.51 1.51C20.63 14.91 21 13.5 21 12c0-4.28-2.99-7.86-7-8.77v2.06c2.89.86 5 3.54 5 6.71zM4.27 3L3 4.27 7.73 9H3v6h4l5 5v-6.73l4.25 4.25c-.67.52-1.42.93-2.25 1.18v2.06c1.38-.31 2.63-.95 3.69-1.81L19.73 21 21 19.73l-9-9L4.27 3zM12 4L9.91 6.09 12 8.18V4z"/></svg>`;

        // Admin Panel Elements
        const adminPanel = document.getElementById('adminPanel');
        const adminLoginBtn = document.getElementById('adminLoginBtn');
        const closeAdminBtn = document.getElementById('closeAdminBtn');
        const give1kBtn = document.getElementById('give1kFumos');
        const give10kBtn = document.getElementById('give10kFumos');
        const resetStatsBtn = document.getElementById('resetStatsBtn');
        
        // --- FUNCTIONS ---
        function formatTime(seconds) {
            const minutes = Math.floor(seconds / 60);
            const secs = Math.floor(seconds % 60);
            return `${minutes.toString().padStart(2, '0')}:${secs.toString().padStart(2, '0')}`;
        }
        function updateDisplay() {
            counterEl.textContent = count;
            clickPowerEl.textContent = clickPower;
            autoClickerPowerEl.textContent = autoClickerPower;
            upgradeCostEl.textContent = upgradeCost;
            autoClickerCostEl.textContent = autoClickerCost;
        }
        function showPopup(message) {
            popup.textContent = message;
            popup.classList.add('show');
            setTimeout(() => { popup.classList.remove('show'); }, 1500);
        }
        function startAutoClickers() {
            if (autoClickerInterval) clearInterval(autoClickerInterval);
            if (autoClickerPower > 0) {
                autoClickerInterval = setInterval(() => {
                    count += autoClickerPower;
                    updateDisplay();
                }, 1000);
            }
        }
        
        // --- EVENT HANDLERS ---
        clickButton.addEventListener('click', () => {
            count += clickPower;
            updateDisplay();
            fumoImage.classList.add('bounce');
            setTimeout(() => fumoImage.classList.remove('bounce'), 150);
            if (Math.random() < 0.01) {
                const bonus = 500;
                count += bonus;
                updateDisplay();
                showPopup(`🎉 Lucky! +${bonus} fumos 🎉`);
            }
        });
        buyUpgradeBtn.addEventListener('click', () => {
            if (count >= upgradeCost) {
                count -= upgradeCost;
                clickPower++;
                upgradeCost = Math.floor(upgradeCost * 1.15);
                updateDisplay();
                showPopup('Click power upgraded!');
            } else {
                showPopup('Not enough Fumos!');
            }
        });
        buyAutoClickerBtn.addEventListener('click', () => {
            if (count >= autoClickerCost) {
                count -= autoClickerCost;
                autoClickerPower++;
                autoClickerCost = Math.floor(autoClickerCost * 1.25);
                updateDisplay();
                showPopup('Auto clicker purchased!');
                startAutoClickers();
            } else {
                showPopup('Not enough Fumos!');
            }
        });
        
        // FULLSCREEN
        fullscreenBtn.addEventListener('click', () => {
            if (!document.fullscreenElement) {
                bgVideoContainer.requestFullscreen();
            } else {
                document.exitFullscreen();
            }
        });

        document.addEventListener('fullscreenchange', () => {
            const isFullscreen = !!document.fullscreenElement;
            bgVideoContainer.classList.toggle('unblurred', isFullscreen);
            document.body.classList.toggle('fullscreen-active', isFullscreen);
        });

        toggleBtn.addEventListener('click', () => {
            const isOpen = sidebar.classList.toggle('open');
            toggleBtn.classList.toggle('open');
            toggleBtn.querySelector('#toggleArrow').textContent = isOpen ? '<' : '>';
        });

        // SONG SELECTION LOGIC
        songItems.forEach(item => {
            item.addEventListener('click', () => {
                const isSelected = item.classList.contains('selected');
                songItems.forEach(s => s.classList.remove('selected'));
                if (isSelected) {
                    audioPlayer.pause();
                    audioControls.classList.add('hidden');
                    bgVideoContainer.style.backgroundImage = '';
                    bgVideoContainer.classList.remove('bg-active-image');
                } else {
                    item.classList.add('selected');
                    const audioSrc = item.dataset.file;
                    const title = item.dataset.title;
                    const imgSrc = item.querySelector('img').src;
                    if (audioSrc) {
                        audioPlayer.src = audioSrc;
                        audioPlayer.play();
                        nowPlayingTitle.textContent = title;
                        audioControls.classList.remove('hidden');
                        showPopup(`Now Playing: ${title}`);
                        bgVideoContainer.style.backgroundImage = `url('${imgSrc}')`;
                        bgVideoContainer.classList.add('bg-active-image');
                    }
                }
            });
        });
        
        // SONG AUDIO PLAYER LISTENERS
        audioPlayer.addEventListener('timeupdate', () => {
            const { currentTime, duration } = audioPlayer;
            if (duration) {
                songProgressBar.style.width = `${(currentTime / duration) * 100}%`;
                songCurrentTimeEl.textContent = formatTime(currentTime);
            }
        });
        audioPlayer.addEventListener('loadedmetadata', () => {
            songTotalDurationEl.textContent = formatTime(audioPlayer.duration);
        });
        songProgressContainer.addEventListener('click', (e) => {
            const width = songProgressContainer.clientWidth;
            const clickX = e.offsetX;
            const duration = audioPlayer.duration;
            if (duration) audioPlayer.currentTime = (clickX / width) * duration;
        });

        // VIDEO CONTROLS LISTENERS
        playPauseBtn.addEventListener('click', () => {
            if (bgVideo.paused) {
                bgVideo.play();
                playPauseBtn.innerHTML = pauseIcon;
            } else {
                bgVideo.pause();
                playPauseBtn.innerHTML = playIcon;
            }
        });

        muteBtn.addEventListener('click', () => {
            bgVideo.muted = !bgVideo.muted;
            muteBtn.innerHTML = bgVideo.muted ? muteIcon : volumeIcon;
        });

        bgVideo.addEventListener('timeupdate', () => {
            const { currentTime, duration } = bgVideo;
            if (duration) {
                videoProgressBar.style.width = `${(currentTime / duration) * 100}%`;
                videoCurrentTimeEl.textContent = formatTime(currentTime);
            }
        });
        bgVideo.addEventListener('loadedmetadata', () => {
            videoTotalDurationEl.textContent = formatTime(bgVideo.duration);
            playPauseBtn.innerHTML = bgVideo.paused ? playIcon : pauseIcon;
            muteBtn.innerHTML = bgVideo.muted ? muteIcon : volumeIcon;
        });
        videoProgressContainer.addEventListener('click', (e) => {
            const width = videoProgressContainer.clientWidth;
            const clickX = e.offsetX;
            const duration = bgVideo.duration;
            if (duration) bgVideo.currentTime = (clickX / width) * duration;
        });
        
        // ADMIN PANEL LOGIC
        adminLoginBtn.addEventListener('click', () => {
            const password = prompt("Enter admin password:");
            if (password === "admin") adminPanel.classList.add('open');
            else if (password) alert("Incorrect password.");
        });
        closeAdminBtn.addEventListener('click', () => {
            adminPanel.classList.remove('open');
        });
        give1kBtn.addEventListener('click', () => { count += 1000; updateDisplay(); showPopup('+1,000 Fumos added!'); });
        give10kBtn.addEventListener('click', () => { count += 10000; updateDisplay(); showPopup('+10,000 Fumos added!'); });
        resetStatsBtn.addEventListener('click', () => {
            if (confirm("Are you sure you want to reset all stats?")) {
                count = 0; clickPower = 1; autoClickerPower = 0;
                upgradeCost = 10; autoClickerCost = 50;
                if(autoClickerInterval) clearInterval(autoClickerInterval);
                autoClickerInterval = null;
                updateDisplay();
                showPopup("Stats have been reset.");
            }
        });

        // --- INITIALIZATION ---
        updateDisplay();
    </script>
</body>
</html>
