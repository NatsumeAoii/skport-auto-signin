<h1 align="center">
    <img width="120" height="120" src="pic/logo.svg" alt=""><br>
    skport-auto-sign
</h1>

<p align="center">
    <img src="https://img.shields.io/github/license/NatsumeAoii/skport-auto-sign?style=flat-square" alt="">
    <img src="https://img.shields.io/github/stars/NatsumeAoii/skport-auto-sign?style=flat-square" alt="">
    <br><a href="/README_zh-tw.md">繁體中文</a>　<b>English</b>　<a href="/README_ru.md">Русский</a>
</p>

A stable, secure, and free script that automatically collect SKPORT daily check in rewards.  
Supports **Arknights:Endfield**. Support multiple accounts.

## Features
* **Stable** - The script only requires minimal configuration and is only 90 lines of code.
* **Secure** - The script can be self-deployed to Google Apps Script, no worries about data leaks.
* **Free** - Google Apps Script is currently a free service.
* **Simple** - The script can run without a browser and will automatically notify you through Discord or Telegram.

## Setup
1. Go to [Google Apps Script](https://script.google.com/home/start) and create a new project with your custom name.
2. Select the editor and paste the code( [main-disc-tele.gs](https://github.com/NatsumeAoii/skport-auto-signin/blob/main/src/main-disc-tele.gs) ). Refer to the instructions below to configure the config file and save it.
3. Select "main" and click the "Run" button at the top.
   Grant the necessary permissions and confirm that the configuration is correct (Execution started > completed).

   ![image](https://github.com/NatsumeAoii/skport-auto-signin/blob/main/pic/02.png)

4. Click the trigger button on the left side and add a new trigger.

   ![image](https://github.com/NatsumeAoii/skport-auto-signin/blob/main/pic/03.png)
   ![image](https://github.com/NatsumeAoii/skport-auto-signin/blob/main/pic/04.png)

   Select the function to run: main
   Select the event source: Time-driven
   Select the type of time based trigger: Day timer
   Select the time of day: recommended to choose any off-peak time between 0900 to 1500.

   ![image](https://github.com/NatsumeAoii/skport-auto-signin/blob/main/pic/05.png)

## Configuration

```javascript
const profiles = [
  {
    SK_OAUTH_CRED_KEY: "", // your skport SK_OAUTH_CRED_KEY in cookie
    SK_TOKEN_CACHE_KEY: "", // [OPTIONAL] Leave blank! The script auto-acquires this.
    id: "", // your Endfield game id
    server: "2", // Asia=2 Americas/Europe=3
    language: "en", // english=en 日本語=ja 繁體中文=zh_Hant 简体中文=zh_Hans 한국어=ko Русский=ru_RU
    accountName: "YOUR NICKNAME"
  }
];

// Discord notification config
const discord_notify = true;
const myDiscordID = ""; 
const discordWebhook = "";

// Telegram notification config
const telegram_notify = false;
const myTelegramID = "";
const telegramBotToken = "";

// Display config
const botDisplayName = "Arknights Endfield Auto Sign-In";
const botAvatarUrl = "https://i.imgur.com/TguAOiA.png";
const embedTitle = "Endfield Daily Check-In";
```

<details>
<summary><b>SKPORT settings</b></summary>

1. **SK_OAUTH_CRED_KEY** - Please enter the cred for SKPORT check-in page.  
2. **SK_TOKEN_CACHE_KEY** - (Optional) **Leave this blank!** The script now automatically acquires and refreshes this token behind the scenes using your `SK_OAUTH_CRED_KEY`.  

   To get your credentials, enter the [SKPORT check-in page](https://game.skport.com/endfield/sign-in), press F12 to enter the console.
   Paste the following code and run it to get the cred. Copy the cred and fill it in "quotes".
   ```javascript
   function getCookie(name) {
   const value = `; ${document.cookie}`;
   const parts = value.split(`; ${name}=`);
   if (parts.length === 2) return parts.pop().split(';').shift();
   }

   let cred = 'Error';
   if (document.cookie.includes('SK_OAUTH_CRED_KEY=')) {
   cred = `${getCookie('SK_OAUTH_CRED_KEY')}`;
   }

   let ask = confirm(cred + '\n\nPress enter, then paste the token into your Google Apps Script Project');
   let msg = ask ? cred : 'Cancel';
   console.log('SK_OAUTH_CRED_KEY:');
   console.log(msg);

   let token = 'Error';
   if (localStorage.getItem('SK_TOKEN_CACHE_KEY')) {
   token = localStorage.getItem('SK_TOKEN_CACHE_KEY');
   }

   let ask2 = confirm(token + '\n\nPress enter, then paste the token into your Google Apps Script Project');
   let msg2 = ask2 ? token : 'Cancel';
   console.log('SK_TOKEN_CACHE_KEY:');
   console.log(msg2);
   ```

3. **id**

   Please enter your Arknights:Endfield game ID here.  
   (should be number)

4. **server**

   Please enter your Arknights:Endfield game server here.  
   If you're in Asia server, please enter `2`,  
   If you're in Americas/Europe server, please enter `3`.

5. **language**

   Please enter your Arknights:Endfield game language here.  
   If you're using english, please enter `en`,  
   If you're using 日本語, please enter `ja`,  
   If you're using 繁體中文, please enter `zh_Hant`,  
   If you're using 简体中文, please enter `zh_Hans`,  
   If you're using 한국어, please enter `ko`,  
   If you're using Русский, please enter `ru_RU`.

6. **accountName** - Please enter your customized nickname.

   Please enter your customized SKPORT or in-game nickname here.

</details>

<details>
<summary><b>Discord notify settings</b></summary>

```javascript
const discord_notify = true
const myDiscordID = "20000080000000040"
const discordWebhook = "https://discord.com/api/webhooks/1050000000000000060/6aXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXnB"
```

1. **discord_notify**

   Whether to enable Discord notify.
   If you want to enable auto check-in notify, set it to true. If not, please set it to false.

2. **myDiscordID** - Please enter your Discord user ID.

   Whether you want to be ping when there is an unsuccessful check-in.
   Copy your Discord user ID which like `23456789012345678` and fill it in "quotes".
   You can refer to [this article](https://support.discord.com/hc/en-us/articles/206346498) to find your Discord user ID.
   If you don't want to be pinged, leave the "quotes" empty.

3. **discordWebhook** - Please enter the Discord webhook for the server channel to send notify.

   You can refer to [this article](https://support.discord.com/hc/en-us/articles/228383668) to create a Discord webhook.
   Once you have finished creating the Discord webhook, you will receive your Discord webhook URL, which like `https://discord.com/api/webhooks/1234567890987654321/PekopekoPekopekoPekopeko06f810494a4dbf07b726924a5f60659f09edcaa1`.
   Copy the webhook URL and paste it in "quotes".

</details>

<details>
<summary><b>Telegram notify settings</b></summary>

```javascript
const telegram_notify = true
const myTelegramID = "1XXXXXXX0"
const telegramBotToken = "6XXXXXXXXX:AAAAAAAAAAXXXXXXXXXX8888888888Peko"
```

1. **telegram_notify**

   Whether to enable Telegram notify.
   If you want to enable auto check in notify, set it to true. If not, please set it to false.

2. **myTelegramID** - Please enter your Telegram ID.

   Use the `/getid` command to find your Telegram user ID by messaging [@IDBot](https://t.me/myidbot).
   Copy your Telegram ID which like `123456780` and fill it in "quotes".

3. **telegramBotToken** - Please enter your Telegram Bot Token.

   Use the `/newbot` command to create a new bot on Telegram by messaging [@BotFather](https://t.me/botfather).
   Once you have finished creating the bot, you will receive your Telegram Bot Token, which like `110201543:AAHdqTcvCH1vGWJxfSeofSAs0K5PALDsaw`.
   Copy your Telegram Bot Token and fill it in "quotes".
   For more detailed instructions, you can refer to [this article](https://core.telegram.org/bots/features#botfather).

</details>

## Demo
If the auto check in process is success, it will send "OK".  
If you have already check in today, it will give a notify.

![image](https://github.com/NatsumeAoii/skport-auto-singin/blob/main/pic/01.png)

## Changelog
2026-01-29 Project launched.  
2026-02-14 Bug fixed. Thanks to Keit(@keit32) for the help.  
2026-02-23 Massive Overhaul: Unified Discord & Telegram into `main-disc-tele.gs`, implemented Rich Embeds/HTML formatting, added automatic `SK_TOKEN_CACHE_KEY` acquisition & refresh, and localized timestamps.

## Credits
* **[canaria](https://github.com/canaria3406)** - The original author and creator of the Skport Auto Sign-In script.
