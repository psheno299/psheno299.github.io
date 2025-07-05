
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
        #chat-container {
            background-color: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            padding: 20px;
            height: 500px;
            display: flex;
            flex-direction: column;
        }
        #messages {
            flex-grow: 1;
            overflow-y: auto;
            margin-bottom: 15px;
            border-bottom: 1px solid #eee;
            padding-bottom: 15px;
        }
        .message {
            margin-bottom: 10px;
            padding: 8px 12px;
            border-radius: 18px;
            max-width: 70%;
            word-wrap: break-word;
        }
        .user-message {
            background-color: #007bff;
            color: white;
            margin-left: auto;
            border-bottom-right-radius: 4px;
        }
        .other-message {
            background-color: #e9ecef;
            color: black;
            margin-right: auto;
            border-bottom-left-radius: 4px;
        }
        #message-form {
            display: flex;
            gap: 10px;
        }
        #message-input {
            flex-grow: 1;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 20px;
            outline: none;
        }
        #send-button {
            padding: 10px 20px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 20px;
            cursor: pointer;
        }
        #send-button:hover {
            background-color: #0056b3;
        }
        #username-input {
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 20px;
            margin-bottom: 15px;
            width: 100%;
            box-sizing: border-box;
        }
    </style>
</head>
<body>
    <div id="chat-container">
        <input type="text" id="username-input" placeholder="Введите ваше имя">
        <div id="messages"></div>
        <form id="message-form">
            <input type="text" id="message-input" placeholder="Напишите сообщение..." autocomplete="off">
            <button type="submit" id="send-button">Отправить</button>
        </form>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const messagesContainer = document.getElementById('messages');
            const messageForm = document.getElementById('message-form');
            const messageInput = document.getElementById('message-input');
            const usernameInput = document.getElementById('username-input');
            
            // Загружаем сообщения из localStorage
            loadMessages();
            
            // Обработчик отправки формы
            messageForm.addEventListener('submit', function(e) {
                e.preventDefault();
                const messageText = messageInput.value.trim();
                const username = usernameInput.value.trim() || 'Аноним';
                
                if (messageText) {
                    // Создаем объект сообщения
                    const message = {
                        username: username,
                        text: messageText,
                        timestamp: new Date().toLocaleTimeString()
                    };
                    
                    // Добавляем сообщение в чат
                    addMessageToChat(message, true);
                    
                    // Сохраняем сообщение в localStorage
                    saveMessage(message);
                    
                    // Очищаем поле ввода
                    messageInput.value = '';
                }
            });
            
            // Функция добавления сообщения в чат
            function addMessageToChat(message, isUser) {
                const messageElement = document.createElement('div');
                messageElement.classList.add('message');
                messageElement.classList.add(isUser ? 'user-message' : 'other-message');
                
                const usernameElement = document.createElement('div');
                usernameElement.style.fontWeight = 'bold';
                usernameElement.style.marginBottom = '4px';
                usernameElement.textContent = message.username;
                
                const textElement = document.createElement('div');
                textElement.textContent = message.text;
                
                const timeElement = document.createElement('div');
                timeElement.style.fontSize = '0.8em';
                timeElement.style.textAlign = isUser ? 'right' : 'left';
                timeElement.style.marginTop = '4px';
                timeElement.textContent = message.timestamp;
                
                messageElement.appendChild(usernameElement);
                messageElement.appendChild(textElement);
                messageElement.appendChild(timeElement);
                
                messagesContainer.appendChild(messageElement);
                
                // Прокручиваем вниз
                messagesContainer.scrollTop = messagesContainer.scrollHeight;
            }
            
            // Функция сохранения сообщения в localStorage
            function saveMessage(message) {
                let messages = JSON.parse(localStorage.getItem('chatMessages') || '[]');
                messages.push(message);
                localStorage.setItem('chatMessages', JSON.stringify(messages));
            }
            
            // Функция загрузки сообщений из localStorage
            function loadMessages() {
                const messages = JSON.parse(localStorage.getItem('chatMessages') || '[]');
                messages.forEach(message => {
                    addMessageToChat(message, false);
                });
            }
            
            // Очистка чата (для тестирования)
            // localStorage.removeItem('chatMessages');
        });
    </script>
</body>
</html>
