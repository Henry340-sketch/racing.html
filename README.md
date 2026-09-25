<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Henry Racing Vertical</title>
<style>
body{margin:0;background:#111;color:white;text-align:center;font-family:Arial}
#game{width:300px;height:500px;background:#333;margin:10px auto;position:relative;overflow:hidden;border:4px solid white}
#road{width:100%;height:100%;position:relative;background: repeating-linear-gradient(90deg, #333 0 98px, #fff 98px 102px, #333 102px 202px, #fff 202px 204px, #333 204px 300px);}
#player{font-size:45px;position:absolute;bottom:20px;left:125px;transform: rotate(-90deg); /* THIS MAKES CAR FACE UP */ z-index:10; transition: left 0.15s}
.enemy{font-size:45px;position:absolute;transform: rotate(-90deg);}
#score{font-size:22px;padding:10px}
.btns button{width:130px;height:70px;font-size:26px;margin:8px;border-radius:15px;border:none;font-weight:bold}
#left{background:#2ecc71} #right{background:#3498db}
</style>
</head>
<body>
<h3>🏁 HENRY RACING - VERTICAL 🏁</h3>
<div id="score">Score: 0</div>
<div id="game"><div id="road"></div><div id="player">🏎️</div></div>
<div class="btns">
<button id="left">⬅️</button>
<button id="right">➡️</button>
</div>
<p>Car now faces UP! Move left/right straight</p>
<script>
let x=125,score=0,over=false;
let player=document.getElementById('player');
let game=document.getElementById('game');
function setX(newX){
 if(newX<5)newX=5; if(newX>250)newX=250;
 x=newX; player.style.left=x+'px';
}
document.getElementById('left').onclick=()=>setX(x-55);
document.getElementById('right').onclick=()=>setX(x+55);
document.getElementById('left').ontouchstart=(e)=>{e.preventDefault(); setX(x-55);};
document.getElementById('right').ontouchstart=(e)=>{e.preventDefault(); setX(x+55);};

// swipe anywhere on game
game.addEventListener('touchmove',function(e){
 e.preventDefault();
 let rect=game.getBoundingClientRect();
 let finger=e.touches[0].clientX - rect.left - 25;
 setX(finger);
},{passive:false});

function spawn(){
 if(over)return;
 let en=document.createElement('div'); en.className='enemy';
 en.innerHTML=['🚕','🚙','🚛','🚗'][Math.floor(Math.random()*4)];
 en.style.left=[10,110,210][Math.floor(Math.random()*3)]+'px';
 en.style.top='-60px'; game.appendChild(en);
 let y=-60;
 let t=setInterval(()=>{
  y+=5; en.style.top=y+'px';
  if(y>380 && y<470 && Math.abs(parseInt(en.style.left)-x)<40){
   over=true; alert('CRASH! Score '+score); location.reload();
  }
  if(y>520){clearInterval(t); en.remove(); score++; document.getElementById('score').innerHTML='Score: '+score;}
 },20);
}
setInterval(spawn,900);
</script>
</body>
</html>
