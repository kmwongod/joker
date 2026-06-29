<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<title>집중력 테스트</title>
<style>
  body { font-family: sans-serif; max-width: 500px; margin: 60px auto; padding: 0 1rem; background: #f5f5f5; }
  h2 { font-size: 20px; margin-bottom: 4px; }
  .sub { color: #888; font-size: 14px; margin-bottom: 1.5rem; }
  .progress { font-size: 12px; color: #aaa; margin-bottom: 1rem; }
  .question { font-size: 16px; font-weight: 600; margin-bottom: 1rem; }
  .opt { display: block; width: 100%; text-align: left; padding: 10px 14px;
    margin-bottom: 8px; border: 1px solid #ddd; border-radius: 8px;
    background: #fff; font-size: 14px; cursor: pointer; }
  .opt:hover { background: #f0f0f0; }
  #overlay { display: none; position: fixed; inset: 0; background: #000;
    align-items: center; justify-content: center; z-index: 9999; }
  #overlay.on { display: flex; }
  #face { font-size: 200px; animation: shake .1s infinite; }
  @keyframes shake {
    0%{transform:translate(-5px,-5px) rotate(-2deg)}
    50%{transform:translate(5px,5px) rotate(2deg)}
    100%{transform:translate(-5px,5px) rotate(1deg)}
  }
  #boo { position: absolute; bottom: 60px; font-size: 32px; font-weight: bold;
    color: #ff3333; animation: blink .4s infinite; }
  @keyframes blink{0%,49%{opacity:1}50%,100%{opacity:0}}
</style>
</head>
<body>
<h2>집중력 테스트</h2>
<p class="sub">간단한 퀴즈입니다. 끝까지 집중해보세요!</p>
<div class="progress" id="prog">문제 1 / 3</div>
<div class="question" id="q"></div>
<div id="opts"></div>
<div id="overlay"><div id="face">👻</div><div id="boo">BOO!</div></div>
<script>
const qs=[
  {q:"한국의 수도는 어디일까요?",opts:["부산","서울","대전","인천"],ans:1},
  {q:"1 + 1 은 얼마일까요?",opts:["1","3","2","4"],ans:2},
  {q:"태양계에서 가장 큰 행성은?",opts:["토성","목성","천왕성","해왕성"],ans:1}
];
let s=0;
function show(){
  document.getElementById('q').textContent=qs[s].q;
  document.getElementById('prog').textContent='문제 '+(s+1)+' / '+qs.length;
  const c=document.getElementById('opts'); c.innerHTML='';
  qs[s].opts.forEach((o,i)=>{
    const b=document.createElement('button');
    b.className='opt'; b.textContent=['①','②','③','④'][i]+' '+o;
    b.onclick=()=>{s++;s<qs.length?show():scare();};
    c.appendChild(b);
  });
}
function scare(){
  document.getElementById('overlay').classList.add('on');
  try{
    const a=new AudioContext(),osc=a.createOscillator(),g=a.createGain();
    osc.connect(g);g.connect(a.destination);osc.type='sawtooth';
    osc.frequency.setValueAtTime(80,a.currentTime);
    osc.frequency.exponentialRampToValueAtTime(1200,a.currentTime+0.3);
    g.gain.setValueAtTime(0.6,a.currentTime);
    g.gain.exponentialRampToValueAtTime(0.001,a.currentTime+1.2);
    osc.start();osc.stop(a.currentTime+1.2);
  }catch(e){}
  setTimeout(()=>{document.getElementById('overlay').classList.remove('on');s=0;show();},3000);
}
show();
</script>
</body>
</html># joker
