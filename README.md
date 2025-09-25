<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Autumnal Music Player</title>
    <style>
        body {
            box-sizing: border-box;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #8B4513 0%, #D2691E 50%, #CD853F 100%);
            min-height: 100vh;
            font-family: 'Georgia', serif;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .music-player {
            background: rgba(139, 69, 19, 0.9);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
            max-width: 400px;
            width: 100%;
            border: 2px solid rgba(255, 140, 0, 0.3);
        }

        .cover-art {
            width: 250px;
            height: 250px;
            margin: 0 auto 25px;
            border-radius: 15px;
            overflow: hidden;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.4);
            position: relative;
            background: linear-gradient(45deg, #FF8C00, #FF6347, #DC143C);
            display: flex;
            align-items: center;
            justify-content: center;
            transition: transform 0.3s ease;
        }

        .cover-art:hover {
            transform: scale(1.02);
        }

        .cover-art svg {
            width: 120px;
            height: 120px;
            opacity: 0.8;
        }

        .song-info {
            text-align: center;
            margin-bottom: 25px;
        }

        .song-title {
            color: #FFE4B5;
            font-size: 18px;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .song-artist {
            color: #DEB887;
            font-size: 14px;
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-bottom: 25px;
        }

        .control-btn {
            background: linear-gradient(145deg, #CD853F, #A0522D);
            border: none;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            color: #FFE4B5;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
        }

        .control-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.4);
            background: linear-gradient(145deg, #D2691E, #B8860B);
        }

        .control-btn:active {
            transform: translateY(0);
        }

        .play-btn {
            width: 60px;
            height: 60px;
        }

        .file-selector {
            margin-bottom: 20px;
        }

        .file-input-wrapper {
            position: relative;
            overflow: hidden;
            display: inline-block;
            width: 100%;
        }

        .file-input {
            position: absolute;
            left: -9999px;
        }

        .file-label {
            background: linear-gradient(145deg, #FF8C00, #FF6347);
            color: white;
            padding: 12px 20px;
            border-radius: 25px;
            cursor: pointer;
            display: block;
            text-align: center;
            transition: all 0.3s ease;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        .file-label:hover {
            background: linear-gradient(145deg, #FF7F50, #FF4500);
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }

        .next-track-btn {
            background: linear-gradient(145deg, #B8860B, #DAA520);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 25px;
            cursor: pointer;
            width: 100%;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        .next-track-btn:hover {
            background: linear-gradient(145deg, #DAA520, #FFD700);
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
        }

        .progress-bar {
            width: 100%;
            height: 6px;
            background: rgba(255, 228, 181, 0.3);
            border-radius: 3px;
            margin: 20px 0;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            background: linear-gradient(90deg, #FF8C00, #FFD700);
            width: 0%;
            transition: width 0.1s ease;
        }

        .time-display {
            display: flex;
            justify-content: space-between;
            color: #DEB887;
            font-size: 12px;
            margin-top: 10px;
        }

        .floating-leaves {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }

        .leaf {
            position: absolute;
            font-size: 20px;
            animation: fall 8s linear infinite;
            opacity: 0.7;
        }

        @keyframes fall {
            0% {
                transform: translateY(-100px) rotate(0deg);
                opacity: 1;
            }
            100% {
                transform: translateY(100vh) rotate(360deg);
                opacity: 0;
            }
        }

        @media (max-width: 480px) {
            .music-player {
                padding: 20px;
                margin: 10px;
            }
            
            .cover-art {
                width: 200px;
                height: 200px;
            }
        }
    </style>
</head>
<body>
    <div class="floating-leaves" id="leaves"></div>
    
    <div class="music-player">
        <div class="cover-art" id="coverArt">
            <svg viewBox="0 0 100 100" fill="currentColor">
                <path d="M20 15 Q25 10, 35 15 Q45 20, 50 30 Q55 40, 45 50 Q35 55, 25 50 Q15 45, 10 35 Q5 25, 15 20 Z" fill="#8B4513"/>
                <path d="M30 25 Q35 20, 45 25 Q50 30, 45 35 Q40 40, 30 35 Q25 30, 30 25 Z" fill="#CD853F"/>
                <path d="M60 20 Q70 15, 80 25 Q85 35, 75 45 Q65 50, 55 40 Q50 30, 60 20 Z" fill="#D2691E"/>
                <path d="M25 60 Q35 55, 45 65 Q50 75, 40 80 Q30 85, 20 75 Q15 65, 25 60 Z" fill="#FF8C00"/>
                <path d="M65 65 Q75 60, 85 70 Q90 80, 80 85 Q70 90, 60 80 Q55 70, 65 65 Z" fill="#FF6347"/>
            </svg>
        </div>

        <div class="song-info">
            <div class="song-title" id="songTitle">Select a cozy autumn track</div>
            <div class="song-artist" id="songArtist">Choose your music to begin</div>
        </div>

        <div class="progress-bar">
            <div class="progress" id="progress"></div>
        </div>

        <div class="time-display">
            <span id="currentTime">0:00</span>
            <span id="duration">0:00</span>
        </div>

        <div class="controls">
            <button class="control-btn" id="prevBtn" title="Previous">
                <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M6 6h2v12H6zm3.5 6l8.5 6V6z"/>
                </svg>
            </button>
            
            <button class="control-btn play-btn" id="playBtn" title="Play">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor" id="playIcon">
                    <path d="M8 5v14l11-7z"/>
                </svg>
                <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor" id="pauseIcon" style="display: none;">
                    <path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"/>
                </svg>
            </button>
            
            <button class="control-btn" id="stopBtn" title="Stop">
                <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor">
                    <rect x="6" y="6" width="12" height="12"/>
                </svg>
            </button>
            
            <button class="control-btn" id="nextBtn" title="Next">
                <svg width="20" height="20" viewBox="0 0 24 24" fill="currentColor">
                    <path d="M6 18l8.5-6L6 6v12zM16 6v12h2V6h-2z"/>
                </svg>
            </button>
        </div>

        <div class="file-selector">
            <div class="file-input-wrapper">
                <input type="file" id="fileInput" class="file-input" accept="audio/*" multiple>
                <label for="fileInput" class="file-label">🍂 Browse Autumn Tracks</label>
            </div>
        </div>

        <button class="next-track-btn" id="nextCozyBtn">🍁 Next Cozy Track</button>
    </div>

    <audio id="audioPlayer" preload="metadata"></audio>

    <script>
        class AutumnMusicPlayer {
            constructor() {
                this.audioPlayer = document.getElementById('audioPlayer');
                this.playlist = [];
                this.currentTrackIndex = 0;
                this.isPlaying = false;
                
                this.initializeElements();
                this.bindEvents();
                this.createFloatingLeaves();
                this.startCoverArtRotation();
            }

            initializeElements() {
                this.playBtn = document.getElementById('playBtn');
                this.stopBtn = document.getElementById('stopBtn');
                this.prevBtn = document.getElementById('prevBtn');
                this.nextBtn = document.getElementById('nextBtn');
                this.nextCozyBtn = document.getElementById('nextCozyBtn');
                this.fileInput = document.getElementById('fileInput');
                this.songTitle = document.getElementById('songTitle');
                this.songArtist = document.getElementById('songArtist');
                this.progress = document.getElementById('progress');
                this.currentTime = document.getElementById('currentTime');
                this.duration = document.getElementById('duration');
                this.playIcon = document.getElementById('playIcon');
                this.pauseIcon = document.getElementById('pauseIcon');
                this.coverArt = document.getElementById('coverArt');
            }

            bindEvents() {
                this.playBtn.addEventListener('click', () => this.togglePlay());
                this.stopBtn.addEventListener('click', () => this.stop());
                this.prevBtn.addEventListener('click', () => this.previousTrack());
                this.nextBtn.addEventListener('click', () => this.nextTrack());
                this.nextCozyBtn.addEventListener('click', () => this.nextTrack());
                this.fileInput.addEventListener('change', (e) => this.handleFileSelection(e));
                
                this.audioPlayer.addEventListener('loadedmetadata', () => this.updateDuration());
                this.audioPlayer.addEventListener('timeupdate', () => this.updateProgress());
                this.audioPlayer.addEventListener('ended', () => this.nextTrack());
            }

            handleFileSelection(event) {
                const files = Array.from(event.target.files);
                this.playlist = files.map((file, index) => ({
                    file: file,
                    name: file.name.replace(/\.[^/.]+$/, ""),
                    url: URL.createObjectURL(file),
                    index: index
                }));
                
                if (this.playlist.length > 0) {
                    this.currentTrackIndex = 0;
                    this.loadTrack(this.currentTrackIndex);
                }
            }

            loadTrack(index) {
                if (index >= 0 && index < this.playlist.length) {
                    const track = this.playlist[index];
                    this.audioPlayer.src = track.url;
                    this.songTitle.textContent = track.name;
                    this.songArtist.textContent = `Track ${index + 1} of ${this.playlist.length}`;
                    this.currentTrackIndex = index;
                    this.updateCoverArt();
                }
            }

            togglePlay() {
                if (this.playlist.length === 0) {
                    alert('Please select some autumn tracks first! 🍂');
                    return;
                }

                if (this.isPlaying) {
                    this.pause();
                } else {
                    this.play();
                }
            }

            play() {
                this.audioPlayer.play();
                this.isPlaying = true;
                this.playIcon.style.display = 'none';
                this.pauseIcon.style.display = 'block';
            }

            pause() {
                this.audioPlayer.pause();
                this.isPlaying = false;
                this.playIcon.style.display = 'block';
                this.pauseIcon.style.display = 'none';
            }

            stop() {
                this.audioPlayer.pause();
                this.audioPlayer.currentTime = 0;
                this.isPlaying = false;
                this.playIcon.style.display = 'block';
                this.pauseIcon.style.display = 'none';
                this.updateProgress();
            }

            nextTrack() {
                if (this.playlist.length === 0) return;
                
                this.currentTrackIndex = (this.currentTrackIndex + 1) % this.playlist.length;
                this.loadTrack(this.currentTrackIndex);
                
                if (this.isPlaying) {
                    setTimeout(() => this.play(), 100);
                }
            }

            previousTrack() {
                if (this.playlist.length === 0) return;
                
                this.currentTrackIndex = this.currentTrackIndex === 0 
                    ? this.playlist.length - 1 
                    : this.currentTrackIndex - 1;
                this.loadTrack(this.currentTrackIndex);
                
                if (this.isPlaying) {
                    setTimeout(() => this.play(), 100);
                }
            }

            updateProgress() {
                if (this.audioPlayer.duration) {
                    const progressPercent = (this.audioPlayer.currentTime / this.audioPlayer.duration) * 100;
                    this.progress.style.width = progressPercent + '%';
                    
                    this.currentTime.textContent = this.formatTime(this.audioPlayer.currentTime);
                }
            }

            updateDuration() {
                this.duration.textContent = this.formatTime(this.audioPlayer.duration);
            }

            formatTime(seconds) {
                if (isNaN(seconds)) return '0:00';
                const mins = Math.floor(seconds / 60);
                const secs = Math.floor(seconds % 60);
                return `${mins}:${secs.toString().padStart(2, '0')}`;
            }

            updateCoverArt() {
                const artworks = [
                    // Maple leaf
                    `<svg viewBox="0 0 100 100" fill="currentColor">
                        <path d="M50 10 Q45 20, 35 25 Q25 30, 20 40 Q15 50, 25 60 Q35 65, 45 70 Q50 80, 55 70 Q65 65, 75 60 Q85 50, 80 40 Q75 30, 65 25 Q55 20, 50 10 Z" fill="#DC143C"/>
                        <path d="M50 15 Q47 22, 42 25 Q37 28, 35 35 Q33 42, 37 47 Q42 50, 47 52 Q50 55, 53 52 Q58 50, 63 47 Q67 42, 65 35 Q63 28, 58 25 Q53 22, 50 15 Z" fill="#FF6347"/>
                    </svg>`,
                    
                    // Oak leaf
                    `<svg viewBox="0 0 100 100" fill="currentColor">
                        <path d="M50 15 Q40 20, 30 30 Q25 40, 30 50 Q35 55, 40 60 Q45 70, 50 85 Q55 70, 60 60 Q65 55, 70 50 Q75 40, 70 30 Q60 20, 50 15 Z" fill="#8B4513"/>
                        <path d="M50 20 Q45 25, 40 30 Q38 35, 40 40 Q42 43, 45 45 Q48 50, 50 60 Q52 50, 55 45 Q58 43, 60 40 Q62 35, 60 30 Q55 25, 50 20 Z" fill="#CD853F"/>
                    </svg>`,
                    
                    // Autumn scene
                    `<svg viewBox="0 0 100 100" fill="currentColor">
                        <circle cx="30" cy="25" r="8" fill="#FF8C00"/>
                        <circle cx="70" cy="35" r="6" fill="#DC143C"/>
                        <circle cx="45" cy="45" r="7" fill="#FFD700"/>
                        <circle cx="25" cy="60" r="5" fill="#FF6347"/>
                        <circle cx="75" cy="65" r="9" fill="#D2691E"/>
                        <circle cx="50" cy="75" r="6" fill="#B8860B"/>
                    </svg>`
                ];
                
                const randomArt = artworks[this.currentTrackIndex % artworks.length];
                this.coverArt.innerHTML = randomArt;
            }

            createFloatingLeaves() {
                const leaves = ['🍂', '🍁', '🍃'];
                const leavesContainer = document.getElementById('leaves');
                
                setInterval(() => {
                    const leaf = document.createElement('div');
                    leaf.className = 'leaf';
                    leaf.textContent = leaves[Math.floor(Math.random() * leaves.length)];
                    leaf.style.left = Math.random() * 100 + '%';
                    leaf.style.animationDuration = (Math.random() * 3 + 5) + 's';
                    leaf.style.animationDelay = Math.random() * 2 + 's';
                    
                    leavesContainer.appendChild(leaf);
                    
                    setTimeout(() => {
                        leaf.remove();
                    }, 8000);
                }, 2000);
            }

            startCoverArtRotation() {
                setInterval(() => {
                    if (this.isPlaying) {
                        this.coverArt.style.transform = `rotate(${Date.now() / 50 % 360}deg)`;
                    }
                }, 100);
            }
        }

        // Initialize the music player when the page loads
        document.addEventListener('DOMContentLoaded', () => {
            new AutumnMusicPlayer();
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'98489afab30c5a00',t:'MTc1ODc4MzIxNi4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>

