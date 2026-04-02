# Quiz Template Reference

This is the complete HTML/CSS/JS template for building interactive quizzes. Copy and adapt it for each quiz, replacing the questions data array with generated content.

## Questions Data Format

```javascript
const Q = [
  {
    q: "Your question text here?",
    opts: ["Option A", "Option B", "Option C", "Option D"],
    ans: [1],           // Array of correct indices. Single element = single choice
    multi: false,       // true for multi-select
    cat: "Category Name",
    fb: {
      0: "Feedback for option A — why it's right or wrong, addressing the specific misconception.",
      1: "Feedback for option B — tailored explanation.",
      2: "Feedback for option C — tailored explanation.",
      3: "Feedback for option D — tailored explanation."
    }
  },
  // Multi-select example:
  {
    q: "Which of the following are correct? (Select all that apply)",
    opts: ["Right 1", "Wrong 1", "Right 2", "Right 3"],
    ans: [0, 2, 3],
    multi: true,
    cat: "Category Name",
    fb: {
      0: "Correct — explanation of why this is a valid answer.",
      1: "Wrong — explanation of why this doesn't belong and what it actually is.",
      2: "Correct — explanation of why this is a valid answer.",
      3: "Correct — explanation of why this is a valid answer."
    }
  }
];
```

## Category Colors

Define as an object mapping category names to hex colors:

```javascript
const catCol = {
  "Category 1": "#534AB7",
  "Category 2": "#1D9E75",
  "Category 3": "#D85A30",
  "Category 4": "#378ADD",
  "Category 5": "#D4537E"
};
```

## Full HTML/CSS/JS Template

Below is the complete working template. Replace `Q`, `catCol`, and update the results message to match the quiz topic.

```html
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:var(--font-sans);color:var(--color-text-primary)}
.qc{max-width:600px;padding:1rem 0}
.prog{display:flex;gap:6px;margin-bottom:1.5rem}
.pd{height:4px;flex:1;border-radius:2px;background:var(--color-border-tertiary);transition:background .3s}
.pd.done{background:#534AB7}
.pd.act{background:#7F77DD}
.qn{font-size:13px;color:var(--color-text-secondary);margin-bottom:4px}
.tg{display:inline-block;margin-left:8px;font-size:11px;padding:2px 8px;border-radius:var(--border-radius-md);background:var(--color-background-secondary);font-weight:500}
.tt{display:inline-block;margin-left:6px;font-size:10px;padding:2px 6px;border-radius:var(--border-radius-md);font-weight:500;background:#EEEDFE;color:#534AB7}
.qq{font-size:17px;font-weight:500;margin-bottom:.5rem;line-height:1.5}
.hn{font-size:13px;color:var(--color-text-tertiary);margin-bottom:1rem;font-style:italic}
.op{width:100%;text-align:left;padding:12px 16px;margin-bottom:8px;border-radius:var(--border-radius-md);border:.5px solid var(--color-border-tertiary);background:var(--color-background-primary);font-size:15px;cursor:pointer;transition:all .15s;font-family:var(--font-sans);color:var(--color-text-primary);line-height:1.5;display:flex;align-items:center;gap:10px}
.op:hover:not(.lk){border-color:var(--color-border-secondary);background:var(--color-background-secondary)}
.op.sel{border-color:#534AB7;background:#EEEDFE;color:#3C3489}
.op.cor{border-color:#639922;background:#EAF3DE;color:#27500A}
.op.wrg{border-color:#A32D2D;background:#FCEBEB;color:#501313}
.op.rev{border-color:#639922;background:#EAF3DE;color:#27500A}
.op.mis{border-color:#BA7517!important;background:#FAC775!important;color:#412402!important}
.ml{font-size:11px;margin-left:auto;font-weight:500;color:#854F0B;white-space:nowrap}
.ci{width:18px;height:18px;border-radius:4px;border:1.5px solid var(--color-border-secondary);flex-shrink:0;display:flex;align-items:center;justify-content:center;font-size:12px}
.ri{width:18px;height:18px;border-radius:50%;border:1.5px solid var(--color-border-secondary);flex-shrink:0}
.op.sel .ci,.op.sel .ri{border-color:#534AB7;background:#534AB7}
.op.sel .ci::after{content:'\2713';color:#fff;font-size:12px}
.op.sel .ri::after{content:'';display:block;width:8px;height:8px;border-radius:50%;background:#fff;margin:auto}
.sb{margin-top:.75rem;padding:10px 24px;border-radius:var(--border-radius-md);border:.5px solid #534AB7;background:#534AB7;color:#fff;font-size:14px;font-weight:500;cursor:pointer;font-family:var(--font-sans);display:none}
.sb:hover{background:#3C3489}
.sb:active{transform:scale(.98)}
.fb{margin-top:12px;padding:12px 16px;border-radius:var(--border-radius-md);font-size:14px;line-height:1.7;margin-bottom:8px}
.fb-w{background:#FCEBEB;border:.5px solid #F09595;color:#501313}
.fb-c{background:#EAF3DE;border:.5px solid #97C459;color:#27500A}
.fb-m{background:#FAEEDA;border:.5px solid #FAC775;color:#412402}
.fb strong{font-weight:500}
.nb{margin-top:1rem;padding:10px 24px;border-radius:var(--border-radius-md);border:.5px solid var(--color-border-secondary);background:transparent;font-size:14px;font-weight:500;cursor:pointer;font-family:var(--font-sans);color:var(--color-text-primary);display:none}
.nb:hover{background:var(--color-background-secondary)}
.nb:active{transform:scale(.98)}
.res{text-align:center;padding:2rem 0}
.sc{font-size:48px;font-weight:500;margin-bottom:4px}
.slb{font-size:14px;color:var(--color-text-secondary);margin-bottom:1rem}
.rbr{height:8px;border-radius:4px;background:var(--color-border-tertiary);margin-bottom:1.5rem;overflow:hidden}
.rfl{height:100%;border-radius:4px;background:#534AB7;transition:width .6s ease}
.bd{text-align:left;margin-bottom:1.5rem;padding:1rem;border-radius:var(--border-radius-lg);background:var(--color-background-secondary)}
.bdt{font-size:13px;color:var(--color-text-secondary);margin-bottom:8px}
.bdr{display:flex;justify-content:space-between;align-items:center;font-size:14px;padding:4px 0}
.bdd{width:8px;height:8px;border-radius:50%;display:inline-block;margin-right:8px}
.rtb{padding:10px 24px;border-radius:var(--border-radius-md);border:.5px solid var(--color-border-secondary);background:transparent;font-size:14px;font-weight:500;cursor:pointer;font-family:var(--font-sans);color:var(--color-text-primary)}
.rtb:hover{background:var(--color-background-secondary)}
</style>
<div class="qc" id="quiz"></div>
<script>
// === REPLACE THIS WITH GENERATED QUESTIONS ===
const Q = [/* question objects here */];
const catCol = {/* category color mapping here */};

// === QUIZ ENGINE (do not modify) ===
let cur=0,score=0,sel=[];
let catS={},catT={};
Q.forEach(q=>{catS[q.cat]=0;catT[q.cat]=(catT[q.cat]||0)+1});
const box=document.getElementById('quiz');

function render(){
  if(cur>=Q.length){showResults();return}
  const q=Q[cur];sel=[];
  const col=catCol[q.cat]||'#888';
  box.innerHTML=`
    <div class="prog">${Q.map((_,i)=>`<div class="pd ${i<cur?'done':i===cur?'act':''}"></div>`).join('')}</div>
    <div class="qn">Question ${cur+1} of ${Q.length}<span class="tg" style="color:${col};">${q.cat}</span><span class="tt">${q.multi?'Select all that apply':'Single answer'}</span></div>
    <div class="qq">${q.q}</div>
    ${q.multi?'<div class="hn">Click all correct answers, then submit</div>':''}
    <div id="opts">${q.opts.map((o,i)=>`<button class="op" data-i="${i}">${q.multi?'<span class="ci"></span>':'<span class="ri"></span>'}<span>${o}</span></button>`).join('')}</div>
    ${q.multi?'<button class="sb" id="subB">Submit answers</button>':''}
    <div id="feedback"></div>
    <button class="nb" id="nxt">${cur===Q.length-1?'See results':'Next question'}</button>`;
  const btns=document.querySelectorAll('.op');
  if(q.multi){
    btns.forEach(b=>b.addEventListener('click',()=>{
      if(b.classList.contains('lk'))return;
      const i=parseInt(b.dataset.i);
      if(sel.includes(i)){sel=sel.filter(s=>s!==i);b.classList.remove('sel')}
      else{sel.push(i);b.classList.add('sel')}
      document.getElementById('subB').style.display=sel.length>0?'inline-block':'none';
    }));
    document.getElementById('subB').addEventListener('click',()=>doMulti());
  } else {
    btns.forEach(b=>b.addEventListener('click',()=>doSingle(parseInt(b.dataset.i))));
  }
  document.getElementById('nxt').addEventListener('click',()=>{cur++;render()});
}

function doSingle(idx){
  const q=Q[cur];
  const btns=document.querySelectorAll('.op');
  btns.forEach(b=>b.classList.add('lk'));
  const right=idx===q.ans[0];
  const fe=document.getElementById('feedback');
  if(right){
    score++;catS[q.cat]++;
    btns[idx].classList.add('cor');
    fe.innerHTML=`<div class="fb fb-c"><strong>Correct!</strong> ${q.fb[idx]}</div>`;
  } else {
    btns[idx].classList.add('wrg');
    btns[q.ans[0]].classList.add('rev');
    fe.innerHTML=`<div class="fb fb-w"><strong>You picked: "${q.opts[idx]}"</strong><br>${q.fb[idx]}</div><div class="fb fb-c"><strong>Correct: "${q.opts[q.ans[0]]}"</strong><br>${q.fb[q.ans[0]]}</div>`;
  }
  document.getElementById('nxt').style.display='inline-block';
}

function doMulti(){
  const q=Q[cur];
  const btns=document.querySelectorAll('.op');
  btns.forEach(b=>{b.classList.add('lk');b.classList.remove('sel')});
  document.getElementById('subB').style.display='none';
  const cS=new Set(q.ans),sS=new Set(sel);
  const perfect=cS.size===sS.size&&[...cS].every(v=>sS.has(v));
  btns.forEach(b=>{
    const i=parseInt(b.dataset.i);
    if(cS.has(i)&&sS.has(i)) b.classList.add('cor');
    else if(!cS.has(i)&&sS.has(i)) b.classList.add('wrg');
    else if(cS.has(i)&&!sS.has(i)){
      b.classList.add('mis');
      const l=document.createElement('span');
      l.className='ml';
      l.textContent='Missed';
      b.appendChild(l);
    }
  });
  const fe=document.getElementById('feedback');
  if(perfect){
    score++;catS[q.cat]++;
    fe.innerHTML=`<div class="fb fb-c"><strong>Correct! You got them all.</strong><br>${q.ans.map(i=>q.fb[i]).join(' ')}</div>`;
  } else {
    let h='';
    const wp=sel.filter(i=>!cS.has(i));
    const mp=[...cS].filter(i=>!sS.has(i));
    const rp=sel.filter(i=>cS.has(i));
    wp.forEach(i=>{h+=`<div class="fb fb-w"><strong>Wrong pick: "${q.opts[i]}"</strong><br>${q.fb[i]}</div>`});
    mp.forEach(i=>{h+=`<div class="fb fb-m"><strong>You missed: "${q.opts[i]}"</strong><br>${q.fb[i]}</div>`});
    if(rp.length>0){h+=`<div class="fb fb-c"><strong>You correctly selected ${rp.length} of ${cS.size}:</strong> ${rp.map(i=>q.opts[i]).join(', ')}</div>`}
    fe.innerHTML=h;
  }
  document.getElementById('nxt').style.display='inline-block';
}

function showResults(){
  const pct=Math.round((score/Q.length)*100);
  // Customize these messages per quiz topic
  const msg=pct===100?'Perfect score!':pct>=80?'Great work!':pct>=60?'Good start — review what you missed.':'Worth revisiting the material.';
  let bh='';
  for(const c of Object.keys(catT)){
    const col=catCol[c]||'#888';
    bh+=`<div class="bdr"><span><span class="bdd" style="background:${col};"></span>${c}</span><span>${catS[c]}/${catT[c]}</span></div>`;
  }
  box.innerHTML=`<div class="res"><div class="sc">${score}/${Q.length}</div><div class="slb">${msg}</div><div class="rbr"><div class="rfl" id="fill"></div></div><div class="bd"><div class="bdt">Score by objective</div>${bh}</div><button class="rtb" id="retry">Try again</button></div>`;
  setTimeout(()=>{document.getElementById('fill').style.width=pct+'%'},50);
  document.getElementById('retry').addEventListener('click',()=>{cur=0;score=0;for(const k in catS)catS[k]=0;render()});
}

render();
</script>
```

## Checklist Before Generating

Before outputting the quiz artifact, verify:

- [ ] Every question has a `fb` object with explanations for ALL 4 options (keys "0" through "3")
- [ ] Wrong-answer feedback explains what that option actually is and why it doesn't fit
- [ ] Correct-answer feedback reinforces why it's right with additional context
- [ ] Multi-select questions have `multi: true` and `ans` contains multiple indices
- [ ] Categories cover all learning objectives provided by the user
- [ ] Category colors are assigned and distinct
- [ ] Results messages are customized to the quiz topic
- [ ] The hint text "Click all correct answers, then submit" appears for multi-select questions
- [ ] Gold missed highlighting uses `!important` overrides
- [ ] The "Missed" label span is appended dynamically to missed option buttons
