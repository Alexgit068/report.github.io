<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
    <title>Отчет о продажах</title>
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .container {
            width: 100%;
            max-width: 500px;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
        }

        .header {
            background: #0088cc;
            color: white;
            padding: 25px 20px;
            text-align: center;
        }

        .header h1 {
            font-size: 24px;
            font-weight: 700;
            margin-bottom: 8px;
        }

        .header p {
            font-size: 14px;
            opacity: 0.9;
        }

        .form {
            padding: 30px;
        }

        .form-group {
            margin-bottom: 24px;
        }

        .form-group label {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 10px;
            font-weight: 600;
            color: #333;
            font-size: 16px;
        }

        .form-group label svg {
            width: 20px;
            height: 20px;
            color: #0088cc;
        }

        .form-control {
            width: 100%;
            padding: 14px;
            border: 2px solid #e1e5e9;
            border-radius: 12px;
            font-size: 16px;
            transition: border-color 0.3s ease;
        }

        .form-control:focus {
            outline: none;
            border-color: #0088cc;
            box-shadow: 0 0 0 3px rgba(0, 136, 204, 0.1);
        }

        .form-control option {
            font-size: 16px;
        }

        .submit-btn {
            width: 100%;
            padding: 16px;
            background: #0088cc;
            color: white;
            border: none;
            border-radius: 12px;
            font-size: 18px;
            font-weight: 600;
            cursor: pointer;
            transition: background 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .submit-btn:hover:not(:disabled) {
            background: #0077b6;
        }

        .submit-btn:disabled {
            opacity: 0.7;
            cursor: not-allowed;
        }

        .spinner {
            width: 20px;
            height: 20px;
            border: 2px solid white;
            border-top: 2px solid transparent;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .message {
            padding: 16px;
            border-radius: 12px;
            margin-top: 20px;
            text-align: center;
            font-weight: 500;
        }

        .success {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .error {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .hidden {
            display: none;
        }

        /* Telegram-specific styling */
        @media (max-width: 500px) {
            .container {
                border-radius: 0;
                box-shadow: none;
            }
            
            body {
                padding: 0;
                background: white;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>📊 Отчет о продажах</h1>
            <p>Заполните данные для анализа</p>
        </div>
        
        <form class="form" id="reportForm">
            <div class="form-group">
                <label>
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
                    </svg>
                    Дата продажи
                </label>
                <input type="date" id="saleDate" class="form-control" required>
            </div>
            
            <div class="form-group">
                <label>
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                    </svg>
                    Менеджер
                </label>
                <select id="manager" class="form-control" required>
                    <option value="">Выберите менеджера</option>
                    <option value="Алексей Иванов">Алексей Иванов</option>
                    <option value="Мария Петрова">Мария Петрова</option>
                    <option value="Дмитрий Сидоров">Дмитрий Сидоров</option>
                    <option value="Елена Козлова">Елена Козлова</option>
                    <option value="Сергей Морозов">Сергей Морозов</option>
                </select>
            </div>
            
            <div class="form-group">
                <label>
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 12l3-3 3 3 4-4M8 21l4-4 4 4M3 4h18M4 4h16v12a1 1 0 01-1 1H5a1 1 0 01-1-1V4z" />
                    </svg>
                    Сегмент
                </label>
                <select id="segment" class="form-control" required>
                    <option value="">Выберите сегмент</option>
                    <option value="B2B">B2B</option>
                    <option value="B2C">B2C</option>
                    <option value="Корпоративный">Корпоративный</option>
                    <option value="Розница">Розница</option>
                    <option value="Онлайн">Онлайн</option>
                </select>
            </div>
            
            <div class="form-group">
                <label>
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 7l-8-4-8 4m16 0l-8 4m8-4v10l-8 4m0-10L4 7m8 4v10M4 7v10l8 4" />
                    </svg>
                    Товар/Услуга
                </label>
                <select id="product" class="form-control" required>
                    <option value="">Выберите товар/услугу</option>
                    <option value="Продукт A">Продукт A</option>
                    <option value="Продукт B">Продукт B</option>
                    <option value="Продукт C">Продукт C</option>
                    <option value="Услуга X">Услуга X</option>
                    <option value="Услуга Y">Услуга Y</option>
                </select>
            </div>
            
            <button type="submit" class="submit-btn" id="submitBtn">
                <span>Отправить отчет</span>
            </button>
            
            <div id="message" class="message hidden"></div>
        </form>
    </div>

    <script>
        // Telegram WebApp initialization
        const tg = window.Telegram.WebApp;
        tg.expand();
        tg.MainButton.textColor = "#FFFFFF";
        tg.MainButton.color = "#0088cc";

        // Set max date to today
        const today = new Date().toISOString().split('T')[0];
        document.getElementById('saleDate').max = today;
        document.getElementById('saleDate').value = today;

        document.getElementById('reportForm').addEventListener('submit', async function(e) {
            e.preventDefault();
            
            const submitBtn = document.getElementById('submitBtn');
            const messageDiv = document.getElementById('message');
            
            // Get form data
            const formData = {
                saleDate: document.getElementById('saleDate').value,
                manager: document.getElementById('manager').value,
                segment: document.getElementById('segment').value,
                product: document.getElementById('product').value
            };
            
            // Basic validation
            if (!formData.saleDate || !formData.manager || !formData.segment || !formData.product) {
                showMessage('Пожалуйста, заполните все поля', 'error');
                return;
            }
            
            // Show loading state
            submitBtn.disabled = true;
            submitBtn.innerHTML = '<div class="spinner"></div> Отправка...';
            
            try {
                // In a real implementation, you would send this data to your backend
                // which would then write to Google Sheets
                // Example: await sendDataToBackend(formData);
                
                // Simulate API call delay
                await new Promise(resolve => setTimeout(resolve, 1500));
                
                // Show success message
                showMessage('✅ Отчет успешно отправлен в Google Таблицу!', 'success');
                
                // Close the WebApp after 2 seconds
                setTimeout(() => {
                    tg.close();
                }, 2000);
                
            } catch (error) {
                console.error('Error:', error);
                showMessage('❌ Ошибка при отправке данных. Попробуйте позже.', 'error');
                submitBtn.disabled = false;
                submitBtn.innerHTML = 'Отправить отчет';
            }
        });
        
        function showMessage(text, type) {
            const messageDiv = document.getElementById('message');
            messageDiv.textContent = text;
            messageDiv.className = `message ${type}`;
            messageDiv.classList.remove('hidden');
            
            // Auto-hide success message
            if (type === 'success') {
                setTimeout(() => {
                    messageDiv.classList.add('hidden');
                }, 3000);
            }
        }
        
        // Handle back button press
        tg.BackButton.show();
        tg.BackButton.onClick(() => {
            tg.close();
        });
    </script>
</body>
</html>
