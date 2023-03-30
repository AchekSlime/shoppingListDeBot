# Cледуйте следующим шагам, чтобы создать своего debot. Выполните следующие команды

##  Для начала вам нужно подготовить несколько файлов.
Эта команда возвращает вам файл `ShopListDebot.abi.json` в шестнадцатеричном формате. Он понадобится нам при вызове функции setABI. Это обязательно.
```shell 
cat ShopListDebot.abi.json | xxd -ps -c 20000
``` 
Поместите вывод этой функции в файл dabi.json. И сделайте следующий формат json `{"dabi":"output of the previous command"}`.

---  
Эта команда возвращает информацию о вашем контракте `shopList.sol`, которая необходима для создания начального состояния вашего списка покупок
```shell 
tonos decode stateinit shopList.tvc --tvc
```  
Поместите вывод этой функции в файл `code.json`.
  
---  
## Автоматический способ с помощью bash  скрипта
1. Вам необходимо подготовить файл `code.file`, как указано выше
2. Просто запустите bash-скрипт `localDebot.sh`.

## Команды для развертывания и запуска debot вручную
1.
```  
tonos genaddr ShopListDebot.tvc --genkey debot.keys.json ShopListDebot.abi.json > log.log  
``` 
Эта команда генерирует пару ключей для вашего debot в файл `debot.keys.json` и помещает информацию об этом в файл `log.log`.

2.
```  
tonos -u http://localhost call 0:b5e9240fc2d2f1ff8cbb1d1dee7fb7cae155e5f6320e585fcc685698994a19a5 --abi giver.abi.json --sign giver.keys.json sendTransaction '{"dest":"<address>","value":10000000000,"bounce":false}'  
``` 
Эта команда отправляет токены на адрес <адрес> из файла `log.log` в поле `Raw address: 0:...` поле.

3.
```  
tonos -u http://localhost deploy ShopListDebot.tvc "{}" --sign debot.keys.json --abi ShopListDebot.abi.json  
```  
Эта команда просто деплоит вашего debot-а.

4.
```  
tonos -u http://localhost call <address> setABI dabi.json --abi ShopListDebot.abi.json --sign debot.keys.json  
```  
Эта функция вызывает необходимую функцию `setABI` для debot.

5.
```  
tonos -u http://localhost call <address> setCode code.json --abi ShopListDebot.abi.json --sign debot.keys.json  
```  
Эта команда устанавливает начальное состояние для вашего debot.

## Запуск debot
```  
tonos -u http://localhost debot fetch <address>  
```