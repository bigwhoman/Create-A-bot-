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
Now lets make our project better by migrating it to docker.<br>
create these files in your directory.<br>
## .env file
```env
BOT_TOKEN=your-api-key
```
## code section
```node
require('dotenv').config();
const TelegramBot = require('node-telegram-bot-api');
const bot = new TelegramBot(process.env.BOT_TOKEN, { polling: true });

bot.on('message', (message) => {
    const chatId = message.chat.id;
    const text = message.text;
  
    if (text === '/start') {
      bot.sendMessage(chatId, 'Welcome! Use the /greet command to say hello.');
    } else if (text === '/greet') {
      bot.sendMessage(chatId, `Hello, ${message.from.first_name}! 🤖`);
    } else if (text === '/gele'){
      bot.sendMessage(chatId, `I don't know what ${text} means.`);
    }
});
```
## package.json
```json
{
    "name": "telegram-bot",
    "version": "1.0.0",
    "description": "A simple Telegram bot",
    "main": "bot_server.js",
    "scripts": {
      "start": "node bot.js"
    },
    "dependencies": {
      "dotenv": "^10.0.0",
      "node-telegram-bot-api": "^0.54.0"
    }
}
```
## Dockerfile
```Dockerfile
FROM node:latest

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "bot_server.js"]
```
## docker compose
```yaml
version: '3.8'
services:
  bot:
    build: .
    environment:
      - BOT_TOKEN=${API_KEY}
    # env_file: .env
    ports:
      - "3000:3000"
    restart: always
```
## Running
Now use the command below (in the projects root directory) to run your bot, -d means that the container is detached and will run in the background.
```shell
docker-compose up --build -d
```
After it is finished, use docker ps to the container
```shell
root@linux:~/bot# docker ps
CONTAINER ID   IMAGE                                           COMMAND                  CREATED          STATUS          PORTS                                        NAMES
7f6e64ddfc59   bot_bot                                         "docker-entrypoint.s…"   16 seconds ago   Up 11 seconds   0.0.0.0:3000->3000/tcp                       bot_bot_1
```
If there is any problem you could log the containter with the command below 
```shell
root@linux:~/bot# docker logs container_id
```
# TODO : Create a ask pdf bot
