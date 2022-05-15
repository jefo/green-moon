---
title: Becktest QQE+HULL Strategy v. 1.0.0
date: "2022-04-22T22:12:03.284Z"
description: "QQE+HULL strategy backtest on different pairs"
---

This is my first post on my new blog! How exciting!

I am glad to present you the first strategy that I recently tested.
I found this strategy at TradeIQ channel.
[embed video https://www.youtube.com/watch?v=wwmv6TU7dzU]

Первые результаты были не очень впечатляющими, но после оптимизации стали выглядеть неплохо.

(вставить картинку с финальными резульяттами)

Но обо всем по порядку.

## Indicators
* [QQE MOD](https://www.tradingview.com/script/TpUW4muw-QQE-MOD/)
First confirmation indicator.

* [HULL SUITE](https://www.tradingview.com/v/hg92pFwS/)
Second confirmation indicator.

* [Volume Oscilator]
Volume indicator to confirm volume increase.

* [ATR Bands](https://ru.tradingview.com/script/ziTzsSfo-ATR-Bands/)
Используется для определения уровня Stop Loss

## Настройки индикаторов
* HULL SUITE
Меняем Length на 60

Остальное оставляем как есть.

## Сигналы

### Long
![QQE+HULL strategy long example](./long_example.png)
* New BLUE histogram appeard
* HULL must be green and price has to be closed above it.
* Volume Oscilator must be greater then zero percent.

When all conditions met we open LONG position after TRIGGER candle closed.
Set Stop Loss below recent swing low and target Take Profit 1.5 the risk.

### Short
![QQE+HULL strategy short example](./short_example.png)
* New RED histogram appeard
* HULL must be red and price has to be closed below it.
* Volume Oscilator must be greater then zero percent.

When all conditions met we open LONG position after TRIGGER candle closed.
Set Stop Loss below recent swing low and target Take Profit 1.5 the risk.

## Backtest
* Ticker: `BTCUSDTPERP`
* Timeframe: `15m`
* Duration: `4 months`

* Начальный капитал: `1000 USDT`
* Risk per trade: `1% от аккаунта`
* Stop Loss: ATR 14; Multiplier: 3
* Take Profit Type: Risk/Reward
* Risk/Reward ratio: 1.5

Первый результаты:
![QQE+HULL strategy first result trades](./result_1_trades.png)
![QQE+HULL strategy first result](./result_1.png)

Давайте попробуем оптимизировать. 

## Оптимизация

Главный минус данной стратегии, то что она часто открывает сделки против основного тренда. Давайте это исправим c помощью [EMA] и [SuperTrend]. А так же отфильтруем сделки, которые открываеются на низковолатильном рынке с помощью [ADX](https://ru.tradingview.com/script/VTPMMOrx-ADX-and-DI).

![QQE+HULL strategy final result trades](./final-result_1_trades.png)
![QQE+HULL strategy equity curve](./result_1_eq.png)
![QQE+HULL strategy summary](./result_1_overview.png)

Дополнительные фильтры:
* `ADX > 15`
* Цена закрытия выше чем EMA 200
* Зеленый SuperTrend
Я решил добавить этот индикатор, потому что у фильтрации по EMA 200 был один минус. Мы получали ложные сигналы во время бокового движения цены. После добавления SuperTrend количество проигрышных сигналов стало меньше, а количество выигрышных практически не изменилось.

Измененные параметры:
* R/R: 1:3
Мы увеличим параметр Risk/Reward и разделим Take Profit на две части. Первую часть будем забирать на соотношении 1:1, вторую на 1:3
После взятия первого TP перемещаем Stop Loss в безубыток.

## Итог

Как мы видим в результате оптимизации удалось повысить превратить убыточную стратегию в прибыльную, winrate повысился на 16% c 36.7% до 51%.

