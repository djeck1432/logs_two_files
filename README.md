# Как использовать библиотеку logging для двух и более файлов 

## Дерево файлов: 

```tg_bot.py```- файл  №1
<br>
```tg_bot.py```- файл №2
<br>
```logs.py```- основной файл для логов
<br>

## Шаг 1```logs.py```
 
1. Задаем базовую конфигурацию библиотеки:```logging```

```
import logging

....

def main():
  logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',level=logging.INFO)
```

2. Потом инициализируем класс ```TelegramLogsHandler```
```
class TelegramLogsHandler(logging.Handler):
    def __init__(self, telegram_log_access_token, telegram_log_chat_id):
        logging.Handler.__init__(self)
        self.telegram_access_token = telegram_log_access_token
        self.telegram_log_chat_id = telegram_log_chat_id
        self.bot_log = telegram.Bot(token=telegram_log_access_token )

    def emit(self, record):
        log_entry = self.format(record)
        return self.bot_log.send_message(chat_id=self.telegram_log_chat_id, text=log_entry)
```
## Шаг 2 ```tg_bot.py```
1. Делаем импорт ```logs.py``` 
2. Cоздаем свой ```logger``` в начале кода 
```
import logging
import logs
...
tg_logger = logging.getLogger('telegram')
...
```
3. Задаем базовую конфигурацию библиотеки ```logging```:
```
...

def main():

logging.basicConfig(format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
                        level=logging.INFO)
```
4. Расставляем в нужный местах сборщик логов ```logger.info('some text')```
5. в функции ```main()``` вызываем :
```
def main():
...
logs.main()
...
```

## Шаг 3 ```logs.py```
1. В функции ```main()``` cоздаем следующий ```logger```:

```
def main():
...
tg_logger = logging.getLogger('telegram')
```
2. Добавляем данному ```tg_logger``` ```Handler``` который и будет отправлять боту в телеграм все логи:

```
def main():
...
tg_logger = logging.getLogger('telegram')
tg_logger.addHandler(bot)
```

#### С другим файлом ```vk_bot.py``` нужно повторить то же, что и в примере с ```tg_bot.py```


