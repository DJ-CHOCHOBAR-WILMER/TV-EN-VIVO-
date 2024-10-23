<html><head><base href=""><link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css" rel="stylesheet">
<style>
:root {
  --primary: #2c3e50;
  --secondary: #e74c3c;
  --background: #ecf0f1;
  --text: #2c3e50;
}

body {
  margin: 0;
  padding: 20px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: var(--background);
  color: var(--text);
}

.player-container {
  max-width: 800px;
  margin: 0 auto;
  background: white;
  border-radius: 15px;
  box-shadow: 0 10px 20px rgba(0,0,0,0.1);
  overflow: hidden;
}

.header {
  background: var(--primary);
  color: white;
  padding: 20px;
  text-align: center;
}

.header h1 {
  margin: 0;
  font-size: 24px;
}

.video-container {
  position: relative;
  padding-bottom: 56.25%;
  height: 0;
  overflow: hidden;
}

.video-container iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  border: none;
}

.controls {
  display: flex;
  justify-content: space-between;
  padding: 20px;
  background: #f8f9fa;
}

.channel-list {
  padding: 20px;
}

.channel-button {
  display: block;
  width: 100%;
  padding: 12px;
  margin: 8px 0;
  background: var(--background);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.channel-button:hover {
  background: var(--secondary);
  color: white;
  transform: translateX(5px);
}

.player-controls {
  display: flex;
  gap: 15px;
}

.control-button {
  background: var(--primary);
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.control-button:hover {
  background: var(--secondary);
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.05); }
  100% { transform: scale(1); }
}

.live-indicator {
  display: inline-block;
  padding: 5px 10px;
  background: var(--secondary);
  color: white;
  border-radius: 15px;
  font-size: 12px;
  animation: pulse 2s infinite;
}
</style>
</head>
<body>
<div class="player-container">
  <div class="header">
    <h1>MuntiRadios TV <span class="live-indicator">EN VIVO</span></h1>
  </div>
  
  <div class="video-container">
    <iframe id="player-iframe" 
            src=" https://www.youtube.com/embed/%20https:/watch?v=qhEkkzsL_MI&list=PLKNhlbs7VCMtErTiCNBhpq_p7bYiOBOVO" 
            allowfullscreen></iframe>
  </div>
  
  <div class="controls">
    <div class="player-controls">
      <button class="control-button" onclick="playPause()">
        <i class="fas fa-play"></i> Play/Pause
      </button>
      <button class="control-button" onclick="muteUnmute()">
        <i class="fas fa-volume-up"></i> Mute
      </button>
    </div>
    <select id="quality-selector" class="control-button" onchange="changeQuality()">
      <option value="auto">Auto</option>
      <option value="1080">1080p</option>
      <option value="720">720p</option>
      <option value="480">480p</option>
    </select>
  </div>
  
  <div class="channel-list">
    <button class="channel-button" onclick="changeChannel(' https://www.youtube.com/embed/%20https:/watch?v=qhEkkzsL_MI&list=PLKNhlbs7VCMtErTiCNBhpq_p7bYiOBOVO')">
      En Vivo desde Camporredondo luya amazonas 
    </button>
    <button class="channel-button" onclick="changeChannel('https://www.youtube.com/embed/https:/youtube.com/playlist?list=PLKNhlbs7VCMtAoId2M8d-KGAwuldQ7oyY&si=6YSMGNlIryFjmAUr')">
      https://wilmerdelgadocieza.blogspot.com 
    </button>
    <button class="channel-button" onclick="changeChannel(' DJ CHOCHOBAR WILMER   ')">
 
      DJ WILMER EN VIVO
    </button>
  </div>
</div>

<script>
let player;
let isMuted = false;

function changeChannel(channelId) {
  const iframe = document.getElementById('player-iframe');
  iframe.src = `https://www.youtube.com/embed/live_stream?channel=${channelId}`;
}

function playPause() {
  const iframe = document.getElementById('player-iframe');
  const message = JSON.stringify({ event: 'command', func: 'togglePlayPause' });
  iframe.contentWindow.postMessage(message, '*');
}

function muteUnmute() {
  const iframe = document.getElementById('player-iframe');
  isMuted = !isMuted;
  const message = JSON.stringify({ 
    event: 'command', 
    func: isMuted ? 'mute' : 'unMute' 
  });
  iframe.contentWindow.postMessage(message, '*');
  
  const muteButton = document.querySelector('.control-button i.fas');
  muteButton.className = isMuted ? 'fas fa-volume-mute' : 'fas fa-volume-up';
}

function changeQuality() {
  const quality = document.getElementById('quality-selector').value;
  const iframe = document.getElementById('player-iframe');
  const message = JSON.stringify({ 
    event: 'command', 
    func: 'setPlaybackQuality', 
    args: [quality] 
  });
  iframe.contentWindow.postMessage(message, '*');
}

// Listener para mensajes del iframe
window.addEventListener('message', (event) => {
  if (event.origin !== 'https://www.youtube.com') return;
  
  try {
    const data = JSON.parse(event.data);
    if (data.event === 'onStateChange') {
      // Actualizar UI basado en el estado del reproductor
      updatePlayerState(data.state);
    }
  } catch (e) {
    console.log('Error parsing message:', e);
  }
});

function updatePlayerState(state) {
  const playButton = document.querySelector('.control-button i.fas');
  if (state === 1) { // reproduciendo
    playButton.className = 'fas fa-pause';
  } else {
    playButton.className = 'fas fa-play';
  }
}
</script>
</body></html>





