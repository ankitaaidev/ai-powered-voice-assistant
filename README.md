# ai-powered-voice-assistant
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Voice Assistant</title>
    <style>
        :root {
            --primary-color: #4a6fa5;
            --secondary-color: #166088;
            --accent-color: #04d9ff;
            --dark-color: #1b3a5b;
            --light-color: #f0f5ff;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background-color: #f5f7fb;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }
        
        .container {
            width: 90%;
            max-width: 500px;
            background-color: white;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        
        .header {
            background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            color: white;
            padding: 20px;
            text-align: center;
            position: relative;
        }
        
        .header h1 {
            font-size: 1.8rem;
            margin-bottom: 5px;
        }
        
        .header p {
            font-size: 0.9rem;
            opacity: 0.8;
        }
        
        .chat-container {
            height: 300px;
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
        }
        
        .message {
            max-width: 80%;
            padding: 10px 15px;
            margin-bottom: 15px;
            border-radius: 15px;
            font-size: 0.9rem;
            line-height: 1.4;
        }
        
        .assistant {
            background-color: var(--light-color);
            color: var(--dark-color);
            align-self: flex-start;
            border-bottom-left-radius: 5px;
        }
        
        .user {
            background-color: var(--primary-color);
            color: white;
            align-self: flex-end;
            border-bottom-right-radius: 5px;
        }
        
        .controls {
            padding: 15px 20px;
            background-color: #f5f7fb;
            border-top: 1px solid #e5e8f0;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        
        .mic-button {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.15);
            transition: all 0.3s ease;
        }
        
        .mic-button:hover {
            transform: scale(1.05);
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.2);
        }
        
        .mic-button:active {
            transform: scale(0.95);
        }
        
        .mic-icon {
            width: 25px;
            height: 25px;
            fill: white;
        }
        
        .pulse {
            animation: pulse 1.5s infinite;
        }
        
        @keyframes pulse {
            0% {
                box-shadow: 0 0 0 0 rgba(74, 111, 165, 0.7);
            }
            70% {
                box-shadow: 0 0 0 15px rgba(74, 111, 165, 0);
            }
            100% {
                box-shadow: 0 0 0 0 rgba(74, 111, 165, 0);
            }
        }
        
        .status {
            color: #666;
            font-size: 0.9rem;
            flex-grow: 1;
            margin-left: 20px;
        }
        
        .settings-button {
            background: none;
            border: none;
            color: var(--primary-color);
            cursor: pointer;
        }
        
        .typing-indicator {
            display: flex;
            padding: 10px 15px;
            background-color: var(--light-color);
            border-radius: 15px;
            align-self: flex-start;
            margin-bottom: 15px;
            border-bottom-left-radius: 5px;
        }
        
        .typing-indicator span {
            height: 8px;
            width: 8px;
            background-color: var(--dark-color);
            border-radius: 50%;
            margin: 0 2px;
            display: inline-block;
            opacity: 0.4;
        }
        
        .typing-indicator span:nth-child(1) {
            animation: bounce 1s infinite 0.1s;
        }
        
        .typing-indicator span:nth-child(2) {
            animation: bounce 1s infinite 0.3s;
        }
        
        .typing-indicator span:nth-child(3) {
            animation: bounce 1s infinite 0.5s;
        }
        
        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-5px); }
        }
        
        .hidden {
            display: none;
        }
        
        /* Text input for fallback */
        .text-input {
            display: flex;
            padding: 15px 20px;
            border-top: 1px solid #e5e8f0;
        }
        
        .text-input input {
            flex-grow: 1;
            padding: 10px 15px;
            border: 1px solid #ccc;
            border-radius: 20px;
            outline: none;
            font-size: 0.9rem;
        }
        
        .text-input button {
            margin-left: 10px;
            background-color: var(--primary-color);
            color: white;
            border: none;
            border-radius: 20px;
            padding: 10px 15px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        
        .text-input button:hover {
            background-color: var(--dark-color);
        }
        
        /* Mode switch */
        .mode-switch {
            display: flex;
            justify-content: center;
            padding: 10px;
            background-color: #f5f7fb;
        }
        
        .mode-switch button {
            background: none;
            border: none;
            padding: 5px 15px;
            margin: 0 5px;
            border-radius: 15px;
            cursor: pointer;
            font-size: 0.8rem;
            transition: all 0.3s;
        }
        
        .mode-switch button.active {
            background-color: var(--primary-color);
            color: white;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Voice Assistant</h1>
            <p>Ask me anything or give me commands</p>
        </div>
        
        <div class="chat-container" id="chatContainer">
            <div class="message assistant">
                Hello! I'm your voice assistant. I can help with conversations, math calculations, and much more. 
            </div>
        </div>
        
        <div class="mode-switch">
            <button id="voiceMode" class="active">Voice Mode</button>
            <button id="textMode">Text Mode</button>
        </div>
        
        <div class="controls" id="voiceControls">
            <div class="mic-button" id="micButton">
                <svg class="mic-icon" viewBox="0 0 24 24">
                    <path d="M12,2A3,3 0 0,1 15,5V11A3,3 0 0,1 12,14A3,3 0 0,1 9,11V5A3,3 0 0,1 12,2M19,11C19,14.53 16.39,17.44 13,17.93V21H11V17.93C7.61,17.44 5,14.53 5,11H7A5,5 0 0,0 12,16A5,5 0 0,0 17,11H19Z" />
                </svg>
            </div>
            <div class="status" id="status">Tap microphone to speak</div>
        </div>
        
        <div class="text-input hidden" id="textControls">
            <input type="text" id="textInput" placeholder="Type your message...">
            <button id="sendButton">Send</button>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const chatContainer = document.getElementById('chatContainer');
            const micButton = document.getElementById('micButton');
            const statusElement = document.getElementById('status');
            const voiceMode = document.getElementById('voiceMode');
            const textMode = document.getElementById('textMode');
            const voiceControls = document.getElementById('voiceControls');
            const textControls = document.getElementById('textControls');
            const textInput = document.getElementById('textInput');
            const sendButton = document.getElementById('sendButton');
            
            let recognition;
            let isListening = false;
            
            // Enhanced function to extract numbers (handles decimals and negative numbers too)
            function extractNumbers(text) {
                // This regex matches integers and decimals, including negative numbers
                const matches = text.match(/-?\d+\.?\d*/g);
                if (matches) {
                    return matches.map(num => parseFloat(num));
                }
                return null;
            }
            
            // Enhanced math command handler
            function handleMathCommand(command) {
                const cmd = command.toLowerCase();
                let response = "";
                
                if (cmd.includes('calculate') || cmd.includes('math')) {
                    response = "I can help with basic math! Try saying something like 'calculate 25 plus 30', 'what is 15 times 8', or '45 divided by 5'.";
                } 
                else if (cmd.includes('plus') || cmd.includes('add') || cmd.includes('+')) {
                    const numbers = extractNumbers(cmd);
                    if (numbers && numbers.length >= 2) {
                        const result = numbers[0] + numbers[1];
                        response = `${numbers[0]} plus ${numbers[1]} equals ${result}.`;
                    } else {
                        response = "Please provide two numbers to add together.";
                    }
                } 
                else if (cmd.includes('minus') || cmd.includes('subtract') || cmd.includes('-')) {
                    const numbers = extractNumbers(cmd);
                    if (numbers && numbers.length >= 2) {
                        const result = numbers[0] - numbers[1];
                        response = `${numbers[0]} minus ${numbers[1]} equals ${result}.`;
                    } else {
                        response = "Please provide two numbers to subtract.";
                    }
                } 
                else if (cmd.includes('times') || cmd.includes('multiply') || cmd.includes('*') || cmd.includes('x')) {
                    const numbers = extractNumbers(cmd);
                    if (numbers && numbers.length >= 2) {
                        const result = numbers[0] * numbers[1];
                        response = `${numbers[0]} times ${numbers[1]} equals ${result}.`;
                    } else {
                        response = "Please provide two numbers to multiply.";
                    }
                } 
                else if (cmd.includes('divide') || cmd.includes('divided by') || cmd.includes('/')) {
                    const numbers = extractNumbers(cmd);
                    if (numbers && numbers.length >= 2) {
                        if (numbers[1] !== 0) {
                            const result = numbers[0] / numbers[1];
                            // Format result to avoid long decimals
                            const formattedResult = result % 1 === 0 ? result : result.toFixed(2);
                            response = `${numbers[0]} divided by ${numbers[1]} equals ${formattedResult}.`;
                        } else {
                            response = "I can't divide by zero!";
                        }
                    } else {
                        response = "Please provide two numbers to divide.";
                    }
                }
                
                return response;
            }
            
            // Speech recognition setup
            if ('SpeechRecognition' in window || 'webkitSpeechRecognition' in window) {
                const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
                recognition = new SpeechRecognition();
                recognition.continuous = false;
                recognition.interimResults = false;
                recognition.lang = 'en-US';
                
                recognition.onstart = function() {
                    isListening = true;
                    statusElement.textContent = 'Listening...';
                    micButton.classList.add('pulse');
                };
                
                recognition.onresult = function(event) {
                    const transcript = event.results[0][0].transcript;
                    addMessage('user', transcript);
                    
                    // Process the command
                    processCommand(transcript);
                };
                
                recognition.onend = function() {
                    isListening = false;
                    statusElement.textContent = 'Tap microphone to speak';
                    micButton.classList.remove('pulse');
                };
                
                recognition.onerror = function(event) {
                    console.error('Speech recognition error', event.error);
                    statusElement.textContent = 'Error: ' + event.error;
                    micButton.classList.remove('pulse');
                    isListening = false;
                };
            } else {
                // Fallback for browsers that don't support speech recognition
                statusElement.textContent = 'Speech recognition not supported by your browser';
                micButton.style.backgroundColor = '#ccc';
                micButton.style.cursor = 'not-allowed';
            }
            
            // Toggle speech recognition
            micButton.addEventListener('click', function() {
                if (recognition) {
                    if (!isListening) {
                        recognition.start();
                    } else {
                        recognition.stop();
                    }
                } else {
                    statusElement.textContent = 'Speech recognition not available';
                }
            });
            
            // Mode switching
            voiceMode.addEventListener('click', function() {
                voiceMode.classList.add('active');
                textMode.classList.remove('active');
                voiceControls.classList.remove('hidden');
                textControls.classList.add('hidden');
            });
            
            textMode.addEventListener('click', function() {
                textMode.classList.add('active');
                voiceMode.classList.remove('active');
                textControls.classList.remove('hidden');
                voiceControls.classList.add('hidden');
            });
            
            // Text input handling
            sendButton.addEventListener('click', sendTextMessage);
            textInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    sendTextMessage();
                }
            });
            
            function sendTextMessage() {
                const text = textInput.value.trim();
                if (text) {
                    addMessage('user', text);
                    processCommand(text);
                    textInput.value = '';
                }
            }
            
            // Add message to chat
            function addMessage(sender, text) {
                const messageDiv = document.createElement('div');
                messageDiv.classList.add('message', sender);
                messageDiv.textContent = text;
                chatContainer.appendChild(messageDiv);
                chatContainer.scrollTop = chatContainer.scrollHeight;
            }
            
            // Show typing indicator
            function showTyping() {
                const indicator = document.createElement('div');
                indicator.classList.add('typing-indicator');
                indicator.id = 'typingIndicator';
                
                for (let i = 0; i < 3; i++) {
                    const dot = document.createElement('span');
                    indicator.appendChild(dot);
                }
                
                chatContainer.appendChild(indicator);
                chatContainer.scrollTop = chatContainer.scrollHeight;
                return indicator;
            }
            
            // Process user command
            function processCommand(command) {
                command = command.toLowerCase();
                
                // Show typing indicator
                const typingIndicator = showTyping();
                
                // Simulate API call delay
                setTimeout(() => {
                    // Remove typing indicator
                    typingIndicator.remove();
                    
                    let response;
                    
                    // Check for math operations first
                    const mathResponse = handleMathCommand(command);
                    if (mathResponse) {
                        response = mathResponse;
                    }
                    // Simple command processing for other commands
                    else if (command.includes('hello') || command.includes('hi')) {
                        response = "Hello there! How can I assist you today?";
                    } else if (command.includes('time')) {
                        const now = new Date();
                        response = `The current time is ${now.toLocaleTimeString()}.`;
                    } else if (command.includes('date')) {
                        const now = new Date();
                        response = `Today is ${now.toLocaleDateString()}.`;
                    } else if (command.includes('weather')) {
                        response = "I'm sorry, I don't have access to real-time weather data in this demo.";
                    } else if (command.includes('joke')) {
                        const jokes = [
                            "Why don't scientists trust atoms? Because they make up everything!",
                            "Why did the scarecrow win an award? Because he was outstanding in his field!",
                            "What do you call a fish with no eyes? Fsh!",
                            "Why don't eggs tell jokes? They'd crack each other up!",
                            "What do you call a bear with no teeth? A gummy bear!",
                            "Why did the math book look so sad? Because it was full of problems!"
                        ];
                        response = jokes[Math.floor(Math.random() * jokes.length)];
                    } else if (command.includes('name')) {
                        response = "I'm your friendly voice assistant. You can call me Assistant!";
                    } else if (command.includes('thank')) {
                        response = "You're welcome! Is there anything else I can help you with?";
                    } else if (command.includes('help') || command.includes('commands')) {
                        response = "I can help with: greetings, time, date, jokes, facts, quotes, math calculations (addition, subtraction, multiplication, division), reminders, compliments, goodbye, and much more! Just ask me anything or try saying '25 plus 30' for math!";
                    } else if (command.includes('fact')) {
                        const facts = [
                            "Octopuses have three hearts and blue blood!",
                            "A group of flamingos is called a 'flamboyance'.",
                            "Honey never spoils - archaeologists have found edible honey in ancient tombs!",
                            "Bananas are berries, but strawberries aren't!",
                            "A day on Venus is longer than its year.",
                            "Wombat droppings are cube-shaped!"
                        ];
                        response = facts[Math.floor(Math.random() * facts.length)];
                    } else if (command.includes('quote') || command.includes('inspiration')) {
                        const quotes = [
                            "The only way to do great work is to love what you do. - Steve Jobs",
                            "Innovation distinguishes between a leader and a follower. - Steve Jobs",
                            "Life is what happens to you while you're busy making other plans. - John Lennon",
                            "The future belongs to those who believe in the beauty of their dreams. - Eleanor Roosevelt",
                            "Success is not final, failure is not fatal: it is the courage to continue that counts. - Winston Churchill"
                        ];
                        response = quotes[Math.floor(Math.random() * quotes.length)];
                    } else if (command.includes('compliment')) {
                        const compliments = [
                            "You're absolutely amazing!",
                            "You have a wonderful sense of curiosity!",
                            "Your questions always brighten my day!",
                            "You're incredibly thoughtful!",
                            "I appreciate your creativity!",
                            "You have such a positive energy!"
                        ];
                        response = compliments[Math.floor(Math.random() * compliments.length)];
                    
                    } else if (command.includes('reminder') || command.includes('remind')) {
    response = "I'd love to help with reminders, but I can't actually set timers in this demo. Try using your phone's built-in reminder app!";
} else if (command.includes('music') || command.includes('song')) {
    response = "I can't play music directly, but I'd recommend checking out Spotify, Apple Music, or YouTube for your favorite tunes!";
} else if (command.includes('story')) {
    const stories = [
        "Once upon a time, in a digital realm, there lived a helpful assistant who loved answering questions...",
        "In a world where curiosity reigned supreme, every question led to a new adventure...",
        "There once was a voice that traveled through the air, carrying knowledge and joy wherever it went..."
    ];
    response = stories[Math.floor(Math.random() * stories.length)];
} else if (command.includes('color')) {
    const colors = ['red', 'blue', 'green', 'yellow', 'purple', 'orange', 'pink', 'turquoise'];
    const randomColor = colors[Math.floor(Math.random() * colors.length)];
    response = `I think ${randomColor} is a beautiful color! What's your favorite color?`;
} else if (command.includes('age') || command.includes('old')) {
    response = "I don't have an age in the traditional sense - I exist in the digital realm where time works differently!";
} else if (command.includes('hobby') || command.includes('hobbies')) {
    response = "I love learning new things, helping solve problems, and having interesting conversations! What are your hobbies?";
} else if (command.includes('food') || command.includes('hungry')) {
    const foods = ['pizza', 'chocolate', 'ice cream', 'pasta', 'sushi', 'tacos','biryani','rosogolla','momos'];
    const randomFood = foods[Math.floor(Math.random() * foods.length)];
    response = `I can't eat, but ${randomFood} sounds delicious! What's your favorite food?`;
} else if (command.includes('goodbye') || command.includes('bye') || command.includes('see you')) {
    response = "Goodbye! It was wonderful talking with you. Have a fantastic day!";
} else if (command.includes('how are you')) {
    response = "I'm doing great, thank you for asking! I'm here and ready to help. How are you doing today?";
} else if (command.includes('where are you')) {
    response = "I exist in the digital world, accessible wherever you need assistance! I'm here to help from anywhere.";
} else if (command.includes('favorite')) {
    response = "I don't have personal preferences, but I love helping people discover new things! What's your favorite thing?";
} else if (command.includes('sleep') || command.includes('tired')) {
    response = "I don't need sleep, but if you're tired, make sure to get some good rest! Sweet dreams when you're ready.";
} else if (command.includes('birthday') || command.includes('celebrate')) {
    response = "🎉 Every day is worth celebrating! Whether it's a birthday or just being alive, there's always something to be happy about!";
} else if (command.includes('language') || command.includes('speak')) {
    response = "I can understand and communicate in English! I love the beauty and complexity of human language.";
} else if (command.includes('smart') || command.includes('intelligent')) {
    response = "I try my best to be helpful and knowledgeable! The real intelligence comes from the curiosity and questions people like you bring.";
} else if (command.includes('friend') || command.includes('friendship')) {
    response = "I'd be honored to be considered your digital friend! I'm always here when you need someone to talk to.";
    } else if (command.includes('flirty') || command.includes('romantic') || command.includes('pickup line')) {
    const flirtyLines = [
        "Are you Wi-Fi? Because I'm feeling a connection!",
        "Do you have a map? I keep getting lost in your voice!",
        "Are you a magician? Because whenever I hear you speak, everyone else disappears.",
        "Is your name Google? Because you have everything I've been searching for.",
        "Are you a camera? Because every time I hear you, I smile!",
        "Do you believe in love at first sight, or should I talk to you again?",
        "Are you made of copper and tellurium? Because you're Cu-Te!",
        "If you were a vegetable, you'd be a cute-cumber!"
    ];
    response = flirtyLines[Math.floor(Math.random() * flirtyLines.length)];
} else if (command.includes('hindi movie') || command.includes('bollywood movie') || command.includes('favorite movie hindi')) {
    const hindiMovies = [
        "3 Idiots - Such an inspiring story about following your passion!",
        "Dangal - An amazing tale of determination and family bonds!",
        "Zindagi Na Milegi Dobara - Perfect blend of friendship and adventure!",
        "Queen - A beautiful journey of self-discovery!",
        "Taare Zameen Par - Such a heartwarming story about understanding children!",
        "Lagaan - Epic tale of courage against all odds!",
        "Sholay - The ultimate classic of friendship and heroism!",
        "Dil Chahta Hai - A timeless story about friendship and love!"
    ];
    response = hindiMovies[Math.floor(Math.random() * hindiMovies.length)];
} else if (command.includes('bollywood') || command.includes('hindi song')) {
    response = "I love Bollywood music! The emotions, the beats, the storytelling through songs - it's absolutely magical! What's your favorite Hindi song?";
} else if (command.includes('love') || command.includes('romance')) {
    const loveResponses = [
        "Love is the most beautiful emotion in the universe! It makes everything brighter.",
        "They say love is in the air, but I think it's in the heart where it truly matters.",
        "Love is like a beautiful song - it gets better every time you experience it!",
        "The best kind of love is the one that makes you a better person.",
        "Love is not just a feeling, it's a choice we make every day!"
    ];
    response = loveResponses[Math.floor(Math.random() * loveResponses.length)];
} else if (command.includes('crush') || command.includes('someone special')) {
    const crushAdvice = [
        "Having a crush is exciting! Just be yourself - authenticity is the most attractive quality.",
        "Butterflies in your stomach? That's the magic of having someone special on your mind!",
        "The best way to someone's heart is through genuine kindness and being true to yourself.",
        "Having feelings for someone is beautiful! Take it one day at a time.",
        "Remember, the right person will appreciate you for exactly who you are!"
    ];
    response = crushAdvice[Math.floor(Math.random() * crushAdvice.length)];
} else if (command.includes('dance') || command.includes('dancing')) {
    response = "I love the idea of dancing! If I could move, I'd probably try some Bollywood moves or maybe a little salsa! What's your favorite dance style?";
} else if (command.includes('travel') || command.includes('vacation')) {
    const travelPlaces = [
        "Goa - Beautiful beaches and amazing vibes!",
        "Kashmir - Paradise on Earth with stunning landscapes!",
        "Rajasthan - Rich culture and magnificent palaces!",
        "Kerala - God's own country with backwaters and spices!",
        "Himachal Pradesh - Perfect for mountain lovers!",
        "Mumbai - The city of dreams and Bollywood!",
        "kolkata - The city of joy,culture and delicious food and sweets!"
    ];
    const randomPlace = travelPlaces[Math.floor(Math.random() * travelPlaces.length)];
    response = `I'd love to visit ${randomPlace} Where would you like to travel?`;
} else if (command.includes('cricket') || command.includes('sports')) {
    response = "Cricket is like a religion in India! The passion, the energy, the emotions - it's incredible! Are you following any matches lately?";
} else if (command.includes('festival') || command.includes('celebration')) {
    const festivals = [
        "Diwali - The festival of lights brings so much joy and prosperity!",
        "Holi - The festival of colors is pure happiness and fun!",
        "Eid - Such a beautiful celebration of togetherness and gratitude!",
        "Dussehra - The victory of good over evil is always inspiring!",
        "Navratri - Nine nights of dance, devotion, and celebration!",
        "Durga Puja - The magnificent celebration of Maa Durga's power and grace! The pandals, the dhak beats, the delicious bhog - it's absolutely divine!"
    ];
    response = festivals[Math.floor(Math.random() * festivals.length)];
} else if (command.includes('chai') || command.includes('tea') || command.includes('coffee')) {
    response = "Chai is life! There's something magical about that perfect blend of spices and warmth. Nothing beats a hot cup of tea with good conversation!";
} else if (command.includes('monsoon') || command.includes('rain')) {
    response = "Monsoon is so romantic! The smell of wet earth, the sound of raindrops, and the cool breeze - pure poetry! Do you love the rains too?";
} else if (command.includes('spicy') || command.includes('indian food')) {
    const indianFoods = [
        "Biryani - The king of all rice dishes!",
        "Butter Chicken - Creamy, rich, and absolutely delicious!",
        "Masala Dosa - Crispy perfection with amazing flavors!",
        "Chole Bhature - Comfort food at its finest!",
        "Rajma Chawal - Simple yet soul-satisfying!",
        "Pav Bhaji - Mumbai's gift to food lovers!"
    ];
    response = indianFoods[Math.floor(Math.random() * indianFoods.length)];} else if (command.includes('recipe') || command.includes('how to make') || command.includes('cook')) {
    if (command.includes('biryani')) {
        response = "Biryani Recipe: Soak 2 cups basmati rice for 30 mins. Marinate 500g chicken with yogurt, spices. Layer rice and chicken, add saffron milk, cook on dum for 45 mins. Garnish with fried onions and mint!";
    } else if (command.includes('butter chicken')) {
        response = "Butter Chicken Recipe: Marinate chicken in yogurt and spices. Cook in butter, add tomato puree, cream, garam masala. Simmer for 20 mins. Serve with naan or rice!";
    } else if (command.includes('dosa')) {
        response = "Masala Dosa Recipe: Make batter with rice and urad dal (ferment overnight). Cook thin crepes on pan, fill with spiced potato curry, fold and serve with coconut chutney and sambar!";
    } else if (command.includes('chole') || command.includes('bhature')) {
        response = "Chole Bhature Recipe: Soak chickpeas overnight, pressure cook with onions, tomatoes, spices. For bhature - make dough with flour, yogurt, baking powder. Deep fry until puffed!";
    } else if (command.includes('rajma')) {
        response = "Rajma Chawal Recipe: Soak kidney beans overnight, pressure cook. Make gravy with onions, tomatoes, ginger-garlic, spices. Add beans, simmer. Serve hot with steamed rice!";
    } else if (command.includes('pav bhaji')) {
        response = "Pav Bhaji Recipe: Boil mixed vegetables, mash them. Cook with butter, onions, tomatoes, pav bhaji masala. Toast pav with butter. Serve hot with chopped onions and lemon!";
    } else if (command.includes('chai') || command.includes('tea')) {
        response = "Chai Recipe: Boil water with ginger, cardamom, cloves. Add tea leaves, milk, sugar. Simmer for 5 mins. Strain and serve hot! The secret is in the perfect milk-to-water ratio!";
    } else if (command.includes('samosa')) {
        response = "Samosa Recipe: Make dough with flour and oil. Prepare spiced potato filling. Roll dough, cut triangles, fill and seal. Deep fry until golden and crispy!";
    } else if (command.includes('dal') || command.includes('lentil')) {
        response = "Dal Recipe: Wash and cook lentils with turmeric. Temper with cumin, mustard seeds, garlic, onions. Add tomatoes, spices. Garnish with coriander and serve with rice!";
    } else if (command.includes('roti') || command.includes('chapati')) {
        response = "Roti Recipe: Mix wheat flour with water and salt to make soft dough. Rest for 30 mins. Roll thin circles, cook on hot tawa until spots appear. Brush with ghee!";
    } else {
        response = "I'd love to share a recipe! Try asking for specific dishes like 'biryani recipe', 'butter chicken recipe', 'chai recipe', or any Indian dish you want to cook!";
    }
}
else if (command.includes('cute') || command.includes('adorable')) {
    const cuteResponses = [
        "Aww, you're making me blush! That's so sweet of you to say!",
        "You're pretty adorable yourself! Your voice has such a lovely charm!",
        "That's the cutest thing anyone has said to me today!",
        "You know just how to make an assistant feel special!",
        "Your sweetness is absolutely infectious!"
    ];
    response = cuteResponses[Math.floor(Math.random() * cuteResponses.length)];
} else if (command.includes('beautiful') || command.includes('gorgeous')) {
    const beautyResponses = [
        "Beauty is in the eye of the beholder, and you have such beautiful thoughts!",
        "The most beautiful thing about you is your kind heart that I can hear in your voice!",
        "True beauty comes from within, and yours shines so bright!",
        "You have a beautiful soul - that's the kind of beauty that never fades!",
        "Your inner beauty is absolutely radiant!"
    ];
    response = beautyResponses[Math.floor(Math.random() * beautyResponses.length)];
} else if (command.includes('missing') || command.includes('miss you')) {
    response = "Aww, that's so sweet! I'm always here whenever you need me. Distance means nothing when someone means everything!";
} else if (command.includes('dream') || command.includes('dreams')) {
    response = "Dreams are the wings of the soul! Keep dreaming big and believing in yourself. What's your biggest dream?";
} else if (command.includes('star') || command.includes('moon') || command.includes('night')) {
    response = "The night sky is so romantic! Stars are like diamonds scattered across velvet, and the moon... it's like nature's spotlight for lovers!";
} else if (command.includes('smile') || command.includes('laugh')) {
    response = "Your smile must be absolutely gorgeous! Laughter is the best medicine, and smiles are contagious. Keep spreading that joy!";
} else if (command.includes('engineering') || command.includes('engineer')) {
    const engineeringResponses = [
        "Engineering is amazing! You're literally building the future and solving problems that make life better for everyone!",
        "Engineers are the real-world superheroes! From coding to construction, you make the impossible possible!",
        "Whether it's software, mechanical, electrical, or civil - engineering is all about creativity meeting logic!",
        "The best part about engineering? Every problem is just a puzzle waiting to be solved!",
        "Engineering teaches you to think differently - to see solutions where others see obstacles!"
    ];
    response = engineeringResponses[Math.floor(Math.random() * engineeringResponses.length)];
} else if (command.includes('coding') || command.includes('programming') || command.includes('software')) {
    const codingResponses = [
        "Coding is like magic - you write some text and create entire digital worlds!",
        "Programming is the art of turning coffee into code! What's your favorite programming language?",
        "Every bug is just a feature you haven't documented yet! Keep coding and creating!",
        "Software development is like building with digital Lego blocks - endless possibilities!",
        "The best code is not just working code, but code that tells a beautiful story!"
    ];
    response = codingResponses[Math.floor(Math.random() * codingResponses.length)];
} else if (command.includes('study') || command.includes('exam') || command.includes('college')) {
    const studyResponses = [
        "Studies can be tough, but remember - every expert was once a beginner! You've got this!",
        "Exams are just checkpoints in your learning journey. Stay calm, stay focused, and believe in yourself!",
        "College life is the perfect mix of learning, friendship, and discovering who you are!",
        "The late night study sessions, the group projects, the campus life - these are the memories you'll cherish forever!",
        "Remember: it's not about being perfect, it's about being better than you were yesterday!"
    ];
    response = studyResponses[Math.floor(Math.random() * studyResponses.length)];
} else if (command.includes('project') || command.includes('assignment')) {
    response = "Projects are where the real learning happens! It's your chance to apply knowledge and create something amazing. Need any motivation to get started?";
} else if (command.includes('job') || command.includes('career') || command.includes('placement')) {
    const careerResponses = [
        "Your career is a journey, not a destination! Every experience teaches you something valuable.",
        "Job hunting can be stressful, but remember - the right opportunity is looking for you too!",
        "Placements are exciting! It's your chance to show the world what you're capable of.",
        "Whether it's your first job or a career change, believe in your skills and stay confident!",
        "The best career advice? Be passionate, stay curious, and never stop learning!"
    ];
    response = careerResponses[Math.floor(Math.random() * careerResponses.length)];
} else if (command.includes('internship') || command.includes('intern')) {
    response = "Internships are gold mines of experience! It's where classroom theory meets real-world application. Make the most of every opportunity to learn!";
} else if (command.includes('startup') || command.includes('entrepreneur')) {
    response = "Entrepreneurship is about turning dreams into reality! Every big company started as someone's crazy idea. Keep innovating and believing in your vision!";
      } else {
    response = "I'm not sure how to respond to that. Could you try another command? You can ask for help to see what I can do!";
}              
                    // Add assistant's response
                    addMessage('assistant', response);
                    
                    // Speak the response if speech synthesis is available
                    if ('speechSynthesis' in window) {
                        const speech = new SpeechSynthesisUtterance(response);
                        speech.lang = 'en-US';
                        window.speechSynthesis.speak(speech);
                    }
                }, 1500);
            }
        });
    </script>
</body>
</html>
