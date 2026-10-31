
<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Simple Ludo Game</title>
<style>
body{
 background:#172033;color:white;
 font-family:Arial;text-align:center;
 margin:0;padding:15px;
}
h1{color:#ffcf33}
button{
 padding:12px 18px;border:0;
 border-radius:10px;font-size:17px;
 margin:5px;cursor:pointer;
 background:#ffcf33;font-weight:bold;
}
#board{
 display:grid;
 grid-template-columns:repeat(5,1fr);
 gap:7px;max-width:360px;
 margin:20px auto;
}
.cell{
 background:#eee;color:#222;
 aspect-ratio:1;border-radius:10px;
 display:flex;align-items:center;
 justify-content:center;font-size:25px;
}
#dice{font-size:65px;margin:15px}
.red{background:#f44336!important;color:white}
.blue{background:#2196f3!important;color:white}
.green{background:#19a65a!important;color:white}
.yellow{background:#e5b800!important;color:white}
#message{font-size:19px;margin:15px}
</style>
</head>
<body>

<h1>🎲 SIMPLE LUDO 🎲</h1>

<div id="menu">
<h3>खिलाड़ी चुनें</h3>
<button onclick="start(2)">2 Players</button>
<button onclick="start(3)">3 Players</button>
<button onclick="start(4)">4 Players</button>
</div>

<div id="game" style="display:none">
<h2 id="turn"></h2>
<div id="dice">🎲</div>
<button onclick="roll()">पासा फेंको</button>

<div id="message">गेम शुरू करें</div>
<div id="board"></div>
<button onclick="reset()">🏠 Home</button>
</div>

<script>
let names=["लाल","नीला","हरा","पीला"];
let colors=["red","blue","green","yellow"];
let players=[];
let positions=[];
let current=0;
let number=0;
let waiting=false;
let finished=false;

function start(n){
 players=Array.from({length:n},(_,i)=>i);
 positions=players.map(()=>0);
 current=0;
 waiting=false;
 finished=false;
 document.getElementById("menu").style.display="none";
 document.getElementById("game").style.display="block";
 draw();
}

function draw(){
 let board=document.getElementById("board");
 board.innerHTML="";
 let total=30;

 for(let i=0;i<total;i++){
  let cell=document.createElement("div");
  cell.className="cell";

  players.forEach((p,pi)=>{
   if(positions[pi]===i+1){
    cell.classList.add(colors[p]);
    cell.textContent="●";
   }
  });

  board.appendChild(cell);
 }

 document.getElementById("turn").textContent=
 names[players[current]]+" की बारी";
}

function roll(){
 if(waiting||finished)return;

 number=Math.floor(Math.random()*6)+1;
 document.getElementById("dice").textContent=
 ["","⚀","⚁","⚂","⚃","⚄","⚅"][number];

 let pos=positions[current];

 if(pos===0&&number!==6){
  document.getElementById("message").textContent=
  "6 आने पर गोटी बाहर निकलेगी";
  next();
  return;
 }

 waiting=true;
 document.getElementById("message").textContent=
 "गोटी चलाने के लिए नीचे क्लिक करें";

 let btn=document.createElement("button");
 btn.id="move";
 btn.textContent="मेरी गोटी चलाओ";
 btn.onclick=move;
 document.getElementById("message").appendChild(btn);
}

function move(){
 if(!waiting||finished)return;

 let pos=positions[current];

 if(pos===0)pos=1;
 else pos+=number;

 if(pos>30)pos=30;

 positions[current]=pos;
 waiting=false;

 if(pos===30){
  finished=true;
  document.getElementById("message").textContent=
  "🏆 "+names[players[current]]+" जीत गया! 👏👏👏";
  draw();
  return;
 }

 document.getElementById("message").textContent=
 "गोटी आगे बढ़ गई!";
 if(number!==6)next();
 draw();
}

function next(){
 waiting=false;
 current=(current+1)%players.length;
 draw();
}

function reset(){
 document.getElementById("game").style.display="none";
 document.getElementById("menu").style.display="block";
 document.getElementById("dice").textContent="🎲";
 document.getElementById("message").textContent="";
}
</script>
</body>
</html>
