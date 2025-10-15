<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>鸡蛋超人无抗小课堂</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Microsoft YaHei', sans-serif; }
        body { background: linear-gradient(135deg, #ffcc00, #ff9900); min-height: 100vh; display: flex; justify-content: center; align-items: center; padding: 20px; }
        .container { background: white; border-radius: 20px; box-shadow: 0 15px 35px rgba(0,0,0,0.3); width: 100%; max-width: 800px; padding: 40px; text-align: center; }
        h1 { color: #e53935; margin-bottom: 20px; font-size: 32px; }
        h2 { color: #2c3e50; margin-bottom: 20px; }
        p { color: #34495e; margin-bottom: 20px; line-height: 1.6; font-size: 18px; }
        .btn { background: #e53935; color: white; border: none; border-radius: 50px; padding: 15px 30px; font-size: 18px; cursor: pointer; margin: 10px; transition: all 0.3s; }
        .btn:hover { background: #c62828; transform: translateY(-3px); }
        .role-selection { display: flex; justify-content: center; flex-wrap: wrap; gap: 20px; margin: 30px 0; }
        .role-card { background: white; border-radius: 15px; padding: 25px 20px; width: 200px; box-shadow: 0 8px 20px rgba(0,0,0,0.1); cursor: pointer; border: 3px solid #ff9800; }
        .role-card:hover { transform: translateY(-5px); box-shadow: 0 15px 30px rgba(0,0,0,0.2); }
        .success { background: #d4edda; border: 1px solid #c3e6cb; border-radius: 10px; padding: 20px; margin: 20px 0; color: #155724; }
        .token { background: #333; color: #fff; padding: 15px; border-radius: 8px; margin: 15px 0; font-family: monospace; }
    </style>
</head>
<body>
    <div class="container">
        <div id="intro-screen">
            <h1>🥚 鸡蛋超人无抗小课堂</h1>
            <p>学习无抗鸡蛋知识，找到最适合你的那一款！</p>
            
            <div class="success">
                <p><strong>✅ GitHub Pages 部署成功！</strong></p>
                <p>网站地址：<code id="site-url">正在加载...</code></p>
            </div>
            
            <h2>选择你的身份：</h2>
            <div class="role-selection">
                <div class="role-card" onclick="startGame('family')">
                    <h3>🛡️ 家庭守护者</h3>
                    <p>守护家人健康</p>
                </div>
                <div class="role-card" onclick="startGame('nutrition')">
                    <h3>🌟 营养未来星</h3>
                    <p>探索美食奥秘</p>
                </div>
                <div class="role-card" onclick="startGame('student')">
                    <h3>🔬 大学生</h3>
                    <p>追求健康生活</p>
                </div>
            </div>
        </div>
        
        <div id="game-screen" style="display: none;">
            <h2 id="game-title">家庭安全守护者课堂</h2>
            <div id="question-container">
                <p id="question-text">什么是健康蛋最重要的特征？</p>
                <div id="options-container"></div>
                <button class="btn" onclick="checkAnswer()" id="submit-btn" style="display: none;">提交答案</button>
            </div>
            <button class="btn" onclick="showIntro()">返回首页</button>
        </div>
        
        <div id="reward-screen" style="display: none;">
            <h1>🏆 恭喜完成！</h1>
            <div class="success">
                <p>你已成功掌握无抗鸡蛋知识！</p>
            </div>
            <div class="token" id="token-display">EGG-TOKEN-12345</div>
            <p>保存此令牌，享受优惠</p>
            <button class="btn" onclick="showIntro()">再次挑战</button>
        </div>
    </div>

    <script>
        // 显示当前网站URL
        document.getElementById('site-url').textContent = window.location.href;
        
        const questions = [
            {
                question: "挑选让家人安心的鸡蛋，你最应该看什么？",
                options: [
                    "A. 是否是品牌鸡蛋",
                    "B. 蛋壳上有没有溯源喷码",
                    "C. 是否有绿色认证标识"
                ],
                correct: 1,
                tip: "品牌蛋并不是衡量健康蛋的标准！正大鸡蛋的喷码'身份证'，一扫就能看到每颗蛋的全程来源与检测报告，安全透明才是真安心。"
            }
        ];
        
        let currentRole = '';
        let selectedAnswer = -1;
        
        function startGame(role) {
            currentRole = role;
            document.getElementById('intro-screen').style.display = 'none';
            document.getElementById('game-screen').style.display = 'block';
            
            if (role === 'family') {
                document.getElementById('game-title').textContent = '家庭安全守护者课堂';
            }
            
            loadQuestion(0);
        }
        
        function loadQuestion(index) {
            const question = questions[index];
            document.getElementById('question-text').textContent = question.question;
            
            const optionsContainer = document.getElementById('options-container');
            optionsContainer.innerHTML = '';
            
            question.options.forEach((option, i) => {
                const div = document.createElement('div');
                div.className = 'role-card';
                div.style.cursor = 'pointer';
                div.style.margin = '10px 0';
                div.textContent = option;
                div.onclick = () => selectAnswer(i, div);
                optionsContainer.appendChild(div);
            });
            
            document.getElementById('submit-btn').style.display = 'none';
            selectedAnswer = -1;
        }
        
        function selectAnswer(index, element) {
            // 移除所有选项的选中样式
            document.querySelectorAll('#options-container .role-card').forEach(card => {
                card.style.background = 'white';
                card.style.borderColor = '#ff9800';
            });
            
            // 选中当前选项
            element.style.background = '#d5f5e3';
            element.style.borderColor = '#27ae60';
            selectedAnswer = index;
            
            document.getElementById('submit-btn').style.display = 'block';
        }
        
        function checkAnswer() {
            if (selectedAnswer === -1) return;
            
            const question = questions[0];
            const options = document.querySelectorAll('#options-container .role-card');
            
            // 显示正确答案
            options[question.correct].style.background = '#d5f5e3';
            options[question.correct].style.borderColor = '#27ae60';
            
            // 显示提示
            const tipDiv = document.createElement('div');
            tipDiv.className = 'success';
            tipDiv.innerHTML = `<p><strong>鸡蛋超人告诉你：</strong><br>${question.tip}</p>`;
            document.getElementById('question-container').appendChild(tipDiv);
            
            // 延迟后显示奖励
            setTimeout(() => {
                document.getElementById('game-screen').style.display = 'none';
                document.getElementById('reward-screen').style.display = 'block';
                
                // 生成随机令牌
                const token = 'EGG-' + currentRole.toUpperCase() + '-' + Math.random().toString(36).substr(2, 8).toUpperCase();
                document.getElementById('token-display').textContent = token;
            }, 3000);
        }
        
        function showIntro() {
            document.getElementById('intro-screen').style.display = 'block';
            document.getElementById('game-screen').style.display = 'none';
            document.getElementById('reward-screen').style.display = 'none';
        }
        
        // 页面加载完成
        console.log('鸡蛋超人游戏已加载');
        console.log('当前URL:', window.location.href);
    </script>
</body>
</html># nana.github.io
