Програма для періодичного скрапінгу платформи AutoRia

1) Встановити й активувати віртуальне оточення, якщо її немає.

2) Переглянути мій requirements.txt.

3) У файлі .env встановити свої дані, для роботи. Спочатку для postgres потім для нової БД, марку і модель.

4) У файлі my_cron у рядку - 18 19 * * * * /usr/bin/python3 /app/main.py> /app/cron_output.log 2>&1 - Поміняти 18 і 19 на ваш час (Тобто 18 - хвилин 19 - годин).

5) У файлі Dockerfile поміняти за бажання ENV TZ=Europe/Kyiv на потрібний вам час. Подивіться які є ще варіанти.

6) Запустити команду - docker-compose up --build.  Якщо сталася помилка поміняйте container_name: auto_ria2 у файлі docker-compose.yml .

7) За бажанням запустити окремо main.py, щоб подивитись словник із данними. Може зайняти 20+ хвилин побудування цього словника.

Додаткова інформація:

Я використовував асинхронне програмування через те, що на сайті понад 1700+ марок, не враховуючи 30-40% тракторів, оскільки під час спроби взяти їхні дані перекидає на agro.ria, 
тому вирішив не прописувати логіку для цих (30-40% тракторів). Так само уявіть скільки у мене вийшло дістати моделей для кожного авто. Я знайшов кілька лазівок, на які витратив багато часу, 
але за допомогою їх я можу надати повний список всіх марок і їхніх моделей (але невелике АЛЕ, оскільки auto.ria цілеспрямовано змушує для розробників використовувати їхнє API, а це API коштує грошей, 
якщо ти хочеш легко і просто отримати всі дані. Але у мене не було особливо вибору і завдяки цим лазівкам і 20+ хвилинам вашого часу очікування, поки програма збере вам словник, ви можете отримати всі дані 
безкоштовно) ще хочу зазначити, якщо на сайті марка авто представлена нашою кирилицею, то доводилося використовувати transliterate.

Також хочу передбачити, що я використовував семафор у моїх асиннхронних функціях, через те що відбувається дуже, дуже багато запитів на сайт і він не завжди допомагає - через це останні 5-9 марок буде без моделей. 
Я пробував вирішити проблему з різними обмеженнями, але, на жаль, тільки два варіанти: 1 - дуже довго, 2 - також Exception.
Якщо ви не хочете чекати 20+ хвилин, просто створіть відразу таблицю users з одним користувачем і дайте йому url.

Я проходив багато тестів і помітив, що на linux (а саме на ubuntu) набагато краще працює, ніж на windows. Також можу помітити, якщо ви вибрали марку і модель і там буде 300+ варіантів - 
це може зайняти кілька підходів через те, що через це вибиває виняток, але я використовував блоки try catch і завдяки цьому - dump все одно працює успішно, якщо програма дійшла успішно до цього моменту. 
Тож, можливо, варто використовувати кілька підходів, якщо буде більше, ніж 100+ варіантів (але це тільки в тому разі, якщо треба додати ці 100+ машин у таблицю).
Тобто я б запускав програму кілька разів на день, а не один як ви вказали в task.

Дякую за чудовий досвід, але це був лютий хардкор для мене з дедлайном :).
