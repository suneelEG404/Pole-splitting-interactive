<!DOCTYPE html>
<html lang="en"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Pole Splitting (Miller Effect) – Interactive</title>
<style>
:root{box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px);--bg:#0e1420;--card:#171f30;--tx:#e6ebf5;--mu:#9aa8c2;--ln:#2a3550;--p1:#5cc8ff;--p2:#ff7a7a;--zr:#ffb454;--gh:#7a869f;color-scheme:dark}
@media (prefers-color-scheme:light){:root:not([data-theme="dark"]){--bg:#f5f7fb;--card:#fff;--tx:#18202e;--mu:#56627a;--ln:#d5dbe8;--p1:#0a6fb0;--p2:#c23030;--zr:#a35a00;--gh:#8a93a6;color-scheme:light}}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
body{margin:0;background:var(--bg);color:var(--tx);font:15px/1.5 system-ui,-apple-system,Segoe UI,Roboto,sans-serif;padding:12px}
main{max-width:760px;margin:0 auto}
h1{font-size:1.25rem;margin:.2em 0}h2{font-size:1rem;margin:0 0 .4em}
.c{background:var(--card);border:1px solid var(--ln);border-radius:12px;padding:12px;margin:12px 0}
.mu{color:var(--mu);font-size:.85rem}
label{display:flex;justify-content:space-between;margin-top:8px}label b{font-variant-numeric:tabular-nums}
input[type=range]{width:100%}
button{background:var(--bg);color:var(--tx);border:1px solid var(--ln);border-radius:8px;padding:6px 10px;margin:8px 6px 0 0;font:inherit}
.g{display:grid;grid-template-columns:repeat(auto-fit,minmax(130px,1fr));gap:8px}
.k{background:var(--bg);border:1px solid var(--ln);border-radius:8px;padding:6px 8px}.k span{display:block;color:var(--mu);font-size:.75rem}.k b{font-size:1.05rem;font-variant-numeric:tabular-nums}
svg{width:100%;height:auto;display:block}.t{fill:var(--tx);font-size:11px}.m{fill:var(--mu);font-size:10px}
</style></head><body><main>
<h1>Pole splitting: how C<sub>c</sub> moves the poles</h1>
<p class="mu">Two-stage Miller amplifier. Drag the sliders and watch p<sub>1</sub> move toward the origin while p<sub>2</sub> moves away. Fixed: g<sub>m1</sub>=210 µS, R<sub>1</sub>=119.2 kΩ, R<sub>2</sub>=16.83 kΩ (worked example).</p>

<div class="c"><h2>Controls</h2>
<label>Miller capacitor C<sub>c</sub><b id="vC"></b></label><input id="C" type="range" min="0" max="3" step="0.01" value="2">
<label>g<sub>m6</sub> (second stage)<b id="vG"></b></label><input id="G" type="range" min="0.5" max="4" step="0.05" value="2.05">
<label>C<sub>2</sub> = C<sub>L</sub> (load)<b id="vL"></b></label><input id="L" type="range" min="1" max="10" step="0.1" value="5">
<label>C<sub>1</sub> (node-1 parasitic)<b id="vP"></b></label><input id="P" type="range" min="0.05" max="1" step="0.01" value="0.2">
<button onclick="set(0)">C<sub>c</sub> = 0 (uncompensated)</button><button onclick="set(2)">C<sub>c</sub> = 2 pF</button><button id="an" onclick="sweep()">▶ Sweep C<sub>c</sub></button>
</div>

<div class="c"><h2>Pole map (log frequency axis, Hz)</h2>
<svg id="pm" viewBox="0 0 600 130" role="img" aria-label="Pole locations"></svg>
<p class="mu">Grey rings: poles with C<sub>c</sub>=0 (1/R<sub>2</sub>C<sub>2</sub> and 1/R<sub>1</sub>C<sub>1</sub>). Coloured ×: poles now. ○ amber: RHP zero g<sub>m6</sub>/C<sub>c</sub>.</p></div>

<div class="g">
<div class="k"><span>|p<sub>1</sub>|/2π</span><b id="r1"></b></div>
<div class="k"><span>|p<sub>2</sub>|/2π</span><b id="r2"></b></div>
<div class="k"><span>separation p<sub>2</sub>/p<sub>1</sub></span><b id="r3"></b></div>
<div class="k"><span>C seen at node 1: C<sub>1</sub>+C<sub>c</sub>(1+g<sub>m6</sub>R<sub>2</sub>)</span><b id="r4"></b></div>
<div class="k"><span>unity-gain freq.</span><b id="r5"></b></div>
<div class="k"><span>phase margin</span><b id="r6"></b></div>
</div>

<div class="c"><h2>Bode plot</h2>
<svg id="bd" viewBox="0 0 600 330" role="img" aria-label="Magnitude and phase"></svg>
<p class="mu">Solid: current C<sub>c</sub>. Dashed grey: C<sub>c</sub>=0. With no C<sub>c</sub> the two poles are close and the phase reaches −180° before the gain falls to 0 dB. As C<sub>c</sub> grows the gain curve becomes single-pole-like around the unity-gain frequency.</p></div>

<div class="c"><h2>Why it happens</h2>
<p>Node 1 sees C<sub>c</sub> multiplied by the second-stage gain (Miller effect), so |p<sub>1</sub>| ≈ 1/[g<sub>m6</sub>R<sub>1</sub>R<sub>2</sub>C<sub>c</sub>] falls as 1/C<sub>c</sub>. At the output, C<sub>c</sub> adds to C<sub>2</sub> in parallel with a strong g<sub>m6</sub> feedback path, so |p<sub>2</sub>| ≈ g<sub>m6</sub>C<sub>c</sub>/[C<sub>1</sub>C<sub>2</sub>+C<sub>c</sub>(C<sub>1</sub>+C<sub>2</sub>)] rises toward g<sub>m6</sub>/(C<sub>1</sub>+C<sub>2</sub>). The price is the RHP zero g<sub>m6</sub>/C<sub>c</sub>, which A5's R<sub>z</sub> removes.</p></div>

<script>
const R1=119.2e3,R2=16.83e3,g1=210e-6,T=2*Math.PI,$=i=>document.getElementById(i);
const f=x=>x>=1e9?(x/1e9).toFixed(2)+' GHz':x>=1e6?(x/1e6).toFixed(2)+' MHz':x>=1e3?(x/1e3).toFixed(1)+' kHz':x.toFixed(0)+' Hz';
function poles(Cc,g6,C1,C2){const a=R1*R2*(C1*C2+Cc*(C1+C2)),b=R1*(C1+Cc)+R2*(C2+Cc)+g6*R1*R2*Cc,d=Math.sqrt(Math.max(b*b-4*a,0));return{p1:2/(b+d),p2:(b+d)/(2*a),z:Cc>0?g6/Cc:Infinity}}
function mag(w,q,A0){return A0*Math.sqrt(1+(w/q.z)**2)/Math.sqrt((1+(w/q.p1)**2)*(1+(w/q.p2)**2))}
function ph(w,q){return -(Math.atan(w/q.z)+Math.atan(w/q.p1)+Math.atan(w/q.p2))*180/Math.PI}
function ugf(q,A0){let w=1e3*T;while(w<1e12*T&&mag(w,q,A0)>1)w*=1.01;return w}
const X=(h,x0,x1,l0,l1)=>x0+(Math.log10(h)-l0)/(l1-l0)*(x1-x0);
function render(){
const Cc=+$('C').value*1e-12,g6=+$('G').value*1e-3,C2=+$('L').value*1e-12,C1=+$('P').value*1e-12,A0=g1*R1*g6*R2;
$('vC').textContent=(Cc*1e12).toFixed(2)+' pF';$('vG').textContent=(g6*1e3).toFixed(2)+' mS';$('vL').textContent=(C2*1e12).toFixed(1)+' pF';$('vP').textContent=(C1*1e12).toFixed(2)+' pF';
const q=poles(Cc,g6,C1,C2),q0=poles(0,g6,C1,C2),h1=q.p1/T,h2=q.p2/T,hz=q.z/T;
$('r1').textContent=f(h1);$('r2').textContent=f(h2);$('r3').textContent=(h2/h1).toFixed(0)+'×';
$('r4').textContent=((C1+Cc*(1+g6*R2))*1e12).toFixed(1)+' pF';
const wu=ugf(q,A0),pm=180+ph(wu,q);$('r5').textContent=f(wu/T);$('r6').textContent=pm.toFixed(1)+'°';$('r6').style.color=pm>=60?'#7be0a0':pm>=45?'var(--zr)':'var(--p2)';
// pole map
let s='';const lo=3,hi=9,xx=h=>X(Math.min(Math.max(h,10**lo),10**hi),30,580,lo,hi),y=70;
s+='<path d="M30 70H580" stroke="var(--mu)"/>';
for(let k=lo;k<=hi;k++)s+=`<path d="M${xx(10**k)} 66V74" stroke="var(--mu)"/><text class="m" x="${xx(10**k)}" y="92" text-anchor="middle">1e${k}</text>`;
const gh=[q0.p1/T,q0.p2/T];gh.forEach(h=>s+=`<circle cx="${xx(h)}" cy="${y}" r="6" fill="none" stroke="var(--gh)" stroke-width="1.5"/>`);
[[gh[0],h1,'--p1','p1'],[gh[1],h2,'--p2','p2']].forEach(([a,b,c,n],i)=>{s+=`<path d="M${xx(a)} ${y-14+i*0}L${xx(b)} ${y-14}" stroke="var(${c})" stroke-dasharray="3 3"/><text x="${xx(b)}" y="${y+4}" text-anchor="middle" fill="var(${c})" font-size="16" font-weight="700">×</text><text class="t" x="${xx(b)}" y="${y-22}" text-anchor="middle" fill="var(${c})">${n}</text>`});
if(isFinite(hz)){s+=`<circle cx="${xx(hz)}" cy="${y}" r="5" fill="none" stroke="var(--zr)" stroke-width="2"/><text class="t" x="${xx(hz)}" y="${y-22}" text-anchor="middle" fill="var(--zr)">z${hz>1e9?' →':''}</text>`}
s+='<text class="m" x="30" y="118">← toward origin (low f)          away from origin (high f) →</text>';
$('pm').innerHTML=s;
// bode
const l0=3,l1=10.5,xb=h=>X(h,40,590,l0,l1),ym=m=>20+(70-m)/110*130,yp=p=>200-(p/(-270))*0+ (0);
let b='';for(let k=3;k<=10;k++)b+=`<path d="M${xb(10**k)} 20V280" stroke="var(--ln)"/><text class="m" x="${xb(10**k)}" y="296" text-anchor="middle">1e${k}</text>`;
for(let m=-40;m<=60;m+=20)b+=`<path d="M40 ${ym(m)}H590" stroke="var(--ln)"/><text class="m" x="36" y="${ym(m)+3}" text-anchor="end">${m}</text>`;
const yq=p=>205+(-p)/270*75;
[0,-90,-180,-270].forEach(p=>b+=`<path d="M40 ${yq(p)}H590" stroke="var(--ln)"/><text class="m" x="36" y="${yq(p)+3}" text-anchor="end">${p}</text>`);
function path(fn,qq,cls,dash){let d='';for(let i=0;i<=240;i++){const h=10**(l0+(l1-l0)*i/240),w=h*T,v=fn(w,qq);d+=(i?'L':'M')+xb(h).toFixed(1)+' '+v.toFixed(1)}return `<path d="${d}" fill="none" stroke="${cls}" stroke-width="2" ${dash?'stroke-dasharray="5 4" opacity=".7"':''}/>`}
const M=(w,qq)=>Math.min(Math.max(ym(20*Math.log10(mag(w,qq,A0))),20),130),Pp=(w,qq)=>Math.min(Math.max(yq(ph(w,qq)),205),285);
b+=path(M,q0,'var(--gh)',1)+path(Pp,q0,'var(--gh)',1)+path(M,q,'var(--p1)')+path(Pp,q,'var(--p2)');
b+=`<path d="M${xb(wu/T)} 20V280" stroke="var(--zr)" stroke-dasharray="2 3"/><text class="t" x="${Math.min(xb(wu/T)+4,520)}" y="32" fill="var(--zr)">UGF</text><path d="M40 ${ym(0)}H590" stroke="var(--mu)"/>`;
b+='<text class="m" x="44" y="16">gain (dB)</text><text class="m" x="44" y="200">phase (deg)</text><text class="m" x="590" y="310" text-anchor="end">frequency (Hz)</text>';
$('bd').innerHTML=b}
['C','G','L','P'].forEach(i=>$(i).addEventListener('input',render));
function set(v){$('C').value=v;render()}
let tm=null;function sweep(){if(tm){clearInterval(tm);tm=null;$('an').textContent='▶ Sweep Cc';return}$('an').textContent='⏸ Stop';let v=0,dir=1;tm=setInterval(()=>{v+=dir*.03;if(v>=3){v=3;dir=-1}if(v<=0){v=0;dir=1}$('C').value=v;render()},40)}
render();
</script></main></body></html>
