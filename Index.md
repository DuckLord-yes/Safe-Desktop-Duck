<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Goose Adventure!</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      background-color: #ccf2ff;
    }
    #stopButton {
      position: absolute;
      top: 10px;
      left: 10px;
      padding: 10px 20px;
      font-size: 18px;
      background-color: #ff4d4d;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      z-index: 9999;
    }
    #stopButton:hover {
      background-color: #e60000;
    }
  </style>
</head>
<body>

<button id="stopButton">Stop Duck</button>

<script>
// Create the duck image
const duck = document.createElement('img');
duck.src = 'https://static.wikia.nocookie.net/fridaynightfunking/images/7/77/GooseDesktopPetAnim.gif/revision/latest?cb=20220831041808';
duck.style.position = 'absolute';
duck.style.width = '100px';
duck.style.height = '100px';
duck.style.top = '50%';
duck.style.left = '50%';
duck.style.transform = 'translate(-50%, -50%)';
document.body.appendChild(duck);

// Add quack sound
const quackAudio = document.createElement('audio');
quackAudio.src = 'https://cdn.pixabay.com/audio/2021/08/04/audio_5f5b9b9b9b.mp3';
document.body.appendChild(quackAudio);

let x = window.innerWidth / 2;
let y = window.innerHeight / 2;
let dx = (Math.random() - 0.5) * 4;
let dy = (Math.random() - 0.5) * 4;
let paused = false;
let running = true;

// Function to change direction randomly every 2-3 seconds
function changeDirection() {
  if (!running) return;
  if (!paused) {
    dx = (Math.random() - 0.5) * 4;
    dy = (Math.random() - 0.5) * 4;

    duck.style.transform = `translate(-50%, -50%) scaleX(${dx >= 0 ? 1 : -1})`;

    quackAudio.currentTime = 0;
    quackAudio.play();
  }
  setTimeout(changeDirection, 2000 + Math.random() * 1000);
}

// Function to move the duck
function moveDuck() {
  if (!running) return;
  if (!paused) {
    x += dx;
    y += dy;
  }

  if (x < 0) {
    hitBarrier('left');
  }
  if (x > window.innerWidth - 100) {
    hitBarrier('right');
  }
  if (y < 0) {
    hitBarrier('top');
  }
  if (y > window.innerHeight - 100) {
    hitBarrier('bottom');
  }

  duck.style.left = x + 'px';
  duck.style.top = y + 'px';

  requestAnimationFrame(moveDuck);
}

// Function to handle hitting screen edges
function hitBarrier(side) {
  if (!paused) {
    paused = true;
    dx = 0;
    dy = 0;

    quackAudio.currentTime = 0;
    quackAudio.play();

    setTimeout(() => {
      paused = false;
      if (side === 'left' || side === 'right') {
        dx = (side === 'left' ? 1 : -1) * (Math.random() * 4 + 1);
        dy = (Math.random() - 0.5) * 4;
      } else {
        dy = (side === 'top' ? 1 : -1) * (Math.random() * 4 + 1);
        dx = (Math.random() - 0.5) * 4;
      }

      duck.style.transform = `translate(-50%, -50%) scaleX(${dx >= 0 ? 1 : -1})`;
    }, 500);
  }
}

// Function to stop everything
function stopDuck() {
  running = false;
  duck.remove();
  quackAudio.remove();
  document.getElementById('stopButton').remove();
  
  // If running in Shortcuts, call completion(result)
  try {
    completion('Duck stopped');
  } catch (e) {
    console.log('Not running inside a Shortcut. Normal exit.');
  }
}

// Button listener
document.getElementById('stopButton').addEventListener('click', stopDuck);

// Initialize
changeDirection();
moveDuck();

</script>

</body>
</html>
