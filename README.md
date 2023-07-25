<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Voice Dictation Calculator</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <input type="text" id="numberInput" placeholder="Type numbers separated by spaces...">
        <button id="clearButton">Clear</button>
        <button id="sumButton">Sum</button>
    </div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/annyang/2.6.1/annyang.min.js"></script>
    <script>
        const numberInput = document.getElementById('numberInput');
        const clearButton = document.getElementById('clearButton');
        const sumButton = document.getElementById('sumButton');

        numberInput.addEventListener('keyup', function(event) {
            if (event.code === 'Space') {
                const numbers = numberInput.value.trim().split(' ');
                const lastNumber = numbers[numbers.length - 1];
                if (!isNaN(lastNumber)) {
                    speakNumber(lastNumber);
                } else if (lastNumber !== '') {
                    alert(`Invalid input: "${lastNumber}". Please enter a valid number.`);
                }
            }
        });

        function speakNumber(number) {
            const utterance = new SpeechSynthesisUtterance(number);
            utterance.rate = 2.5; // Adjust the rate value to control speaking speed
            speechSynthesis.speak(utterance);
        }

        function sumNumbers(numbers) {
            const numArr = numbers.split(' ').map(Number);
            const sum = numArr.reduce((acc, curr) => acc + curr, 0);
            return sum;
        }

        // Initialize annyang
        if (annyang) {
            const commands = {
                'clear': function () {
                    numberInput.value = '';
                },
                'calculate *expr': function (expr) {
                    try {
                        const result = eval(expr);
                        numberInput.value = result;
                        speakNumber(result);
                    } catch (error) {
                        alert('Invalid expression. Please try again.');
                    }
                },
                'sum numbers': function () {
                    const numbers = numberInput.value.trim();
                    const sum = sumNumbers(numbers);
                    numberInput.value = sum;
                    speakNumber(sum);
                }
            };

            annyang.addCommands(commands);
            annyang.start();
        }

        clearButton.addEventListener('click', function () {
            numberInput.value = '';
        });

        sumButton.addEventListener('click', function () {
            const numbers = numberInput.value.trim();
            const sum = sumNumbers(numbers);
            numberInput.value = sum;
        });
    </script>
    <style>
        /* ... (the rest of the CSS styles remain unchanged) ... */
    </style>
</body>
</html>

