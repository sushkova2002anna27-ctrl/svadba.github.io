const mailto = mailto:${RSVP_EMAIL}?subject=${subject}&body=${body};
  window.location.href = mailto;

  // также сохраняем локально как дубликат
  const submissions = JSON.parse(localStorage.getItem('rsvp_submissions') || '[]');
  submissions.push({name,email,phone,attend,note,timestamp:new Date().toISOString()});
  localStorage.setItem('rsvp_submissions', JSON.stringify(submissions));
  showMsg('Спасибо! Открылся ваш почтовый клиент для подтверждения (если не открылся, проверьте блокировку pop-up).', false);
  return false;
}

function showMsg(text, isError){
  const el = document.getElementById('rsvp-msg');
  el.textContent = text;
  el.style.color = isError ? '#c0392b' : 'var(--muted)';
  setTimeout(()=>{ el.textContent = ''; }, 7000);
}

/* Save draft to localStorage */
function saveDraft(){
  const draft = {
    name: document.getElementById('name').value,
    email: document.getElementById('email').value,
    phone: document.getElementById('phone').value,
    attend: document.getElementById('attend').value,
    note: document.getElementById('note').value,
    savedAt: new Date().toISOString()
  };
  localStorage.setItem('rsvp_draft', JSON.stringify(draft));
  showMsg('Черновик сохранён в браузере', false);
}

/* Load draft automatically */
(function loadDraft(){
  try{
    const d = JSON.parse(localStorage.getItem('rsvp_draft'));
    if(d){
      if(d.name) document.getElementById('name').value = d.name;
      if(d.email) document.getElementById('email').value = d.email;
      if(d.phone) document.getElementById('phone').value = d.phone;
      if(d.attend) document.getElementById('attend').value = d.attend;
      if(d.note) document.getElementById('note').value = d.note;
    }
  }catch(e){}
})();

/* Accessibility: focus outline for keyboard users */
document.body.addEventListener('keydown', function(e){
  if(e.key === 'Tab') document.body.classList.add('show-focus');
});
</script>
</body>
</html>
<!doctype html>
<html lang="ru">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Приглашение на свадьбу — Анна &amp; Денис</title>

<!-- Open Graph -->
<meta property="og:title" content="Анна &amp; Денис — Приглашение на свадьбу" />
<meta property="og:description" content="Мы женимся! 15 ноября 2025 • Прованс hall, г. Архангельск" />
<meta property="og:image" content="https://via.placeholder.com/1200x630.png?text=Anna+%26+Denis+Wedding" />
<meta property="og:type" content="website" />

<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700&family=Inter:wght@300;400;600&display=swap" rel="stylesheet">

<style>
:root{
  --accent:#b76e79;
  --muted:#6b6b6b;
  --bg:#faf8f6;
  --card:#ffffff;
  --maxw:1000px;
  --shadow: 0 6px 30px rgba(20,20,30,0.06);
  font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
  color:#222;
}
*{box-sizing:border-box}
body{
  margin:0;
  background:linear-gradient(180deg, var(--bg), #fff 60%);
  -webkit-font-smoothing:antialiased;
  padding:30px 16px 80px;
  display:flex;
  justify-content:center;
}
.container{
  width:100%;
  max-width:var(--maxw);
  background:transparent;
}

/* Card */
.card{
  background:var(--card);
  border-radius:14px;
  padding:28px;
  box-shadow:var(--shadow);
  margin-bottom:20px;
}

/* Header */
.header{
  text-align:center;
  padding:28px 12px 10px;
}
.title{
  font-family: 'Playfair Display', serif;
  font-size:40px;
  margin:6px 0 4px;
  color:#2a2a2a;
}
.subtitle{
  color:var(--muted);
  font-size:15px;
  margin-bottom:12px;
}

/* Hero */
.hero{
  display:flex;
  gap:20px;
  align-items:center;
  justify-content:space-between;
  flex-wrap:wrap;
}
.hero-left{
  flex:1 1 340px;
}
.hero-right{
  flex:0 0 280px;
  text-align:center;
}
.badge{
  display:inline-block;
  background:linear-gradient(90deg, rgba(183,110,121,0.14), rgba(183,110,121,0.06));
  color:var(--accent);
  font-weight:600;
  padding:8px 12px;
  border-radius:999px;
  font-size:13px;
}
.info{
  margin-top:12px;
  color:var(--muted);
  font-size:15px;
}

/* Countdown */
.countdown{
  display:flex;
  gap:10px;
  margin-top:18px;
  justify-content:flex-start;
  flex-wrap:wrap;
}
.countdown .unit{
  background:#fff;
  border-radius:10px;
  padding:10px 12px;
  text-align:center;
  min-width:70px;
  box-shadow: 0 3px 14px rgba(30,30,40,0.04);
}
.unit .num{font-weight:700; font-size:18px;}
.unit .lbl{font-size:12px; color:var(--muted)}

/* Sections */
.grid{
  display:grid;
  grid-template-columns:1fr 360px;
  gap:20px;
}
@media (max-width:980px){
  .grid{grid-template-columns:1fr;}
  .hero-right{order:-1; margin-bottom:10px}
}

/* Gallery */
.gallery{
  display:flex;
  gap:8px;
  flex-wrap:wrap;
}
.gallery img{
  width:calc(33.333% - 5px);
  border-radius:8px;
  object-fit:cover;
  height:120px;
  box-shadow: 0 6px 20px rgba(20,20,30,0.05);
}
@media (max-width:600px){ .gallery img{width:calc(50% - 4px)} }

/* RSVP */
.rsvp-form input, .rsvp-form textarea, .rsvp-form select{
  width:100%;
  padding:10px 12px;
  margin:6px 0 12px;
  border-radius:8px;
  border:1px solid #e6e6e6;
  font-size:14px;
}
.button{
  background:var(--accent);
  color:white;
  border:none;
  padding:10px 14px;
  border-radius:10px;
  font-weight:600;
  cursor:pointer;
}
.small{font-size:13px; color:var(--muted)}

/* Map */
.map{
  border-radius:10px;
  overflow:hidden;
  height:260px;
  margin-top:8px;
  background:#eee;
}

/* Footer */
.footer{
  text-align:center;
  color:var(--muted);
  font-size:13px;
  margin-top:14px;
}

/* Print */
@media print{
  body{padding:0; background:white}
  .card{box-shadow:none}
  nav, .footer, .rsvp-form .button{display:none}
}
</style>
</head>
<body>
  <div class="container">
    <header class="card header">
      <div class="badge">Мы женимся</div>
      <h1 class="title">Анна &amp; Денис</h1>
      <div class="subtitle">15 ноября 2025 • 16:00 • Прованс hall</div>

      <div class="hero">
        <div class="hero-left">
const mailto = mailto:${RSVP_EMAIL}?subject=${subject}&body=${body};
  window.location.href = mailto;

  // также сохраняем локально как дубликат
  const submissions = JSON.parse(localStorage.getItem('rsvp_submissions') || '[]');
  submissions.push({name,email,phone,attend,note,timestamp:new Date().toISOString()});
  localStorage.setItem('rsvp_submissions', JSON.stringify(submissions));
  showMsg('Спасибо! Открылся ваш почтовый клиент для подтверждения (если не открылся, проверьте блокировку pop-up).', false);
  return false;
}

function showMsg(text, isError){
  const el = document.getElementById('rsvp-msg');
  el.textContent = text;
  el.style.color = isError ? '#c0392b' : 'var(--muted)';
  setTimeout(()=>{ el.textContent = ''; }, 7000);
}

/* Save draft to localStorage */
function saveDraft(){
  const draft = {
    name: document.getElementById('name').value,
    email: document.getElementById('email').value,
    phone: document.getElementById('phone').value,
    attend: document.getElementById('attend').value,
    note: document.getElementById('note').value,
    savedAt: new Date().toISOString()
  };
  localStorage.setItem('rsvp_draft', JSON.stringify(draft));
  showMsg('Черновик сохранён в браузере', false);
}

/* Load draft automatically */
(function loadDraft(){
  try{
    const d = JSON.parse(localStorage.getItem('rsvp_draft'));
    if(d){
      if(d.name) document.getElementById('name').value = d.name;
      if(d.email) document.getElementById('email').value = d.email;
      if(d.phone) document.getElementById('phone').value = d.phone;
      if(d.attend) document.getElementById('attend').value = d.attend;
      if(d.note) document.getElementById('note').value = d.note;
    }
  }catch(e){}
})();

/* Accessibility: focus outline for keyboard users */
document.body.addEventListener('keydown', function(e){
  if(e.key === 'Tab') document.body.classList.add('show-focus');
});
</script>
</body>
</html>
<!doctype html>
<html lang="ru">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Приглашение на свадьбу — Анна &amp; Денис</title>

<!-- Open Graph -->
<meta property="og:title" content="Анна &amp; Денис — Приглашение на свадьбу" />
<meta property="og:description" content="Мы женимся! 15 ноября 2025 • Прованс hall, г. Архангельск" />
<meta property="og:image" content="https://via.placeholder.com/1200x630.png?text=Anna+%26+Denis+Wedding" />
<meta property="og:type" content="website" />

<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700&family=Inter:wght@300;400;600&display=swap" rel="stylesheet">

<style>
:root{
  --accent:#b76e79;
  --muted:#6b6b6b;
  --bg:#faf8f6;
  --card:#ffffff;
  --maxw:1000px;
  --shadow: 0 6px 30px rgba(20,20,30,0.06);
  font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
  color:#222;
}
*{box-sizing:border-box}
body{
  margin:0;
  background:linear-gradient(180deg, var(--bg), #fff 60%);
  -webkit-font-smoothing:antialiased;
  padding:30px 16px 80px;
  display:flex;
  justify-content:center;
}
.container{
  width:100%;
  max-width:var(--maxw);
  background:transparent;
}

/* Card */
.card{
  background:var(--card);
  border-radius:14px;
  padding:28px;
  box-shadow:var(--shadow);
  margin-bottom:20px;
}

/* Header */
.header{
  text-align:center;
  padding:28px 12px 10px;
}
.title{
  font-family: 'Playfair Display', serif;
  font-size:40px;
  margin:6px 0 4px;
  color:#2a2a2a;
}
.subtitle{
  color:var(--muted);
  font-size:15px;
  margin-bottom:12px;
}

/* Hero */
.hero{
  display:flex;
  gap:20px;
  align-items:center;
  justify-content:space-between;
  flex-wrap:wrap;
}
.hero-left{
  flex:1 1 340px;
}
.hero-right{
  flex:0 0 280px;
  text-align:center;
}
.badge{
  display:inline-block;
  background:linear-gradient(90deg, rgba(183,110,121,0.14), rgba(183,110,121,0.06));
  color:var(--accent);
  font-weight:600;
  padding:8px 12px;
  border-radius:999px;
  font-size:13px;
}
.info{
  margin-top:12px;
  color:var(--muted);
  font-size:15px;
}

/* Countdown */
.countdown{
  display:flex;
  gap:10px;
  margin-top:18px;
  justify-content:flex-start;
  flex-wrap:wrap;
}
.countdown .unit{
  background:#fff;
  border-radius:10px;
  padding:10px 12px;
  text-align:center;
  min-width:70px;
  box-shadow: 0 3px 14px rgba(30,30,40,0.04);
}
.unit .num{font-weight:700; font-size:18px;}
.unit .lbl{font-size:12px; color:var(--muted)}

/* Sections */
.grid{
  display:grid;
  grid-template-columns:1fr 360px;
  gap:20px;
}
@media (max-width:980px){
  .grid{grid-template-columns:1fr;}
  .hero-right{order:-1; margin-bottom:10px}
}

/* Gallery */
.gallery{
  display:flex;
  gap:8px;
  flex-wrap:wrap;
}
.gallery img{
  width:calc(33.333% - 5px);
  border-radius:8px;
  object-fit:cover;
  height:120px;
  box-shadow: 0 6px 20px rgba(20,20,30,0.05);
}
@media (max-width:600px){ .gallery img{width:calc(50% - 4px)} }

/* RSVP */
.rsvp-form input, .rsvp-form textarea, .rsvp-form select{
  width:100%;
  padding:10px 12px;
  margin:6px 0 12px;
  border-radius:8px;
  border:1px solid #e6e6e6;
  font-size:14px;
}
.button{
  background:var(--accent);
  color:white;
  border:none;
  padding:10px 14px;
  border-radius:10px;
  font-weight:600;
  cursor:pointer;
}
.small{font-size:13px; color:var(--muted)}

/* Map */
.map{
  border-radius:10px;
  overflow:hidden;
  height:260px;
  margin-top:8px;
  background:#eee;
}

/* Footer */
.footer{
  text-align:center;
  color:var(--muted);
  font-size:13px;
  margin-top:14px;
}

/* Print */
@media print{
  body{padding:0; background:white}
  .card{box-shadow:none}
  nav, .footer, .rsvp-form .button{display:none}
}
</style>
</head>
<body>
  <div class="container">
    <header class="card header">
      <div class="badge">Мы женимся</div>
      <h1 class="title">Анна &amp; Денис</h1>
      <div class="subtitle">15 ноября 2025 • 16:00 • Прованс hall</div>

      <div class="hero">
        <div class="hero-left">

<p class="info">Дорогие друзья и родные! Мы счастливы пригласить вас разделить с нами этот особенный день — будет любовь, радость и незабываемые впечатления. Подробности ниже.</p>

          <div class="countdown" id="countdown">
            <div class="unit"><div class="num" id="days">--</div><div class="lbl">дней</div></div>
            <div class="unit"><div class="num" id="hours">--</div><div class="lbl">часов</div></div>
            <div class="unit"><div class="num" id="minutes">--</div><div class="lbl">минут</div></div>
            <div class="unit"><div class="num" id="seconds">--</div><div class="lbl">секунд</div></div>
          </div>
        </div>

        <div class="hero-right">
          <div style="font-size:13px;color:var(--muted)">Официальная церемония</div>
          <div style="font-weight:700;margin:6px 0;font-size:18px">16:00</div>
          <div class="small">Пожалуйста, подтвердите присутствие заранее</div>
          <div style="margin-top:12px">
            <button class="button" onclick="document.getElementById('rsvp').scrollIntoView({behavior:'smooth'})">Подтвердить участие</button>
          </div>
        </div>
      </div>
    </header>

    <div class="grid">
      <section class="card">
        <h2 style="font-family:'Playfair Display',serif">О нашем дне</h2>
        <p>Наша история началась давно, а теперь мы готовы сделать следующий шаг и пригласить вас отпраздновать этот момент вместе с нами!</p>

        <h3 style="margin-top:18px">Программа вечера</h3>
        <ul>
          <li><strong>15:30</strong> — Сбор гостей</li>
          <li><strong>16:00</strong> — Церемония</li>
          <li><strong>17:00</strong> — Фуршет и фотографии</li>
          <li><strong>19:00</strong> — Ужин и танцы</li>
          <li><strong>23:00</strong> — Завершение праздника</li>
        </ul>

        <h3 style="margin-top:18px">Галерея</h3>
        <div class="gallery">
          <img src="https://via.placeholder.com/600x400.png?text=Анна+и+Денис+1" alt="Фото 1">
          <img src="https://via.placeholder.com/600x400.png?text=Анна+и+Денис+2" alt="Фото 2">
          <img src="https://via.placeholder.com/600x400.png?text=Анна+и+Денис+3" alt="Фото 3">
          <img src="https://via.placeholder.com/600x400.png?text=Анна+и+Денис+4" alt="Фото 4">
          <img src="https://via.placeholder.com/600x400.png?text=Анна+и+Денис+5" alt="Фото 5">
          <img src="https://via.placeholder.com/600x400.png?text=Анна+и+Денис+6" alt="Фото 6">
        </div>
      </section>

      <aside class="card">
        <h2 style="font-family:'Playfair Display',serif">Место проведения</h2>
        <p><strong>Адрес:</strong> г. Архангельск, Прованс hall, проспект Никольский 38</p>

        <div class="map">
          <iframe
            src="https://www.google.com/maps?q=Архангельск,+проспект+Никольский+38&output=embed"
            width="100%" height="100%" frameborder="0" style="border:0;" title="Карта места"></iframe>
        </div>

        <h3 style="margin-top:12px">Как добраться</h3>
        <p class="small">Парковка у зала. Такси и автобусы легко добираются по адресу.</p>

        <h3 style="margin-top:12px">Контакт</h3>
        <p class="small">По вопросам — пишите на <a href="mailto:sushkova-2002@inbox.ru">sushkova-2002@inbox.ru</a></p>
      </aside>
    </div>

    <section class="card" id="rsvp">
      <h2 style="font-family:'Playfair Display',serif">Подтвердите участие (RSVP)</h2>
      <form class="rsvp-form" onsubmit="return handleRsvp(event)">
        <label>
          Ваше имя
          <input type="text" id="name" required placeholder="Ваше имя">
        </label>
        <label>
          E-mail
          <input type="email" id="email" required placeholder="you@example.com">
        </label>
        <label>
          Телефон
          <input type="tel" id="phone" placeholder="+7 999 123 45 67">
        </label>
        <label>
          Будете ли вы присутствовать?
          <select id="attend" required>
            <option value="">Выберите</option>
            <option value="Да">Да</option>
const mailto = mailto:${RSVP_EMAIL}?subject=${subject}&body=${body};
  window.location.href = mailto;

  // также сохраняем локально как дубликат
  const submissions = JSON.parse(localStorage.getItem('rsvp_submissions') || '[]');
  submissions.push({name,email,phone,attend,note,timestamp:new Date().toISOString()});
  localStorage.setItem('rsvp_submissions', JSON.stringify(submissions));
  showMsg('Спасибо! Открылся ваш почтовый клиент для подтверждения (если не открылся, проверьте блокировку pop-up).', false);
  return false;
}

function showMsg(text, isError){
  const el = document.getElementById('rsvp-msg');
  el.textContent = text;
  el.style.color = isError ? '#c0392b' : 'var(--muted)';
  setTimeout(()=>{ el.textContent = ''; }, 7000);
}

/* Save draft to localStorage */
function saveDraft(){
  const draft = {
    name: document.getElementById('name').value,
    email: document.getElementById('email').value,
    phone: document.getElementById('phone').value,
    attend: document.getElementById('attend').value,
    note: document.getElementById('note').value,
    savedAt: new Date().toISOString()
  };
  localStorage.setItem('rsvp_draft', JSON.stringify(draft));
  showMsg('Черновик сохранён в браузере', false);
}

/* Load draft automatically */
(function loadDraft(){
  try{
    const d = JSON.parse(localStorage.getItem('rsvp_draft'));
    if(d){
      if(d.name) document.getElementById('name').value = d.name;
      if(d.email) document.getElementById('email').value = d.email;
      if(d.phone) document.getElementById('phone').value = d.phone;
      if(d.attend) document.getElementById('attend').value = d.attend;
      if(d.note) document.getElementById('note').value = d.note;
    }
  }catch(e){}
})();

/* Accessibility: focus outline for keyboard users */
document.body.addEventListener('keydown', function(e){
  if(e.key === 'Tab') document.body.classList.add('show-focus');
});
</script>
</body>
</html>
<!doctype html>
<html lang="ru">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Приглашение на свадьбу — Анна &amp; Денис</title>

<!-- Open Graph -->
<meta property="og:title" content="Анна &amp; Денис — Приглашение на свадьбу" />
<meta property="og:description" content="Мы женимся! 15 ноября 2025 • Прованс hall, г. Архангельск" />
<meta property="og:image" content="https://via.placeholder.com/1200x630.png?text=Anna+%26+Denis+Wedding" />
<meta property="og:type" content="website" />

<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700&family=Inter:wght@300;400;600&display=swap" rel="stylesheet">

<style>
:root{
  --accent:#b76e79;
  --muted:#6b6b6b;
  --bg:#faf8f6;
  --card:#ffffff;
  --maxw:1000px;
  --shadow: 0 6px 30px rgba(20,20,30,0.06);
  font-family: 'Inter', system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
  color:#222;
}
*{box-sizing:border-box}
body{
  margin:0;
  background:linear-gradient(180deg, var(--bg), #fff 60%);
  -webkit-font-smoothing:antialiased;
  padding:30px 16px 80px;
  display:flex;
  justify-content:center;
}
.container{
  width:100%;
  max-width:var(--maxw);
  background:transparent;
}

/* Card */
.card{
  background:var(--card);
  border-radius:14px;
  padding:28px;
  box-shadow:var(--shadow);
  margin-bottom:20px;
}

/* Header */
.header{
  text-align:center;
  padding:28px 12px 10px;
}
.title{
  font-family: 'Playfair Display', serif;
  font-size:40px;
  margin:6px 0 4px;
  color:#2a2a2a;
}
.subtitle{
  color:var(--muted);
  font-size:15px;
  margin-bottom:12px;
}

/* Hero */
.hero{
  display:flex;
  gap:20px;
  align-items:center;
  justify-content:space-between;
  flex-wrap:wrap;
}
.hero-left{
  flex:1 1 340px;
}
.hero-right{
  flex:0 0 280px;
  text-align:center;
}
.badge{
  display:inline-block;
  background:linear-gradient(90deg, rgba(183,110,121,0.14), rgba(183,110,121,0.06));
  color:var(--accent);
  font-weight:600;
  padding:8px 12px;
  border-radius:999px;
  font-size:13px;
}
.info{
  margin-top:12px;
  color:var(--muted);
  font-size:15px;
}

/* Countdown */
.countdown{
  display:flex;
  gap:10px;
  margin-top:18px;
  justify-content:flex-start;
  flex-wrap:wrap;
}
.countdown .unit{
  background:#fff;
  border-radius:10px;
  padding:10px 12px;
  text-align:center;
  min-width:70px;
  box-shadow: 0 3px 14px rgba(30,30,40,0.04);
}
.unit .num{font-weight:700; font-size:18px;}
.unit .lbl{font-size:12px; color:var(--muted)}

/* Sections */
.grid{
  display:grid;
  grid-template-columns:1fr 360px;
  gap:20px;
}
@media (max-width:980px){
  .grid{grid-template-columns:1fr;}
  .hero-right{order:-1; margin-bottom:10px}
}

/* Gallery */
.gallery{
  display:flex;
  gap:8px;
  flex-wrap:wrap;
}
.gallery img{
  width:calc(33.333% - 5px);
  border-radius:8px;
  object-fit:cover;
  height:120px;
  box-shadow: 0 6px 20px rgba(20,20,30,0.05);
}
@media (max-width:600px){ .gallery img{width:calc(50% - 4px)} }

/* RSVP */
.rsvp-form input, .rsvp-form textarea, .rsvp-form select{
  width:100%;
  padding:10px 12px;
  margin:6px 0 12px;
  border-radius:8px;
  border:1px solid #e6e6e6;
  font-size:14px;
}
.button{
  background:var(--accent);
  color:white;
  border:none;
  padding:10px 14px;
  border-radius:10px;
  font-weight:600;
  cursor:pointer;
}
.small{font-size:13px; color:var(--muted)}

/* Map */
.map{
  border-radius:10px;
  overflow:hidden;
  height:260px;
  margin-top:8px;
  background:#eee;
}

/* Footer */
.footer{
  text-align:center;
  color:var(--muted);
  font-size:13px;
  margin-top:14px;
}

/* Print */
@media print{
  body{padding:0; background:white}
  .card{box-shadow:none}
  nav, .footer, .rsvp-form .button{display:none}
}
</style>
</head>
<body>
  <div class="container">
    <header class="card header">
      <div class="badge">Мы женимся</div>
      <h1 class="title">Анна &amp; Денис</h1>
      <div class="subtitle">15 ноября 2025 • 16:00 • Прованс hall</div>

      <div class="hero">
        <div class="hero-left">
<p class="info">Дорогие друзья и родные! Мы счастливы пригласить вас разделить с нами этот особенный день — будет любовь, радость и незабываемые впечатления. Подробности ниже.</p>

          <div class="countdown" id="countdown">
            <div class="unit"><div class="num" id="days">--</div><div class="lbl">дней</div></div>
            <div class="unit"><div class="num" id="hours">--</div><div class="lbl">часов</div></div>
            <div class="unit"><div class="num" id="minutes">--</div><div class="lbl">минут</div></div>
            <div class="unit"><div class="num" id="seconds">--</div><div class="lbl">секунд</div></div>
          </div>
        </div>

        <div class="hero-right">
          <div style="font-size:13px;color:var(--muted)">Официальная церемония</div>
          <div style="font-weight:700;margin:6px 0;font-size:18px">16:00</div>
          <div class="small">Пожалуйста, подтвердите присутствие заранее</div>
          <div style="margin-top:12px">
            <button class="button" onclick="document.getElementById('rsvp').scrollIntoView({behavior:'smooth'})">Подтвердить участие</button>
          </div>
        </div>
      </div>
    </header>

    <div class="grid">
      <section class="card">
        <h2 style="font-family:'Playfair Display',serif">О нашем дне</h2>
        <p>Наша история началась давно, а теперь мы готовы сделать следующий шаг и пригласить вас отпраздновать этот момент вместе с нами!</p>

        <h3 style="margin-top:18px">Программа вечера</h3>
        <ul>
          <li><strong>15:30</strong> — Сбор гостей</li>
          <li><strong>16:00</strong> — Церемония</li>
          <li><strong>17:00</strong> — Фуршет и фотографии</li>
          <li><strong>19:00</strong> — Ужин и танцы</li>
          <li><strong>23:00</strong> — Завершение праздника</li>
        </ul>

        <h3 style="margin-top:18px">Галерея</h3>
        <div class="gallery">
          <img src="https://via.placeholder.com/600x400.png?text=Анна+и+Денис+1" alt="Фото 1">
          <img src="https://via.placeholder.com/600x400.png?text=Анна+и+Денис+2" alt="Фото 2">
          <img src="https://via.placeholder.com/600x400.png?text=Анна+и+Денис+3" alt="Фото 3">
          <img src="https://via.placeholder.com/600x400.png?text=Анна+и+Денис+4" alt="Фото 4">
          <img src="https://via.placeholder.com/600x400.png?text=Анна+и+Денис+5" alt="Фото 5">
          <img src="https://via.placeholder.com/600x400.png?text=Анна+и+Денис+6" alt="Фото 6">
        </div>
      </section>

      <aside class="card">
        <h2 style="font-family:'Playfair Display',serif">Место проведения</h2>
        <p><strong>Адрес:</strong> г. Архангельск, Прованс hall, проспект Никольский 38</p>

        <div class="map">
          <iframe
            src="https://www.google.com/maps?q=Архангельск,+проспект+Никольский+38&output=embed"
            width="100%" height="100%" frameborder="0" style="border:0;" title="Карта места"></iframe>
        </div>

        <h3 style="margin-top:12px">Как добраться</h3>
        <p class="small">Парковка у зала. Такси и автобусы легко добираются по адресу.</p>

        <h3 style="margin-top:12px">Контакт</h3>
        <p class="small">По вопросам — пишите на <a href="mailto:sushkova-2002@inbox.ru">sushkova-2002@inbox.ru</a></p>
      </aside>
    </div>

    <section class="card" id="rsvp">
      <h2 style="font-family:'Playfair Display',serif">Подтвердите участие (RSVP)</h2>
      <form class="rsvp-form" onsubmit="return handleRsvp(event)">
        <label>
          Ваше имя
          <input type="text" id="name" required placeholder="Ваше имя">
        </label>
        <label>
          E-mail
          <input type="email" id="email" required placeholder="you@example.com">
        </label>
        <label>
          Телефон
          <input type="tel" id="phone" placeholder="+7 999 123 45 67">
        </label>
        <label>
          Будете ли вы присутствовать?
          <select id="attend" required>
            <option value="">Выберите</option>
            <option value="Да">Да</option>
<option value="Нет">Нет</option>
            <option value="Возможно">Возможно</option>
          </select>
        </label>
        <label>
          Комментарий
          <textarea id="note" rows="3" placeholder="Аллергии, пожелания..."></textarea>
        </label>
        <button class="button" type="submit">Отправить RSVP</button>
        <div id="rsvp-msg" class="small" style="margin-top:10px;"></div>
      </form>
    </section>

    <footer class="footer card">
      Мы будем рады видеть вас на нашей свадьбе ❤️  
    </footer>
  </div>

<script>
/* Дата свадьбы */
const EVENT_ISO = "2025-11-15T16:00:00";
const RSVP_EMAIL = "sushkova-2002@inbox.ru";

/* Таймер обратного отсчёта */
function updateCountdown(){
  const target = new Date(EVENT_ISO);
  const now = new Date();
  const diff = target - now;
  if(diff <= 0){
    document.getElementById('days').textContent = '0';
    document.getElementById('hours').textContent = '0';
    document.getElementById('minutes').textContent = '0';
    document.getElementById('seconds').textContent = '0';
    return;
  }
  const s = Math.floor(diff/1000);
  const days = Math.floor(s/86400);
  const hours = Math.floor((s%86400)/3600);
  const minutes = Math.floor((s%3600)/60);
  const seconds = s%60;
  document.getElementById('days').textContent = days;
  document.getElementById('hours').textContent = hours;
  document.getElementById('minutes').textContent = minutes;
  document.getElementById('seconds').textContent = seconds;
}
updateCountdown();
setInterval(updateCountdown,1000);

/* RSVP */
function handleRsvp(e){
  e.preventDefault();
  const name = document.getElementById('name').value.trim();
  const email = document.getElementById('email').value.trim();
  const phone = document.getElementById('phone').value.trim();
  const attend = document.getElementById('attend').value;
  const note = document.getElementById('note').value.trim();

  if(!name  !email  !attend) {
    document.getElementById('rsvp-msg').textContent = 'Заполните обязательные поля';
    return false;
  }

  const subject = encodeURIComponent('RSVP: ' + name + ' — ' + attend);
  const body = encodeURIComponent(
    Имя: ${name}\nE-mail: ${email}\nТелефон: ${phone}\nПридёт: ${attend}\nКомментарий: ${note}
  );
  window.location.href = mailto:${RSVP_EMAIL}?subject=${subject}&body=${body};
  document.getElementById('rsvp-msg').textContent = 'Ваш ответ отправлен. Спасибо!';
  return false;
}
</script>
</body>
</html>
