<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>LKA</title>
<meta name="theme-color" content="#07060d">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Unbounded:wght@500;800&family=Manrope:wght@400;600&display=swap" rel="stylesheet">
<style>
:root{
  --ink:#07060d; --glass:rgba(14,10,24,.55); --line:rgba(255,255,255,.14);
  --text:#f4f1ff; --muted:#b4acd0; --accent:#ff2e88; --accent2:#8a5bff;
  --display:'Unbounded','Arial Black',sans-serif; --body:'Manrope','Segoe UI',Arial,sans-serif;
}
*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%}
body{background:var(--ink);color:var(--text);font-family:var(--body);overflow:hidden;-webkit-tap-highlight-color:transparent}

/* Hintergrund-Video */
#bg{position:fixed;inset:0;overflow:hidden;z-index:0;pointer-events:none}
#bg iframe,#bg #bgPlayer{position:absolute;top:50%;left:50%;width:100vw;height:56.25vw;min-height:100vh;min-width:177.78vh;transform:translate(-50%,-50%);border:0}
.shade{position:fixed;inset:0;z-index:1;pointer-events:none;
  background:radial-gradient(ellipse at center,rgba(7,6,13,.25) 0%,rgba(7,6,13,.78) 100%)}
.grain{position:fixed;inset:0;z-index:2;pointer-events:none;opacity:.07;mix-blend-mode:overlay;
  background-image:url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='160' height='160'><filter id='n'><feTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='2'/></filter><rect width='100%' height='100%' filter='url(%23n)'/></svg>")}
#musicHost{position:fixed;width:1px;height:1px;opacity:0;pointer-events:none;left:-10px;top:-10px;overflow:hidden}

/* Enter-Screen */
#enter{position:fixed;inset:0;z-index:50;display:grid;place-items:center;background:var(--ink);cursor:pointer;
  transition:opacity .9s ease,visibility .9s}
#enter.gone{opacity:0;visibility:hidden}
#enter p{font-family:var(--display);font-weight:500;font-size:clamp(.8rem,2.6vw,1.05rem);letter-spacing:.4em;color:var(--muted);
  animation:pulse 2.2s ease-in-out infinite}
@keyframes pulse{0%,100%{opacity:.35}50%{opacity:1}}

/* Karte */
main{position:relative;z-index:10;height:100%;display:grid;place-items:center;padding:20px;
  padding-top:max(20px,env(safe-area-inset-top,0px));padding-bottom:max(20px,env(safe-area-inset-bottom,0px))}
.card{width:min(440px,100%);padding:34px 28px 26px;text-align:center;border-radius:26px;
  background:var(--glass);border:1px solid var(--line);
  backdrop-filter:blur(18px) saturate(140%);-webkit-backdrop-filter:blur(18px) saturate(140%);
  box-shadow:0 30px 80px rgba(0,0,0,.6),0 0 0 1px rgba(255,46,136,.08),0 0 90px rgba(138,91,255,.18);
  transform-style:preserve-3d;transition:transform .15s ease-out;
  opacity:0;transform:translateY(24px) scale(.97)}
body.in .card{opacity:1;transform:none;transition:opacity 1s .2s ease,transform 1s .2s cubic-bezier(.2,.8,.2,1)}
body.in .card.tilting{transition:transform .12s ease-out}

.ring{width:104px;height:104px;margin:0 auto 18px;border-radius:50%;padding:3px;
  background:conic-gradient(from 0deg,var(--accent),var(--accent2),#35e0ff,var(--accent));animation:spin 6s linear infinite}
.ring div{width:100%;height:100%;border-radius:50%;overflow:hidden;background:#0d0818;animation:spin 6s linear infinite reverse}
.ring img{width:100%;height:100%;object-fit:cover;display:block;filter:contrast(1.05)}
.alias{display:flex;justify-content:center;flex-wrap:wrap;gap:8px;margin-top:12px}
.alias span{padding:4px 12px;border-radius:999px;border:1px solid var(--line);background:rgba(255,255,255,.06);
  font-size:.8rem;font-weight:600;color:var(--muted);letter-spacing:.02em}
@keyframes spin{to{transform:rotate(360deg)}}

h1{font-family:var(--display);font-weight:800;font-size:clamp(1.5rem,6.4vw,2.05rem);letter-spacing:-.02em;line-height:1.1;
  position:relative;display:inline-block;cursor:default;word-break:break-all}
h1::before,h1::after{content:attr(data-t);position:absolute;inset:0;opacity:0;pointer-events:none}
h1::before{color:var(--accent);transform:translate(-3px,0)}
h1::after{color:#35e0ff;transform:translate(3px,0)}
h1:hover::before,h1:hover::after{opacity:.8;animation:glitch .35s steps(2) infinite}
@keyframes glitch{0%{clip-path:inset(10% 0 60% 0)}50%{clip-path:inset(55% 0 10% 0)}100%{clip-path:inset(30% 0 35% 0)}}

.tag{min-height:1.5em;margin-top:10px;color:var(--muted);font-size:.98rem}
.tag i{display:inline-block;width:2px;height:1em;background:var(--accent);margin-left:3px;vertical-align:-2px;animation:blink 1s steps(1) infinite}
@keyframes blink{50%{opacity:0}}

.links{display:grid;gap:12px;margin-top:26px}
.link{display:flex;align-items:center;gap:14px;padding:14px 16px;border-radius:16px;text-decoration:none;color:var(--text);
  background:rgba(255,255,255,.05);border:1px solid var(--line);text-align:left;font:inherit;cursor:pointer;width:100%;
  transition:transform .2s,border-color .2s,background .2s,box-shadow .2s}
.link:hover,.link:focus-visible{transform:translateY(-3px);border-color:var(--c);background:rgba(255,255,255,.09);
  box-shadow:0 10px 30px -10px var(--c);outline:none}
.link svg{width:26px;height:26px;flex:none;fill:var(--c)}
.link b{display:block;font-weight:600;font-size:1rem}
.link small{display:block;color:var(--muted);font-size:.82rem;margin-top:1px}
.link .go{margin-left:auto;color:var(--muted);transition:transform .2s,color .2s}
.link:hover .go{transform:translateX(4px);color:var(--c)}

.foot{margin-top:22px;display:flex;align-items:center;justify-content:center;gap:16px;color:var(--muted);font-size:.82rem}
.views{display:flex;align-items:center;gap:6px}
.views svg{width:15px;height:15px;fill:currentColor}

/* Musik-Steuerung */
.player{position:fixed;z-index:20;right:max(16px,env(safe-area-inset-right,0px));bottom:max(16px,env(safe-area-inset-bottom,0px));
  display:flex;align-items:center;gap:10px;padding:8px 14px 8px 8px;border-radius:999px;
  background:var(--glass);border:1px solid var(--line);backdrop-filter:blur(14px);-webkit-backdrop-filter:blur(14px)}
.player button{width:34px;height:34px;border-radius:50%;border:0;cursor:pointer;color:#fff;
  background:linear-gradient(135deg,var(--accent),var(--accent2));display:grid;place-items:center}
.player button svg{width:16px;height:16px;fill:#fff}
.player input{width:84px;accent-color:var(--accent)}

/* Toast */
#toast{position:fixed;z-index:60;left:50%;top:max(22px,env(safe-area-inset-top,0px));transform:translate(-50%,-90px);
  padding:11px 20px;border-radius:999px;background:#fff;color:#14101f;font-weight:600;font-size:.9rem;
  transition:transform .35s cubic-bezier(.2,.9,.3,1.2);box-shadow:0 10px 30px rgba(0,0,0,.4)}
#toast.show{transform:translate(-50%,0)}

@media (max-width:480px){.player input{width:64px}.card{padding:28px 20px 22px}}
@media (prefers-reduced-motion:reduce){*{animation:none!important;transition:none!important}}
</style>
</head>
<body>

<div id="bg"><div id="bgPlayer"></div></div>
<div class="shade"></div><div class="grain"></div>
<div id="musicHost"><div id="musicPlayer"></div></div>

<div id="enter" role="button" tabindex="0" aria-label="Seite betreten"><p>KLICK ZUM BETRETEN</p></div>

<main>
  <section class="card" id="card">
    <div class="ring"><div><img src="data:image/jpeg;base64,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" alt="Profilbild"></div></div>
    <h1 data-t="LKA">LKA</h1>
    <div class="alias"><span>LKA.com</span><span>LTC.com</span></div>
    <p class="tag"><span id="typed"></span><i></i></p>
    <nav class="links" id="links" aria-label="Links"></nav>
    <div class="foot">
      <span class="views"><svg viewBox="0 0 24 24"><path d="M12 5C6.5 5 2.7 9 1.5 12c1.2 3 5 7 10.5 7s9.3-4 10.5-7C21.3 9 17.5 5 12 5zm0 11a4 4 0 110-8 4 4 0 010 8zm0-6a2 2 0 100 4 2 2 0 000-4z"/></svg><span id="views">0</span></span>
    </div>
  </section>
</main>

<div class="player">
  <button id="pp" aria-label="Musik an/aus"><svg viewBox="0 0 24 24" id="ppIcon"><path d="M6 5h4v14H6zm8 0h4v14h-4z"/></svg></button>
  <input id="vol" type="range" min="0" max="100" value="40" aria-label="Lautstärke">
</div>
<div id="toast"></div>

<script>
/* ============ HIER DEINE LINKS EINTRAGEN ============
   type "link"  = öffnet die URL
   type "copy"  = kopiert den Text in die Zwischenablage                */
const PROFILE = {
  tagline: ["Willkommen auf meiner Seite.", "Hier findest du mich.", "Schreib mir einfach."]
};
const LINKS = [
  { type:"copy", name:"Discord", sub:"_zynx.__.1 – Tippen zum Kopieren", value:"_zynx.__.1", color:"#7b8cff",
    icon:"M20.3 4.4A17 17 0 0016 3l-.2.4a12 12 0 013.6 1.8 15 15 0 00-14.8 0A12 12 0 019 3.4L8.9 3a17 17 0 00-4.3 1.4C1.9 8.5 1.2 12.5 1.5 16.4a17 17 0 005.2 2.6l1.1-1.800a11 11 0 01-1.700-.8l.4-.3a12 12 0 0010.600 0l.4.3c-.5.3-1.100.6-1.700.8l1.100 1.800a17 17 0 005.200-2.600c.4-4.600-.7-8.600-2.800-12zM8.700 14c-1 0-1.800-.9-1.800-2s.8-2 1.800-2 1.800.9 1.800 2-.8 2-1.800 2zm6.600 0c-1 0-1.800-.9-1.800-2s.8-2 1.800-2 1.800.9 1.800 2-.8 2-1.800 2z" },
  { type:"link", name:"Snapchat", sub:"lka_ff26", value:"https://www.snapchat.com/add/lka_ff26", color:"#fffc00",
    icon:"M12 2c3 0 5 2.100 5 5v2.300c.3.200.8.300 1.200.300.400 0 .800.200.800.600 0 .500-.700.800-1.500 1 .500 1.200 1.600 2.200 2.800 2.800.300.100.300.500 0 .700-.600.400-1.400.500-2 .600-.200.500-.300 1-.700 1.100-.500.100-1.200-.200-2.100.100-.900.300-1.500 1.600-3.500 1.600s-2.600-1.300-3.500-1.600c-.900-.300-1.600 0-2.100-.100-.400-.100-.500-.600-.700-1.100-.600-.100-1.400-.200-2-.600-.300-.200-.300-.600 0-.700 1.200-.600 2.300-1.600 2.800-2.800C4.700 10.700 4 10.400 4 9.900c0-.400.400-.600.800-.600.400 0 .900-.100 1.200-.300V7c0-2.900 2-5 5-5z" }
  ,{ type:"link", name:"TikTok", sub:"lka_ff0", value:"https://www.tiktok.com/@lka_ff0", color:"#25f4ee",
    icon:"M16.6 5.8A4.3 4.3 0 0115.5 3h-3.1v12.4a2.6 2.6 0 11-2.6-2.6c.3 0 .5 0 .8.1V9.7a5.7 5.7 0 105 5.7V9a7.4 7.4 0 004.3 1.4V7.3a4.3 4.3 0 01-2.3-1.5z" }
  /* Neuen Link hinzufügen – Beispiel:
  ,{ type:"link", name:"TikTok", sub:"@name", value:"https://tiktok.com/@name", color:"#25f4ee", icon:"" }
  */
];
const MUSIC_ID = "qqZ8BKhb5Ac";
const BG_ID    = "egJqcKHjveA";
/* =================================================== */

const $ = s => document.querySelector(s);

/* Links rendern */
const arrow = '<span class="go">&rsaquo;</span>';
$('#links').innerHTML = LINKS.map((l,i)=>{
  const ic = `<svg viewBox="0 0 24 24"><path d="${l.icon||'M10 6v2H5v8h5v2H3V6h7zm4 0h7v12h-7v-2h5V8h-5V6zm-4 5h4v2h-4v-2z'}"/></svg>`;
  const inner = `${ic}<span><b>${l.name}</b><small>${l.sub||''}</small></span>${arrow}`;
  return l.type==='link'
    ? `<a class="link" style="--c:${l.color}" href="${l.value}" target="_blank" rel="noopener">${inner}</a>`
    : `<button class="link" style="--c:${l.color}" data-copy="${l.value}">${inner}</button>`;
}).join('');

/* Kopieren + Toast */
let tt; function toast(m){const t=$('#toast');t.textContent=m;t.classList.add('show');clearTimeout(tt);tt=setTimeout(()=>t.classList.remove('show'),1800)}
document.addEventListener('click',e=>{
  const b=e.target.closest('[data-copy]'); if(!b) return;
  const v=b.dataset.copy;
  (navigator.clipboard?navigator.clipboard.writeText(v):Promise.reject()).catch(()=>{
    const a=document.createElement('textarea');a.value=v;document.body.appendChild(a);a.select();try{document.execCommand('copy')}catch(_){}a.remove();
  }).finally(()=>toast('Kopiert: '+v));
});

/* Tipp-Effekt */
(function(){
  const el=$('#typed'),L=PROFILE.tagline;let i=0,c=0,del=false;
  (function tick(){
    const w=L[i];el.textContent=w.slice(0,c);
    if(!del&&c===w.length){del=true;return setTimeout(tick,1800)}
    if(del&&c===0){del=false;i=(i+1)%L.length}
    c+=del?-1:1;setTimeout(tick,del?28:60);
  })();
})();

/* 3D-Neigung */
(function(){
  const card=$('#card');if(matchMedia('(pointer:coarse)').matches)return;
  document.addEventListener('mousemove',e=>{
    if(!document.body.classList.contains('in'))return;
    const x=(e.clientX/innerWidth-.5),y=(e.clientY/innerHeight-.5);
    card.classList.add('tilting');
    card.style.transform=`perspective(900px) rotateY(${x*9}deg) rotateX(${-y*9}deg)`;
  });
  document.addEventListener('mouseleave',()=>{card.style.transform=''});
})();

/* Aufrufe (lokal im Browser) */
(function(){
  let n=0;try{n=parseInt(localStorage.getItem('v')||'0',10)+1;localStorage.setItem('v',n)}catch(_){n=1}
  $('#views').textContent=n.toLocaleString('de-DE');
})();

/* YouTube-Player */
let music,bg,musicReady=false,bgReady=false,entered=false,playing=false;
const tag=document.createElement('script');tag.src='https://www.youtube.com/iframe_api';document.head.appendChild(tag);
window.onYouTubeIframeAPIReady=function(){
  bg=new YT.Player('bgPlayer',{videoId:BG_ID,playerVars:{autoplay:1,mute:1,controls:0,loop:1,playlist:BG_ID,modestbranding:1,rel:0,playsinline:1,disablekb:1,iv_load_policy:3,fs:0},
    events:{onReady:e=>{bgReady=true;e.target.mute();e.target.playVideo()}}});
  music=new YT.Player('musicPlayer',{videoId:MUSIC_ID,playerVars:{controls:0,loop:1,playlist:MUSIC_ID,playsinline:1},
    events:{onReady:e=>{musicReady=true;e.target.setVolume(+$('#vol').value);if(entered)startMusic()},
            onStateChange:e=>{playing=e.data===1;setIcon()}}});
};
function startMusic(){if(!musicReady)return;music.unMute();music.setVolume(+$('#vol').value);music.playVideo();if(bgReady)bg.playVideo()}
function setIcon(){$('#ppIcon').innerHTML=playing?'<path d="M6 5h4v14H6zm8 0h4v14h-4z"/>':'<path d="M7 4l13 8-13 8z"/>'}

/* Enter */
function enter(){
  if(entered)return;entered=true;
  $('#enter').classList.add('gone');document.body.classList.add('in');
  startMusic();
}
$('#enter').addEventListener('click',enter);
$('#enter').addEventListener('keydown',e=>{if(e.key==='Enter'||e.key===' ')enter()});

$('#pp').addEventListener('click',()=>{if(!musicReady)return;playing?music.pauseVideo():startMusic()});
$('#vol').addEventListener('input',e=>{if(musicReady)music.setVolume(+e.target.value)});
</script>
</body>
</html>
