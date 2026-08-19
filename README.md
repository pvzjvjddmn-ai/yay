<!DOCTYPE html>
<html lang="sq">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1, user-scalable=no">
<title>Për Denin ❤</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,500;0,700;1,600&family=Cormorant+Garamond:ital,wght@0,400;0,500;0,600;1,500&family=Caveat:wght@500;700&display=swap" rel="stylesheet">
<style>
  :root{
    --wine:#5c2436;
    --wine-deep:#3d1826;
    --blush:#e7a9ad;
    --blush-soft:#f3d3d2;
    --cream:#fbf1e7;
    --gold:#c69a4b;
    --gold-soft:#e6c98a;
  }

  *{box-sizing:border-box;}

  html,body{
    margin:0;
    padding:0;
    min-height:100%;
    background:
      radial-gradient(circle at 20% 15%, #7a3145 0%, transparent 55%),
      radial-gradient(circle at 85% 80%, #4a1e30 0%, transparent 50%),
      linear-gradient(160deg, var(--wine-deep) 0%, var(--wine) 55%, #6e2c40 100%);
    font-family:'Cormorant Garamond', serif;
    color:var(--cream);
    overflow-x:hidden;
  }

  .stage{
    min-height:100svh;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:32px 18px;
    position:relative;
  }

  /* ambient floating hearts */
  .hearts{
    position:fixed;
    inset:0;
    pointer-events:none;
    overflow:hidden;
    z-index:0;
  }
  .hearts span{
    position:absolute;
    bottom:-10%;
    font-size:18px;
    color:var(--blush-soft);
    opacity:0.55;
    animation:float linear infinite;
  }
  @keyframes float{
    0%{ transform:translateY(0) translateX(0) rotate(0deg); opacity:0; }
    10%{ opacity:0.6; }
    90%{ opacity:0.5; }
    100%{ transform:translateY(-110vh) translateX(var(--drift,20px)) rotate(360deg); opacity:0; }
  }

  /* ---------- ENVELOPE ---------- */
  #envelope-wrap{
    position:relative;
    z-index:2;
    width:100%;
    max-width:380px;
    perspective:1400px;
  }

  .envelope{
    position:relative;
    width:100%;
    aspect-ratio:3/2;
    cursor:pointer;
    filter:drop-shadow(0 18px 30px rgba(0,0,0,0.45));
  }

  .env-body{
    position:absolute;
    inset:0;
    background:linear-gradient(160deg, var(--blush) 0%, var(--blush-soft) 100%);
    border-radius:10px;
    overflow:hidden;
  }
  .env-body::before, .env-body::after{
    content:"";
    position:absolute;
    width:0; height:0;
    border-style:solid;
  }
  /* side triangles of the envelope pocket */
  .env-body::before{
    left:0; bottom:0;
    border-width:0 0 100% 50%;
    border-color:transparent transparent rgba(255,255,255,0.18) transparent;
  }
  .env-body::after{
    right:0; bottom:0;
    border-width:0 50% 100% 0;
    border-color:transparent transparent rgba(0,0,0,0.06) transparent;
  }

  .env-flap{
    position:absolute;
    top:0; left:0; right:0;
    height:62%;
    background:linear-gradient(160deg, var(--blush-soft) 0%, var(--blush) 100%);
    clip-path:polygon(0 0, 100% 0, 50% 100%);
    transform-origin:top;
    transition:transform 0.9s cubic-bezier(.6,-0.1,.25,1);
    z-index:3;
  }

  .seal{
    position:absolute;
    top:52%;
    left:50%;
    width:56px;
    height:56px;
    transform:translate(-50%,-50%);
    background:radial-gradient(circle at 35% 30%, var(--gold-soft), var(--gold) 60%, #a97e2f 100%);
    border-radius:50%;
    display:flex;
    align-items:center;
    justify-content:center;
    box-shadow:0 4px 10px rgba(0,0,0,0.35), inset 0 2px 3px rgba(255,255,255,0.5);
    z-index:5;
    transition:transform 0.5s ease, opacity 0.5s ease;
  }
  .seal svg{ width:26px; height:26px; fill:var(--wine-deep); }

  .prompt{
    text-align:center;
    margin-top:22px;
    font-family:'Caveat', cursive;
    font-size:1.35rem;
    color:var(--gold-soft);
    letter-spacing:0.5px;
  }

  .envelope.open .env-flap{
    transform:rotateX(180deg);
    z-index:1;
  }
  .envelope.open .seal{
    opacity:0;
    transform:translate(-50%,-50%) scale(0.4) rotate(25deg);
  }

  /* letter peeking out, then flying up when opened */
  .letter-peek{
    position:absolute;
    left:6%; right:6%;
    bottom:6%;
    height:60%;
    background:var(--cream);
    border-radius:6px 6px 2px 2px;
    box-shadow:0 -2px 6px rgba(0,0,0,0.1);
    z-index:2;
    transition:transform 0.8s ease 0.15s;
  }
  .envelope.open .letter-peek{
    transform:translateY(-230%) scale(1.04);
  }

  /* ---------- LETTER MODAL ---------- */
  #letter-overlay{
    position:fixed;
    inset:0;
    z-index:10;
    display:flex;
    align-items:flex-start;
    justify-content:center;
    padding:min(6vh,48px) 16px 60px;
    background:rgba(30,10,20,0.55);
    backdrop-filter:blur(3px);
    opacity:0;
    pointer-events:none;
    transition:opacity 0.5s ease;
    overflow-y:auto;
  }
  #letter-overlay.show{
    opacity:1;
    pointer-events:auto;
  }

  .letter-card{
    position:relative;
    width:100%;
    max-width:560px;
    background:
      radial-gradient(circle at 90% 5%, rgba(198,154,75,0.12), transparent 40%),
      var(--cream);
    color:#3f2430;
    border-radius:4px;
    padding:38px 26px 34px;
    box-shadow:0 30px 60px rgba(0,0,0,0.45);
    transform:translateY(24px) scale(0.97);
    opacity:0;
    transition:transform 0.6s cubic-bezier(.2,.8,.2,1), opacity 0.6s ease;
  }
  #letter-overlay.show .letter-card{
    transform:translateY(0) scale(1);
    opacity:1;
  }

  .letter-card::before{
    content:"";
    position:absolute;
    inset:10px;
    border:1px solid rgba(92,36,54,0.18);
    border-radius:2px;
    pointer-events:none;
  }

  .close-btn{
    position:absolute;
    top:12px;
    right:14px;
    width:34px; height:34px;
    border:none;
    background:transparent;
    color:var(--wine);
    font-size:1.6rem;
    line-height:1;
    cursor:pointer;
    font-family:serif;
    opacity:0.6;
  }
  .close-btn:hover{ opacity:1; }

  .letter-title{
    font-family:'Playfair Display', serif;
    font-style:italic;
    font-weight:600;
    font-size:1.5rem;
    color:var(--wine);
    text-align:center;
    margin:2px 0 4px;
  }
  .letter-sub{
    text-align:center;
    font-size:0.9rem;
    letter-spacing:2px;
    text-transform:uppercase;
    color:var(--gold);
    margin-bottom:22px;
  }

  .letter-body p{
    font-size:1.14rem;
    line-height:1.75;
    margin:0 0 20px;
    text-align:left;
    text-wrap:pretty;
  }

  .letter-body .ps{
    font-size:1.02rem;
    font-style:italic;
    color:#6b3f4d;
    border-top:1px dashed rgba(92,36,54,0.25);
    padding-top:16px;
    margin-top:6px;
  }

  .signoff{
    text-align:center;
    font-family:'Caveat', cursive;
    font-weight:700;
    font-size:2.4rem;
    color:var(--wine);
    margin-top:6px;
  }
  .signoff .heart{
    color:#b23a52;
  }

  .little-hearts-row{
    text-align:center;
    margin-top:6px;
    letter-spacing:8px;
    color:var(--blush);
    font-size:1rem;
  }

  @media (min-width:600px){
    .letter-card{ padding:48px 46px 40px; }
    .letter-title{ font-size:1.9rem; }
  }
</style>
</head>
<body>

<div class="hearts" id="hearts"></div>

<div class="stage">
  <div id="envelope-wrap">
    <div class="envelope" id="envelope">
      <div class="letter-peek"></div>
      <div class="env-body"></div>
      <div class="env-flap"></div>
      <div class="seal">
        <svg viewBox="0 0 24 24"><path d="M12 21s-7.5-4.9-10.2-9.8C.3 8.6 1.6 5 5 5c2 0 3.4 1.1 4 2.2C9.6 6.1 11 5 13 5c3.4 0 4.7 3.6 3.2 6.2C13.5 16.1 12 21 12 21z"/></svg>
      </div>
    </div>
    <div class="prompt" id="prompt">prek zarfin për ta hapur ✧</div>
  </div>
</div>

<div id="letter-overlay">
  <div class="letter-card">
    <button class="close-btn" id="closeBtn" aria-label="Mbyll">×</button>
    <div class="letter-title">Për Denin</div>
    <div class="letter-sub">Gëzuar ditëlindjen</div>

    <div class="letter-body">
      <p>Denii dashuria ime edhe 100 jet, e di qe do jet nj urim si shum te tjer por mbi te gjitha dua qe te te shpreh se jm shum mirnjohse qe te njoha ty ti e di qe un te adhuroj komplet per cdo gje (dhe e di qe do thuash “vrt?” por qe po vrtet gjithcka qe po t them esht e vrtet) je personi me pozitiv dhe personi i duhur qe i duhej jetes time,</p>

      <p>okej nuk kemi shum qe kemi kete qe kemi por e di shmr qe ka qen nje nga gjerat me te mira qe me ka ndodh edhe pse kaq pak koh kmi kaluar a lot of things i must say nd i adore that ab us I value what we have the most sepse me duket real me ben te ndihem special in a way or another dhe never left out,</p>

      <p>un te dua shum dhe ti prolly e di por qe do te ta them der sa ta fiksosh sic ke fiksuar mostly of physics, I hope that u have luck on everything (qe esht mese normale) dhe cdo enderr e jotja to come to reality, sic me premtove dhe ti te premtoj dhe un I’ll be there by ur side (u can’t ignore the chemistry we have sou yeah) nd je stuck me mua jet e km then.</p>

      <p>I could talk for hours to you dhe te te shojegoj how much i love nd for what po thjesht nuk mendoj se nj mszh do mjaftonte (SE SDI CTE SHKRUAJ MPARA THERES TOO MUCH) por q I’ll get to do that sometimes I hope nd when I do maybe I’ll prove to you that you’re so special too me.</p>

      <p class="ps">p.s Mfal pr gabimet ne shkrim nd shit po jm pak emotional to write clearly dhe jam shum shum sorry i couldn’t get u a gift se sjm aty i kisha br plan ndryshe.</p>
    </div>

    <div class="signoff">Te dua shum edhe 100 prap shpirt<br>mwahhh <span class="heart">❤</span></div>
    <div class="little-hearts-row">♥ ♥ ♥</div>
  </div>
</div>

<script>
  // floating ambient hearts
  const heartsContainer = document.getElementById('hearts');
  const glyphs = ['❤','♥','✧'];
  for(let i=0;i<16;i++){
    const s = document.createElement('span');
    s.textContent = glyphs[Math.floor(Math.random()*glyphs.length)];
    s.style.left = Math.random()*100 + '%';
    s.style.setProperty('--drift', (Math.random()*60-30)+'px');
    s.style.fontSize = (12 + Math.random()*16) + 'px';
    s.style.animationDuration = (10 + Math.random()*10) + 's';
    s.style.animationDelay = (Math.random()*10) + 's';
    heartsContainer.appendChild(s);
  }

  const envelope = document.getElementById('envelope');
  const overlay = document.getElementById('letter-overlay');
  const prompt = document.getElementById('prompt');
  const closeBtn = document.getElementById('closeBtn');

  function openLetter(){
    envelope.classList.add('open');
    prompt.style.opacity = '0';
    setTimeout(()=>{
      overlay.classList.add('show');
    }, 550);
  }

  function closeLetter(){
    overlay.classList.remove('show');
  }

  envelope.addEventListener('click', openLetter);
  closeBtn.addEventListener('click', closeLetter);
  overlay.addEventListener('click', (e)=>{
    if(e.target === overlay) closeLetter();
  });
</script>

</body>
</html>
