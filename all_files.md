# Набір файлів для виконання практичного завдання з CI/CD

Нижче наведено готові файли, які ви можете використовувати як основу для виконання вашого практичного завдання. Скопіюйте їх у відповідні файли у вашому проекті.

## 1. `index.html` - Головна сторінка

```html
<!DOCTYPE html>
<html lang="uk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Освітній Квіз | CI/CD Демо</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <h1>Освітній Квіз</h1>
        <p>Перевірте свої знання з основ програмування</p>
    </header>

    <main>
        <div class="quiz-container">
            <div id="quiz-progress">Питання <span id="current-question">1</span> з <span id="total-questions">5</span></div>
            
            <div id="question-container">
                <h2 id="question-text">Питання завантажується...</h2>
                
                <div class="options-container" id="options-container">
                    <!-- Опції будуть додані з JavaScript -->
                </div>
            </div>
            
            <div class="controls">
                <button id="prev-btn" disabled>Попереднє</button>
                <button id="next-btn">Наступне</button>
                <button id="submit-btn" style="display: none;">Завершити</button>
            </div>
            
            <div id="results" style="display: none;">
                <h2>Ваш результат:</h2>
                <p><span id="score">0</span> з <span id="max-score">0</span> правильних відповідей</p>
                <button id="restart-btn">Спробувати знову</button>
            </div>
        </div>
    </main>

    <footer>
        <p>© <span id="current-year"></span> Освітній проект з CI/CD. Створено для демонстрації принципів CI/CD.</p>
    </footer>

    <script src="app.js"></script>
</body>
</html>
```

## 2. `styles.css` - Файл стилів

```css
/* Базові стилі та скидання */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    line-height: 1.6;
    background-color: #f8f9fa;
    color: #333;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
}

/* Стилі заголовка */
header {
    text-align: center;
    margin-bottom: 40px;
    padding: 20px;
    background-color: #4b6cb7;
    background-image: linear-gradient(to right, #4b6cb7, #182848);
    color: white;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

header h1 {
    margin-bottom: 10px;
    font-size: 2.5rem;
}

/* Стилі для контейнера квізу */
.quiz-container {
    background-color: white;
    border-radius: 8px;
    padding: 30px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    margin-bottom: 30px;
}

#quiz-progress {
    text-align: right;
    font-size: 0.9rem;
    color: #666;
    margin-bottom: 20px;
}

#question-text {
    margin-bottom: 25px;
    font-size: 1.4rem;
    color: #2c3e50;
}

/* Стилі для варіантів відповідей */
.options-container {
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-bottom: 30px;
}

.option {
    padding: 15px;
    background-color: #f1f5f9;
    border: 2px solid #e2e8f0;
    border-radius: 6px;
    cursor: pointer;
    transition: all 0.2s;
}

.option:hover {
    background-color: #e2e8f0;
}

.option.selected {
    background-color: #bcdefa;
    border-color: #3498db;
}

.option.correct {
    background-color: #c8f7c5;
    border-color: #27ae60;
}

.option.incorrect {
    background-color: #f8d7da;
    border-color: #e74c3c;
}

/* Стилі для кнопок керування */
.controls {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
}

button {
    padding: 12px 24px;
    border: none;
    border-radius: 6px;
    background-color: #4b6cb7;
    color: white;
    font-size: 1rem;
    cursor: pointer;
    transition: background-color 0.2s;
}

button:hover {
    background-color: #3a5795;
}

button:disabled {
    background-color: #cccccc;
    cursor: not-allowed;
}

#submit-btn {
    background-color: #27ae60;
}

#submit-btn:hover {
    background-color: #219653;
}

/* Стилі для результатів */
#results {
    text-align: center;
    padding: 20px;
    background-color: #f1f5f9;
    border-radius: 8px;
}

#results h2 {
    margin-bottom: 15px;
    color: #2c3e50;
}

#results p {
    font-size: 1.2rem;
    margin-bottom: 20px;
}

#restart-btn {
    background-color: #e67e22;
}

#restart-btn:hover {
    background-color: #d35400;
}

/* Стилі для футера */
footer {
    text-align: center;
    font-size: 0.9rem;
    color: #777;
    margin-top: 30px;
    padding-top: 20px;
    border-top: 1px solid #eee;
}

/* Медіа-запити для адаптивності */
@media (max-width: 600px) {
    body {
        padding: 10px;
    }
    
    header {
        padding: 15px;
    }
    
    header h1 {
        font-size: 2rem;
    }
    
    .quiz-container {
        padding: 20px;
    }
    
    button {
        padding: 10px 20px;
    }
}
```

## 3. `app.js` - JavaScript файл з логікою

```javascript
// Масив з питаннями квізу
const quizQuestions = [
    {
        question: "Що таке CI/CD?",
        options: [
            "Continuous Integration/Continuous Delivery",
            "Code Integration/Code Development",
            "Computer Interface/Computer Development",
            "Cloud Integration/Cloud Deployment"
        ],
        correctAnswer: 0
    },
    {
        question: "Який з цих інструментів НЕ є інструментом CI/CD?",
        options: [
            "Jenkins",
            "Travis CI",
            "Photoshop",
            "GitHub Actions"
        ],
        correctAnswer: 2
    },
    {
        question: "Яка основна перевага використання CI?",
        options: [
            "Зменшення часу розробки",
            "Раннє виявлення помилок",
            "Збільшення зарплати розробників",
            "Зменшення кількості коду"
        ],
        correctAnswer: 1
    },
    {
        question: "Що означає CD в CI/CD?",
        options: [
            "Compact Disc",
            "Code Documentation",
            "Continuous Delivery або Continuous Deployment",
            "Computer Development"
        ],
        correctAnswer: 2
    },
    {
        question: "Яка практика є ключовою для CI/CD?",
        options: [
            "Написання документації в останню чергу",
            "Автоматизація тестування",
            "Ручне розгортання",
            "Рідкі але великі коміти в репозиторій"
        ],
        correctAnswer: 1
    }
];

// Ініціалізація змінних
let currentQuestion = 0;
let score = 0;
let userAnswers = Array(quizQuestions.length).fill(null);

// DOM елементи
const questionText = document.getElementById('question-text');
const optionsContainer = document.getElementById('options-container');
const prevButton = document.getElementById('prev-btn');
const nextButton = document.getElementById('next-btn');
const submitButton = document.getElementById('submit-btn');
const currentQuestionSpan = document.getElementById('current-question');
const totalQuestionsSpan = document.getElementById('total-questions');
const resultsDiv = document.getElementById('results');
const scoreSpan = document.getElementById('score');
const maxScoreSpan = document.getElementById('max-score');
const restartButton = document.getElementById('restart-btn');
const currentYearSpan = document.getElementById('current-year');

// Функція для ініціалізації квізу
function initQuiz() {
    // Встановлюємо поточний рік у футері
    currentYearSpan.textContent = new Date().getFullYear();
    
    // Встановлюємо загальну кількість питань
    totalQuestionsSpan.textContent = quizQuestions.length;
    
    // Встановлюємо максимальний можливий бал
    maxScoreSpan.textContent = quizQuestions.length;
    
    // Завантажуємо перше питання
    loadQuestion();
    
    // Встановлюємо обробники подій для кнопок
    prevButton.addEventListener('click', goToPreviousQuestion);
    nextButton.addEventListener('click', goToNextQuestion);
    submitButton.addEventListener('click', submitQuiz);
    restartButton.addEventListener('click', restartQuiz);
}

// Функція для завантаження питання
function loadQuestion() {
    const question = quizQuestions[currentQuestion];
    questionText.textContent = question.question;
    
    // Оновлюємо номер поточного питання
    currentQuestionSpan.textContent = currentQuestion + 1;
    
    // Очищаємо контейнер варіантів
    optionsContainer.innerHTML = '';
    
    // Додаємо варіанти відповідей
    question.options.forEach((option, index) => {
        const optionElement = document.createElement('div');
        optionElement.className = 'option';
        if (userAnswers[currentQuestion] === index) {
            optionElement.classList.add('selected');
        }
        optionElement.textContent = option;
        optionElement.addEventListener('click', () => selectOption(index));
        optionsContainer.appendChild(optionElement);
    });
    
    // Оновлюємо стан кнопок
    updateButtonsState();
}

// Функція для вибору варіанту відповіді
function selectOption(index) {
    userAnswers[currentQuestion] = index;
    
    // Оновлюємо відображення вибраного варіанту
    const options = optionsContainer.querySelectorAll('.option');
    options.forEach((option, i) => {
        if (i === index) {
            option.classList.add('selected');
        } else {
            option.classList.remove('selected');
        }
    });
    
    // Оновлюємо стан кнопок
    updateButtonsState();
}

// Функція для переходу до попереднього питання
function goToPreviousQuestion() {
    if (currentQuestion > 0) {
        currentQuestion--;
        loadQuestion();
    }
}

// Функція для переходу до наступного питання
function goToNextQuestion() {
    if (currentQuestion < quizQuestions.length - 1) {
        currentQuestion++;
        loadQuestion();
    }
}

// Функція для оновлення стану кнопок
function updateButtonsState() {
    // Керування кнопкою "Попереднє"
    prevButton.disabled = currentQuestion === 0;
    
    // Керування кнопкою "Наступне" і "Завершити"
    if (currentQuestion === quizQuestions.length - 1) {
        nextButton.style.display = 'none';
        submitButton.style.display = 'block';
        
        // Перевіряємо, чи відповів користувач на всі питання
        const allQuestionsAnswered = userAnswers.every(answer => answer !== null);
        submitButton.disabled = !allQuestionsAnswered;
    } else {
        nextButton.style.display = 'block';
        submitButton.style.display = 'none';
    }
}

// Функція для завершення квізу і показу результатів
function submitQuiz() {
    // Підраховуємо кількість правильних відповідей
    score = 0;
    userAnswers.forEach((answer, index) => {
        if (answer === quizQuestions[index].correctAnswer) {
            score++;
        }
    });
    
    // Показуємо результати
    scoreSpan.textContent = score;
    document.getElementById('question-container').style.display = 'none';
    document.getElementById('quiz-progress').style.display = 'none';
    document.querySelector('.controls').style.display = 'none';
    resultsDiv.style.display = 'block';
}

// Функція для перезапуску квізу
function restartQuiz() {
    currentQuestion = 0;
    userAnswers = Array(quizQuestions.length).fill(null);
    document.getElementById('question-container').style.display = 'block';
    document.getElementById('quiz-progress').style.display = 'block';
    document.querySelector('.controls').style.display = 'flex';
    resultsDiv.style.display = 'none';
    loadQuestion();
}

// Ініціалізуємо квіз після завантаження DOM
document.addEventListener('DOMContentLoaded', initQuiz);
```

## 4. `README.md` - Опис проекту

```markdown
# Освітній Квіз | CI/CD Демо Проект

Цей проект створено як демонстраційний приклад для практичного завдання з CI/CD для студентів спеціальності "Педагогіка".

## Опис проекту

Проект представляє собою простий веб-додаток з освітнім квізом на тему CI/CD та програмування. Він демонструє основні принципи розробки веб-додатків та дозволяє студентам практично застосувати знання з CI/CD для автоматизації розгортання веб-додатку.

### Функціональність

- Інтерактивний квіз з питаннями з тематики CI/CD
- Можливість навігації між питаннями
- Підрахунок результатів
- Адаптивний дизайн (працює на мобільних пристроях та ПК)

## Технічні деталі

- Проект реалізовано з використанням чистого HTML, CSS та JavaScript
- Не використовуються сторонні бібліотеки чи фреймворки
- Підтримує сучасні браузери (Chrome, Firefox, Safari, Edge)

## Структура проекту

```
├── index.html          # Головна HTML сторінка
├── styles.css          # Стилі для веб-додатку
├── app.js              # JavaScript логіка квізу
├── README.md           # Документація проекту
└── .github/
    └── workflows/
        └── ci.yml      # Конфігурація GitHub Actions для CI/CD
```

## Налаштування CI/CD

Проект використовує GitHub Actions для налаштування Continuous Integration та Continuous Deployment:

- **CI (Continuous Integration)**: автоматична перевірка коду на помилки, перевірка стилів CSS, валідація HTML
- **CD (Continuous Deployment)**: автоматичне розгортання веб-додатку на GitHub Pages при внесенні змін в головну гілку

## Як запустити проект локально

1. Клонуйте репозиторій:
   ```
   git clone https://github.com/ваш-логін/назва-репозиторію.git
   ```

2. Відкрийте файл `index.html` у вашому браузері.

3. Готово! Експериментуйте з кодом та спостерігайте за роботою CI/CD пайплайну при внесенні змін.

## Як розширити проект

Можливі напрямки для розширення проекту:

- Додавання більшої кількості питань
- Впровадження таймера для кожного питання
- Додавання рівнів складності
- Збереження результатів у локальному сховищі браузера
- Впровадження авторизації та серверної частини

## Ліцензія

Цей проект створено з освітньою метою і може вільно використовуватись для навчання.
```

## 5. `.github/workflows/ci.yml` - Конфігурація для GitHub Actions

```yaml
name: Education Quiz CI/CD

on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]

jobs:
  validate:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'
    
    - name: Install dependencies
      run: npm install -g htmlhint stylelint jshint
      
    - name: Validate HTML
      run: htmlhint index.html
      
    - name: Validate CSS
      run: stylelint styles.css
      
    - name: Validate JavaScript
      run: jshint app.js
  
  deploy:
    needs: validate
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' || github.ref == 'refs/heads/master'
    
    steps:
    - uses: actions/checkout@v2
    
    - name: Deploy to GitHub Pages
      uses: JamesIves/github-pages-deploy-action@4.1.4
      with:
        branch: gh-pages
        folder: .
```

## 6. `.gitignore` - Файл для ігнорування файлів Git

```
# Ігноруємо системні файли
.DS_Store
Thumbs.db

# Ігноруємо залежності npm
node_modules/
package-lock.json
npm-debug.log

# Ігноруємо кеш і логи
.cache/
logs/
*.log

# Ігноруємо файли IDE
.idea/
.vscode/
*.sublime-*

# Ігноруємо персональні налаштування
.env
.env.local
```

## Як використовувати ці файли:

1. Створіть новий проект на GitHub
2. Клонуйте його на свій комп'ютер
3. Створіть ці файли в точно такій же структурі
4. Для GitHub Actions створіть директорію `.github/workflows/` і покладіть туди файл `ci.yml`
5. Виконайте команди:
   ```
   git add .
   git commit -m "Початковий шаблон проекту для CI/CD практики"
   git push origin main
   ```

6. Перевірте, що GitHub Actions запустився після вашого пушу у репозиторій

Ці файли надають повноцінний робочий шаблон для виконання практичного завдання по налаштуванню CI/CD пайплайну. Студенти можуть змінювати та розширювати цей код для кращого розуміння принципів CI/CD.