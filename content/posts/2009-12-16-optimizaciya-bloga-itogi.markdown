---
title: "Оптимизация блога - итоги"
date: "2009-12-16T00:00:00+0300"
tags:
  - wordpress
  - apache
keywords: wordpress,apache
---
Добился определенных результатов в оптимизации своего блога.

Теперь он загружается в три раза быстрее, чем раньше, при этом повторные открытия страниц тратят минимум трафика и осуществляются еще быстрее. Расскажу, как я этого добился.

Во-первых, однозначного ответа дать невозможно. В каждом случае приходиться анализировать
сам процесс отдачи страниц сервером, и на основании этого анализа уже принимать
определенные решения. Для анализа используем уже упоминаемый мной ресурс <a
href="http://www.webpagetest.org" rel="nofollow">www.webpagetest.org</a>. В результате получаем графики загрузок, обращений к серверу, на основании которых становиться понятным, что именно расходует много времени, какой компонент и в принципе уже становиться ясно, что нужно делать.

Напоминаю, до оптимизации ситуация выглядела следующим образом:

Первая загрузка (с чистым кэшем браузера) занимала 8.941s и на графиках выглядела так:

![](https://static.juev.org/2009/12/First-a.png)

![](https://static.juev.org/2009/12/First.png)

Повторная загрузка проходила быстрее, то есть 3.035s и на графиках выглядела так:

![](https://static.juev.org/2009/12/Two-a.png)

![](https://static.juev.org/2009/12/Two.png)

Рисунки пришлось разбивать на две части, заголовок и тело, ввиду того, что на экране таблицы не вмещались.

В результате анализа графиков становиться понятно, что очень много времени тратиться на загрузку следующих вещей: плагин wordpress-seo-pager, который я по сути не использовал, так как стоит All in One SEO Pack, затем на первом графике видно, как много времени потребляет статистика wordpress, сторонние счетчики, плагин WP-SpamFree и что еще характерно, при повторных обращениях к сайту производиться практически его полная загрузка, сокращение времени обусловлено только тем, что производиться проверка каждого из загруженных файлов и только в том случае, если он не менялся, загружается то, что в кэше. На проверку так же тратиться время.

Первое, что сделал, это выключил те плагины, что тратили время, единственно, оставил в работе WP-SpamFree, просто потому, что альтернативы ему нет. Избавился от всех вариантов статистики, жить стало легче. Затем нашел способ задействовать кеш браузера более полно, для это пришлось прописывать определенные значения в файл .htaccess на сервере:

```apache
FileETag MTime Size
<ifmodule mod_expires.c>
   <filesmatch "\.(jpg|gif|png|css|js)$">
     ExpiresActive on
     ExpiresDefault "access plus 1 year"
   </filesmatch>
</ifmodule>
```

В результате стало видна еще одна проблема, причиной которой была выбранная тема. Для отрисовки заголовков использовался скрипт cufon, который во-первых весит порядка 300 килобайт, и во-вторых довольно долго загружался, отказываясь при этом кешироваться. Выключил в параметрах темы использование данного скрипта. Все изображения, которые подгружались со сторонних серверов перенес к себе.

Затем начал экспериментировать с различными плагинами. Оказалось, что google analitics потребляет совсем немного времени, потому можно от него не отказываться, а вот с WP-SpamFree просто приходиться считаться. Порядка полсекунды добавляет, но без данного плагина довольно тяжко, потому решил оставить. В итоге получилась следующая картина:

Первая загрузка занимает 1.790s  и на графиках выглядит так:

![](https://static.juev.org/2009/12/First-optim.png)

Последующие загрузки 0.994s и график:

![](https://static.juev.org/2009/12/Two-optim.png)

То есть первое обращение к блоге теперь занимает порядка 2 секунд, а каждое последующее не более одной секунды. То есть в результате анализа были выявлены слабые места и скорость загрузки изменилась примерно в 3-4 раза.

В обоих случаях (до оптимизации и после) использовался плагин WP Super Cache в режиме полного кэширования. Перед каждым тестом кэш сбрасывался.

Продолжаю думать на переносом блога в Movable Type...
