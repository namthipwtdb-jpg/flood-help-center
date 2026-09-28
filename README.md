from pathlib import Path
import zipfile

root = Path("/mnt/data/flood-help-center-full")
root.mkdir(parents=True, exist_ok=True)

index = """<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<title>ศูนย์แจ้งเหตุและช่วยเหลืออุทกภัย</title>
<link rel="preconnect" href="https://fonts.googleapis.com"><link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Thai:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<link rel="stylesheet" href="style.css">
</head>
<body>
<header><div class="wrap nav">
<a class="brand" href="#home"><span class="logo">≋</span><span><b>ศูนย์แจ้งเหตุ</b><small>และช่วยเหลืออุทกภัย</small></span></a>
<nav><a href="#report">แจ้งเหตุ</a><a href="#track">ติดตามสถานะ</a><a href="#dashboard">เจ้าหน้าที่</a><a href="#guide">คำแนะนำ</a></nav>
<a class="hotline" href="tel:1784">สายด่วน 1784</a>
</div></header>

<main id="home">
<section class="hero"><div class="wrap hero-grid"><div>
<div class="eyebrow">● ระบบรับแจ้งเหตุและประสานความช่วยเหลือ</div>
<h1>แจ้งเหตุวันนี้<br><span>เพื่อให้ความช่วยเหลือไปถึงเร็วขึ้น</span></h1>
<p>ระบบต้นแบบศูนย์แจ้งเหตุและช่วยเหลืออุทกภัย สำหรับรับแจ้งสถานการณ์ จุดเกิดเหตุ และความต้องการความช่วยเหลืออย่างเป็นระบบ</p>
<div class="actions"><a class="btn primary" href="#report">🚨 แจ้งเหตุขอความช่วยเหลือ</a><a class="btn light" href="#track">🔎 ติดตามสถานะ</a></div>
<div class="trust"><span>✓ รองรับ GPS</span><span>✓ รองรับมือถือ</span><span>✓ มีระบบติดตามเลขที่แจ้งเหตุ</span></div>
</div>
<div class="hero-card"><div class="card-title"><b>ภาพรวมระบบ</b><span>● ออนไลน์</span></div>
<div class="bigstat"><strong id="count">0</strong><span>รายการทดสอบ<br>ในอุปกรณ์นี้</span></div>
<div class="hot-grid"><div><b>191</b><small>เหตุด่วน</small></div><div><b>1669</b><small>การแพทย์</small></div><div><b>1784</b><small>ปภ.</small></div></div>
<p class="small">กรณีมีอันตรายต่อชีวิต ให้โทรสายด่วนโดยตรง</p></div>
</div></section>

<section class="section" id="report"><div class="wrap">
<div class="heading"><div><label>01 / REPORT</label><h2>แบบแจ้งเหตุและขอรับความช่วยเหลือ</h2><p>กรอกข้อมูลที่ทราบ ระบบจะสร้างเลขที่แจ้งเหตุให้อัตโนมัติ</p></div><span>* ข้อมูลจำเป็น</span></div>
<form id="reportForm" class="card">
<h3>ข้อมูลผู้แจ้ง</h3><div class="grid">
<label>ชื่อผู้แจ้ง *<input name="name" required placeholder="ชื่อ-นามสกุล"></label>
<label>เบอร์โทรศัพท์ *<input name="phone" required type="tel" placeholder="08x-xxx-xxxx"></label></div>
<h3>รายละเอียดเหตุการณ์</h3><div class="grid">
<label>ประเภทเหตุ *<select name="type" required><option value="">เลือกประเภทเหตุ</option><option>อพยพ / ติดค้าง</option><option>ผู้สูงอายุ / ผู้ป่วย / ผู้พิการ</option><option>อาหาร / น้ำดื่ม / ถุงยังชีพ</option><option>เรือ / รถ / การเดินทาง</option><option>บ้านเรือนเสียหาย</option><option>ถนน / สะพาน / เส้นทางถูกตัดขาด</option><option>ไฟฟ้า / ประปา / สาธารณูปโภค</option><option>แจ้งระดับน้ำ</option><option>อื่น ๆ</option></select></label>
<label>ระดับน้ำ<select name="water"><option>ไม่ทราบ</option><option>ต่ำกว่า 30 ซม.</option><option>30–50 ซม.</option><option>50–100 ซม.</option><option>มากกว่า 100 ซม.</option></select></label>
<label>จำนวนผู้ได้รับผลกระทบ<input name="people" type="number" min="1" placeholder="คน"></label>
<label>ความเร่งด่วน<select name="urgency"><option>ทั่วไป</option><option>เร่งด่วน</option><option>ฉุกเฉิน / มีผู้เสี่ยงอันตราย</option></select></label>
<label class="full">สถานที่ / ที่อยู่เกิดเหตุ *<textarea name="location" required rows="3" placeholder="บ้านเลขที่ หมู่ ตำบล อำเภอ หรือจุดสังเกต"></textarea></label>
</div>
<div class="gps"><div><b>📍 ตำแหน่งจุดเกิดเหตุ</b><small>สามารถใช้ GPS จากอุปกรณ์ได้</small></div><button type="button" class="btn outline" id="gps">ใช้ตำแหน่งปัจจุบัน</button>
<div class="grid"><input id="lat" name="lat" placeholder="ละติจูด" readonly><input id="lng" name="lng" placeholder="ลองจิจูด" readonly></div></div>
<h3>ข้อมูลเพิ่มเติม</h3><div class="grid"><label class="full">รายละเอียดเพิ่มเติม<textarea name="detail" rows="4" placeholder="สถานการณ์และสิ่งที่ต้องการให้ช่วยเหลือ"></textarea></label>
<label class="full">รูปภาพประกอบ <input type="file" accept="image/*"><small class="hint">ต้นแบบนี้ยังไม่อัปโหลดไฟล์ไปฐานข้อมูลกลาง</small></label></div>
<label class="consent"><input type="checkbox" required> ข้าพเจ้ายืนยันข้อมูลตามที่ทราบ และยินยอมให้ใช้ข้อมูลเพื่อประสานการช่วยเหลือ</label>
<button class="btn primary fullbtn">ส่งแจ้งเหตุและขอรับความช่วยเหลือ →</button>
</form></div></section>

<section class="section alt" id="track"><div class="wrap two">
<div><label>02 / TRACK</label><h2>ติดตามสถานะการแจ้งเหตุ</h2><p>กรอกเลขที่ได้รับหลังส่งแบบฟอร์ม</p>
<form id="trackForm" class="track"><input id="trackId" placeholder="เช่น FH-2026-12345" required><button class="btn primary">ตรวจสอบ</button></form><div id="result"></div></div>
<div class="steps"><div><b>01</b><span><strong>รับแจ้ง</strong><small>ระบบบันทึกข้อมูล</small></span></div><div><b>02</b><span><strong>ตรวจสอบ</strong><small>เจ้าหน้าที่ตรวจสอบ</small></span></div><div><b>03</b><span><strong>กำลังช่วยเหลือ</strong><small>ประสานทีมปฏิบัติการ</small></span></div><div><b>04</b><span><strong>ปิดเรื่อง</strong><small>บันทึกผลการช่วยเหลือ</small></span></div></div>
</div></section>

<section class="section" id="dashboard"><div class="wrap">
<div class="heading"><div><label>03 / OFFICER</label><h2>Dashboard เจ้าหน้าที่</h2><p>หน้าจอต้นแบบสำหรับพัฒนาระบบหลังบ้านต่อ</p></div><button class="btn outline" id="demoBtn">โหลดข้อมูลตัวอย่าง</button></div>
<div class="stats"><div><span>ทั้งหมด</span><b id="all">0</b></div><div><span>ฉุกเฉิน</span><b id="urgent">0</b></div><div><span>กำลังดำเนินการ</span><b id="doing">0</b></div><div><span>ปิดเรื่อง</span><b id="closed">0</b></div></div>
<div class="card tablewrap"><table><thead><tr><th>เลขที่</th><th>ประเภท</th><th>ความเร่งด่วน</th><th>สถานที่</th><th>สถานะ</th></tr></thead><tbody id="table"></tbody></table></div>
</div></section>

<section class="section alt" id="guide"><div class="wrap"><div class="heading"><div><label>04 / SAFETY</label><h2>คำแนะนำเมื่อเกิดอุทกภัย</h2></div></div>
<div class="guides"><article>🚨<h3>อันตรายต่อชีวิต</h3><p>โทร 191 หรือ 1669 และแจ้งจุดเกิดเหตุให้ชัดเจน</p></article><article>🔌<h3>ระวังไฟฟ้า</h3><p>หลีกเลี่ยงน้ำท่วมใกล้สายไฟ เสาไฟ และอุปกรณ์ไฟฟ้า</p></article><article>🎒<h3>เตรียมของจำเป็น</h3><p>เอกสาร ยาประจำตัว โทรศัพท์ แบตเตอรี่สำรอง น้ำและอาหาร</p></article></div></div></section>
</main>
<footer><div class="wrap footer"><div><b>ศูนย์แจ้งเหตุและช่วยเหลืออุทกภัย</b><small>ระบบต้นแบบสำหรับนำเสนอและพัฒนาต่อ</small></div><div><a href="tel:191">191</a><a href="tel:1669">1669</a><a href="tel:1784">1784</a></div></div></footer>
<div class="modal" id="modal"><div class="modalbox"><button id="close">×</button><div class="ok">✓</div><h2>รับแจ้งเหตุเรียบร้อยแล้ว</h2><p>เลขที่แจ้งเหตุของคุณคือ</p><strong id="newId"></strong><p>กรุณาเก็บเลขนี้ไว้สำหรับติดตามสถานะ</p><button class="btn primary" id="toTrack">ไปติดตามสถานะ</button></div></div>
<script src="script.js"></script>
</body></html>"""

css = """:root{--navy:#082f52;--blue:#0877bd;--cyan:#0aa7c9;--bg:#f4f8fb;--text:#17324a;--muted:#687f91;--line:#dce7ef;--white:#fff;--shadow:0 18px 50px rgba(10,50,80,.1)}*{box-sizing:border-box}html{scroll-behavior:smooth}body{margin:0;font-family:"Noto Sans Thai",sans-serif;color:var(--text);background:var(--bg);line-height:1.7}.wrap{width:min(1120px,92%);margin:auto}header{background:#fff;border-bottom:1px solid var(--line);position:sticky;top:0;z-index:10}.nav{height:72px;display:flex;align-items:center;gap:28px}.brand{display:flex;align-items:center;gap:11px;color:var(--navy);text-decoration:none;margin-right:auto}.brand small{display:block;color:var(--muted);font-size:11px;line-height:1}.logo{width:42px;height:42px;border-radius:12px;background:linear-gradient(135deg,var(--navy),var(--cyan));color:#fff;display:grid;place-items:center;font-size:25px}.nav nav{display:flex;gap:22px}.nav nav a{color:var(--text);text-decoration:none;font-size:14px}.hotline{background:#eaf5ff;color:var(--blue);padding:8px 15px;border-radius:9px;text-decoration:none;font-weight:700;font-size:14px}.hero{background:linear-gradient(120deg,#072b4b,#0b4e7c 60%,#087eaa);color:#fff;padding:80px 0}.hero-grid{display:grid;grid-template-columns:1.2fr .8fr;gap:65px;align-items:center}.eyebrow{font-size:13px;margin-bottom:20px}.hero h1{font-size:clamp(38px,5vw,62px);line-height:1.15;margin:0 0 20px}.hero h1 span{color:#aee8f7}.hero p{font-size:17px;color:#d8edf6;max-width:680px}.actions{display:flex;gap:12px;margin:28px 0}.btn{border:0;border-radius:10px;padding:12px 18px;font:inherit;font-weight:700;cursor:pointer;text-decoration:none;display:inline-flex;align-items:center;justify-content:center}.primary{background:linear-gradient(135deg,#08a5ce,#0872bd);color:#fff}.light{background:rgba(255,255,255,.08);border:1px solid rgba(255,255,255,.3);color:#fff}.outline{background:#fff;border:1px solid #9bc5dd;color:var(--blue)}.trust{display:flex;gap:20px;flex-wrap:wrap;font-size:12px;color:#c8e5f1}.hero-card,.card{background:#fff;border:1px solid var(--line);border-radius:18px;box-shadow:var(--shadow)}.hero-card{color:var(--text);padding:24px}.card-title{display:flex;justify-content:space-between;font-size:13px}.card-title span{color:#148b70}.bigstat{display:flex;gap:15px;align-items:center;padding:25px 0;border-bottom:1px solid var(--line)}.bigstat strong{font-size:52px;color:var(--blue)}.bigstat span{font-size:13px;color:var(--muted)}.hot-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;padding-top:18px}.hot-grid div{background:#f1f7fb;padding:10px;border-radius:9px}.hot-grid b{display:block;color:var(--navy);font-size:20px}.hot-grid small,.small{color:var(--muted);font-size:10px}.section{padding:70px 0}.alt{background:#eaf2f7}.heading{display:flex;justify-content:space-between;align-items:end;margin-bottom:25px}.heading label,.two>div>label{font-size:11px;letter-spacing:1.7px;color:var(--blue);font-weight:800}.heading h2,h2{font-size:31px;color:var(--navy);margin:4px 0}.heading p{color:var(--muted);margin:0}.card{padding:30px}.grid{display:grid;grid-template-columns:1fr 1fr;gap:17px}.grid .full{grid-column:1/-1}label{font-size:13px;font-weight:600}input,select,textarea{width:100%;margin-top:7px;border:1px solid #cddde7;border-radius:9px;padding:12px;font:inherit;background:#fbfdff;color:var(--text)}textarea{resize:vertical}.form-card h3{font-size:17px;color:var(--navy);margin:0 0 15px}.form-card h3:not(:first-child){margin-top:28px}.gps{background:#f2f9fc;border:1px solid #d5e8f1;border-radius:12px;padding:17px;margin-top:18px;display:grid;grid-template-columns:1fr auto;gap:12px}.gps small,.hint{display:block;color:var(--muted);font-size:11px;font-weight:400}.gps .grid{grid-column:1/-1}.consent{display:flex;gap:8px;color:var(--muted);font-size:12px;margin:20px 0}.consent input{width:auto;margin-top:5px}.fullbtn{width:100%}.two{display:grid;grid-template-columns:1fr 1fr;gap:80px}.track{display:flex;gap:10px;margin-top:20px}.track input{margin:0}.steps{display:grid;gap:12px}.steps>div{display:flex;gap:15px;align-items:center;background:#fff;border:1px solid var(--line);border-radius:12px;padding:14px}.steps b{width:38px;height:38px;border-radius:50%;background:#eaf5fb;color:var(--blue);display:grid;place-items:center}.steps strong,.steps small{display:block}.steps small{font-size:11px;color:var(--muted)}.status{margin-top:15px;padding:15px;background:#fff;border:1px solid var(--line);border-radius:10px}.status b{color:var(--blue)}.stats{display:grid;grid-template-columns:repeat(4,1fr);gap:15px;margin-bottom:18px}.stats div{background:#fff;border:1px solid var(--line);border-radius:13px;padding:18px}.stats span,.stats b{display:block}.stats span{font-size:12px;color:var(--muted)}.stats b{font-size:30px;color:var(--blue)}.tablewrap{padding:0;overflow:auto}table{width:100%;border-collapse:collapse;font-size:13px}th,td{text-align:left;padding:14px;border-bottom:1px solid var(--line)}th{background:#f4f8fb;color:var(--navy)}.guides{display:grid;grid-template-columns:repeat(3,1fr);gap:18px}.guides article{background:#fff;border:1px solid var(--line);border-radius:14px;padding:23px;font-size:25px}.guides h3{font-size:17px}.guides p{font-size:13px;color:var(--muted);margin:0}footer{background:#06263f;color:#c4d7e4;padding:30px 0}.footer{display:flex;justify-content:space-between;align-items:center}.footer b,.footer small{display:block}.footer small{font-size:11px;color:#89a6b8}.footer a{color:#fff;text-decoration:none;margin-left:9px;background:rgba(255,255,255,.08);padding:7px 13px;border-radius:8px}.modal{position:fixed;inset:0;background:rgba(3,25,42,.65);display:none;place-items:center;padding:20px;z-index:30}.modal.show{display:grid}.modalbox{position:relative;background:#fff;border-radius:18px;padding:35px;max-width:420px;width:100%;text-align:center}.modalbox>#close{position:absolute;right:14px;top:8px;border:0;background:none;font-size:28px;color:#789;cursor:pointer}.ok{width:58px;height:58px;border-radius:50%;background:#dff7ee;color:#11926d;font-size:34px;display:grid;place-items:center;margin:auto}.modalbox strong{display:block;color:var(--blue);background:#eef8fc;padding:12px;border-radius:9px;font-size:24px;margin:15px 0}.modalbox .btn{width:100%}@media(max-width:800px){.nav nav{display:none}.hero-grid,.two{grid-template-columns:1fr;gap:35px}.grid,.guides,.stats{grid-template-columns:1fr}.grid .full{grid-column:auto}.gps{grid-template-columns:1fr}.section{padding:55px 0}.heading{align-items:flex-start;flex-direction:column;gap:12px}.footer{flex-direction:column;align-items:flex-start;gap:18px}}"""

js = """const KEY="flood_help_full_v1";
const get=()=>JSON.parse(localStorage.getItem(KEY)||"[]");
const save=x=>localStorage.setItem(KEY,JSON.stringify(x));
function refresh(){
 const r=get(); document.getElementById("count").textContent=r.length;
 document.getElementById("all").textContent=r.length;
 document.getElementById("urgent").textContent=r.filter(x=>x.urgency.includes("ฉุกเฉิน")).length;
 document.getElementById("doing").textContent=r.filter(x=>x.status==="กำลังช่วยเหลือ").length;
 document.getElementById("closed").textContent=r.filter(x=>x.status==="ปิดเรื่อง").length;
 document.getElementById("table").innerHTML=r.map(x=>"<tr><td><b>"+x.id+"</b></td><td>"+x.type+"</td><td>"+x.urgency+"</td><td>"+x.location+"</td><td>"+x.status+"</td></tr>").join("");
}
refresh();

document.getElementById("gps").onclick=()=>{
 if(!navigator.geolocation){alert("อุปกรณ์นี้ไม่รองรับ GPS");return}
 const b=document.getElementById("gps");b.textContent="กำลังค้นหา…";
 navigator.geolocation.getCurrentPosition(p=>{
  document.getElementById("lat").value=p.coords.latitude.toFixed(6);
  document.getElementById("lng").value=p.coords.longitude.toFixed(6);
  b.textContent="✓ ได้ตำแหน่งแล้ว";
 },()=>{alert("กรุณาอนุญาต Location ในเบราว์เซอร์");b.textContent="ใช้ตำแหน่งปัจจุบัน"},{enableHighAccuracy:true,timeout:10000});
};

document.getElementById("reportForm").onsubmit=e=>{
 e.preventDefault(); const f=new FormData(e.target);
 const id="FH-"+new Date().getFullYear()+"-"+Math.floor(10000+Math.random()*90000);
 const r={id,name:f.get("name"),phone:f.get("phone"),type:f.get("type"),water:f.get("water"),people:f.get("people"),urgency:f.get("urgency"),location:f.get("location"),lat:f.get("lat"),lng:f.get("lng"),detail:f.get("detail"),status:"รับแจ้งเหตุแล้ว",createdAt:new Date().toISOString()};
 const a=get();a.unshift(r);save(a);refresh();
 document.getElementById("newId").textContent=id;document.getElementById("modal").classList.add("show");
 e.target.reset();document.getElementById("lat").value="";document.getElementById("lng").value="";document.getElementById("gps").textContent="ใช้ตำแหน่งปัจจุบัน";
};

document.getElementById("close").onclick=()=>document.getElementById("modal").classList.remove("show");
document.getElementById("toTrack").onclick=()=>{
 const id=document.getElementById("newId").textContent;document.getElementById("modal").classList.remove("show");
 document.getElementById("trackId").value=id;document.getElementById("track").scrollIntoView({behavior:"smooth"});
 setTimeout(()=>document.getElementById("trackForm").requestSubmit(),400);
};
document.getElementById("trackForm").onsubmit=e=>{
 e.preventDefault();const id=document.getElementById("trackId").value.trim().toUpperCase();const r=get().find(x=>x.id===id);const box=document.getElementById("result");
 if(!r){box.innerHTML="<div class='status'>ไม่พบเลขที่แจ้งเหตุนี้ในอุปกรณ์นี้ กรุณาตรวจสอบเลขอีกครั้ง</div>";return}
 const map=r.lat&&r.lng?"<br><a target='_blank' href='https://www.google.com/maps?q="+r.lat+","+r.lng+"'>เปิดตำแหน่งบน Google Maps ↗</a>":"";
 box.innerHTML="<div class='status'><b>"+r.id+"</b><br>สถานะ: "+r.status+"<br>ประเภทเหตุ: "+r.type+"<br>สถานที่: "+r.location+map+"</div>";
};
document.getElementById("demoBtn").onclick=()=>{
 const demo=[{id:"FH-2026-10001",name:"ข้อมูลตัวอย่าง",phone:"0800000000",type:"อพยพ / ติดค้าง",urgency:"ฉุกเฉิน / มีผู้เสี่ยงอันตราย",location:"หมู่บ้านตัวอย่าง",status:"กำลังช่วยเหลือ"},{id:"FH-2026-10002",name:"ข้อมูลตัวอย่าง",phone:"0811111111",type:"อาหาร / น้ำดื่ม / ถุงยังชีพ",urgency:"เร่งด่วน",location:"ตำบลตัวอย่าง",status:"รับแจ้งเหตุแล้ว"}];
 save(demo.concat(get()));refresh();
 alert("โหลดข้อมูลตัวอย่างแล้ว");
};
"""
