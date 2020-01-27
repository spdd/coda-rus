---
layout: archive
permalink: /node-operator/
title: ""
author_profile: false
header:
  image: "/images/coda.jpg"
---

# Станьте оператором ноды

<Alert kind="danger">
  
  Команды для управления ноды все еще доводятся до стабильного состояния поэтому в будущем могут измениться. Если вы обнаружили ошибки не стесняйтесь исправлять их, открывайте запросы на исправление (pull request) в репозитории разработчиков Coda!

</Alert>

Итак, мы уже настроили нашу Coda-ноду и отправили нашу первую транзакцию, давайте рассмотрим другие способы взаимодействия с сетью Coda, а именно участие в консенсусе и помощь в сжатии данных путем создания zk-SNARKs. Управляя узлом, который помогает защитить сеть, вы можете получить coda за ваши усилия.

## Участие в консенсусе

Сеть Coda защищена алгоритмом консенсуса [Proof-of-Stake (подтверждение доли)](/docs/glossary/#proof-of-stake). С этой моделью консенсуса вам не нужно иметь дорогое оборудование, как при майнинге биткойнов. Просто имея в своем кошельке coda (доля необходимая для запуска ноды), мы можем либо держать свою ноду, либо делегировать долю другому узлу. Давайте сначала рассмотрим как запутить свою ноду:

### Staking coda

<!-- Так как в нашем кошельке уже есть coda из [предыдущего шага](/docs/my-first-transaction), мы можем настроить этот кошелек для размещения(staking) coda, введя следующую команду, передав путь к файлу для соответствующего закрытого ключа (ранее мы создали пару ключей `keys/my-wallet`): -->

<!-- coda client set-staking -privkey-path keys/my-wallet -->

Так как мы уже имеем средства в нашем кошельке из [предыдущего шага](/docs/my-first-transaction), мы можем запустить демон с флагом `-propose-key`, чтобы начать размещать(staking) coda. Давайте остановим наш текущий процесс демона и перезапустим его с помощью следующей команды, передавая путь к файлу для соответствующего закрытого ключа (ранее мы создали пару ключей `keys/my-wallet`):

    coda daemon \
        -discovery-port 8303 \
        -peer /dns4/seed-one.genesis.o1test.net/tcp/10002/ipfs/12D3KooWP7fTKbyiUcYJGajQDpCFo2rDexgTHFJTxCH8jvcL1eAH \
        -peer /dns4/seed-two.genesis.o1test.net/tcp/10002/ipfs/12D3KooWL9ywbiXNfMBqnUKHSB1Q1BaHFNUzppu6JLMVn9TTPFSA \
        -propose-key keys/my-wallet
        
<Alert>
  
  Вы можете предоставить список файлов c ключами, для того чтобы ключить staking для нескольких кошельков одновременно.

</Alert>

Мы всегда можем проверить с какими кошельками в данный момент делаем staking, используя команду `coda client status`:

    coda client status
    
    Coda Daemon Status 
    -----------------------------------
    
    Global number of accounts:                     10045
    Block height:                                  249
    Max observed block length:                     249
    Local Uptime:                                  36s
    Ledger Merkle Root:                            ...
    Staged-ledger Hash:                            ...
    Staged Hash:                                   ...
    GIT SHA1:                                      ...
    Configuration Directory:                       ...
    Peers:                                         ...
    User_commands Sent:                            0
    SNARK worker:                                  None
    SNARK work fee:                                1
    Sync Status:                                   Offline
    Block producers running:                       Total: 1 (4vsRCVzBeSxp3iBQ1C3ahHyKjKVbPd93JLSAsqtRtmjB9Xhn29NBdnzT4o6Hb3iNwaFECrh18YsxhAkqMY8nZQrN8jRX5LfbB9h4p5csrRe8xza4VWToXnFaHtGx6gB9FKAr1eKebSiPyH5c)
    Best tip consensus time:                       epoch=6, slot=88
    Next proposal:                                 None this epoch…
    Consensus time now:                            epoch=6, slot=93
    Consensus Mechanism:                           proof_of_stake
    ...

Поле `Block producers running` в выводе команды, число аккаунтов которые в данный момент выполняют staking, в скобках связанный с аккаунтом открытый ключ.

<Alert kind="warning">

  Обратите внимание, что если вы выполняте staking независимо от средств в кошельке, вам необходимо всегда оставаться подключенным к сети, чтобы быть избранным производителем блоков. Если вы часто бываете все сети, возможно, лучше делегировать свою долю.

</Alert>

### Делегирование coda

Делегирование coda это альтернатива самостоятельному staking, с тем преимуществом, что вам уже не нужно постоянно оставаться подключенным к сети. Однако имейте в виду, что:  

- чтобы внести изменение делегата, вам нужно будет заплатить небольшую комиссию за транзакцию, так как это изменение записывается в блокчейне как транзакция

- делегат, делающий staking для вас, может принять решение взимать с вас комиссию за предоставление услуги staking

