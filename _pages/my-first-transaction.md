---
layout: archive
permalink: /my-first-transaction/
title: ""
author_profile: false
header:
  image: "/images/coda3.jpg"
---


# Моя первая транзакция

В этом разделе мы отправим нашу первую транзакцию в сеть Coda. После [установки coda](../getting-started), нам нужно создать новый аккаунт  перед тем как отправлять и получать coda. Давайте сначала запустим ноду чтобы начать набирать команды.

## Запускаем ноду

Введите следующие команды для запуска и подключения к сети ноды Coda:

    coda daemon \
        -discovery-port 8303 \
        -peer /dns4/seed-one.genesis.o1test.net/tcp/10002/ipfs/12D3KooWP7fTKbyiUcYJGajQDpCFo2rDexgTHFJTxCH8jvcL1eAH \
        -peer /dns4/seed-two.genesis.o1test.net/tcp/10002/ipfs/12D3KooWL9ywbiXNfMBqnUKHSB1Q1BaHFNUzppu6JLMVn9TTPFSA

Порт и хосты (peer) указанные выше относятся к seed адресам - это начальные узлы, к котором мы будем подключаться к сети для синхронизации ноды. Поскольку Coda является [одноранговым(peer-to-peer)](../glossary/#peer-to-peer) протоколом, у сети Coda нет центрального сервера. Если вы перенаправили свои порты (не TCP 8303 - порт используется по умолчанию) вам нужен дополнительный флаг к команде выше:  `-external-port <custom-TCP-port>`.

<Alert>
  
  Демон coda (coda daemon) должен быть запущен всегда, когда вы запускаете команды из `coda client`, поэтому убедитесь, что вы не уничтожили его случайно.

</Alert>

Смотри [здесь](/coda-rus/troubleshooting/) описаны общие проблемы при первом запуске узла.

## Проверка подключения

Теперь, когда мы запустили ноду и запустили демон Coda, откройте еще одну командную оболочку(терминал) и выполните следующую команду:

    coda client status

<Alert>

  На данный момент может пройти около минуты, прежде чем `coda client status` подключится к демону при первом запуске. Поэтому если вы видите `Error: daemon not running. See coda daemon`, просто немного подождите и попробуйте снова.

</Alert>



Вывод будет содержать следующие поля (показаны не все поля):

    ...
    Peers:                                         Total: 4 (...)
    ...
    Sync Status:                                   Bootstrap

Если вы видите `Sync Status: Bootstrap`, это означает что нода Coda загружается и должна синхронизироваться с остальной частью сети. Нода должна получить все необходимые данные, это займет какое-то время, наберитесь немного терпения. Когда статус синхронизации изменится на `Synced` и узел подключен к одному или более пирам (одноранговым узлам), это означает что мы полностью синхронизировали нашу ноду и подключились к сети!

## Создание новых аккаунтов

После синхронизации нашей ноды мы должны создать пару открытых/закрытых ключей (public/private), чтобы мы могли подписывать транзакции и cгенерировать адрес для приема платежей. Из соображений безопасности мы должны поместить ключи в каталог(папку), доступ к которой должен быть только у вас (осталяя ключи в общедоступном месте вы рискуете потерять все монеты coda).

Запустите одну из следующих команд, которая создаст открытый и закрытый ключи `my-wallet` и` my-wallet.pub` в каталоге `keys`:

    coda client-old generate-keypair -privkey-path keys/my-wallet 
  
    coda accounts create
    
<Alert kind="warning">

  Публичный ключ можно свободно передать кому угодно, но будьте очень осторожны с файлом закрытого ключа. Никогда не передавайте этот закрытый ключ кому-либо, так как он является эквивалентом пароля для ваших средств.

</Alert>

Вывод команды будет выглядеть примерно так:

    Keypair generated
    Public key:  4vsRCVbLm6LvUyzYWT95WaCzyi4D4UHxRpLBhMn7q2mRPgNCgRG3Jr3tDuhgQdmzbvCcBxhUwB3REpY2Dyf1NAxSSs8Q2vdJX93pT7eyqcyRU2S9UpDddDgovj46BSknNjzydKoopebp5Kva
    
Поскольку открытый ключ довольно длинный и его трудно запомнить, давайте сохраним его в [переменные среды](https://ru.wikipedia.org/wiki/%D0%9F%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D0%BD%D0%BD%D0%B0%D1%8F_%D1%81%D1%80%D0%B5%D0%B4%D1%8B):

    export CODA_PK=<public-key>

Теперь мы можем получить доступ к переменной $CODA_PK из любого места и даже из других программ. Обратите внимание, что переменная будет сохранена только для текущего сеанса командной оболочки (терминала), поэтому, если вы хотите использовать ее в будущем, вы можете добавить ее в свой профиль командной оболочки `~/.profile` или `~/.bash_profile`.

## Запросить coda (тестовые)

Для того чтобы отправить нашу первую транзакцию, мы сначала должны получить немного тестовых coda. Перейдите на наш [Coda Discord сервер](https://bit.ly/CodaDiscord) присоединитесь к каналу `#faucet` и попросите бота Tiny выслать вам немного coda (обычно он высылает 100 тестовых coda). Пример как сделать запрос у Tiny:

    $request <public-key>
    
Как только Tiny одобрит ваш запрос, следите за Discord каналом, чтобы увидеть, когда транзакция будет одобрена. Наберитесь терпения ваши тестовые coda прийти через несколько минут.

Можно проверить баланс, чтобы убедиться что мы получили coda, выполнив следующую команду, передав наш открытый ключ:

    coda client get-balance -public-key $CODA_PUBLIC_KEY

Вы можете увидеть `No account found at that public_key (zero balance)`. Наберитесь терпения! В зависимости от трафика в сети, до завершения транзакции может пройти несколько блоков.

Пока вы ждете, взгляните на логи своего coda демона для новых блоков, которые будут сгенерированы. Выполните следующую команду, чтобы увидеть текущий номер блока:

    coda client status

## Совершаем платеж

Наконец мы добрались до самого интересного места, отправки нашей первой транзакции! Для тестирования уже есть [эхо сервис](https://github.com/CodaProtocol/coda-automation/tree/master/services/echo) отправив на него ваши coda, он немедленно вернет ваш платеж за вычетом комиссии за транзакцию.

<Alert kind="warning">

В данный момент есть известная проблема с эхо сервисом которая препятствует возврату вашего платежа! Не волнуйтесь мы все еще даем тестнет очки[\*](#disclaimer) за выполнения заданий.

</Alert>

Давайте уже отправим недавно полученные coda на этот сервис чтобы увидеть как выглядит наш платеж:

    coda client send-payment \
      -amount 20 \
      -receiver 4vsRCVNep7JaFhtySu6vZCjnArvoAhkRscTy5TQsGTsKM4tJcYVc3uNUMRxQZAwVzSvkHDGWBmvhFpmCeiPASGnByXqvKzmHt4aR5uAWAQf3kqhwDJ2ZY3Hw4Dzo6awnJkxY338GEp12LE4x \
      -fee 5 \
      -sender $YOUR_PUBLIC_KEY
      
Если вам интересно что мы передали в команде выше:

- В `amount` мы отправляем наши тестовые `20` coda
- `receiver` - это открытый ключ [эхо сервиса](https://github.com/CodaProtocol/coda-automation/tree/master/services/echo)
- Для `fee` давайте использовать текущий рыночный курс комиссии для транзакций `5` coda
- `sender` - это полный открытый ключ который расположен в `*.pub`. Например:
`4vsRCVNep7JaFhtySu6vZCjnArvoAhkRscTy5TQsGTsKM4tJcYVc3uNUMRxQZAwVzSvkHDGWBmvhFpmCeiPASGnByXqvKzmHt4aR5uAWAQf3kqhwDJ2ZY3Hw4Dzo6awnJkxY338GEp12LE4x`

Если команда составлена правильно, мы должны получить ответ, который выглядит следующим образом:

    Dispatched payment with ID 3XCgvAHLAqz9VVbU7an7f2L5ffJtZoFega7jZpVJrPCYA4j5HEmUAx51BCeMc232eBWVz6q9t62Kp2cNvQZoNCSGqJ1rrJpXFqMN6NQe7x987sAC2Sd6wu9Vbs9xSr8g1AkjJoB65v3suPsaCcvvCjyUvUs8c3eVRucH4doa2onGj41pjxT53y5ZkmGaPmPnpWzdJt4YJBnDRW1GcJeyqj61GKWcvvrV6KcGD25VEeHQBfhGppZc7ewVwi3vcUQR7QFFs15bMwA4oZDEfzSbnr1ECoiZGy61m5LX7afwFaviyUwjphtrzoPbQ2QAZ2w2ypnVUrcJ9oUT4y4dvDJ5vkUDazRdGxjAA6Cz86bJqqgfMHdMFqpkmLxCdLbj2Nq3Ar2VpPVvfn2kdKoxwmAGqWCiVhqYbTvHkyZSc4n3siGTEpTGAK9usPnBnqLi53Z2bPPaJ3PuZTMgmdZYrRv4UPxztRtmyBz2HdQSnH8vbxurLkyxK6yEwS23JSZWToccM83sx2hAAABNynBVuxagL8aNZF99k3LKX6E581uSVSw5DAJ2S198DvZHXD53QvjcDGpvB9jYUpofkk1aPvtW7QZkcofBYruePM7kCHjKvbDXSw2CV5brHVv5ZBV9DuUcuFHfcYAA2TVuDtFeNLBjxDumiBASgaLvcdzGiFvSqqnzmS9MBXxYybQcmmz1WuKZHjgqph99XVEapwTsYfZGi1T8ApahcWc5EX9
    Receipt chain hash is now A3gpLyBJGvcpMXny2DsHjvE5GaNFn2bbpLLQqTCHuY3Nd7sqy8vDbM6qHTwHt8tcfqqBkd36LuV4CC6hVH6YsmRqRp4Lzx77WnN9gnRX7ceeXdCQUVB7B2uMo3oCYxfdpU5Q2f2KzJQ46

## Проверка баланса

Теперь когда мы можем совершать транзакции, может быть полезно знать наш баланс, чтобы не растратить все наши тестовые монеты! Давайте проверим наш текущий баланс, выполнив следующую команду передав открытый ключ созданного ранее нами аккаунта:

    coda client get-balance -public-key $CODA_PK
    
Ответ будет выглядеть примерно так(ваш баланс может быть другой):

    Balance: 50 coda
    
Как только вы освоитесь с основами создания адреса(открытый ключ), отправки и получения coda, можно перейти к действительно уникальным особенностям сети Coda - [участвовать в консенсусе и помогать сжимать блокчейн](/coda-rus/node-operator).

<Alert>
    
<span id="disclaimer">
  Тестнет баллы предназначены исключительно для отслеживания вклада в Тестовую сеть и эти баллы не имеют какой либо ценности в денежном эквиваленте. Тестнет баллы не подлежат передаче и обмену или обмену на любые криптовалюты или цифровые активы. Мы можем в любое время изменить или исключить тестнет баллы.
</span>

</Alert>
