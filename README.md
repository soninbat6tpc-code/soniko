!DOCTYPE html>
<html lang="mn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Монголын Түүхийн Тест (1189–1206 он)</title>
    <!-- Tailwind CSS-ийг дуудах -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700;800&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #eef2ff; /* Цайвар хөх дэвсгэр */
        }
        .question-card {
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            transition: transform 0.2s;
        }
        .option-label {
            cursor: pointer;
            transition: all 0.15s;
        }
        .option-label:hover {
            background-color: #e0f2fe; /* Цайвар хөх hover */
        }
        .submit-button {
            transition: background-color 0.2s, transform 0.2s;
        }
        .submit-button:hover {
            transform: translateY(-2px);
        }
    </style>
</head>
<body class="p-4 md:p-8">

    <div class="max-w-4xl mx-auto bg-white rounded-2xl p-6 md:p-10 shadow-2xl">
        <header class="text-center mb-10">
            <h1 class="text-3xl md:text-4xl font-extrabold text-indigo-700 mb-2">
                Монголын Түүхийн Тест
            </h1>
            <h2 class="text-xl font-semibold text-gray-600">
                Эзэнт Гүрний Үндэс (1189–1206 он)
            </h2>
        </header>

        <form id="quizForm">
            <!-- Асуултууд энд динамикаар байрлана -->
            <div id="questionsContainer" class="space-y-8">
                <!-- JavaScript-ээр бөглөгдөнө -->
            </div>
            
            <div class="flex justify-center mt-12">
                <button type="submit" id="submitBtn" class="submit-button bg-green-600 text-white font-bold py-3 px-10 rounded-xl text-lg shadow-lg hover:bg-green-700 focus:outline-none focus:ring-4 focus:ring-green-300">
                    Хариултыг Шалгах
                </button>
            </div>
        </form>

        <!-- Үр дүнгийн хэсэг -->
        <div id="results" class="mt-12 pt-8 border-t-2 border-gray-200 hidden">
            <h2 class="text-3xl font-bold text-center mb-6 text-indigo-800">
                Тестийн Үр Дүн
            </h2>
            <div id="scoreDisplay" class="text-center text-4xl font-extrabold text-green-600">
                <!-- Оноо энд харагдана -->
            </div>
            <div id="feedbackContainer" class="mt-8 space-y-4">
                <!-- Асуултын дэлгэрэнгүй хариу энд харагдана -->
            </div>
            <div class="flex justify-center mt-8">
                <button onclick="resetQuiz()" class="submit-button bg-red-500 text-white font-bold py-3 px-8 rounded-xl shadow-lg hover:bg-red-600 focus:outline-none focus:ring-4 focus:ring-red-300">
                    Дахин Эхлэх
                </button>
            </div>
        </div>

    </div>

    <script>
        // 1. Тестийн өгөгдөл (Өгөгдсөн файлын асуултууд)
        const quizData = [
            {
                question: "Тэмүжин Их Монгол Улсыг байгуулж, Чингис хаан цолыг хүртсэн түүхэн он хэд вэ?",
                options: ["1189 он", "1200 он", "1206 он", "1211 он"],
                correctAnswerIndex: 2 // В. 1206 он
            },
            {
                question: "1206 онд Чингис хаан цол хүртсэн Их Хуралдайг ямар голын эхэнд зохион байгуулсан бэ?",
                options: ["Сэлэнгэ мөрөн", "Орхон гол", "Онон гол", "Туул гол"],
                correctAnswerIndex: 2 // В. Онон гол
            },
            {
                question: "Тэмүжин бага насандаа болон залуу үедээ хамгийн ойр холбоотой байсан, хожим өрсөлдөгч болсон нэгэн хэн бэ?",
                options: ["Ван Хан (Тоорил)", "Таян хан", "Хүчүлүг", "Жамуха"],
                correctAnswerIndex: 3 // Г. Жамуха
            },
            {
                question: "1189 оны орчимд Тэмүжин Кэрэйдийн Ван Ханд хандан тусламж хүссэн гол шалтгаан юу байсан бэ?",
                options: ["Найманыг дайлахаар", "Мэргидээс эхнэр Бөртэ-г аврахаар", "Татаруудыг дайлахаар", "Хятадын Сүн улстай холбоо тогтоохоор"],
                correctAnswerIndex: 1 // Б. Мэргидээс эхнэр Бөртэ-г аврахаар
            },
            {
                question: "Тэмүжин 1202 онд аав Есүхэйгээ хордуулсан гэж үздэг, олон жилийн дайсан байсан аль аймгийг бут цохиж, улмаар тэднийг газар дээрээс нь арчин устгасан бэ?",
                options: ["Найманууд", "Мэргидүүд", "Татарууд", "Хэрэйдүүд"],
                correctAnswerIndex: 2 // В. Татарууд
            },
            {
                question: "1203 онд болсон чухал тулалдаан бөгөөд Тэмүжин Кэрэйдийн аймгийн хүчийг бүрмөсөн дарж, Ван Хан (Тоорил) болон Сэнгүм нарыг зугтахад хүргэсэн тулаан юу вэ?",
                options: ["Далан-Балжудын тулалдаан", "Халхын голын тулалдаан", "Хараун-Жидуний тулалдаан", "Цахир малцгийн тулалдаан"],
                correctAnswerIndex: 3 // Г. Цахир малцгийн тулалдаан
            },
            {
                question: "1204 онд Тэмүжинд цохигдсон, Монголын нэгдэлд хамгийн сүүлд орсон томоохон аймгуудын нэг бөгөөд соёл болон бичиг үсэгтэй байсан аймгийг нэрлэнэ үү.",
                options: ["Мэргид", "Наймаан", "Хэрэйд", "Онгуд"],
                correctAnswerIndex: 1 // Б. Наймаан
            },
            {
                question: "1206 он хүртэл Тэмүжин улс төрийн ямар гол үйл явц буюу зорилгыг бүрэн хэрэгжүүлсэн бэ?",
                options: ["Хятадын эсрэг дайн эхлүүлэх", "Монголын бүх овог аймгийг нэгтгэх", "Тэнгис далай руу гарах", "Өрнө зүг рүү дайрах"],
                correctAnswerIndex: 1 // Б. Монголын бүх овог аймгийг нэгтгэх
            },
            {
                question: "Тэмүжин 1189-1206 оны хооронд цэрэг, улс төрийн хувьд ямар чухал шинэчлэл хийсэн бэ?",
                options: ["Зөвхөн цэргийн хүчийг нэмэгдүүлсэн.", "Хүчтэй нууц алба байгуулсан.", "Хуучин овог аймгийн бүтэц дээр суурилсан цэргийн зохион байгуулалтыг арвантын системээр халсан.", "Шинэ бичиг үсэг зохиосон."],
                correctAnswerIndex: 2 // В. Хуучин овог аймгийн бүтэц дээр суурилсан цэргийн зохион байгуулалтыг арвантын системээр халсан.
            },
            {
                question: "\"Чингис хаан\" гэдэг цолны утга санаа юуг илэрхийлдэг вэ?",
                options: ["Агуу баатар", "Тэнгэрээс заяат", "Далай, Түгээмэл хаан", "Эрэмгий удирдагч"],
                correctAnswerIndex: 2 // В. Далай, Түгээмэл хаан
            }
        ];

        // Асуултын дугаарыг харуулдаг үсэг (А, Б, В, Г)
        const optionLetters = ["А", "Б", "В", "Г"];

        // 2. Асуултуудыг дэлгэцэнд байрлуулах функц
        function renderQuiz() {
            const container = document.getElementById('questionsContainer');
            container.innerHTML = ''; // Өмнөх агуулгыг цэвэрлэх

            quizData.forEach((q, index) => {
                const questionNumber = index + 1;
                let optionsHtml = '';
                
                // Хариултын сонголтуудыг үүсгэх
                q.options.forEach((option, i) => {
                    optionsHtml += `
                        <label class="option-label flex items-center p-3 rounded-lg border border-gray-200 bg-gray-50 hover:bg-indigo-50 transition-all">
                            <input type="radio" name="question-${questionNumber}" value="${i}" class="w-4 h-4 text-indigo-600 border-gray-300 focus:ring-indigo-500 mr-3" required>
                            <span class="font-medium text-gray-800">${optionLetters[i]}. ${option}</span>
                        </label>
                    `;
                });

                // Асуултын картыг үүсгэх
                const questionCard = `
                    <div class="question-card bg-white p-6 rounded-xl border border-indigo-200">
                        <p class="text-xl font-semibold mb-4 text-gray-800">
                            ${questionNumber}. ${q.question}
                        </p>
                        <div class="space-y-3" id="options-${questionNumber}">
                            ${optionsHtml}
                        </div>
                    </div>
                `;
                container.innerHTML += questionCard;
            });
        }

        // 3. Хариултуудыг шалгах функц
        function checkAnswers(event) {
            event.preventDefault();

            let score = 0;
            const totalQuestions = quizData.length;
            const feedbackContainer = document.getElementById('feedbackContainer');
            feedbackContainer.innerHTML = '';
            
            quizData.forEach((q, index) => {
                const questionNumber = index + 1;
                const form = document.getElementById('quizForm');
                const selectedOption = form.querySelector(`input[name="question-${questionNumber}"]:checked`);
                const optionsDiv = document.getElementById(`options-${questionNumber}`);
                
                // Сонголтын хариуг тэмдэглэх
                optionsDiv.querySelectorAll('input[type="radio"]').forEach((input, i) => {
                    const label = input.closest('label');
                    label.classList.remove('bg-green-100', 'bg-red-100', 'border-green-500', 'border-red-500', 'bg-indigo-50', 'border-gray-200');
                    label.classList.add('bg-gray-50', 'border-gray-200');
                    
                    if (i === q.correctAnswerIndex) {
                        // Зөв хариуг ногооноор тэмдэглэх
                        label.classList.add('bg-green-100', 'border-green-500');
                    } else if (selectedOption && parseInt(selectedOption.value) === i) {
                        // Буруу сонголтыг улаанаар тэмдэглэх
                        label.classList.add('bg-red-100', 'border-red-500');
                    }
                    
                    // Бүх сонголтыг идэвхгүй болгох
                    input.disabled = true;
                });
                
                let isCorrect = false;
                if (selectedOption) {
                    const selectedIndex = parseInt(selectedOption.value);
                    if (selectedIndex === q.correctAnswerIndex) {
                        score++;
                        isCorrect = true;
                    }
                }

                // Үр дүнгийн дэлгэрэнгүй мэдээлэл
                const feedbackItem = `
                    <div class="p-4 rounded-xl shadow-md border ${isCorrect ? 'border-green-400 bg-green-50' : 'border-red-400 bg-red-50'}">
                        <p class="font-bold text-lg text-gray-800">${questionNumber}. ${q.question}</p>
                        <p class="mt-2 text-sm">
                            <span class="font-semibold ${isCorrect ? 'text-green-700' : 'text-red-700'}">Таны хариулт:</span> ${selectedOption ? optionLetters[parseInt(selectedOption.value)] + ". " + q.options[parseInt(selectedOption.value)] : 'Хариулаагүй'}
                        </p>
                        <p class="text-sm font-semibold text-green-700">
                            Зөв хариулт: ${optionLetters[q.correctAnswerIndex]}. ${q.options[q.correctAnswerIndex]}
                        </p>
                    </div>
                `;
                feedbackContainer.innerHTML += feedbackItem;
            });

            // Нийт оноог харуулах
            document.getElementById('scoreDisplay').innerHTML = `${score} / ${totalQuestions}`;
            
            // Үр дүнгийн хэсгийг харуулах
            document.getElementById('results').classList.remove('hidden');
            document.getElementById('submitBtn').classList.add('hidden');
            
            // Дэлгэцийг үр дүн рүү гүйлгэх
            document.getElementById('results').scrollIntoView({ behavior: 'smooth' });
        }

        // 4. Тестийг дахин эхлүүлэх функц
        function resetQuiz() {
            document.getElementById('quizForm').reset();
            document.getElementById('results').classList.add('hidden');
            document.getElementById('submitBtn').classList.remove('hidden');
            
            // Бүх сонголтын идэвхжүүлэлтийг арилгах
            quizData.forEach((q, index) => {
                const questionNumber = index + 1;
                const optionsDiv = document.getElementById(`options-${questionNumber}`);
                optionsDiv.querySelectorAll('input[type="radio"]').forEach((input, i) => {
                    input.disabled = false;
                    const label = input.closest('label');
                    label.classList.remove('bg-green-100', 'border-green-500', 'bg-red-100', 'border-red-500');
                    label.classList.add('bg-gray-50', 'border-gray-200');
                });
            });

            // Дэлгэцийг эхлэл рүү гүйлгэх
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }


        // 5. Үйл явдлын сонсогч
        document.addEventListener('DOMContentLoaded', () => {
            renderQuiz();
            document.getElementById('quizForm').addEventListener('submit', checkAnswers);
        });
    </script>
</body>
</html>
