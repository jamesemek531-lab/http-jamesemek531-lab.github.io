# http-jamesemek531-lab.github.io
<!DOCTYPE html>
<html><head><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<title>Crash Win Signal</title>
<style>
  body{background:#000;color:#0f0;font-family:Arial;margin:0;padding:10px;text-align:center}
  h1{color:#0f0}
  .s{font-size:28px;font-weight:bold;padding:20px;border-radius:15px;margin:15px 0}
  .r{background:#f00;color:#fff;animation:p 1s infinite}
  .y{background:#ff0;color:#000}
  .g{background:#0f0;color:#000}
  @keyframes p{0%,100%{opacity:1}50%{opacity:0.6}}
  #h{height:200px;overflow:auto;background:#111;padding:10px;border-radius:10px}
  .c{display:inline-block;padding:6px 12px;margin:4px;border-radius:8px;background:#333}
</style></head><body>
<h1>CRASH WIN SIGNAL</h1>
<div class="s g" id="sig">Loading live crashes…</div>
<div style="font-size:20px">Streak: <b id="i">0</b> | Chance: <b id="p">—</b></div>
<div id="h">Fetching data…</div>
<button onclick="beep()" style="padding:15px;margin:10px;background:#0f0;color:#000;border:none;border-radius:10px;font-size:18px">Test Alert</button>

<script>
let c=[];const h=document.getElementById('h'),s=document.getElementById('sig'),i=document.getElementById('i'),p=document.getElementById('p');

async function get(){try{
  const r=await fetch('https://api.allorigins.lol/get?method=raw&url='+encodeURIComponent('https://bc.game/crash/history'));
  const t=await r.text();
  const m=t.match(/"crashValue":(\d+\.\d+)/g);
  const n=m?m.map(x=>parseFloat(x.match(/\d+\.\d+/)[0])).slice(0,100):[];
  if(n.length>c.length){c=n;u();}
}catch(e){setTimeout(get,8000);}}get();setInterval(get,8000);

function u(){
  const r=c.slice(0,15);
  const l=r.filter(x=>x<1.5).length;
  const hi=r.filter(x=>x>=3).length;
  let txt="",cl="g",pr=65;
  if(hi>=3){txt="YELLOW – CASH 1.4–1.6× SAFE";cl="y";pr=82+hi*2;beep();}
  else if(l>=5){txt="RED – AGGRESSIVE 3.5–10×+";cl="r";pr=78+l*4;beep();}
  else{txt="GREEN – Play normal";cl="g";}
  s.textContent=txt;s.className="s "+cl;
  i.textContent=l+" lows | "+hi+" highs";p.textContent=pr+"%";

  h.innerHTML="";c.slice(0,25).forEach(x=>{let e=document.createElement('span');e.className='c';
  e.textContent=x.toFixed(2)+"× ";if(x<1.5)e.style.background='#f00';if(x>=3)e.style.background='#0f0';h.appendChild(e);});
}

function beep(){navigator.vibrate([300,100,300]);new Audio('https://cdn.pixabay.com/download/audio/2023/01/26/audio_3e3e1d8d8f.mp3').play();}
</script>
</body></html>
