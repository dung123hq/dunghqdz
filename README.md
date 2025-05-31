<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Đình Dũng</title>
  <style>
    html, body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      width: 100%;
      height: 100%;
      touch-action: none;
      font-family: 'Arial', sans-serif;
      background: black;
      perspective: 1000px;
    }
    body {
      transform-style: preserve-3d;
    }

    .container {
      width: 100vw;
      height: 100vh;
      position: relative;
      transform-style: preserve-3d;
      transform: rotateY(20deg);
      transition: transform 0.3s ease;
    }

    .falling-layer, .star-layer {
      position: absolute;
      width: 100%;
      height: 100%;
      transform-style: preserve-3d;
    }

    .message {
      position: absolute;
      color: #fff;
      text-shadow: 0 0 10px #ff9999, 0 0 20px #ff666b;
      white-space: nowrap;
      animation: fallText linear infinite;
      z-index: 2;
    }

    .star {
      position: absolute;
      background: white;
      border-radius: 50%;
      animation: twinkle linear infinite;
    }

    .heart {
      position: absolute;
      filter: drop-shadow(0 0 8px #f24d4d);
      animation: fallHeart linear infinite;
    }

    @keyframes fallText {
      0% { transform: translateY(-100px); opacity: 1; }
      100% { transform: translateY(100vh); opacity: 0; }
    }
    @keyframes twinkle {
      0%,100% { opacity: 0; transform: scale(0.5); }
      50% { opacity: 1; transform: scale(1); }
    }
    @keyframes fallHeart {
      0% { transform: translateY(-100px) rotateY(0); opacity: 1; }
      100% { transform: translateY(100vh) rotateY(360deg); opacity: 0; }
    }

    /* Media Queries cho mobile */
    @media (max-width: 768px) {
      .container { transform: rotateY(10deg); }
      .message { font-size: clamp(16px, 4vw, 25px); }
    }
    @media (max-width: 480px) {
      .container { transform: rotateY(5deg); }
      .message { font-size: clamp(14px, 3.5vw, 20px); }
    }

    .start-button {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: #ff666b;
      color: white;
      padding: 15px 30px;
      font-size: 18px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      z-index: 1000;
      animation: pulse 2s infinite;
    }
    @keyframes pulse {
      0% { transform: translate(-50%, -50%) scale(1); }
      50% { transform: translate(-50%, -50%) scale(1.1); }
      100% { transform: translate(-50%, -50%) scale(1); }
    }

    .copyright {
      position: fixed;
      bottom: 10px;
      right: 10px;
      color: rgba(255, 255, 255, 0.078);
      font-size: clamp(8px, 2vw, 12px);
      pointer-events: none;
    }
  </style>
</head>
<body>

<audio id="bgMusic" loop>
  <source src="music.mp3" type="audio/mpeg">
</audio>

<button id="startButton" class="start-button">Bấm để bắt đầu ♡</button>

<div class="stars-container">
  <div class="star-layer" style="transform: translateZ(-1000px);"></div>
  <div class="star-layer" style="transform: translateZ(0);"></div>
  <div class="star-layer" style="transform: translateZ(1000px);"></div>
</div>

<div class="container">
  <div class="falling-layer" style="transform: translateZ(-1000px);"></div>
  <div class="falling-layer" style="transform: translateZ(-500px);"></div>
  <div class="falling-layer" style="transform: translateZ(0);"></div>
  <div class="falling-layer" style="transform: translateZ(500px);"></div>
  <div class="falling-layer" style="transform: translateZ(1000px);"></div>
</div>

<div class="copyright">Longdev@2025</div>

<script>
const messages = ['Dũng ♡ Quỳnh', '1/6 vui vẻ nha bé iuu♡', 'ai lớp diu nọong♡', 'cá điếp nọng '];
const container = document.querySelector('.container');
const layers = document.querySelectorAll('.falling-layer');
const bgMusic = document.getElementById('bgMusic');
const startButton = document.getElementById('startButton');

// Nút bắt đầu
startButton.addEventListener('click', () => {
  startButton.style.display = 'none';
  bgMusic.play();
  initEffects();
});

// Tạo hiệu ứng
function initEffects() {
  layers.forEach(layer => {
    const depth = parseInt(layer.style.transform.match(/([-0-9]+)/)[1]);
    const count = (window.innerWidth <= 480) ? 5 : (window.innerWidth <= 768) ? 10 : 15;
    for (let i = 0; i < count; i++) createMessage(layer);
    for (let i = 0; i < count; i++) createHeart(layer);
  });
  createStars();
}

function createMessage(layer) {
  const msg = document.createElement('div');
  msg.className = 'message';
  msg.textContent = messages[Math.floor(Math.random()*messages.length)];
  msg.style.left = Math.random() * window.innerWidth + 'px';
  msg.style.top = Math.random() * window.innerHeight + 'px';
  const depth = parseInt(layer.style.transform.match(/([-0-9]+)/)[1]);
  const size = (35 - Math.abs(depth)/100) + 'px';
  msg.style.fontSize = size;
  msg.style.animationDuration = (9 + Math.random()*9) + 's';
  msg.addEventListener('animationend', ()=>{ msg.remove(); createMessage(layer); });
  layer.appendChild(msg);
}

function createHeart(layer) {
  const heart = document.createElementNS('http://www.w3.org/2000/svg','svg');
  heart.setAttribute('viewBox','0 0 32 29.6');
  heart.innerHTML = `<path d="M23.6,0c-2.6,0-5,1.3-6.6,3.3C15.4,1.3,13,0,10.4,0C4.7,0,0,4.7,0,10.4c0,11.1,16,19.2,16,19.2s16-8.1,16-19.2C32,4.7,27.3,0,23.6,0z" fill="#ff6699"/>`;
  heart.classList.add('heart');
  heart.style.left = Math.random()*window.innerWidth+'px';
  heart.style.top = Math.random()*window.innerHeight+'px';
  heart.style.width='32px'; heart.style.height='30px';
  heart.style.animationDuration = (9 + Math.random()*9)+'s';
  heart.addEventListener('animationend', ()=>{ heart.remove(); createHeart(layer); });
  layer.appendChild(heart);
}

function createStars() {
  const starLayers = document.querySelectorAll('.star-layer');
  starLayers.forEach(layer=>{
    const depth = parseInt(layer.style.transform.match(/([-0-9]+)/)[1]);
    const count = (window.innerWidth <= 480) ? 50 : 100;
    for(let i=0;i<count;i++){
      const star = document.createElement('div');
      star.className='star';
      star.style.left = Math.random()*window.innerWidth+'px';
      star.style.top = Math.random()*window.innerHeight+'px';
      const size = (1 + Math.random()*2)+'px';
      star.style.width=size; star.style.height=size;
      star.style.animationDuration=(1+Math.random()*2)+'s';
      layer.appendChild(star);
    }
  });
}

// Xoay khi chạm
let startX=0, rotation=20;
document.addEventListener('touchstart',e=>{startX=e.touches[0].clientX;});
document.addEventListener('touchmove',e=>{
  e.preventDefault();
  let diff=e.touches[0].clientX-startX;
  rotation=Math.max(-45,Math.min(45,rotation+diff*0.1));
  container.style.transform=`rotateY(${rotation}deg)`;
  startX=e.touches[0].clientX;
},{passive:false});

document.addEventListener('mousedown',e=>{isDown=true;startX=e.clientX;});
document.addEventListener('mousemove',e=>{
  if(!isDown)return;
  let diff=e.clientX-startX;
  rotation=Math.max(-45,Math.min(45,rotation+diff*0.1));
  container.style.transform=`rotateY(${rotation}deg)`;
  startX=e.clientX;
});
document.addEventListener('mouseup',()=>isDown=false);
</script>

</body>
</html>
