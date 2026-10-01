```html
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Romantic Love Card - Boss & Kaew</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Canvas Confetti -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts: Prompt & Dancing Script -->
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@600;700&family=Prompt:ital,wght@0,300;0,400;0,500;0,600;0,700;1,300&display=swap" rel="stylesheet">

    <style>
        * {
            box-sizing: border-box;
            user-select: none;
        }

        body {
            font-family: 'Prompt', sans-serif;
            background: linear-gradient(135deg, #fff0f3 0%, #ffccd5 35%, #ffb3c1 70%, #f4acb7 100%);
            min-height: 100vh;
            overflow-x: hidden;
            margin: 0;
            padding: 0;
        }

        .font-cursive {
            font-family: 'Dancing Script', cursive;
        }

        /* Glassmorphism Styling */
        .glass-card {
            background: rgba(255, 255, 255, 0.88);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.95);
            box-shadow: 0 20px 50px rgba(244, 114, 182, 0.25);
        }

        .glass-player {
            background: rgba(255, 245, 248, 0.92);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(244, 172, 183, 0.6);
        }

        .glass-modal {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(24px);
            -webkit-backdrop-filter: blur(24px);
            border: 1px solid rgba(255, 255, 255, 0.8);
        }

        /* 3D Flip Card Effect */
        .perspective-1000 {
            perspective: 1200px;
        }
        
        .transform-style-3d {
            transform-style: preserve-3d;
            transition: transform 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        
        .backface-hidden {
            backface-visibility: hidden;
            -webkit-backface-visibility: hidden;
        }
        
        .rotate-y-180 {
            transform: rotateY(180deg);
        }

        /* Animations */
        @keyframes softFloat {
            0%, 100% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-10px) rotate(1deg); }
        }
        .animate-soft-float {
            animation: softFloat 5s ease-in-out infinite;
        }

        @keyframes pulseGlow {
            0%, 100% { box-shadow: 0 0 20px rgba(244, 114, 182, 0.3), 0 10px 40px rgba(255, 182, 193, 0.4); }
            50% { shadow: 0 0 35px rgba(244, 114, 182, 0.6), 0 15px 50px rgba(255, 105, 180, 0.5); }
        }
        .animate-glow {
            animation: pulseGlow 3.5s infinite;
        }

        @keyframes heartBeat {
            0%, 100% { transform: scale(1); }
            14% { transform: scale(1.15); }
            28% { transform: scale(1); }
            42% { transform: scale(1.15); }
            70% { transform: scale(1); }
        }
        .animate-heartbeat {
            animation: heartBeat 2s infinite ease-in-out;
        }

        #particleCanvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
        }

        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(255, 204, 213, 0.3);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(244, 114, 182, 0.6);
            border-radius: 10px;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between items-center p-4 md:p-8 relative">

    <!-- Background Canvas for Particles -->
    <canvas id="particleCanvas"></canvas>

    <!-- Header / Navigation Actions -->
    <header class="w-full max-w-4xl flex justify-between items-center z-20 mb-4">
        <div class="flex items-center space-x-2 bg-white/60 backdrop-blur-md px-4 py-2 rounded-full border border-white/80 shadow-sm">
            <i class="fa-solid fa-heart text-rose-500 animate-pulse"></i>
            <span class="text-sm font-semibold text-rose-700 font-cursive text-xl">Boss & Kaew's Special Space</span>
        </div>
        <div class="flex space-x-2">
            <button id="customizeBtn" title="ปรับแต่งการ์ด" class="p-2.5 bg-white/70 hover:bg-white/90 text-rose-600 rounded-full shadow-md backdrop-blur-md transition-all duration-300 active:scale-95">
                <i class="fa-solid fa-pen-to-square text-lg"></i>
            </button>
            <button id="toggleMusicBtn" title="เปิด/ปิด เสียงเพลง" class="p-2.5 bg-white/70 hover:bg-white/90 text-rose-600 rounded-full shadow-md backdrop-blur-md transition-all duration-300 active:scale-95">
                <i id="musicIcon" class="fa-solid fa-music text-lg"></i>
            </button>
        </div>
    </header>

    <!-- Main Card Container -->
    <main class="w-full max-w-md my-auto z-10 perspective-1000 animate-soft-float">
        <div id="cardInner" class="relative w-full h-[580px] md:h-[620px] transform-style-3d cursor-pointer">
            
            <!-- FRONT OF CARD -->
            <div class="absolute inset-0 w-full h-full glass-card rounded-3xl p-6 md:p-8 flex flex-col justify-between items-center text-center shadow-2xl backface-hidden animate-glow border-2 border-white/80">
                <!-- Top Badge -->
                <div class="flex items-center space-x-1.5 bg-rose-100/80 px-3.5 py-1 rounded-full border border-rose-200">
                    <i class="fa-solid fa-sparkles text-rose-500 text-xs"></i>
                    <span id="cardCategory" class="text-xs font-semibold text-rose-600">Boss & Kaew ❤️</span>
                </div>

                <!-- Central Heart Icon / Custom Photo Frame -->
                <div class="relative w-36 h-36 md:w-44 md:h-44 my-2 flex justify-center items-center">
                    <div id="heartIconGroup" class="relative flex justify-center items-center">
                        <div class="absolute inset-0 rounded-full bg-rose-300/30 blur-xl animate-pulse"></div>
                        <i class="fa-solid fa-heart text-7xl md:text-8xl text-rose-500 drop-shadow-lg animate-heartbeat"></i>
                    </div>
                    <img id="photoFrame" src="https://images.unsplash.com/photo-1518199266791-5375a83190b7?auto=format&fit=crop&w=500&q=80" alt="Love Memory" class="hidden absolute inset-0 w-full h-full object-cover rounded-2xl border-4 border-white shadow-md">
                </div>

                <!-- Title & Text Content -->
                <div class="space-y-2">
                    <h1 id="frontTitle" class="font-cursive text-4xl md:text-5xl text-rose-600 font-bold leading-tight drop-shadow-sm">
                        Boss & Kaew
                    </h1>
                    <p id="frontSubtitle" class="text-gray-600 text-sm md:text-base font-light">
                        แด่แก้ว...คนที่ทำให้ทุกวันของบอสมีความหมาย ❤️
                    </p>
                    <span class="inline-block text-xs text-rose-400 bg-rose-50 px-3 py-1 rounded-full border border-rose-100 font-medium">
                        <i class="fa-solid fa-hand-pointer mr-1"></i> คลิกเพื่อพลิกดูข้อความความรู้สึก
                    </span>
                </div>

                <!-- Audio Player Widget -->
                <div id="playerBox" class="glass-player w-full p-3 rounded-2xl flex items-center gap-3 shadow-sm border border-rose-200/60 transition-all hover:bg-white/90">
                    <button id="playPauseBtn" class="w-10 h-10 bg-gradient-to-tr from-rose-500 to-pink-400 text-white rounded-full flex items-center justify-center shadow-md hover:scale-105 transition-transform active:scale-95">
                        <i id="playIcon" class="fa-solid fa-play text-sm ml-0.5"></i>
                    </button>
                    <div class="text-left flex-1 min-w-0">
                        <div class="flex justify-between items-center">
                            <p id="songTitle" class="text-xs font-bold text-gray-700 truncate">Boss & Kaew's Song</p>
                            <span id="currentTime" class="text-[10px] text-rose-500 font-mono">00:00</span>
                        </div>
                        <div class="w-full bg-rose-200/60 rounded-full h-1.5 mt-1.5 overflow-hidden">
                            <div id="progressBar" class="bg-rose-500 h-full w-0 rounded-full transition-all duration-300"></div>
                        </div>
                    </div>
                    <label title="อัปโหลดเพลง MP3" class="cursor-pointer text-rose-400 hover:text-rose-600 p-1">
                        <i class="fa-solid fa-file-audio text-base"></i>
                        <input type="file" id="audioUpload" accept="audio/*" class="hidden">
                    </label>
                </div>
            </div>

            <!-- BACK OF CARD -->
            <div class="absolute inset-0 w-full h-full glass-card rounded-3xl p-6 md:p-8 flex flex-col justify-between items-center text-center shadow-2xl backface-hidden rotate-y-180 border-2 border-white/80">
                <div>
                    <h2 id="backHeader" class="font-cursive text-3xl md:text-4xl text-rose-600 font-bold mb-1">
                        ข้อความจากใจถึงแก้ว
                    </h2>
                    <div class="w-12 h-0.5 bg-rose-300 mx-auto rounded-full"></div>
                </div>

                <!-- Message Box -->
                <div class="my-2 px-2 max-h-[180px] overflow-y-auto">
                    <p id="backMessage" class="text-gray-700 leading-relaxed text-sm md:text-base font-light italic">
                        "ขอบคุณสำหรับทุกๆ รอยยิ้ม ความเข้าใจ และทุกๆ ช่วงเวลาพิเศษที่เราได้ใช้ร่วมกัน 273 วันที่ผ่านมามีความหมายและมีคุณค่ามากๆ ไม่ว่าวันข้างหน้าจะเป็นอย่างไร บอสสัญญาว่าจะรักและดูแลแก้วให้ดีที่สุดในทุกๆ วันนะครับ ❤️"
                    </p>
                </div>

                <!-- Prominent Relationship Time Breakdown Card -->
                <div class="w-full bg-white/70 p-3 rounded-2xl border border-rose-100/80 shadow-sm space-y-1.5">
                    <p class="text-xs text-rose-500 font-medium flex items-center justify-center space-x-1">
                        <i class="fa-solid fa-heart text-rose-400 text-[10px]"></i>
                        <span>ระยะเวลาที่ บอส & แก้ว คบกันมาแล้ว</span>
                        <i class="fa-solid fa-heart text-rose-400 text-[10px]"></i>
                    </p>
                    <p id="daysCount" class="text-2xl font-extrabold text-rose-600 font-mono tracking-tight">
                        273 วัน
                    </p>
                    <!-- Time Sub-Breakdown -->
                    <div class="grid grid-cols-3 gap-1 pt-1 text-center border-t border-rose-100 text-[10px] md:text-xs text-gray-600">
                        <div>
                            <span id="hoursCount" class="font-bold text-rose-500 block font-mono">6,552</span>
                            <span class="text-[9px] text-gray-400">ชั่วโมง</span>
                        </div>
                        <div>
                            <span id="minutesCount" class="font-bold text-rose-500 block font-mono">393,120</span>
                            <span class="text-[9px] text-gray-400">นาที</span>
                        </div>
                        <div>
                            <span id="secondsCount" class="font-bold text-rose-500 block font-mono">23,587,200</span>
                            <span class="text-[9px] text-gray-400">วินาที</span>
                        </div>
                    </div>
                </div>

                <!-- Celebration Action -->
                <div class="w-full space-y-1">
                    <button id="confettiBtn" onclick="triggerCelebration(event)" class="w-full bg-gradient-to-r from-rose-500 to-pink-500 hover:from-rose-600 hover:to-pink-600 text-white font-medium py-2.5 px-6 rounded-2xl shadow-lg hover:shadow-rose-300/50 transition-all duration-300 active:scale-95 flex items-center justify-center space-x-2">
                        <i class="fa-solid fa-gift text-sm"></i>
                        <span>ฉลองครบรอบ 273 วัน Boss & Kaew! 🎉</span>
                    </button>
                    <p class="text-[10px] text-gray-400">คลิกที่การ์ดเพื่อพลิกกลับ</p>
                </div>
            </div>
        </div>
    </main>

    <!-- Footer Information -->
    <footer class="z-10 mt-4 text-center">
        <p class="text-xs text-rose-800/70 font-light">
            Made with ❤️ for Kaew from Boss
        </p>
    </footer>

    <!-- Customizer Modal -->
    <div id="customizeModal" class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="glass-modal w-full max-w-lg rounded-3xl p-6 shadow-2xl space-y-4 max-h-[90vh] overflow-y-auto">
            <div class="flex justify-between items-center border-b border-rose-100 pb-3">
                <h3 class="font-bold text-lg text-rose-600 flex items-center">
                    <i class="fa-solid fa-sliders mr-2"></i> ปรับแต่งเนื้อหาการ์ด Boss & Kaew
                </h3>
                <button id="closeModalBtn" class="text-gray-400 hover:text-gray-600 p-1">
                    <i class="fa-solid fa-xmark text-xl"></i>
                </button>
            </div>

            <div class="space-y-3 text-sm text-left">
                <div>
                    <label class="block text-gray-700 font-medium mb-1">หัวข้อหน้าการ์ด</label>
                    <input type="text" id="inputCategory" value="Boss & Kaew ❤️" class="w-full px-3 py-2 rounded-xl border border-rose-200 focus:outline-none focus:ring-2 focus:ring-rose-400 bg-white/80">
                </div>
                <div>
                    <label class="block text-gray-700 font-medium mb-1">ข้อความหลักด้านหน้า</label>
                    <input type="text" id="inputTitle" value="Boss & Kaew" class="w-full px-3 py-2 rounded-xl border border-rose-200 focus:outline-none focus:ring-2 focus:ring-rose-400 bg-white/80">
                </div>
                <div>
                    <label class="block text-gray-700 font-medium mb-1">คำบรรยายด้านหน้า</label>
                    <input type="text" id="inputSubtitle" value="แด่แก้ว...คนที่ทำให้ทุกวันของบอสมีความหมาย ❤️" class="w-full px-3 py-2 rounded-xl border border-rose-200 focus:outline-none focus:ring-2 focus:ring-rose-400 bg-white/80">
                </div>
                <div>
                    <label class="block text-gray-700 font-medium mb-1">ข้อความในซองการ์ด (ด้านหลัง)</label>
                    <textarea id="inputMessage" rows="3" class="w-full px-3 py-2 rounded-xl border border-rose-200 focus:outline-none focus:ring-2 focus:ring-rose-400 bg-white/80">ขอบคุณสำหรับทุกๆ รอยยิ้ม ความเข้าใจ และทุกๆ ช่วงเวลาพิเศษที่เราได้ใช้ร่วมกัน 273 วันที่ผ่านมามีความหมายและมีคุณค่ามากๆ ไม่ว่าวันข้างหน้าจะเป็นอย่างไร บอสสัญญาว่าจะรักและดูแลแก้วให้ดีที่สุดในทุกๆ วันนะครับ ❤️</textarea>
                </div>
                <div>
                    <label class="block text-gray-700 font-medium mb-1">จำนวนวันที่คบกัน (วัน)</label>
                    <input type="number" id="inputDays" value="273" class="w-full px-3 py-2 rounded-xl border border-rose-200 focus:outline-none focus:ring-2 focus:ring-rose-400 bg-white/80">
                </div>
                <div>
                    <label class="block text-gray-700 font-medium mb-1">URL รูปภาพรูปตรงกลาง (เว้นว่างไว้หากใช้รูปหัวใจ)</label>
                    <input type="text" id="inputPhotoUrl" placeholder="https://..." class="w-full px-3 py-2 rounded-xl border border-rose-200 focus:outline-none focus:ring-2 focus:ring-rose-400 bg-white/80">
                </div>
            </div>

            <div class="pt-2 flex space-x-2">
                <button id="saveCustomizeBtn" class="flex-1 bg-rose-500 hover:bg-rose-600 text-white font-medium py-2.5 rounded-xl shadow-md transition-all">
                    บันทึกการเปลี่ยนแปลง
                </button>
            </div>
        </div>
    </div>

    <audio id="customAudio" class="hidden"></audio>

    <script>
        const canvas = document.getElementById('particleCanvas');
        const ctx = canvas.getContext('2d');
        let particles = [];

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        class HeartParticle {
            constructor() {
                this.reset();
            }

            reset() {
                this.x = Math.random() * canvas.width;
                this.y = canvas.height + Math.random() * 50;
                this.size = Math.random() * 14 + 8;
                this.speedY = Math.random() * 1.2 + 0.5;
                this.speedX = Math.sin(Math.random() * Math.PI) * 0.5;
                this.opacity = Math.random() * 0.5 + 0.3;
                this.rotation = Math.random() * 360;
                this.rotationSpeed = (Math.random() - 0.5) * 1.5;
                this.color = `hsl(${Math.random() * 30 + 340}, 85%, 70%)`;
            }

            update() {
                this.y -= this.speedY;
                this.x += Math.sin(this.y * 0.01) * 0.5;
                this.rotation += this.rotationSpeed;
                if (this.y < -30) {
                    this.reset();
                }
            }

            draw() {
                ctx.save();
                ctx.translate(this.x, this.y);
                ctx.rotate((this.rotation * Math.PI) / 180);
                ctx.globalAlpha = this.opacity;
                ctx.fillStyle = this.color;
                
                ctx.beginPath();
                const topCurveHeight = this.size * 0.3;
                ctx.moveTo(0, topCurveHeight);
                ctx.bezierCurveTo(0, 0, -this.size / 2, 0, -this.size / 2, topCurveHeight);
                ctx.bezierCurveTo(-this.size / 2, (this.size + topCurveHeight) / 2, 0, this.size, 0, this.size);
                ctx.bezierCurveTo(0, this.size, this.size / 2, (this.size + topCurveHeight) / 2, this.size / 2, topCurveHeight);
                ctx.bezierCurveTo(this.size / 2, 0, 0, 0, 0, topCurveHeight);
                ctx.closePath();
                ctx.fill();
                
                ctx.restore();
            }
        }

        function initParticles() {
            particles = [];
            const count = Math.min(Math.floor(window.innerWidth / 25), 35);
            for (let i = 0; i < count; i++) {
                particles.push(new HeartParticle());
            }
        }
        initParticles();

        function animateParticles() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            particles.forEach(p => {
                p.update();
                p.draw();
            });
            requestAnimationFrame(animateParticles);
        }
        animateParticles();

        let audioCtx = null;
        let isPlaying = false;
        let melodyInterval = null;
        let currentNoteIndex = 0;
        let customAudioActive = false;
        const customAudio = document.getElementById('customAudio');

        const romanticMelody = [
            261.63, 329.63, 392.00, 523.25, 392.00, 329.63,
            220.00, 261.63, 329.63, 440.00, 329.63, 261.63,
            174.61, 220.00, 261.63, 349.
