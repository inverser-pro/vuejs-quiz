# vuejs-quiz
Create simple Vue JS Quiz (Poll) with my code. It really simply

Visit page https://inverser.pro/169-komponent-oprosa-dlya-vuejs

First:

Install NodeJS to you PC.

Open NodeJS (or other) command line and enter:
node -v.
If you see it:
v12.0.0.x

All it's ok!

////////////
Second:

install git (search: Git download)

Open Git command line an write:
cd /d/nodejsprojects/downloaded

run command:
git clone https://github.com/inverser-pro/vuejs-quiz.git

Or download in this page ZIP file with my code.

//////////

In folder D:/nodejsprojects/downloaded/vue-quiz-master (for example)

run command:

npm install

If you want to see it on you PC, RUN:

npm run serve

And open in your browser:
http://localhost:8080

If you want to build this project:
npm run build

After generating all the files, you can copy and place everything from the "dist" folder on your hosting.

Quiz data located here:
../public/questions.json

Change this file and add you questions.

---------------
Русский:
Создайте простой компонент опроса для своего сайта.

Здесь код PHP и многое другое...
https://inverser.pro/169-komponent-oprosa-dlya-vuejs

Все очень просто и понятно. Чтобы все заработало, установите NodeJS на Ваш PC. Гугли: nodejs install. Скачайте файл и установите программу.
После нужно установить Git или просто скачать и разархивировать мой проект.

Например, Вы разархивировали все в папку
D:/nodejsprojects/downloaded/vue-quiz-master

Далее через командную строку (Win+R) или через командную строку nodeJS:

cd /d D:/nodejsprojects/downloaded/vue-quiz-master
вы окажитесь в этой папке командной строкой.
Далее:
npm install

После того, как все пакеты установятся, запустите команду:

npm run serve

и откройте в браузере (после того, как все сгенерируется):
http://localhost:8080

Вы увидите проект.

Чтобы его выгрузить себе на хостинг:

npm run build

После компиляции файлов, появится папка dist там же. Все её содержимое выгружайте на хостинг.

Чтобы создать свой опрос, необходимо изменить все в файле
../public/questions.json

Там есть 4 вопроса для примера с разными вариантами отображения.
Например, есть чекбоксы, где можно выбрать несколько вариантов.
Есть радио переключалетели (да/нет, или что-то подобное).

Можно также ПРИ УСЛОВИИ, если человек отвечает на ДА/НЕТ (всего два вопроса) — отвечает ДА, то можно также добавить форму Текстовый input, числовой input и textarea.

Все это вполне понятно по записям в json файле:

"type": "single", //single (один вариант из нескольких [да/нет, услуга/товар/еще что-то]) или multiple (много вариантов можно выбрать)
"question": "Искали ли Вы сайты конкурентов?", //сам вопрос
"answers": [//ответы
"Да",
"Нет"
],
"ifYes":true,//если отвечаем на первый (ДА или УСЛУГА [см. файл ../public/questions.json]), то можно здесь поставить 'true' и ниже написать описание
"ifYesDataType":"textarea",//тип (texarea,input)
/*Здесь в файле примера есть еще 
"ifYesDataTypeInput":"number",
это значит, что тип input будет с номером (ввод номера или количества цифрами), например: ваше количество товара: 700 штук
*/
"ifYesDataText":"Введите ссылку или ссылки, если есть"//пояснение, что нужно вводить в поле texarea или input
