<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>مشروع نايف علي اللواتي</title>
    <style>
        body { margin: 0; background: #02050f; color: #00ff00; font-family: 'Courier New', Tahoma, sans-serif; overflow: hidden; height: 100vh; display: flex; flex-direction: column; position: relative; }
        .stars { position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -1; background-image: radial-gradient(2px 2px at 20px 30px, #eee, rgba(0,0,0,0)), radial-gradient(2px 2px at 40px 70px, #fff, rgba(0,0,0,0)); background-size: 300px 300px; animation: move-it 100s linear infinite; }
        @keyframes move-it { from { background-position: 0 0; } to { background-position: 1000px 1000px; } }
        
        .main-title { font-size: 16px; font-weight: bold; text-shadow: 2px 2px #000; display: block; margin-top: 15px; animation: rainbow-text 3s linear infinite, shake 0.5s ease-in-out infinite; }
        @keyframes rainbow-text { 0% { color: #ff0000; } 20% { color: #ffff00; } 40% { color: #00ff00; } 60% { color: #00ffff; } 80% { color: #0000ff; } 100% { color: #ff00ff; } }
        @keyframes shake { 0%, 100% { transform: translate(0,0); } 25% { transform: translate(1px, 1px); } 50% { transform: translate(-1px, -1px); } 75% { transform: translate(1px, -1px); } }

        .container { display: flex; flex-grow: 1; height: calc(100vh - 80px); }
        .sidebar { width: 115px; background: rgba(0,0,0,0.8); border-left: 2px solid #00ff00; display: flex; flex-direction: column; padding: 5px; gap: 5px; text-align: center; z-index: 10; }
        .sidebar button { background: #000; border: 2px outset #00ff00; color: #00ff00; padding: 8px 2px; font-size: 11px; cursor: pointer; font-weight: bold; width: 100%; }
        
        .content-area { flex-grow: 1; position: relative; display: flex; flex-direction: column; align-items: center; justify-content: flex-start; overflow: hidden; padding-top: 10px; }
        .section { display: none; width: 100%; height: 100%; flex-direction: column; align-items: center; justify-content: flex-start; }
        .active-section { display: flex; }

        /* الرسام المطور */
        .paint-tools { display: flex; flex-wrap: wrap; gap: 6px; justify-content: center; background: #000; padding: 8px; width: 95%; border: 1px solid #00ff00; margin-bottom: 5px; align-items: center; }
        .tool-btn { padding: 4px 6px; cursor: pointer; border: 2px outset #00ff00; color: #00ff00; background: #000; font-size: 10px; font-weight: bold; }
        .color-dot { width: 22px; height: 22px; border: 1px solid #fff; cursor: pointer; border-radius: 50%; }
        #paint-wrap { width: 95%; height: 50%; background: #fff; border: 2px solid #00ff00; cursor: crosshair; touch-action: none; position: relative; }
        canvas#canv { width: 100%; height: 100%; display: block; }
        .size-label { font-size: 10px; color: #fff; }

        /* باك مان */
        #pacman-canvas { background: #000; border: 2px solid #00f; }
        .pac-controls { display: grid; grid-template-columns: repeat(3, 50px); gap: 10px; margin-top: 10px; }
        .pac-btn { width: 50px; height: 50px; background: #000; color: #00ff00; border: 2px outset #00ff00; display: flex; align-items: center; justify-content: center; font-size: 20px; cursor: pointer; }

        #custom-alert { display: none; position: fixed; top: 50%; left: 50%; transform: translate(-50%, -50%); background: #000; border: 3px solid #f00; padding: 20px; z-index: 1000; text-align: center; }
        #custom-alert button { background: #333; color: #fff; border: 2px solid #fff; padding: 10px 20px; cursor: pointer; font-weight: bold; }

        .marquee-container { position: fixed; bottom: 0; width: 100%; background: #000080; color: #fff; white-space: nowrap; overflow: hidden; padding: 6px 0; border-top: 2px solid #00ff00; }
        .marquee-text { display: inline-block; padding-right: 100%; animation: marquee-anim 25s linear infinite; }
        @keyframes marquee-anim { 0% { transform: translateX(-100%); } 100% { transform: translateX(100%); } }
    </style>
</head>
<body>
    <div class="stars"></div>
    <div id="custom-alert"><button onclick="retryPacman()">إعادة محاولة</button></div>

    <div class="container">
        <div class="sidebar">
            <button onclick="show('home')">الرئيسية</button>
            <button onclick="show('q-sec')">لو خيروك</button>
            <button onclick="show('joke-sec')">النكت</button>
            <button onclick="show('pac-sec')">باك مان</button>
            <button onclick="show('paint-sec')">الرسام</button>
            <button onclick="show('forum-sec')">المنتدى</button>
            <div class="main-title">تطوير نايف</div>
        </div>

        <div class="content-area">
            <div id="home" class="section active-section"><h2>موقع جميل.. صح؟</h2><p>تم صناعة الموقع في 2024</p></div>
            
            <div id="q-sec" class="section">
                <div id="q-blue" class="choice-box" style="width:85%; padding:15px; background:#000080; border:3px double #00f; color:#fff; cursor:pointer;" onclick="nextQ()"></div>
                <div style="color:#fff">أو</div>
                <div id="q-red" class="choice-box" style="width:85%; padding:15px; background:#800000; border:3px double #f00; color:#fff; cursor:pointer;" onclick="nextQ()"></div>
            </div>

            <div id="joke-sec" class="section">
                <div id="joke-box" style="width:85%; padding:15px; border:2px dashed #ffff00; color:#ffff00; background:rgba(0,0,0,0.7); min-height:80px; text-align:center;">اضغط التالي لتضحك</div>
                <button onclick="nextJ()" style="background:#0f0; padding:8px 25px; margin-top:10px; font-weight:bold; cursor:pointer;">التالي</button>
            </div>

            <div id="pac-sec" class="section">
                <div style="color: yellow;">النقاط: <span id="score">0</span></div>
                <canvas id="pacman-canvas" width="240" height="240"></canvas>
                <div class="pac-controls">
                    <div></div><div class="pac-btn" onclick="setPacDir(0, -1)">↑</div><div></div>
                    <div class="pac-btn" onclick="setPacDir(-1, 0)">←</div><div class="pac-btn" onclick="setPacDir(0, 1)">↓</div><div class="pac-btn" onclick="setPacDir(1, 0)">→</div>
                </div>
            </div>

            <div id="paint-sec" class="section">
                <div class="paint-tools">
                    <button class="tool-btn" onclick="setTool('pen')">قلم</button>
                    <button class="tool-btn" onclick="setTool('eraser')">محاية</button>
                    <button class="tool-btn" onclick="setTool('fill')">سطل</button>
                    <button class="tool-btn" onclick="undo()">تراجع</button>
                    <span class="size-label">الحجم:</span>
                    <input type="range" id="size-slider" min="1" max="50" value="5" style="width:60px">
                    <div class="color-dot" style="background:red" onclick="setColor('red')"></div>
                    <div class="color-dot" style="background:yellow" onclick="setColor('yellow')"></div>
                    <div class="color-dot" style="background:blue" onclick="setColor('blue')"></div>
                    <div class="color-dot" style="background:green" onclick="setColor('green')"></div>
                    <div class="color-dot" style="background:black" onclick="setColor('black')"></div>
                    <button class="tool-btn" onclick="clearCanvas()">مسح</button>
                </div>
                <div id="paint-wrap"><canvas id="canv"></canvas></div>
            </div>

            <div id="forum-sec" class="section">
                <iframe src="https://www5.cbox.ws/box/?boxid=961537&boxtag=R2z6Bk" style="width:90%; height:40%;"></iframe>
            </div>
        </div>
    </div>

    <div class="marquee-container"><div class="marquee-text">صنعت الموقع هذا في 2024 و على اساس انه من 2004 او من ذيك الحقبة و انا صف خامس الأن أيضا مع مساعدة الذكاء الاصطناعي</div></div>

    <script>
        function show(id) {
            var sections = document.getElementsByClassName('section');
            for (var i = 0; i < sections.length; i++) { sections[i].classList.remove('active-section'); }
            document.getElementById(id).classList.add('active-section');
            if(id !== 'pac-sec') { stopPacman(); document.getElementById('custom-alert').style.display='none'; } else { startPacman(); }
            if(id === 'paint-sec') setTimeout(initCanv, 100);
        }

        const qs = [
            {a:"تاكل بصل ني كل صباح", b:"تشرب عصير ثوم كل ليلة"}, {a:"تعيش في بيت مسكون", b:"تنام في غابة كلها وحوش"},
            {a:"تكون مشهور ومكروه", b:"تكون مجهول ومحبوب"}, {a:"تفقد ذاكرتك تماماً", b:"تفقد قدرتك على النوم"},
            {a:"تعيش بدون جوال سنة", b:"تعيش بدون سيارة سنة"}, {a:"تكون خارق السرعة", b:"تكون خارق القوة"},
            {a:"تسمع نكت سخيفة للأبد", b:"ما تضحك أبداً طول حياتك"}, {a:"تلبس شتوي في الصيف", b:"تلبس صيفي في الشتاء"},
            {a:"تصحي الصبح تلاقي نفسك ملك", b:"تصحي تلاقي نفسك طفل"}, {a:"تاكل وجبتك المفضلة فقط", b:"تجرب أكلة جديدة كل يوم غصباً عنك"}
        ];
        let qI=0; function nextQ(){ document.getElementById('q-blue').innerText=qs[qI].a; document.getElementById('q-red').innerText=qs[qI].b; qI=(qI+1)%qs.length; } nextQ();

        const js = [
            "محشش سأل أخوه: ليش أنت أصلع؟ قال: من كثر التفكير. قال المحشش: وأنا ليش شعري كثيف؟ قال: من كثر ما تسأل!",
            "غبي في حصة علوم سأله المدرس: ايش فوائد الخشب؟ قال: الخشب منه نصنع الملاعق الخشبية عشان ما تجرح القدر!",
            "واحد بخيل بنى بيت وحط فيه بابين.. واحد للدخول وواحد عشان يطلع منه الضيوف بسرعة!",
            "محشش شاف لوحة 'ممنوع الوقوف' قام انسدح!",
            "غبي ضاع شاحنه، قال لمرته: خلاص ضاع الشاحن. قالت: دور زين. قال: لو كان إبرة كان لقيته!",
            "عجوز راحت للدكتور، قالها: عندك ضغط. قالت: والله ما عندي غير بنتين وهربوا مع أزواجهم!",
            "واحد سأل محشش: الساعة كم؟ قال: مدري بس الرقمين اللي فوق بعض طاحوا!",
            "محشش يقول لخويه: تصدق الدنيا هذي غريبة، الواحد يموت وما يدري متى يعزونه!",
            "واحد عصبي تزوج وحدة عصبية، جابوا ولد أول ما طلع قال للممرضة: وش تبين تناظرين؟!",
            "بخيل دخل الحمام وما طلع.. ليش؟ لأن مكتوب على الباب 'ادفع'!"
        ];
        let jI=0; function nextJ(){ document.getElementById('joke-box').innerText=js[jI]; jI=(jI+1)%js.length; } nextJ();

        // --- الرسام (إصلاح كامل) ---
        let canv=document.getElementById('canv'), ctx=canv.getContext('2d'), drawing=false, history=[], tool='pen', color='black';
        let sizeSlider = document.getElementById('size-slider');

        function initCanv(){
            let wrap = document.getElementById('paint-wrap');
            if(canv.width !== wrap.clientWidth){
                canv.width = wrap.clientWidth; canv.height = wrap.clientHeight;
                ctx.fillStyle="#fff"; ctx.fillRect(0,0,canv.width,canv.height);
                saveHistory();
            }
        }
        function saveHistory(){ history.push(canv.toDataURL()); if(history.length>20) history.shift(); }
        function undo(){ if(history.length>1){ history.pop(); let img=new Image(); img.src=history[history.length-1]; img.onload=()=>ctx.drawImage(img,0,0); } }
        function getPos(e){
            let r=canv.getBoundingClientRect();
            let x = (e.touches ? e.touches[0].clientX : e.clientX) - r.left;
            let y = (e.touches ? e.touches[0].clientY : e.clientY) - r.top;
            return {x:x, y:y};
        }
        function start(e){ drawing=true; ctx.beginPath(); let p=getPos(e); ctx.moveTo(p.x, p.y); }
        function move(e){ 
            if(!drawing) return; 
            let p=getPos(e); 
            ctx.strokeStyle = (tool==='eraser') ? '#fff' : color; 
            ctx.lineWidth = sizeSlider.value; 
            ctx.lineCap = 'round'; 
            ctx.lineTo(p.x, p.y); 
            ctx.stroke(); 
        }
        function stop(){ if(drawing){ drawing=false; saveHistory(); } }
        
        canv.addEventListener('mousedown', start); canv.addEventListener('mousemove', move); window.addEventListener('mouseup', stop);
        canv.addEventListener('touchstart', (e)=>{ e.preventDefault(); start(e); }); canv.addEventListener('touchmove', (e)=>{ e.preventDefault(); move(e); }); canv.addEventListener('touchend', stop);

        function setTool(t){ tool=t; if(t==='fill'){ ctx.fillStyle=color; ctx.fillRect(0,0,canv.width,canv.height); saveHistory(); } }
        function setColor(c){ color=c; tool='pen'; } function clearCanvas(){ ctx.fillStyle="#fff"; ctx.fillRect(0,0,canv.width,canv.height); saveHistory(); }

        // --- باك مان (ذكاء مطور) ---
        let pC=document.getElementById('pacman-canvas'), pCtx=pC.getContext('2d'), grid=20;
        let pac={x:20,y:20,dx:0,dy:0}, ghost={x:220,y:220}, dots=[], score=0, pacInt=null;
        let lastGhostMove = {x:0, y:0};

        function startPacman(){ if(pacInt)return; dots=[]; for(let i=20;i<240;i+=grid)for(let j=20;j<240;j+=grid)dots.push({x:i,y:j,a:true}); pacInt=setInterval(updatePac, 300); }
        function stopPacman(){ clearInterval(pacInt); pacInt=null; }
        function setPacDir(x,y){ pac.dx=x*grid; pac.dy=y*grid; }
        function retryPacman(){ document.getElementById('custom-alert').style.display='none'; score=0; pac={x:20,y:20,dx:0,dy:0}; ghost={x:220,y:220}; startPacman(); }

        function updatePac(){
            pac.x+=pac.dx; pac.y+=pac.dy;
            if(pac.x<20)pac.x=220; if(pac.x>220)pac.x=20; if(pac.y<20)pac.y=220; if(pac.y>220)pac.y=20;

            // ذكاء الشبح المطور: يمنع الشبح من الدوران حول نفسه ويجبره على المطاردة
            let ms = [{x:grid,y:0},{x:-grid,y:0},{x:0,y:grid},{x:0,y:-grid}];
            
            // فلترة الحركات عشان ما يرجع للخلف مباشرة إلا إذا علق
            let filteredMs = ms.filter(m => !(m.x === -lastGhostMove.x && m.y === -lastGhostMove.y));
            if(filteredMs.length === 0) filteredMs = ms;

            filteredMs.sort((a,b) => {
                let dA = Math.abs((ghost.x+a.x)-pac.x) + Math.abs((ghost.y+a.y)-pac.y);
                let dB = Math.abs((ghost.x+b.x)-pac.x) + Math.abs((ghost.y+b.y)-pac.y);
                return dA - dB;
            });

            let bestMove = filteredMs[0];
            ghost.x += bestMove.x; ghost.y += bestMove.y;
            lastGhostMove = bestMove;

            if(ghost.x<20)ghost.x=220; if(ghost.x>220)ghost.x=20; if(ghost.y<20)ghost.y=220; if(ghost.y>220)ghost.y=20;

            pCtx.fillStyle="black"; pCtx.fillRect(0,0,240,240);
            pCtx.fillStyle="pink"; dots.forEach(d=>{if(d.a){pCtx.beginPath(); pCtx.arc(d.x,d.y,2,0,7); pCtx.fill(); if(pac.x===d.x&&pac.y===d.y){d.a=false; score+=10; document.getElementById('score').innerText=score;}}});
            pCtx.fillStyle="yellow"; pCtx.beginPath(); pCtx.arc(pac.x,pac.y,8,0.2*Math.PI,1.8*Math.PI); pCtx.lineTo(pac.x,pac.y); pCtx.fill();
            pCtx.fillStyle="red"; pCtx.beginPath(); pCtx.arc(ghost.x,ghost.y-2,8,Math.PI,0); pCtx.fillRect(ghost.x-8,ghost.y-2,16,8); pCtx.fill();
            pCtx.fillStyle="white"; pCtx.beginPath(); pCtx.arc(ghost.x-3,ghost.y-3,2,0,7); pCtx.arc(ghost.x+3,ghost.y-3,2,0,7); pCtx.fill();
            if(pac.x===ghost.x&&pac.y===ghost.y){ stopPacman(); document.getElementById('custom-alert').style.display='block'; }
        }
    </script>
</body>
</html>
