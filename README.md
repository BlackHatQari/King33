<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Name Drop Game</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #1d1d1d;
            margin: 0;
            font-family: 'Arial', sans-serif;
            overflow: hidden;
            color: #fff;
        }

        .input-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-bottom: 20px;
        }

        input {
            padding: 10px;
            font-size: 20px;
            border: none;
            border-radius: 5px;
            margin-bottom: 10px;
        }

        button {
            padding: 10px 20px;
            font-size: 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .word-container {
            display: flex;
            padding: 10px;
            margin: 20px;
            justify-content: center;
            white-space: nowrap; /* Prevent line break */
            font-size: 60px; /* Default font size */
            font-weight: bold;
        }

        .letter {
            opacity: 0;
            position: relative;
            margin: 0 5px;
            transform: translateY(-100vh);
            animation: dropFromTop 1s forwards, colorChase 2s infinite alternate;
        }

        @keyframes dropFromTop {
            0% { transform: translateY(-100vh); opacity: 0; }
            100% { transform: translateY(0); opacity: 1; }
        }

        @keyframes colorChase {
            0% { color: red; }
            25% { color: orange; }
            50% { color: yellow; }
            75% { color: green; }
            100% { color: blue; }
        }

        #footer {
            margin-top: 20px;
            font-size: 20px;
            opacity: 0;
            transition: opacity 1s ease;
            white-space: nowrap;
            overflow: hidden;
            animation: typing 4s steps(30) forwards;
            text-align: center;
        }

        #playAgainButton {
            display: none;
            margin-top: 20px;
        }

    </style>
</head>
<body>

    <div class="input-container">
        <input type="text" id="username" placeholder="Enter your name" />
        <button id="startButton">Start</button>
    </div>

    <div id="container"></div>
    <div id="footer"></div>
    <button id="playAgainButton">Play Again</button>

    <script>
        const footer = document.getElementById('footer');
        const playAgainButton = document.getElementById('playAgainButton');

        document.getElementById('startButton').addEventListener('click', function() {
            const username = document.getElementById('username').value.trim();
            const container = document.getElementById('container');
            container.innerHTML = ''; // Clear previous characters
            footer.style.opacity = '0'; // Hide footer initially
            footer.textContent = ''; // Clear footer text

            if (username) {
                const wordContainer = document.createElement('div');
                wordContainer.classList.add('word-container');

                // Adjust font size if name has more than 10 characters
                if (username.length > 10) {
                    wordContainer.style.fontSize = '40px'; // Smaller font size for longer names
                } else {
                    wordContainer.style.fontSize = '60px'; // Default font size
                }

                // Create each letter and add animation
                [...username].forEach((letter, index) => {
                    const span = document.createElement('span');
                    span.classList.add('letter');
                    span.textContent = letter;

                    // Add drop animation and color chasing effect with delay for each letter
                    span.style.animationDelay = `${index * 0.3}s`; // Delay for dropping effect
                    wordContainer.appendChild(span);
                });

                container.appendChild(wordContainer);

                // Show footer after a delay
                setTimeout(() => {
                    footer.textContent = 'Developed by Qari King';
                    footer.style.opacity = '1';
                    playTypingEffect(footer.textContent);
                }, 1000 + username.length * 300);
            } else {
                alert('Please enter a name!');
            }

            // Hide the input and button
            document.querySelector('.input-container').style.display = 'none';
        });

        // Function to play typing effect
        function playTypingEffect(text) {
            footer.textContent = '';
            let i = 0;

            const typingInterval = setInterval(() => {
                if (i < text.length) {
                    footer.textContent += text.charAt(i);
                    i++;
                } else {
                    clearInterval(typingInterval);
                    playAgainButton.style.display = 'block';
                }
            }, 150);
        }

        // Play again button functionality
        playAgainButton.addEventListener('click', function() {
            document.getElementById('username').value = '';
            document.querySelector('.input-container').style.display = 'flex';
            playAgainButton.style.display = 'none';
            footer.style.opacity = '0';
            footer.textContent = '';
            document.getElementById('container').innerHTML = '';
        });
    </script>

</body>
</html>
