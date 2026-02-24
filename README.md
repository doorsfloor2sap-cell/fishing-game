<!DOCTYPE html>
<html lang="en">
<head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0"><title>Fishing Game</title><style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',sans-serif;}
body{background:linear-gradient(to bottom,#1a2980,#26d0ce);color:white;min-height:100vh;display:flex;flex-direction:column;align-items:center;padding:20px;}
.container{max-width:1000px;width:100%;}
header{text-align:center;margin-bottom:20px;}
h1{font-size:3rem;margin-bottom:10px;color:#FFD700;}
.game-container{display:flex;flex-direction:column;width:100%;background-color:rgba(0,30,60,0.7);border-radius:15px;padding:20px;box-shadow:0 10px 30px rgba(0,0,0,0.5);}
.stats{display:flex;justify-content:space-between;background-color:rgba(0,20,40,0.8);padding:15px;border-radius:10px;margin-bottom:20px;}
.stat-box{text-align:center;flex:1;margin:0 10px;}
.stat-value{font-size:2.5rem;font-weight:bold;color:#FFD700;}
.stat-label{font-size:1rem;opacity:0.8;}
.game-area{display:flex;flex-direction:column;align-items:center;position:relative;height:500px;overflow:hidden;background:linear-gradient(to bottom,#1e3c72,#2a5298);border-radius:10px;border:5px solid #0a1931;}
.sky{position:absolute;top:0;left:0;width:100%;height:30%;background:linear-gradient(to bottom,#87CEEB,#E0F6FF);z-index:1;}
.sun{position:absolute;top:20px;right:40px;width:60px;height:60px;background-color:#FFD700;border-radius:50%;box-shadow:0 0 40px #FFA500;}
.water{position:absolute;bottom:0;left:0;width:100%;height:70%;z-index:2;}
.light-water{position:absolute;top:0;left:0;width:100%;height:30%;background:linear-gradient(to bottom,rgba(255,255,255,0.3),rgba(38,208,206,0.7));z-index:3;}
.green-water{position:absolute;top:30%;left:0;width:100%;height:30%;background:linear-gradient(to bottom,rgba(38,208,206,0.7),rgba(0,128,0,0.7));z-index:2;}
.dark-water{position:absolute;bottom:0;left:0;width:100%;height:40%;background:linear-gradient(to top,#1a2980,rgba(26,41,128,0.8));z-index:2;}
.boat{position:absolute;bottom:70%;left:50%;transform:translateX(-50%);width:120px;height:50px;background-color:#8B4513;border-radius:10px 10px 0 0;z-index:4;}
.boat::before{content:'';position:absolute;top:-40px;left:50%;transform:translateX(-50%);width:5px;height:40px;background-color:#654321;}
.boat::after{content:'';position:absolute;top:-35px;left:50%;width:40px;height:20px;background-color:#FFD700;transform:translateX(-50%);clip-path:polygon(0% 0%,100% 50%,0% 100%);}
.fishing-line{position:absolute;top:30%;left:50%;transform:translateX(-50%);width:2px;height:0;background-color:rgba(255,255,255,0.7);z-index:3;transition:height 0.3s ease;}
.fishing-line.highlight{background-color:#FFD700;box-shadow:0 0 10px #FFD700;}
.hook{position:absolute;bottom:95%;left:50%;transform:translateX(-50%);width:20px;height:20px;z-index:5;transition:bottom 0.1s ease;}
.hook-circle{position:absolute;width:100%;height:100%;border:3px solid #C0C0C0;border-radius:50%;}
.hook-tip{position:absolute;bottom:2px;left:50%;transform:translateX(-50%);width:8px;height:10px;border-left:3px solid #C0C0C0;border-bottom:3px solid #C0C0C0;transform:rotate(-45deg) translateX(-50%);}
.bait{position:absolute;bottom:-5px;left:50%;transform:translateX(-50%);width:12px;height:12px;background-color:#FF6347;border-radius:50%;z-index:6;}
.fish{position:absolute;width:60px;height:30px;z-index:4;}
.fish-body{width:100%;height:100%;background-color:#FF6347;border-radius:50%;position:relative;}
.fish-tail{position:absolute;left:-10px;top:50%;transform:translateY(-50%);width:0;height:0;border-top:15px solid transparent;border-bottom:15px solid transparent;border-right:20px solid #FF6347;}
.fish-fin{position:absolute;top:-5px;left:20px;width:10px;height:15px;background-color:#FF4500;border-radius:50%;}
.fish-eye{position:absolute;right:10px;top:8px;width:8px;height:8px;background-color:white;border-radius:50%;}
.fish-eye::after{content:'';position:absolute;top:2px;left:2px;width:4px;height:4px;background-color:black;border-radius:50%;}
.shiny-effect{position:absolute;top:-5px;left:-5px;width:70px;height:40px;background:radial-gradient(circle,#FFD700,rgba(255,215,0,0)70%);opacity:0.7;z-index:3;border-radius:50%;animation:shine 1.5s infinite alternate;}
@keyframes shine{from{opacity:0.4;transform:scale(1);}to{opacity:0.8;transform:scale(1.1);}}
.controls{display:flex;justify-content:center;margin-top:20px;gap:10px;flex-wrap:wrap;}
.control-group{display:flex;align-items:center;gap:5px;}
button{padding:15px 30px;font-size:1.2rem;font-weight:bold;border:none;border-radius:50px;cursor:pointer;transition:all 0.2s ease;display:flex;align-items:center;justify-content:center;gap:10px;}
button:hover{transform:translateY(-3px);box-shadow:0 5px 15px rgba(0,0,0,0.3);}
#cast-btn{background:linear-gradient(to right,#4CAF50,#2E8B57);color:white;}
#reel-btn{background:linear-gradient(to right,#2196F3,#0D47A1);color:white;}
#reset-btn{background:linear-gradient(to right,#FF9800,#E65100);color:white;}
#cast-btn:disabled,#reel-btn:disabled{opacity:0.6;cursor:not-allowed;}
.arrow-btn{width:60px;height:60px;padding:0;background:#FF9800;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:1.8rem;transition:all 0.1s ease;}
.arrow-btn:active{transform:scale(0.95);background:#E65100;}
.arrow-btn:disabled{opacity:0.6;cursor:not-allowed;}
.message{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);background-color:rgba(0,0,0,0.8);padding:15px 30px;border-radius:10px;font-size:1.5rem;font-weight:bold;z-index:10;text-align:center;display:none;}
.caught-indicator{position:absolute;top:-30px;left:50%;transform:translateX(-50%);background-color:rgba(255,215,0,0.9);color:#1a2980;padding:5px 10px;border-radius:5px;font-weight:bold;display:none;z-index:8;}
@media(max-width:768px){.stats{flex-direction:column;gap:15px;}.controls{flex-direction:column;align-items:center;}.control-group{width:100%;justify-content:center;}button{width:100%;max-width:300px;}.arrow-btn{width:50px;height:50px;}.game-area{height:400px;}}</style></head>
<body>
<div class="container">
<header><h1>🎣 Fishing Game</h1></header>
<div class="game-container">
<div class="stats">
<div class="stat-box"><div class="stat-value" id="score">0</div><div class="stat-label">SCORE</div></div>
<div class="stat-box"><div class="stat-value" id="fish-count">0</div><div class="stat-label">FISH CAUGHT</div></div>
<div class="stat-box"><div class="stat-value" id="cast-count">0</div><div class="stat-label">CASTS</div></div>
<div class="stat-box"><div class="stat-value" id="level">1</div><div class="stat-label">LEVEL</div></div>
</div>
<div class="game-area">
<div class="sky"><div class="sun"></div></div>
<div class="water">
<div class="light-water"></div>
<div class="green-water"></div>
<div class="dark-water"></div>
</div>
<div class="boat"></div>
<div class="fishing-line" id="fishing-line"></div>
<div class="hook" id="hook">
<div class="hook-circle"></div>
<div class="hook-tip"></div>
<div class="bait" id="bait"></div>
</div>
<div class="message" id="message"></div>
<div class="caught-indicator" id="caught-indicator">FISH ON HOOK!</div>
</div>
<div class="controls">
<div class="control-group">
<button id="cast-btn">🎣 Cast Line</button>
<button class="arrow-btn" id="up-btn" disabled>⬆️</button>
</div>
<div class="control-group">
<button id="reel-btn" disabled>🎣 Reel In</button>
<button class="arrow-btn" id="down-btn" disabled>⬇️</button>
</div>
<button id="reset-btn">🔄 Reset Game</button>
</div>
</div>
</div>
<script>
let score=0,fishCaught=0,casts=0,level=1;
let isCasting=false,isReeling=false,currentFish=null;
let fishInterval,fishList=[],hookIsTouched=false;
let currentDepth=5,maxDepth=95;
let moveInterval=null;
let upPressed=false,downPressed=false;
let goldfishInterval,troutInterval,tunaBassShinyInterval;
let yellowFishCount=0,greenCycleCounter=0,deepCycleCounter=0;
let totalFishCount=0;
const MAX_FISH=10;
const MAX_LEVEL=5;

const scoreEl=document.getElementById('score');
const fishCountEl=document.getElementById('fish-count');
const castCountEl=document.getElementById('cast-count');
const levelEl=document.getElementById('level');
const fishingLine=document.getElementById('fishing-line');
const hook=document.getElementById('hook');
const bait=document.getElementById('bait');
const castBtn=document.getElementById('cast-btn');
const reelBtn=document.getElementById('reel-btn');
const upBtn=document.getElementById('up-btn');
const downBtn=document.getElementById('down-btn');
const messageEl=document.getElementById('message');
const caughtIndicator=document.getElementById('caught-indicator');
const gameArea=document.querySelector('.game-area');

const fishTypes=[
{name:"Goldfish",color:"#FFD700",points:1},
{name:"Trout",color:"#8B4513",points:2},
{name:"Salmon",color:"#FA8072",points:3},
{name:"Tuna",color:"#1E90FF",points:5},
{name:"Bass",color:"#32CD32",points:10},
{name:"Shiny Fish",color:"#FF1493",points:20}
];

function createInitialFish(){
document.querySelectorAll('.fish').forEach(fish=>fish.remove());
fishList=[];
yellowFishCount=0;
greenCycleCounter=0;
deepCycleCounter=0;
totalFishCount=0;

clearInterval(goldfishInterval);
clearInterval(troutInterval);
clearInterval(tunaBassShinyInterval);

goldfishInterval=setInterval(()=>{
if(totalFishCount<MAX_FISH){
createFish('Goldfish');
}
},2500);

troutInterval=setInterval(()=>{
if(totalFishCount<MAX_FISH){
if(greenCycleCounter<3){
createFish('Trout');
}else{
createFish('Salmon');
}
greenCycleCounter=(greenCycleCounter+1)%4;
}
},3000);

tunaBassShinyInterval=setInterval(()=>{
if(totalFishCount<MAX_FISH){
if(deepCycleCounter<3){
createFish('Tuna');
}else if(deepCycleCounter===3){
createFish('Bass');
}else if(deepCycleCounter<7){
createFish('Tuna');
}else{
createFish('Shiny Fish');
}
deepCycleCounter=(deepCycleCounter+1)%8;
}
},4000);
}

function createFish(fishName){
const fishType=fishTypes.find(f=>f.name===fishName);
if(!fishType)return;

const fishEl=document.createElement('div');
fishEl.className='fish';
fishEl.dataset.type=fishType.name;
fishEl.dataset.points=fishType.points;

const leftPos=Math.random()*50+100;
let bottomPos;

if(fishType.name==="Goldfish"){
bottomPos=Math.random()*20+50;
yellowFishCount++;
}else if(fishType.name==="Trout"||fishType.name==="Salmon"){
bottomPos=Math.random()*20+30;
}else if(fishType.name==="Tuna"||fishType.name==="Bass"||fishType.name==="Shiny Fish"){
bottomPos=Math.random()*25+5;
}

fishEl.style.left=`${leftPos}%`;
fishEl.style.bottom=`${bottomPos}%`;
fishEl.style.transform='scaleX(-1)';

const fishBody=document.createElement('div');
fishBody.className='fish-body';
fishBody.style.backgroundColor=fishType.color;

if(fishType.name==="Shiny Fish"){
const shinyEffect=document.createElement('div');
shinyEffect.className='shiny-effect';
fishBody.appendChild(shinyEffect);
}

const fishTail=document.createElement('div');
fishTail.className='fish-tail';
fishTail.style.borderRightColor=fishType.color;

const fishFin=document.createElement('div');
fishFin.className='fish-fin';
fishFin.style.backgroundColor=fishType.color.replace(')',',0.8)').replace('rgb','rgba');

const fishEye=document.createElement('div');
fishEye.className='fish-eye';

fishBody.appendChild(fishTail);
fishBody.appendChild(fishFin);
fishBody.appendChild(fishEye);
fishEl.appendChild(fishBody);
gameArea.appendChild(fishEl);

const baseSpeed=fishType.name==="Shiny Fish"?7:3.5;
const fish={
element:fishEl,
speed:baseSpeed*level,
type:fishType,
isNearHook:false,
isTouchingHook:false
};
fishList.push(fish);
totalFishCount++;
}

function moveFish(){
fishList.forEach((fish,index)=>{
const fishEl=fish.element;
let currentLeft=parseFloat(fishEl.style.left);

currentLeft-=fish.speed*0.1;
fishEl.style.left=`${currentLeft}%`;

if(isCasting && !isReeling){
const hookRect=hook.getBoundingClientRect();
const fishRect=fishEl.getBoundingClientRect();
    
const isColliding=fishRect.right>hookRect.left&&fishRect.left<hookRect.right&&
                  fishRect.bottom>hookRect.top&&fishRect.top<hookRect.bottom;
    
if(isColliding && !fish.isTouchingHook){
fish.isTouchingHook=true;
if(!hookIsTouched){
fishOnHook(fish);
}
}else if(!isColliding && fish.isTouchingHook){
fish.isTouchingHook=false;
if(currentFish===fish){
fishOffHook();
}
}
}

if(currentLeft<-10){
fishEl.remove();
fishList.splice(index,1);
totalFishCount--;
if(fish.type.name==="Goldfish")yellowFishCount--;
}
});
}

function fishOnHook(fish){
currentFish=fish;
hookIsTouched=true;
fishingLine.classList.add('highlight');
caughtIndicator.style.display='block';
caughtIndicator.style.top=`${currentDepth-30}px`;
bait.style.animation='flash 0.5s infinite alternate';
hook.style.transform='translateX(-50%) scale(1.2)';
}

function fishOffHook(){
if(currentFish){
currentFish.isNearHook=false;
currentFish.isTouchingHook=false;
}
currentFish=null;
hookIsTouched=false;
fishingLine.classList.remove('highlight');
caughtIndicator.style.display='none';
bait.style.animation='';
hook.style.transform='translateX(-50%) scale(1)';
}

function moveHook(direction){
if(!isCasting||isReeling)return;
const moveAmount=3;
const newDepth=currentDepth+(direction*moveAmount);
if(newDepth>=5&&newDepth<=maxDepth){
currentDepth=newDepth;
hook.style.bottom=`${100-currentDepth}%`;
fishingLine.style.height=`${currentDepth}%`;
caughtIndicator.style.top=`${currentDepth-30}px`;
}
}

function startMovingHook(direction){
if(moveInterval)clearInterval(moveInterval);
moveInterval=setInterval(()=>moveHook(direction),30);
}

function stopMovingHook(){
if(moveInterval){
clearInterval(moveInterval);
moveInterval=null;
}
}

function castLine(){
if(isCasting)return;
isCasting=true;
casts++;
castBtn.disabled=true;
reelBtn.disabled=false;
upBtn.disabled=false;
downBtn.disabled=false;

currentDepth=5;
fishingLine.style.height=`${currentDepth}%`;

setTimeout(()=>{
hook.style.bottom=`${100-currentDepth}%`;
fishInterval=setInterval(moveFish,50);
},100);
updateStats();
}

function reelIn(){
if(!isCasting||isReeling)return;
isReeling=true;
reelBtn.disabled=true;
upBtn.disabled=true;
downBtn.disabled=true;
stopMovingHook();

if(currentFish&&hookIsTouched){
const points=parseInt(currentFish.element.dataset.points);
score+=points;
fishCaught++;

if(currentFish.type.name==="Shiny Fish"){
if(level < MAX_LEVEL){
level++;
showMessage(`✨ LEVEL ${level} UNLOCKED! Catch a Shiny Fish at Level 5 to win! ✨`,2000);
}else if(level === MAX_LEVEL){
showMessage(`🎉 CONGRATULATIONS! YOU WON THE GAME! 🎉\nFinal Score: ${score}\nFish Caught: ${fishCaught}`,5000);
isCasting=false;
isReeling=false;
castBtn.disabled=true;
reelBtn.disabled=true;
upBtn.disabled=true;
downBtn.disabled=true;
clearInterval(fishInterval);
clearInterval(goldfishInterval);
clearInterval(troutInterval);
clearInterval(tunaBassShinyInterval);
setTimeout(()=>{
resetGame();
},5000);
return;
}
}else{
showMessage(`Caught ${currentFish.element.dataset.type} for ${points} points!`,1500);
}

const fishEl=currentFish.element;
fishEl.style.transition='transform 1.5s ease, top 1.5s ease';
fishEl.style.transform='translateY(-100px) scale(0.5)';
fishEl.style.top='0';

setTimeout(()=>{
fishEl.remove();
const index=fishList.indexOf(currentFish);
if(index>-1){
fishList.splice(index,1);
totalFishCount--;
if(currentFish.type.name==="Goldfish")yellowFishCount--;
}
},1500);

currentFish=null;
hookIsTouched=false;
}else{
score=Math.max(0,score-3);
showMessage("No fish on hook! -3 points",1500);
}

caughtIndicator.style.display='none';
updateStats();

setTimeout(()=>{
isReeling=false;
reelBtn.disabled=false;
upBtn.disabled=false;
downBtn.disabled=false;
hideMessage();
bait.style.animation='';
hook.style.transform='translateX(-50%) scale(1)';
},2000);
}

function resetGame(){
score=0;fishCaught=0;casts=0;level=1;
isCasting=false;isReeling=false;currentFish=null;hookIsTouched=false;
currentDepth=5;maxDepth=95;
fishingLine.style.height='0';
fishingLine.classList.remove('highlight');
hook.style.bottom='95%';
castBtn.disabled=false;
reelBtn.disabled=true;
upBtn.disabled=true;
downBtn.disabled=true;
caughtIndicator.style.display='none';
bait.style.animation='';
hook.style.transform='translateX(-50%) scale(1)';
stopMovingHook();
document.querySelectorAll('.fish').forEach(fish=>fish.remove());
clearInterval(fishInterval);
clearInterval(goldfishInterval);
clearInterval(troutInterval);
clearInterval(tunaBassShinyInterval);
hideMessage();
createInitialFish();
updateStats();
}

function updateStats(){
scoreEl.textContent=score;
fishCountEl.textContent=fishCaught;
castCountEl.textContent=casts;
levelEl.textContent=level;
}

function showMessage(text,duration=0){
messageEl.textContent=text;
messageEl.style.display='block';
if(duration>0){
setTimeout(()=>hideMessage(),duration);
}
}

function hideMessage(){
messageEl.style.display='none';
}

const style=document.createElement('style');
style.textContent=`
@keyframes flash{from{background-color:#FF6347;}to{background-color:#FFD700;}}
@keyframes shine{from{opacity:0.4;transform:scale(1);}to{opacity:0.8;transform:scale(1.1);}}
`;
document.head.appendChild(style);

castBtn.addEventListener('click',castLine);
reelBtn.addEventListener('click',reelIn);

upBtn.addEventListener('mousedown',()=>startMovingHook(-1));
upBtn.addEventListener('mouseup',stopMovingHook);
upBtn.addEventListener('mouseleave',stopMovingHook);
upBtn.addEventListener('touchstart',(e)=>{e.preventDefault();startMovingHook(-1);});
upBtn.addEventListener('touchend',(e)=>{e.preventDefault();stopMovingHook();});

downBtn.addEventListener('mousedown',()=>startMovingHook(1));
downBtn.addEventListener('mouseup',stopMovingHook);
downBtn.addEventListener('mouseleave',stopMovingHook);
downBtn.addEventListener('touchstart',(e)=>{e.preventDefault();startMovingHook(1);});
downBtn.addEventListener('touchend',(e)=>{e.preventDefault();stopMovingHook();});

document.getElementById('reset-btn').addEventListener('click',resetGame);
document.addEventListener('keydown',(e)=>{
if(e.code==='Space'&&!isCasting)castLine();
else if(e.code==='KeyR'&&isCasting&&!isReeling)reelIn();
else if(e.code==='ArrowUp'&&isCasting&&!isReeling){e.preventDefault();startMovingHook(-1);}
else if(e.code==='ArrowDown'&&isCasting&&!isReeling){e.preventDefault();startMovingHook(1);}
else if(e.code==='Escape')resetGame();
});

document.addEventListener('keyup',(e)=>{
if(e.code==='ArrowUp'||e.code==='ArrowDown')stopMovingHook();
});

createInitialFish();
</script>
</body>
</html>