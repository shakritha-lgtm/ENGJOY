<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>English Learning Adventure</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            text-align: center;
            color: white;
            margin-bottom: 30px;
        }

        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .user-info {
            background: rgba(255,255,255,0.1);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: white;
        }

        .user-stats {
            display: flex;
            gap: 30px;
        }

        .stat {
            text-align: center;
        }

        .stat-value {
            font-size: 1.8rem;
            font-weight: bold;
            display: block;
        }

        .stat-label {
            font-size: 0.9rem;
            opacity: 0.8;
        }

        .levels-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .level-card {
            background: white;
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .level-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 40px rgba(0,0,0,0.2);
        }

        .level-card.locked {
            opacity: 0.6;
            background: #f5f5f5;
            cursor: not-allowed;
        }

        .level-card.completed {
            background: linear-gradient(135deg, #4CAF50, #45a049);
            color: white;
        }

        .level-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .level-number {
            background: #667eea;
            color: white;
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
        }

        .level-card.completed .level-number {
            background: #2e7d32;
        }

        .level-card.locked .level-number {
            background: #ccc;
        }

        .level-status {
            font-size: 0.9rem;
            padding: 5px 15px;
            border-radius: 20px;
            font-weight: bold;
        }

        .status-unlocked {
            background: #e3f2fd;
            color: #1976d2;
        }

        .status-completed {
            background: rgba(255,255,255,0.2);
            color: white;
        }

        .status-locked {
            background: #f5f5f5;
            color: #999;
        }

        .level-title {
            font-size: 1.3rem;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .level-description {
            color: #666;
            margin-bottom: 15px;
            line-height: 1.5;
        }

        .level-card.completed .level-description {
            color: rgba(255,255,255,0.9);
        }

        .level-progress {
            margin-bottom: 15px;
        }

        .progress-bar {
            background: #e0e0e0;
            height: 8px;
            border-radius: 4px;
            overflow: hidden;
        }

        .progress-fill {
            background: #667eea;
            height: 100%;
            border-radius: 4px;
            transition: width 0.3s ease;
        }

        .level-card.completed .progress-fill {
            background: #2e7d32;
        }

        .level-actions {
            display: flex;
            gap: 10px;
        }

        .btn {
            padding: 12px 20px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
            transition: all 0.3s ease;
            flex: 1;
            text-align: center;
        }

        .btn-primary {
            background: #667eea;
            color: white;
        }

        .btn-primary:hover {
            background: #5a67d8;
            transform: translateY(-2px);
        }

        .btn-success {
            background: #4CAF50;
            color: white;
        }

        .btn-disabled {
            background: #ccc;
            color: #999;
            cursor: not-allowed;
        }

        .achievements {
            background: white;
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
        }

        .achievements h3 {
            margin-bottom: 20px;
            color: #333;
        }

        .achievement-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
        }

        .achievement {
            text-align: center;
            padding: 15px;
            border-radius: 15px;
            background: #f8f9fa;
            transition: all 0.3s ease;
        }

        .achievement.earned {
            background: linear-gradient(135deg, #ffd700, #ffed4e);
            transform: scale(1.05);
        }

        .achievement-icon {
            font-size: 2rem;
            margin-bottom: 8px;
        }

        .achievement-name {
            font-size: 0.9rem;
            font-weight: bold;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }

        .modal.show {
            display: flex;
        }

        .modal-content {
            background: white;
            border-radius: 20px;
            padding: 30px;
            max-width: 500px;
            width: 90%;
            text-align: center;
        }

        .close-modal {
            background: #667eea;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 10px;
            cursor: pointer;
            margin-top: 20px;
        }

        @media (max-width: 768px) {
            .user-info {
                flex-direction: column;
                gap: 15px;
            }

            .user-stats {
                gap: 20px;
            }

            .levels-grid {
                grid-template-columns: 1fr;
            }

            .header h1 {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🎓 English Learning Adventure</h1>
            <p>เรียนรู้ภาษาอังกฤษผ่านเกมแบบด่าน</p>
        </div>

        <div class="user-info">
            <div>
                <h3>👋 สวัสดี นักเรียน!</h3>
                <p>พร้อมผจญภัยในโลกภาษาอังกฤษหรือยัง?</p>
            </div>
            <div class="user-stats">
                <div class="stat">
                    <span class="stat-value" id="totalScore">0</span>
                    <span class="stat-label">คะแนนรวม</span>
                </div>
                <div class="stat">
                    <span class="stat-value" id="completedLevels">0</span>
                    <span class="stat-label">ด่านที่ผ่าน</span>
                </div>
                <div class="stat">
                    <span class="stat-value" id="currentStreak">0</span>
                    <span class="stat-label">เรียนต่อเนื่อง</span>
                </div>
            </div>
        </div>

        <div class="levels-grid" id="levelsGrid">
            <!-- Levels will be generated by JavaScript -->
        </div>

        <div class="achievements">
            <h3>🏆 ความสำเร็จ</h3>
            <div class="achievement-grid" id="achievementGrid">
                <!-- Achievements will be generated by JavaScript -->
            </div>
        </div>
    </div>

    <!-- Modal -->
    <div class="modal" id="gameModal">
        <div class="modal-content">
            <h3 id="modalTitle">ด่านที่ 1: ทักทายภาษาอังกฤษ</h3>
            <p id="modalDescription">ในด่านนี้ คุณจะได้เรียนรู้การทักทายเบื้องต้น</p>
            <div style="margin: 20px 0;">
                <p><strong>🎯 เป้าหมาย:</strong> ทำคะแนนได้ 80% ขึ้นไป</p>
                <p><strong>⭐ รางวัล:</strong> 100 คะแนน</p>
            </div>
            <button class="btn btn-primary" onclick="startLevel()">เริ่มเล่น</button>
            <button class="close-modal" onclick="closeModal()">ปิด</button>
        </div>
    </div>

    <script>
        // ข้อมูลระบบ
        let gameData = {
            currentLevel: 1,
            totalScore: 0,
            completedLevels: 0,
            currentStreak: 5,
            userProgress: {}
        };

        // รายการด่าน
        const levels = [
            {
                id: 1,
                title: "ทักทายภาษาอังกฤษ",
                description: "เรียนรู้การทักทายเบื้องต้น Hello, Hi, Good morning",
                geniallyUrl: "https://view.genial.ly/your-greeting-game",
                requiredScore: 80,
                reward: 100,
                unlocked: true
            },
            {
                id: 2,
                title: "แนะนำตัวเอง",
                description: "My name is, I am from, I like...",
                geniallyUrl: "https://view.genial.ly/your-introduction-game",
                requiredScore: 80,
                reward: 150,
                unlocked: false
            },
            {
                id: 3,
                title: "สีสันในภาษาอังกฤษ",
                description: "เรียนรู้คำศัพท์สี Red, Blue, Green, Yellow",
                geniallyUrl: "https://view.genial.ly/your-colors-game",
                requiredScore: 80,
                reward: 120,
                unlocked: false
            },
            {
                id: 4,
                title: "ตัวเลขและการนับ",
                description: "Numbers 1-20 และการใช้งานเบื้องต้น",
                geniallyUrl: "https://view.genial.ly/your-numbers-game",
                requiredScore: 80,
                reward: 180,
                unlocked: false
            },
            {
                id: 5,
                title: "ครอบครัวของฉัน",
                description: "Family members: Father, Mother, Sister, Brother",
                geniallyUrl: "https://view.genial.ly/your-family-game",
                requiredScore: 80,
                reward: 200,
                unlocked: false
            },
            {
                id: 6,
                title: "อาหารและเครื่องดื่ม",
                description: "Food & Drinks vocabulary และการสั่งอาหาร",
                geniallyUrl: "https://view.genial.ly/your-food-game",
                requiredScore: 80,
                reward: 250,
                unlocked: false
            }
        ];

        // รายการความสำเร็จ
        const achievements = [
            {
                id: 'first_level',
                name: 'ก้าวแรก',
                icon: '🎯',
                description: 'ผ่านด่านแรก',
                earned: false
            },
            {
                id: 'perfectionist',
                name: 'นักสมบูรณ์แบบ',
                icon: '💯',
                description: 'ทำคะแนนเต็ม 100%',
                earned: false
            },
            {
                id: 'streak_master',
                name: 'เรียนสม่ำเสมอ',
                icon: '🔥',
                description: 'เรียนต่อเนื่อง 7 วัน',
                earned: true
            },
            {
                id: 'level_crusher',
                name: 'นักผจญภัย',
                icon: '⚡',
                description: 'ผ่าน 5 ด่าน',
                earned: false
            }
        ];

        // โหลดข้อมูลจาก localStorage (จำลอง database)
        function loadGameData() {
            const saved = localStorage.getItem('englishGameData');
            if (saved) {
                gameData = JSON.parse(saved);
            }
            updateUI();
        }

        // บันทึกข้อมูล
        function saveGameData() {
            localStorage.setItem('englishGameData', JSON.stringify(gameData));
        }

        // อัพเดต UI
        function updateUI() {
            document.getElementById('totalScore').textContent = gameData.totalScore;
            document.getElementById('completedLevels').textContent = gameData.completedLevels;
            document.getElementById('currentStreak').textContent = gameData.currentStreak;
            
            renderLevels();
            renderAchievements();
        }

        // แสดงด่านทั้งหมด
        function renderLevels() {
            const grid = document.getElementById('levelsGrid');
            grid.innerHTML = '';

            levels.forEach(level => {
                const isCompleted = gameData.userProgress[level.id]?.completed || false;
                const isUnlocked = level.unlocked || isCompleted || (level.id > 1 && gameData.userProgress[level.id - 1]?.completed);
                const userScore = gameData.userProgress[level.id]?.score || 0;
                
                let statusClass = 'locked';
                let statusText = '🔒 ล็อค';
                let buttonText = 'ล็อค';
                let buttonClass = 'btn-disabled';
                
                if (isCompleted) {
                    statusClass = 'completed';
                    statusText = '✅ ผ่านแล้ว';
                    buttonText = 'เล่นอีกครั้ง';
                    buttonClass = 'btn-success';
                } else if (isUnlocked) {
                    statusClass = 'unlocked';
                    statusText = '🎮 พร้อมเล่น';
                    buttonText = 'เริ่มเล่น';
                    buttonClass = 'btn-primary';
                }

                const progressPercent = isCompleted ? 100 : (userScore / level.requiredScore * 100);

                const levelCard = `
                    <div class="level-card ${statusClass}">
                        <div class="level-header">
                            <div class="level-number">${level.id}</div>
                            <div class="level-status status-${statusClass}">${statusText}</div>
                        </div>
                        <h3 class="level-title">${level.title}</h3>
                        <p class="level-description">${level.description}</p>
                        <div class="level-progress">
                            <div class="progress-bar">
                                <div class="progress-fill" style="width: ${progressPercent}%"></div>
                            </div>
                            <small>ความก้าวหน้า: ${Math.round(progressPercent)}%</small>
                        </div>
                        <div class="level-actions">
                            <button class="btn ${buttonClass}" 
                                    onclick="openLevel(${level.id})" 
                                    ${!isUnlocked ? 'disabled' : ''}>
                                ${buttonText}
                            </button>
                        </div>
                        ${isCompleted ? `<div style="margin-top: 10px;"><small>คะแนนสูงสุด: ${userScore}/${level.requiredScore}</small></div>` : ''}
                    </div>
                `;
                
                grid.innerHTML += levelCard;
            });
        }

        // แสดงความสำเร็จ
        function renderAchievements() {
            const grid = document.getElementById('achievementGrid');
            grid.innerHTML = '';

            achievements.forEach(achievement => {
                const earnedClass = achievement.earned ? 'earned' : '';
                const achievementDiv = `
                    <div class="achievement ${earnedClass}">
                        <div class="achievement-icon">${achievement.icon}</div>
                        <div class="achievement-name">${achievement.name}</div>
                    </div>
                `;
                grid.innerHTML += achievementDiv;
            });
        }

        // เปิดด่าน
        function openLevel(levelId) {
            const level = levels.find(l => l.id === levelId);
            if (!level) return;

            document.getElementById('modalTitle').textContent = `ด่านที่ ${level.id}: ${level.title}`;
            document.getElementById('modalDescription').textContent = level.description;
            document.getElementById('gameModal').classList.add('show');
            
            // เก็บ ID ด่านปัจจุบัน
            window.currentLevelId = levelId;
        }

        // ปิด modal
        function closeModal() {
            document.getElementById('gameModal').classList.remove('show');
        }

        // เริ่มเล่นด่าน (จำลอง)
        function startLevel() {
            const levelId = window.currentLevelId;
            closeModal();
            
            // จำลองการเล่นเกม
            setTimeout(() => {
                // สุ่มคะแนน (จำลอง)
                const score = Math.floor(Math.random() * 40) + 60; // 60-100
                const level = levels.find(l => l.id === levelId);
                
                completeLevel(levelId, score);
                
                alert(`🎉 เล่นจบแล้ว!\nคะแนน: ${score}/${level.requiredScore}\n${score >= level.requiredScore ? 'ผ่าน! 🎊' : 'ยังไม่ผ่าน ลองใหม่อีกครั้ง 💪'}`);
            }, 2000);
        }

        // จบด่าน
        function completeLevel(levelId, score) {
            const level = levels.find(l => l.id === levelId);
            const passed = score >= level.requiredScore;
            
            // อัพเดทข้อมูล
            if (!gameData.userProgress[levelId]) {
                gameData.userProgress[levelId] = {};
            }
            
            gameData.userProgress[levelId].score = Math.max(gameData.userProgress[levelId].score || 0, score);
            
            if (passed && !gameData.userProgress[levelId].completed) {
                gameData.userProgress[levelId].completed = true;
                gameData.completedLevels++;
                gameData.totalScore += level.reward;
                
                // ปลดล็อกด่านต่อไป
                if (levelId < levels.length) {
                    levels[levelId].unlocked = true;
                }
                
                // เช็คความสำเร็จ
                checkAchievements();
            }
            
            saveGameData();
            updateUI();
        }

        // เช็คความสำเร็จ
        function checkAchievements() {
            // ก้าวแรก
            if (gameData.completedLevels >= 1) {
                achievements.find(a => a.id === 'first_level').earned = true;
            }
            
            // นักผจญภัย
            if (gameData.completedLevels >= 5) {
                achievements.find(a => a.id === 'level_crusher').earned = true;
            }
        }

        // รีเซ็ตเกม (สำหรับทดสอบ)
        function resetGame() {
            if (confirm('ต้องการรีเซ็ตความก้าวหน้าทั้งหมดหรือไม่?')) {
                localStorage.removeItem('englishGameData');
                gameData = {
                    currentLevel: 1,
                    totalScore: 0,
                    completedLevels: 0,
                    currentStreak: 5,
                    userProgress: {}
                };
                
                // รีเซ็ตการปลดล็อก
                levels.forEach((level, index) => {
                    level.unlocked = index === 0;
                });
                
                // รีเซ็ตความสำเร็จ
                achievements.forEach(achievement => {
                    achievement.earned = achievement.id === 'streak_master';
                });
                
                updateUI();
            }
        }

        // เริ่มต้นระบบ
        document.addEventListener('DOMContentLoaded', function() {
            loadGameData();
        });

        // เพิ่มปุ่มรีเซ็ตสำหรับทดสอบ (ซ่อนในเวอร์ชันจริง)
        document.addEventListener('keydown', function(e) {
            if (e.ctrlKey && e.shiftKey && e.key === 'R') {
                resetGame();
            }
        });
    </script>
</body>
</html>
