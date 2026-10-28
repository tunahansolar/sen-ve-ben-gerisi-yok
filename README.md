<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sadece Sen ve Ben</title>
    <style>
        :root {
            --pink-main: #ff7495;
            --pink-light: #ffb7c5;
            --pink-bg: #fff0f3;
        }
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { background-color: var(--pink-bg); overflow-x: hidden; position: relative; min-height: 100vh; }
        #login-screen { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: linear-gradient(135deg, var(--pink-light), #fff); display: flex; flex-direction: column; justify-content: center; align-items: center; z-index: 9999; transition: all 0.5s ease; }
        .login-box { background: white; padding: 30px; border-radius: 20px; box-shadow: 0 10px 25px rgba(255, 116, 149, 0.2); text-align: center; max-width: 90%; width: 350px; }
        .login-box h2 { color: var(--pink-main); margin-bottom: 20px; }
        .login-box input { width: 100%; padding: 12px; border: 2px solid var(--pink-light); border-radius: 10px; margin-bottom: 15px; outline: none; text-align: center; font-size: 16px; }
        .login-box button { background: var(--pink-main); color: white; border: none; padding: 12px 25px; border-radius: 10px; cursor: pointer; font-size: 16px; font-weight: bold; transition: 0.3s; }
        #main-content { display: none; opacity: 0; transition: opacity 1s ease; padding: 20px; max-width: 600px; margin: 0 auto; text-align: center; }
        h1 { color: var(--pink-main); margin: 20px 0; font-size: 24px; }
        .music-box { background: white; padding: 15px; border-radius: 15px; margin-bottom: 25px; box-shadow: 0 5px 15px rgba(0,0,0,0.05); }
        .photo-container { width: 100%; border-radius: 20px; overflow: hidden; box-shadow: 0 8px 20px rgba(0,0,0,0.1); margin-bottom: 25px; border: 5px solid white; }
        .photo-container img { width: 100%; display: block; }
        .counter-box { background: white; padding: 20px; border-radius: 20px; box-shadow: 0 5px 15px rgba(0,0,0,0.05); margin-bottom: 25px; }
        .counter-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; margin-top: 15px; }
        .counter-item { background: var(--pink-bg); padding: 10px 5px; border-radius: 10px; }
        .counter-value { font-size: 20px; font-weight: bold; color: var(--pink-main); }
        .counter-label { font-size: 11px; color: #666; }
        .gift-section { margin-top: 30px; margin-bottom: 50px; }
        .gift-box { font-size: 60px; cursor: pointer; display: inline-block; transition: transform 0.3s; animation: bounce 2s infinite; margin: 0 10px; }
        @keyframes bounce { 0%, 20%, 50%, 80%, 100% { transform: translateY(0); } 40% { transform: translateY(-15px); } 60% { transform: translateY(-7px); } }
        .note-card { background: #fff; border-left: 5px solid var(--pink-main); padding: 15px; border-radius: 0 15px 15px 0; margin: 15px auto; text-align: left; box-shadow: 0 4px 10px rgba(0,0,0,0.05); display: none; }
        .bubble { position: absolute; bottom: -50px; background: rgba(255, 183, 197, 0.4); border-radius: 50%; pointer-events: none; animation: floatUp infinite linear; }
        @keyframes floatUp { 0% { transform: translateY(0) translateX(0); opacity: 0; } 10% { opacity: 0.6; } 90% { opacity: 0.6; } 100% { transform: translateY(-100vh) translateX(50px); opacity: 0; } }
    </style>
</head>
<body>
    <div id="login-screen">
        <div class="login-box">
            <h2>Giriş Şifresi 💕</h2>
            <input type="password" id="password-input" placeholder="Şifremiz...">
            <button onclick="checkPassword()">Dünyamıza Gir</button>
            <p id="error-msg" style="color: red; margin-top: 10px; font-size: 14px; display: none;">Hatalı şifre, tekrar dene aşkım! 🥺</p>
        </div>
    </div>
    <div id="main-content">
        <h1>Sadece Sen ve Ben İçin Hazırlanan Özel Bir Dünya ✨</h1>
        <div class="music-box">
            <p style="font-size: 14px; margin-bottom: 8px; color: #555;">🎵 Sizin Şarkınız Çalıyor...</p>
            <audio id="bg-music" controls loop style="width: 100%;">
                <source src="https://soundhelix.com" type="audio/mpeg">
            </audio>
        </div>
        <div class="photo-container">
            <img src="https://unsplash.com" alt="Bizim Anımız">
        </div>
        <div class="counter-box">
            <p style="font-weight: bold; color: #555;">Aşkımızın Başlangıcından Beri;</p>
            <div class="counter-grid">
                <div class="counter-item"><div class="counter-value" id="days">0</div><div class="counter-label">Gün</div></div>
                <div class="counter-item"><div class="counter-value" id="hours">0</div><div class="counter-label">Saat</div></div>
                <div class="counter-item"><div class="counter-value" id="minutes">0</div><div class="counter-label">Dakika</div></div>
                <div class="counter-item"><div class="counter-value" id="seconds">0</div><div class="counter-label">Saniye</div></div>
            </div>
        </div>
        <div class="gift-section">
            <p style="font-weight: bold; margin-bottom: 10px; color: #555;">Sana Özel Hediyelerim Var, Üzerlerine Tıkla! 🎁</p>
            <div class="gift-box" onclick="openGift(1)">🎁</div>
            <div class="gift-box" onclick="openGift(2)">🎁</div>
            <div class="gift-box" onclick="openGift(3)">🎁</div>
            <div id="note-1" class="note-card"><strong>💌 1. Özel Not:</strong><br>Hayatımın en güzel kararı seninle başlamaktı. İyi ki varsın sevgilim.</div>
            <div id="note-2" class="note-card"><strong>💝 2. Özel Not:</strong><br>Seninle geçirdiğim her saniye, hayatımın en değerli anı. Seni çok seviyorum.</div>
            <div id="note-3" class="note-card"><strong>✨ 3. Özel Not:</strong><br>Gelecekteki tüm güzel günleri seninle yan yana, el ele geçirmek dileğiyle...</div>
        </div>
    </div>
    <script>
        const GECERLI_SIFRE = "1234"; 
        const TANISMA_TARIHI = new Date("2025-01-01T00:00:00");
        function checkPassword() {
            const input = document.getElementById('password-input').value;
            const error = document.getElementById('error-msg');
            if (input === GECERLI_SIFRE) {
                document.getElementById('login-screen').style.display = 'none';
                document.getElementById('main-content').style.display = 'block';
                document.getElementById('main-content').style.opacity = '1';
                document.getElementById('bg-music').play().catch(e => console.log("Müzik başlatılamadı"));
            } else { error.style.display = 'block'; }
        }
        function updateCounter() {
            const difference = new Date() - TANISMA_TARIHI;
            document.getElementById('days').innerText = Math.floor(difference / (1000 * 60 * 60 * 24));
            document.getElementById('hours').innerText = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            document.getElementById('minutes').innerText = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
            document.getElementById('seconds').innerText = Math.floor((difference % (1000 * 60)) / 1000);
        }
        setInterval(updateCounter, 1000); updateCounter();
        function openGift(id) {
            const note = document.getElementById(`note-${id}`);
            note.style.display = note.style.display === 'block' ? 'none' : 'block';
        }
        function createBubble() {
            const bubble = document.createElement('div'); bubble.classList.add('bubble');
            const size = Math.random() * 40 + 10 + 'px'; bubble.style.width = size; bubble.style.height = size;
            bubble.style.left = Math.random() * 100 + 'vw'; bubble.style.animationDuration = Math.random() * 4 + 4 + 's';
            document.body.appendChild(bubble); setTimeout(() => { bubble.remove(); }, 8000);
        }
        setInterval(createBubble, 400);
    </script>
</body>
</html>
