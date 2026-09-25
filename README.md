<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Henry Racing</title>
<style>
body{margin:0;background:#111;color:white;text-align:center;font-family:Arial;touch-action:none}
#game{width:300px;height:500px;background:linear-gradient(#333 0%, #333 90%, #222 100%);margin:0 auto;position:relative;overflow:hidden;border:4px solid white;border-radius:10px}
.lane{position:absolute;top:0;width:2px;height:100%;background:dashed white;opacity:0.3}
#player{font-size:50px;position:absolute;bottom:20px;left:125px;transition:0.1s}
.enemy{font-size:50px;position:absolute}
#score{font-size:24px;font-weight:bold;padding:10px}
.controls{display:flex;justify-content:center;gap:20px;margin-top:10px}
.controls button{width:120px;height:80px;font-size:30px;border-radius:20px;border:none;background:#00ff88}
</style>
</head>
<body>
<h2>🏁 HENRY RACING 🏁</h2>
<div id="score">Score: 0</div>
<div id="game">
  <div class="lane" style="left:98px"></div>
  <div class="lane" style="left:198px"></div>
  <div id="player">🏎️</div>
</div>
<div class="controls">
  <button id="left">⬅️</button>
  <button id="right">➡️</button>
</div>
<p>Swipe on road or tap buttons!</p>
<script>
let player=document.getElementById('player'), game=document.getElementById('game');
let x=125, score=0, over=false;
const cars=['🚕','🚙','🚌','🚚','🚗'];
function move(dir){
 if(over) return;
 x+=dir;
 if(x<0)x=0; if(x>250)x=250;
 player.style.left=x+'px';
}
document.getElementById('left').addEventListener('touchstart',()=>move(-50));
document.getElementById('right').addEventListener('touchstart',()=>move(50));
document.getElementById('left').addEventListener('click',()=>move(-50));
document.getElementById('right').addEventListener('click',()=>move(50));

// swipe to move
let startX=0;
game.addEventListener('touchstart',e=>{startX=e.touches[0].clientX});
game.addEventListener('touchmove',e=>{
 let diff=e.touches[0].clientX-startX;
 if(Math.abs(diff)>20){ move(diff>0?25:-25); startX=e.touches[0].clientX; }
});

function spawn(){
 if(over) return;
 let e=document.createElement('div'); e.className='enemy';
 e.innerText=cars[Math.floor(Math.random()*cars.length)];
 e.style.left=[0,100,200][Math.floor(Math.random()*3)]+'px';
 e.style.top='-60px';
 game.appendChild(e);
 let y=-60;
 let id=setInterval(()=>{
  y+=4; e.style.top=y+'px';
  if(y>380 && y<460 && Math.abs(parseInt(e.style.left)-x)<50){
   over=true; alert('💥 CRASH! Score: '+score); location.reload();
  }
  if(y>520){ clearInterval(id); e.remove(); score++; document.getElementById('score').innerText='Score: '+score; }
 },20);
}
setInterval(spawn,900);
</script>
</body>
</html>
