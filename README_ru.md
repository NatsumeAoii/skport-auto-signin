<h1 align="center">
    <img width="120" height="120" src="pic/logo.svg" alt=""><br>
    skport-auto-sign
</h1>

<p align="center">
    <img src="https://img.shields.io/github/license/canaria3406/skport-auto-sign?style=flat-square" alt="">
    <img src="https://img.shields.io/github/stars/canaria3406/skport-auto-sign?style=flat-square" alt="">
    <br><a href="/README_zh-tw.md">繁體中文</a>　<a href="/README.md">English</a> <b>Русский</b>
</p>

Легковесный, безопасный и бесплатный скрипт для автоматического получения ежедневных наград SKPORT.  
Поддерживает **Arknights: Endfield**. Поддерживает несколько учетных записей.

---

## Возможности

* **Легковесность** — скрипт требует минимальной настройки и состоит всего из 90 строк кода.
* **Безопасность** — скрипт можно самостоятельно развернуть в Google Apps Script без риска утечки данных.
* **Бесплатность** — Google Apps Script является бесплатным сервисом.
* **Простота** — скрипт может работать без браузера и автоматически отправлять уведомления через Discord или Telegram.

---

## Установка

1. Перейдите в https://script.google.com/home/start и создайте новый проект, задав ему произвольное имя.
2. Откройте редактор и вставьте код  
   (Discord версия: https://github.com/canaria3406/skport-auto-sign/blob/main/src/main-discord.gs  
   Telegram версия: https://github.com/canaria3406/skport-auto-sign/blob/main/src/main-telegram.gs).  
   Затем настройте конфигурацию согласно инструкции ниже и сохраните проект.
3. Выберите функцию `main` и нажмите кнопку **Run** в верхней части страницы.  
   Предоставьте необходимые разрешения и убедитесь, что выполнение завершилось успешно (Execution started → completed).
4. Перейдите в раздел **Triggers** слева и добавьте новый триггер:
   - Функция для запуска: `main`
   - Источник события: по времени
   - Тип триггера: ежедневно
   - Время выполнения: рекомендуется выбрать любое непиковое время с 09:00 до 15:00.

---

## Конфигурация

```javascript
const profiles = [
  {
    SK_OAUTH_CRED_KEY: "", // ваш SK_OAUTH_CRED_KEY в cookie
    SK_TOKEN_CACHE_KEY: "", // ваш SK_TOKEN_CACHE_KEY в localStorage
    id: "", // ваш игровой ID в Arknights: Endfield
    server: "3", // Азия=2 Америка/Европа=3
    language: "ru_RU", // english=en 日本語=ja 繁體中文=zh_Hant 简体中文=zh_Hans 한국어=ko Русский=ru_RU
    accountName: "ВАШ НИК"
  }
];
```

<details>
<summary><b>Настройки SKPORT</b></summary>

1. **SK_OAUTH_CRED_KEY** — введите данные страницы ежедневных наград SKPORT.  
2. **SK_TOKEN_CACHE_KEY** — введите токен страницы ежедневных наград SKPORT.  

После входа на страницу https://game.skport.com/endfield/sign-in  
нажмите F12, чтобы открыть консоль разработчика.

Вставьте следующий код и выполните его, чтобы получить данные. Скопируйте значения и вставьте их в кавычки:

```javascript
function getCookie(name) {
  const value = `; ${document.cookie}`;
  const parts = value.split(`; ${name}=`);
  if (parts.length === 2) return parts.pop().split(';').shift();
}

let cred = 'Ошибка';
if (document.cookie.includes('SK_OAUTH_CRED_KEY=')) {
  cred = `${getCookie('SK_OAUTH_CRED_KEY')}`;
}

let ask = confirm(cred + '\n\nНажмите Enter, затем вставьте токен в ваш проект Google Apps Script');
let msg = ask ? cred : 'Отмена';
console.log('SK_OAUTH_CRED_KEY:');
console.log(msg);

let token = 'Ошибка';
if (localStorage.getItem('SK_TOKEN_CACHE_KEY')) {
  token = localStorage.getItem('SK_TOKEN_CACHE_KEY');
}

let ask2 = confirm(token + '\n\nНажмите Enter, затем вставьте токен в ваш проект Google Apps Script');
let msg2 = ask2 ? token : 'Отмена';
console.log('SK_TOKEN_CACHE_KEY:');
console.log(msg2);
```

3. **id**  
Введите ваш игровой ID в Arknights: Endfield (должен быть числом).

4. **server**  
Введите сервер:
- `2` — Азия  
- `3` — Америка/Европа  

5. **language**  
Введите язык игры:
- `en` — English  
- `ja` — 日本語  
- `zh_Hant` — 繁體中文  
- `zh_Hans` — 简体中文  
- `ko` — 한국어  
- `ru_RU` — Русский  

6. **accountName**  
Введите ваш никнейм SKPORT или внутриигровой никнейм.

</details>

---

<details>
<summary><b>Настройки уведомлений Discord (только для Discord версии)</b></summary>

```javascript
const discord_notify = true
const myDiscordID = "20000080000000040"
const discordWebhook = "https://discord.com/api/webhooks/..."
```

1. **discord_notify**  
Включение уведомлений Discord.  
Установите `true`, чтобы получать уведомления о сборе ежедневных наград, или `false`, чтобы отключить их.

2. **myDiscordID**  
Введите ваш ID Discord.  
Если хотите получать уведомления о неудачных попытках сбора наград, вставьте ID (например, `23456789012345678`) в кавычки.  
Если уведомления не нужны — оставьте пустым (`""`).

3. **discordWebhook**  
Введите URL вебхука Discord для канала, куда будут отправляться уведомления.

</details>

---

<details>
<summary><b>Настройки уведомлений Telegram (только для Telegram версии)</b></summary>

```javascript
const telegram_notify = true
const myTelegramID = "1XXXXXXX0"
const telegramBotToken = "6XXXXXXXXX:AAAAAAAAAAXXXXXXXXXX8888888888Peko"
```

1. **telegram_notify**  
Установите `true`, чтобы получать уведомления, или `false`, чтобы отключить их.

2. **myTelegramID**  
Введите ваш ID Telegram.  
Получить его можно через команду `/getid`, отправив сообщение боту @IDBot.

3. **telegramBotToken**  
Введите токен вашего Telegram-бота.  
Создать бота можно через @BotFather с помощью команды `/newbot`.

</details>

---

## Примеры

Если процесс автоматического сбора ежедневных наград прошёл успешно — будет отправлено сообщение `"OK"`.  
Если награды уже были получены сегодня, вы получите соответствующее уведомление.

![image](https://github.com/canaria3406/skport-auto-sign/blob/main/pic/01.png)

---

## Журнал изменений

2026-01-29 — Проект запущен.  
2026-02-14 — Исправлен баг. Спасибо Keit (@keit32) за помощь.
