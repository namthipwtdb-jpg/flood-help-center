from pathlib import Path
import zipfile, textwrap

out = Path("/mnt/data/flood-help-center")
out.mkdir(exist_ok=True)

index_html = r'''<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>ศูนย์แจ้งเหตุและช่วยเหลืออุทกภัย</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Thai:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<link rel="stylesheet" href="style.css">
</head>
<body>
<header class="topbar"><div class="container nav">
<a class="brand" href="#home"><span class="brand-mark">≋</span><span><b>ศูนย์แจ้งเหตุ</b><small>และช่วยเหลืออุทกภัย</small></span></a>
<nav><a href="#report">แจ้งเหตุ</a><a href="#status">ติดตามสถานะ</a><a href="#guide">คำแนะนำ</a></nav>
<a class="hotline" href="tel:1784">สายด่วน 1784</a>
</div></header>

<main id="home">
<section class="hero"><div class="container hero-grid"><div>
<div class="eyebrow"><span class="pulse"></span> ระบบรับแจ้งเหตุและประสานความช่วยเหลือ</div>
<h1>แจ้งเหตุวันนี้<br><span>เพื่อให้ความช่วยเหลือไปถึงเร็วขึ้น</span></h1>
<p>แจ้งจุดเกิดเหตุ ผู้ได้รับผลกระทบ และความต้องการความช่วยเหลือได้ในแบบฟอร์มเดียว พร้อมบันทึกเลขที่แจ้งเหตุเพื่อติดตามผล</p>
<div class="hero-actions"><a class="btn primary" href="#report">🚨 แจ้งเหตุขอความช่วยเหลือ</a><a class="btn ghost" href="#status">🔎 ตรวจสอบสถานะ</a></div>
<div class="trust"><span>✓ ใช้งานได้บนมือถือ</span><span>✓ รองรับการส่งตำแหน่ง</span><span>✓ ออกแบบสำหรับงานภาครัฐ</span></div>
</div>
<div class="hero-card"><div class="card-head"><span>สถานการณ์รับแจ้ง</span><span class="live"><i></i> ระบบออนไลน์</span></div>
<div class="stat"><strong id="reportCount">0</strong><span>รายการแจ้งเหตุ<br>ในอุปกรณ์นี้</span></div>
<div class="mini-grid"><div><b>191</b><small>เหตุด่วนเหตุร้าย</small></div><div><b>1669</b><small>การแพทย์ฉุกเฉิน</small></div><div><b>1784</b><small>ปภ.</small></div></div>
<p class="note">หากเป็นเหตุฉุกเฉินที่มีอันตรายต่อชีวิต ให้โทรสายด่วนโดยตรง</p></div></div></section>

<section class="section" id="report"><div class="container">
<div class="section-title"><div><span class="kicker">01 / REPORT</span><h2>แบบแจ้งเหตุและขอรับความช่วยเหลือ</h2><p>กรอกข้อมูลเท่าที่ทราบ ระบบจะสร้างเลขที่แจ้งเหตุให้โดยอัตโนมัติ</p></div><span class="required">* ข้อมูลที่จำเป็น</span></div>
<form id="reportForm" class="form-card">
<div class="form-section"><h3>ข้อมูลผู้แจ้ง</h3><div class="form-grid">
<label>ชื่อผู้แจ้ง *<input name="name" required placeholder="ระบุชื่อ-นามสกุล"></label>
<label>เบอร์โทรศัพท์ *<input name="phone" required type="tel" placeholder="เช่น 08x-xxx-xxxx"></label>
</div></div>
<div class="form-section"><h3>รายละเอียดเหตุการณ์</h3><div class="form-grid">
<label>ประเภทเหตุ *<select name="type" required><option value="">เลือกประเภทเหตุ</option><option>อพยพ / ติดค้าง</option><option>ผู้สูงอายุ / ผู้ป่วย / ผู้พิการ</option><option>อาหาร / น้ำดื่ม / ถุงยังชีพ</option><option>เรือ / รถ / การเดินทาง</option><option>บ้านเรือนเสียหาย</option><option>ถนน / สะพาน / เส้นทางถูกตัดขาด</option><option>ไฟฟ้า / ประปา / สาธารณูปโภค</option><option>แจ้งระดับน้ำ</option><option>อื่น ๆ</option></select></label>
<label>ระดับน้ำโดยประมาณ<select name="water"><option>ไม่ทราบ</option><option>ต่ำกว่า 30 ซม.</option><option>30–50 ซม.</option><option>50–100 ซม.</option><option>มากกว่า 100 ซม.</option></select></label>
<label>จำนวนผู้ได้รับผลกระทบ<input name="people" type="number" min="1" placeholder="คน"></label>
<label>ความเร่งด่วน<select name="urgency"><option>ทั่วไป</option><option>เร่งด่วน</option><option>ฉุกเฉิน / มีผู้เสี่ยงอันตราย</option></select></label>
<label class="full">สถานที่ / ที่อยู่เกิดเหตุ *<textarea name="location" required rows="3" placeholder="บ้านเลขที่ หมู่ ตำบล อำเภอ หรือจุดสังเกต"></textarea></label>
</div>
<div class="location-box"><div><b>📍 ตำแหน่งจุดเกิดเหตุ</b><small>กดปุ่มเพื่อใช้ตำแหน่ง GPS ของเครื่อง</small></div>
<button type="button" class="btn outline" id="gpsBtn">ใช้ตำแหน่งปัจจุบัน</button>
<div class="coords"><input id="lat" name="lat" placeholder="ละติจูด" readonly><input id="lng" name="lng" placeholder="ลองจิจูด" readonly></div></div>
</div>
<div class="form-section"><h3>ข้อมูลเพิ่มเติม</h3><div class="form-grid">
<label class="full">รายละเอียดเพิ่มเติม<textarea name="detail" rows="4" placeholder="อธิบายสถานการณ์ สิ่งที่ต้องการ หรือข้อมูลที่เป็นประโยชน์ต่อเจ้าหน้าที่"></textarea></label>
<label class="full">รูปภาพประกอบ <input name="photo" type="file" accept="image/*"><small class="hint">ต้นแบบนี้ยังไม่อัปโหลดรูปขึ้นฐานข้อมูลกลาง</small></label>
</div></div>
<label class="consent"><input type="checkbox" required> ข้าพเจ้ายืนยันว่าข้อมูลที่แจ้งเป็นข้อมูลตามที่ทราบ และยินยอมให้หน่วยงานใช้ข้อมูลเพื่อประสานและให้ความช่วยเหลือ</label>
<button class="btn primary submit" type="submit">ส่งแจ้งเหตุและขอรับความช่วยเหลือ →</button>
</form></div></section>

<section class="section alt" id="status"><div class="container two-col"><div>
<span class="kicker">02 / TRACK</span><h2>ติดตามสถานะการแจ้งเหตุ</h2><p>กรอกเลขที่แจ้งเหตุที่ได้รับหลังส่งแบบฟอร์ม</p>
<form id="statusForm" class="track-form"><input id="statusId" placeholder="เช่น FH-2026-12345" required><button class="btn primary">ตรวจสอบ</button></form>
<div id="statusResult"></div>
</div>
<div class="steps"><div><b>01</b><span><strong>รับแจ้ง</strong><small>ระบบบันทึกข้อมูล</small></span></div><div><b>02</b><span><strong>ตรวจสอบ</strong><small>เจ้าหน้าที่พิจารณาเหตุ</small></span></div><div><b>03</b><span><strong>ประสานความช่วยเหลือ</strong><small>ส่งต่อหน่วยงาน/ทีมปฏิบัติการ</small></span></div><div><b>04</b><span><strong>ปิดเหตุ</strong><small>บันทึกผลการช่วยเหลือ</small></span></div></div>
</div></section>

<section class="section" id="guide"><div class="container">
<div class="section-title"><div><span class="kicker">03 / SAFETY</span><h2>คำแนะนำเมื่อเกิดอุทกภัย</h2></div></div>
<div class="guide-grid"><article><span>🚨</span><h3>หากมีอันตรายต่อชีวิต</h3><p>โทร 191 หรือ 1669 ทันที และแจ้งจุดเกิดเหตุให้ชัดเจน</p></article>
<article><span>🔌</span><h3>ระวังกระแสไฟฟ้า</h3><p>หลีกเลี่ยงพื้นที่น้ำท่วมใกล้เสาไฟ สายไฟ หรืออุปกรณ์ไฟฟ้า</p></article>
<article><span>🎒</span><h3>เตรียมของจำเป็น</h3><p>เอกสารสำคัญ ยาประจำตัว โทรศัพท์ แบตเตอรี่สำรอง น้ำและอาหาร</p></article></div>
</div></section>
</main>

<footer><div class="container footer"><div><b>ศูนย์แจ้งเหตุและช่วยเหลืออุทกภัย</b><small>ต้นแบบระบบสำหรับนำเสนอและพัฒนาต่อ</small></div>
<div class="footer-links"><a href="tel:191">191</a><a href="tel:1669">1669</a><a href="tel:1784">1784</a></div></div></footer>

<div class="modal" id="successModal"><div class="modal-box"><button class="close" id="closeModal">×</button>
<div class="success-icon">✓</div><h2>รับแจ้งเหตุเรียบร้อยแล้ว</h2><p>กรุณาเก็บเลขที่แจ้งเหตุไว้สำหรับติดตามสถานะ</p>
<div class="incident-id" id="incidentId"></div><button class="btn primary" id="goStatus">ไปติดตามสถานะ</button></div></div>
<script src="script.js"></script>
</body></html>'''

style_css = r''':root{--navy:#092d4f;--blue:#0869b7;--cyan:#0ea5c9;--bg:#f4f8fb;--text:#17324a;--muted:#657b8e;--line:#dce7ef;--shadow:0 18px 50px rgba(10,50,80,.1)}*{box-sizing:border-box}html{scroll-behavior:smooth}body{margin:0;font-family:"Noto Sans Thai",sans-serif;color:var(--text);background:var(--bg);line-height:1.7}.container{width:min(1120px,92%);margin:auto}.topbar{background:#fff;border-bottom:1px solid var(--line);position:sticky;top:0;z-index:20}.nav{height:72px;display:flex;align-items:center;gap:28px}.brand{display:flex;align-items:center;gap:11px;text-decoration:none;color:var(--navy);margin-right:auto}.brand-mark{width:42px;height:42px;border-radius:12px;background:linear-gradient(135deg,var(--navy),var(--cyan));color:white;display:grid;place-items:center;font-size:25px;font-weight:800}.brand small{display:block;color:var(--muted);font-size:11px;line-height:1}.nav nav{display:flex;gap:24px}.nav nav a,.footer a{color:var(--text);text-decoration:none;font-size:14px}.hotline{background:#eaf5ff;color:var(--blue);padding:8px 15px;border-radius:10px;text-decoration:none;font-weight:700;font-size:14px}.hero{background:linear-gradient(120deg,#082b4b,#0b4e7c 60%,#087eaa);color:white;padding:82px 0 88px}.hero-grid{display:grid;grid-template-columns:1.25fr .75fr;gap:70px;align-items:center}.eyebrow{font-size:13px;margin-bottom:20px}.pulse{display:inline-block;width:8px;height:8px;background:#54e6c2;border-radius:50%;margin-right:7px}.hero h1{font-size:clamp(38px,5vw,62px);line-height:1.15;margin:0 0 22px;letter-spacing:-1.5px}.hero h1 span{color:#aee8f7}.hero h2,h2{font-size:32px;line-height:1.3;margin:5px 0 8px;color:var(--navy)}h3{margin:0 0 7px}.hero p{font-size:17px;max-width:670px;color:#d9edf7}.hero-actions{display:flex;gap:12px;margin:30px 0}.btn{border:0;border-radius:10px;padding:12px 19px;font-family:inherit;font-weight:700;cursor:pointer;text-decoration:none;display:inline-flex;align-items:center;justify-content:center;gap:7px}.primary{background:linear-gradient(135deg,#08a5ce,#0872bd);color:#fff;box-shadow:0 9px 25px rgba(0,112,180,.22)}.ghost{border:1px solid rgba(255,255,255,.3);color:white;background:rgba(255,255,255,.06)}.outline{background:white;border:1px solid #9bc5dd;color:var(--blue)}.trust{display:flex;gap:20px;flex-wrap:wrap;font-size:12px;color:#c8e5f1}.hero-card{background:#fff;color:var(--text);border-radius:18px;padding:24px;box-shadow:var(--shadow)}.card-head{display:flex;justify-content:space-between;font-size:13px;font-weight:700}.live{color:#16866f}.live i{display:inline-block;width:7px;height:7px;border-radius:50%;background:#24b78f;margin-right:5px}.stat{display:flex;align-items:center;gap:15px;padding:27px 0;border-bottom:1px solid var(--line)}.stat strong{font-size:52px;color:var(--blue);line-height:1}.stat span{font-size:13px;color:var(--muted)}.mini-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;padding-top:18px}.mini-grid div{background:#f1f7fb;border-radius:10px;padding:11px}.mini-grid b{display:block;color:var(--navy);font-size:20px}.mini-grid small{font-size:10px;color:var(--muted)}.note{font-size:11px;color:var(--muted);margin:15px 0 0}.section{padding:76px 0}.alt{background:#eaf2f7}.section-title{display:flex;justify-content:space-between;align-items:end;margin-bottom:27px}.kicker{font-size:11px;letter-spacing:1.8px;color:var(--blue);font-weight:800}.section-title p{color:var(--muted);margin:0}.required{font-size:12px;color:#b34d4d}.form-card{background:white;border:1px solid var(--line);border-radius:18px;box-shadow:var(--shadow);padding:30px}.form-section{padding:0 0 27px;margin-bottom:27px;border-bottom:1px solid var(--line)}.form-section h3{font-size:17px;color:var(--navy);margin-bottom:17px}.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:18px}.form-grid label{font-size:13px;font-weight:600}.form-grid .full{grid-column:1/-1}input,select,textarea{width:100%;margin-top:7px;border:1px solid #cfdee8;border-radius:9px;padding:12px 13px;font:inherit;color:var(--text);background:#fbfdff;outline:none}input:focus,select:focus,textarea:focus{border-color:#52a8cf;box-shadow:0 0 0 3px rgba(14,165,201,.1)}textarea{resize:vertical}.location-box{display:grid;grid-template-columns:1fr auto;gap:13px;align-items:center;background:#f3f9fc;border:1px solid #d5e8f1;border-radius:12px;padding:17px;margin-top:5px}.location-box small,.hint{display:block;color:var(--muted);font-weight:400;font-size:11px}.coords{grid-column:1/-1;display:grid;grid-template-columns:1fr 1fr;gap:10px}.consent{display:flex;gap:9px;align-items:flex-start;font-size:12px;color:var(--muted);margin:20px 0}.consent input{width:auto;margin-top:5px}.submit{width:100%;padding:14px}.two-col{display:grid;grid-template-columns:1fr 1fr;gap:80px;align-items:start}.track-form{display:flex;gap:10px;margin-top:22px}.track-form input{margin:0}.steps{display:grid;gap:12px}.steps>div{display:flex;gap:15px;align-items:center;background:#fff;border:1px solid var(--line);border-radius:12px;padding:15px}.steps b{width:38px;height:38px;border-radius:50%;background:#eaf5fb;color:var(--blue);display:grid;place-items:center;font-size:12px}.steps strong,.steps small{display:block}.steps small{color:var(--muted);font-size:11px}.guide-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:18px}.guide-grid article{background:#fff;border:1px solid var(--line);border-radius:14px;padding:24px}.guide-grid article>span{font-size:26px}.guide-grid p{color:var(--muted);font-size:13px;margin:0}.status-card{margin-top:16px;padding:15px;border-radius:10px;background:#fff;border:1px solid var(--line);font-size:13px}.status-card b{color:var(--blue)}footer{background:#06263f;color:#c4d7e4;padding:30px 0}.footer{display:flex;justify-content:space-between;align-items:center}.footer b,.footer small{display:block}.footer small{font-size:11px;color:#89a6b8}.footer-links{display:flex;gap:12px}.footer-links a{color:white;background:rgba(255,255,255,.08);padding:7px 14px;border-radius:8px}.modal{position:fixed;inset:0;background:rgba(3,25,42,.62);display:none;place-items:center;z-index:50;padding:20px}.modal.show{display:grid}.modal-box{background:#fff;border-radius:18px;padding:35px;max-width:430px;width:100%;text-align:center;position:relative}.close{position:absolute;right:15px;top:10px;border:0;background:none;font-size:28px;color:#718899;cursor:pointer}.success-icon{width:58px;height:58px;border-radius:50%;background:#dff7ee;color:#11926d;font-size:34px;display:grid;place-items:center;margin:auto}.incident-id{font-size:24px;font-weight:800;color:var(--blue);background:#eef8fc;padding:12px;border-radius:10px;margin:18px 0}.modal .btn{width:100%}@media(max-width:800px){.nav nav{display:none}.hero{padding:55px 0}.hero-grid,.two-col{grid-template-columns:1fr;gap:35px}.form-grid,.guide-grid{grid-template-columns:1fr}.form-grid .full{grid-column:auto}.section{padding:55px 0}.section-title{align-items:start;gap:10px;flex-direction:column}.location-box{grid-template-columns:1fr}.coords{grid-template-columns:1fr}.footer{gap:20px;align-items:flex-start;flex-direction:column}}'''

script_js = r'''const KEY="flood_help_reports_v1";
const getReports=()=>JSON.parse(localStorage.getItem(KEY)||"[]");
const saveReports=r=>localStorage.setItem(KEY,JSON.stringify(r));
const countEl=document.getElementById("reportCount");
function refreshCount(){countEl.textContent=getReports().length}
refreshCount();

document.getElementById("gpsBtn").addEventListener("click",function(){
 if(!navigator.geolocation){alert("อุปกรณ์นี้ไม่รองรับการระบุตำแหน่ง");return}
 var b=document.getElementById("gpsBtn");b.textContent="กำลังค้นหาตำแหน่ง…";
 navigator.geolocation.getCurrentPosition(function(p){
  document.getElementById("lat").value=p.coords.latitude.toFixed(6);
  document.getElementById("lng").value=p.coords.longitude.toFixed(6);
  b.textContent="✓ ได้ตำแหน่งแล้ว";
 },function(){
  alert("ไม่สามารถเข้าถึงตำแหน่งได้ กรุณาอนุญาต Location ในเบราว์เซอร์");
  b.textContent="ใช้ตำแหน่งปัจจุบัน";
 },{enableHighAccuracy:true,timeout:10000});
});

document.getElementById("reportForm").addEventListener("submit",function(e){
 e.preventDefault();
 var f=new FormData(e.target);
 var id="FH-"+new Date().getFullYear()+"-"+Math.floor(10000+Math.random()*90000);
 var report={id:id,name:f.get("name"),phone:f.get("phone"),type:f.get("type"),water:f.get("water"),people:f.get("people"),urgency:f.get("urgency"),location:f.get("location"),lat:f.get("lat"),lng:f.get("lng"),detail:f.get("detail"),status:"รับแจ้งเหตุแล้ว",createdAt:new Date().toISOString()};
 var reports=getReports();reports.unshift(report);saveReports(reports);refreshCount();
 document.getElementById("incidentId").textContent=id;
 document.getElementById("successModal").classList.add("show");
 e.target.reset();document.getElementById("lat").value="";document.getElementById("lng").value="";
 document.getElementById("gpsBtn").textContent="ใช้ตำแหน่งปัจจุบัน";
});

document.getElementById("closeModal").onclick=function(){
 document.getElementById("successModal").classList.remove("show");
};
document.getElementById("goStatus").onclick=function(){
 var id=document.getElementById("incidentId").textContent;
 document.getElementById("successModal").classList.remove("show");
 document.getElementById("statusId").value=id;
 document.getElementById("status").scrollIntoView({behavior:"smooth"});
 setTimeout(function(){document.getElementById("statusForm").dispatchEvent(new Event("submit",{cancelable:true}))},400);
};

document.getElementById("statusForm").addEventListener("submit",function(e){
 e.preventDefault();
 var id=document.getElementById("statusId").value.trim().toUpperCase();
 var r=getReports().find(function(x){return x.id===id});
 var box=document.getElementById("statusResult");
 if(!r){box.innerHTML="<div class='status-card'>ไม่พบข้อมูลเลขที่แจ้งเหตุนี้ในอุปกรณ์นี้ กรุณาตรวจสอบเลขอีกครั้ง</div>";return}
 var map=r.lat&&r.lng?"<br><a href='https://www.google.com/maps?q="+r.lat+","+r.lng+"' target='_blank' rel='noopener'>เปิดตำแหน่งบน Google Maps ↗</a>":"";
 box.innerHTML="<div class='status-card'><b>"+r.id+"</b><br>สถานะ: "+r.status+"<br>ประเภทเหตุ: "+r.type+"<br>สถานที่: "+r.location+map+"</div>";
});
window.addEventListener("click",function(e){
 if(e.target.id==="successModal")document.getElementById("successModal").classList.remove("show");
});'''

(out/"index.html").write_text(index_html, encoding="utf-8")
(out/"style.css").write_text(style_css, encoding="utf-8")
(out/"script.js").write_text(script_js, encoding="utf-8")
(out/"README.txt").write_text(
"""ศูนย์แจ้งเหตุและช่วยเหลืออุทกภัย - เว็บต้นแบบ

วิธีเปิด:
1. แตกไฟล์ ZIP
2. ดับเบิลคลิก index.html
3. เว็บจะเปิดใน Chrome

หมายเหตุ:
เว็บต้นแบบนี้เก็บข้อมูลการทดสอบไว้ใน Local Storage ของเบราว์เซอร์เครื่องนั้น
ยังไม่ใช่ฐานข้อมูลกลางสำหรับใช้งานจริงกับประชาชนจำนวนมาก

ไฟล์:
- index.html = หน้าเว็บ
- style.css = รูปแบบและหน้าตา
- script.js = การทำงานของแบบฟอร์ม
"""
, encoding="utf-8")

zip_path = Path("/mnt/data/flood-help-center.zip")
with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as z:
    for p in out.iterdir():
        z.write(p, arcname=p.name)

print(f"สร้างไฟล์สำเร็จ: {zip_path}")
print("ภายในมี index.html, style.css, script.js และ README.txt")
