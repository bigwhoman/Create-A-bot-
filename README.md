# Create-A-bot-
Step by step guide on how to create a bot on telegram with node.js as backend
# Requirements
first of all we need to install the requirements
```shell
sudo apt install nodejs
sudo apt install npm
```
Afterwards, we need some dependencies. <br>
We will use express as our bots backend.
```shell
npm install express --save
npm install --save telegram-node-bot-api
```
Then, open telegram https://t.me/BotFather and generate your bot, it will give you an API token which we will need.
# Code
Now, open your server and copy this code. <br>
```node
var express = require('express');
const TelegramBot = require('node-telegram-bot-api');
const token = 'YOUR_API_KEY';
const bot = new TelegramBot(token, {polling: true});

var app = express();

app.set('port', (process.env.PORT || 5000));

app.get('/', function(request, response) {});

app.listen(app.get('port'), function() {
  console.log('Node app is running on port', app.get('port'));
});

bot.on('message', (message) => {
    const chatId = message.chat.id;
    const text = message.text;
  
    if (text === '/start') {
      bot.sendMessage(chatId, 'Welcome! Use the /greet command to say hello.');
    } else if (text === '/greet') {
      bot.sendMessage(chatId, `Hello, ${message.from.first_name}! 🤖`);
    } else {
      bot.sendMessage(chatId, `I don't know what ${text} means.`);
    }
});
```
Change 'YOUR_API_KEY' with the one botFather gave you. <br>
Congrats !!! we have a simple bot which we could do things on. <br>
Now lets upgrade our bot and add support of gpt.
