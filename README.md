<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Happy New Year 🎉</title>
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',Tahoma,Geneva,Verdana,sans-serif;}
body{height:100vh;background:#010017;color:white;display:flex;align-items:center;justify-content:center;overflow:hidden;text-align:center;position:relative;}
canvas{position:absolute;inset:0;z-index:1;}
.container{position:relative;z-index:2;padding:25px 20px;border-radius:20px;background:rgba(10,10,30,0.85);backdrop-filter:blur(8px);box-shadow:0 20px 40px rgba(0,0,0,0.5);max-width:90%;transition:all .5s;}
h1{font-size:2.4rem;margin-bottom:10px;background:linear-gradient(90deg,#7df9ff,#ff7de5,#7df9ff);-webkit-background-clip:text;-webkit-text-fill-color:transparent;animation:shimmer 4s infinite linear;}
h2{font-size:1.4rem;margin-bottom:15px;opacity:.9;}
p{font-size:1rem;line-height:1.5;margin-bottom:18px;}
.name{margin-top:10px;font-size:1.1rem;color:#7df9ff;}
.footer{margin-top:15px;font-size:.8rem;opacity:.8;}
.btn{margin-top:12px;padding:8px 16px;border:none;border-radius:25px;background:linear-gradient(90deg,#ff7de5,#7df9ff);color:#010017;font-weight:600;cursor:pointer;transition:transform .3s ease,box-shadow .3s ease;}
.btn:hover{transform:translateY(-3px);box-shadow:0 8px 20px rgba(255,125,229,.5);}
@keyframes shimmer{0%{background-position:0%}100%{background-position:200%;}}
.overlay{position:fixed;inset:0;background:rgba(0,0,0,.9);display:flex;align-items:center;justify-content:center;z-index:5;transition:opacity .8s;}
.overlay.hidden{opacity:0;pointer-events:none;}
.countdown{font-size:2rem;letter-spacing:.1em;}
.sub{opacity:.8;font-size:.9rem;margin-top:10px;}
.chatbot{position:fixed;bottom:15px;right:10px;z-index:6;width:90%;max-width:300px;border-radius:18px;background:rgba(10,10,30,.95);backdrop-filter:blur(12px);box-shadow:0 15px 40px rgba(0,0,0,.7);overflow:hidden;font-size:.9rem;}
.chat-head{padding:8px 10px;background:rgba(0,0,50,.5);font-weight:600;font-size:.9rem;}
.chat-body{padding:8px;height:180px;overflow-y:auto;}
.msg{margin-bottom:6px;opacity:.9;font-size:.9rem;}
.bot{color:#7df9ff;}
.user{color:#ff7de5;text-align:right;}
.chat-input{display:flex;border-top:1px solid rgba(255,255,255,.15);}
.chat-input input{flex:1;background:transparent;border:none;color:white;padding:6px;outline:none;font-size:.9rem;}
.chat-input button{background:none;border:none;color:#7df9ff;padding:6px;cursor:pointer;}
.bot-buttons{display:flex;flex-wrap:wrap;gap:5px;margin-top:5px;justify-content:center;}
.bot-buttons button{flex:1 1 45%;padding:6px 5px;font-size:.75rem;border-radius:10px;background:linear-gradient(90deg,#7df9ff,#ff7de5);border:none;color:#010017;cursor:pointer;}
@media (min-width:600px){
  .container{padding:30px 35px;}
  h1{font-size:3rem;}
  h2{font-size:1.6rem;}
  .chat-body{height:200px;}
  .bot-buttons button{font-size:.8rem;}
}
</style>
</head>
<body>

<canvas id="fx"></canvas>

<div class="overlay" id="overlay">
  <div>
    <div class="countdown" id="cd">00:00:00</div>
    <div class="sub">Waiting for midnight…</div>
  </div>
</div>

<div class="container">
  <h1>🎉 Happy New Year 🎉</h1>
  <h2>Wishing You a Bright Year Ahead ✨</h2>
  <p id="wishPara">As the New Year begins, I just wanted to wish you lots of happiness, good health, and success 🌟 I’m really glad to have a friend like you, and I hope this year brings you many smiles 😊</p>
  <button class="btn" id="nextWishBtn">My Next Wish</button>
  <div class="name">— Your friend 🤝</div>
</div>

<div class="chatbot" id="bot">
  <div class="chat-head">🌟 FactBot 3 InnovEx</div>
  <div class="chat-body" id="chatBody">
    <div class="msg bot">Hi 🙂 I’m FactBot 3 InnovEx, your midnight companion tonight 🌙✨</div>
  </div>
  <div class="bot-buttons">
    <button onclick="quickReply('fact')">Fact</button>
    <button onclick="quickReply('joke')">Joke</button>
    <button onclick="quickReply('quote')">Quote</button>
    <button onclick="quickReply('tip')">Tip</button>
  </div>
  <div class="chat-input">
    <input id="chatInput" placeholder="Say something…">
    <button onclick="sendMsg()">➤</button>
  </div>
</div>

<script>
// Countdown
const cd=document.getElementById('cd');
const overlay=document.getElementById('overlay');
function nextMidnight(){const n=new Date();const m=new Date(n);m.setHours(24,0,0,0);return m-n}
const t=setInterval(()=>{
  const d=nextMidnight();
  const h=String(Math.floor(d/36e5)).padStart(2,'0');
  const m=String(Math.floor(d%36e5/6e4)).padStart(2,'0');
  const s=String(Math.floor(d%6e4/1e3)).padStart(2,'0');
  cd.textContent=`${h}:${m}:${s}`;
  if(d<=1000){clearInterval(t);overlay.classList.add('hidden');nonStopFireworks();}
},250);

// Fireworks
const c=document.getElementById('fx'),x=c.getContext('2d');
let W,H;
function rs(){W=c.width=innerWidth;H=c.height=innerHeight;}
addEventListener('resize',rs);rs();
const P=[];
function burst(){for(let i=0;i<120;i++)P.push({x:W/2,y:H*.6,a:Math.random()*Math.PI*2,v:2+Math.random()*4,l:80+Math.random()*40});}
(function anim(){requestAnimationFrame(anim);x.clearRect(0,0,W,H);for(const p of P){p.x+=Math.cos(p.a)*p.v;p.y+=Math.sin(p.a)*p.v;p.l--;x.fillStyle=`hsla(${(p.l*4)%360},100%,60%,.8)`;x.fillRect(p.x,p.y,2,2)}for(let i=P.length-1;i>=0;i--)if(P[i].l<=0)P.splice(i,1);})();

function nonStopFireworks(){
  setInterval(burst,200);
}

// Heartwarming wishes
const wishes=[
  "Wishing you endless joy, laughter, and unforgettable moments this year! 🌟",
  "May every day of this year be as bright as your smile 🙂",
  "Sending you mountains of happiness and good vibes for the year ahead 🌈",
  "I hope this year gives you many reasons to celebrate and be proud of yourself 🎉",
  "May friendship, love, and laughter follow you everywhere you go 💫",
  "Here’s to new adventures, small wins, and big memories 🌱",
  "May your days be filled with peace, positivity, and delightful surprises ✨",
  "I wish you strength to face any challenge and courage to chase your dreams 💪",
  "Hoping every moment this year brings you closer to happiness and success 🏆",
  "May this year be gentle, kind, and full of wins—big and small 🌷"
];
let currentWish=0;
const wishPara=document.getElementById('wishPara');
document.getElementById('nextWishBtn').addEventListener('click',()=>{
  currentWish = (currentWish+1)%wishes.length;
  wishPara.textContent=wishes[currentWish];
});

// FactBot functionality
const chatBody=document.getElementById('chatBody');
const chatInput=document.getElementById('chatInput');
const factReplies=[
  "The Earth's inner core is a solid ball of iron and nickel the size of the moon!",
  "Honey never spoils. Archaeologists found 3000-year-old honey that's still edible.",
  "A snail can sleep for three years. Deep sleep indeed!",
  "One day on Venus is longer than a year on Venus.",
  "Bananas are berries, but strawberries are not. Mind blown! 🍌🍓"
];
const jokes=[
  "Why don't scientists trust atoms? Because they make up everything!",
  "What do you call a fake noodle? An impasta!",
  "Why did the scarecrow win an award? Because he was outstanding in his field!"
];
const quotes=[
  "Believe you can and you're halfway there. – Theodore Roosevelt",
  "Strive not to be a success, but rather to be of value. – Albert Einstein",
  "The mind is everything. What you think you become. – Buddha"
];
const tips=[
  "Remember to smile tonight 🙂 Small joys count!",
  "A little gratitude can make your day brighter 🌟",
  "Start the year gently, one step at a time 🌱"
];
const companionReplies=[
  "This year is starting gently 🌙",
  "Someone made this page just to wish you well 🙂",
  "You deserve a calm and happy year ✨",
  "I hope tonight feels peaceful for you 💙",
  "New beginnings can be soft too 🌱"
];

function addMsg(text,type='bot'){
  const div=document.createElement('div');
  div.className='msg '+type;
  div.textContent=text;
  chatBody.appendChild(div);
  chatBody.scrollTop=chatBody.scrollHeight;
}

function processInput(input){
  const text=input.toLowerCase();
  let response=[];
  const greetings=["hi","hello","hey","good morning","good evening"];
  if(greetings.some(g=>text.includes(g))) response.push("Hello! I’m FactBot 3 InnovEx, your friendly midnight companion 🌙✨");
  if(text.includes("fact")) response.push(factReplies[Math.floor(Math.random()*factReplies.length)]);
  if(text.includes("joke")) response.push(jokes[Math.floor(Math.random()*jokes.length)]);
  if(text.includes("quote")) response.push(quotes[Math.floor(Math.random()*quotes.length)]);
  if(text.includes("tip")) response.push(tips[Math.floor(Math.random()*tips.length)]);
  if(response.length===0) response.push(companionReplies[Math.floor(Math.random()*companionReplies.length)]);
  response.forEach((r,i)=>setTimeout(()=>addMsg(r),700*i));
}

function sendMsg(){
  const input=chatInput.value.trim();
  if(!input) return;
  addMsg(input,'user');
  chatInput.value='';
  processInput(input);
}

// Quick reply buttons
function quickReply(type){
  let pool=[];
  if(type==='fact') pool=factReplies;
  else if(type==='joke') pool=jokes;
  else if(type==='quote') pool=quotes;
  else if(type==='tip') pool=tips;
  addMsg(pool[Math.floor(Math.random()*pool.length)]);
}

// Auto drop messages
function autoDrop(){
  const pool=[...factReplies,...quotes,...tips];
  const msg=pool[Math.floor(Math.random()*pool.length)];
  addMsg(msg);
  const next=15000+Math.random()*15000;
  setTimeout(autoDrop,next);
}
autoDrop();

</script>
</body>
</html>
