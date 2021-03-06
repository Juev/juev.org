---
title: "Jekyll & Hyde"
date: "2010-11-13T00:00:00+0300"
tags:
  - jekyll
  - hosting
keywords: jekyll,хостинг,github
---
На сегодняшний день создается множество различных движков для создания сайтов. Самые популярные это <code>WordPress</code>, <code>Drupal</code>, <code>Joomla</code>, есть еще множество других, отличающихся по своим функциям и возможностям.

Перечисленные выше генераторы работают по принципу -- генерировать все так, как это было определенно и делать это каждый раз заново при каждом обращении к странице. Естественно возникает проблема в том, что даже для небольшого сайта, состоящего из нескольких страниц требуется довольно существенные процессорные затраты. И эти затраты тем больше, чем больше посетителей. И в последнее время я все меньше понимаю, зачем использовать данный способ генерации сайтов.

К другому классу относятся генераторы статических сайтов. Это скрипты/программы, которые позволяют на основании исходного кода и шаблонов страниц, подготовить готовые html-страницы, которые можно использовать на любом сервере. Таких генераторов не так много, но они есть. Самые известные -- это <code>Emacs Muse</code> и <code>Jekyll</code>.

Про использование Emacs Muse очень хорошо описано на сайте Alex Ott в статье <a href="http://alexott.net/ru/writings/EmacsMuseMyPage.html" rel="nofollow">Как делался этот сайт</a>, поэтому останавливаться на этом способе я не буду.

А затрону движок <code>Jekyll</code>, который используется в социальной сети <a href="http://pages.github.com">Github.com</a>.

<a href="http://pages.github.com" rel="nofollow">Github.com</a> предоставляет при регистрации несколько вариантов тарифных планов, самый распространенный -- бесплатный, который позволяет использовать на сервере до 300 мегабайт дискового пространства для размещения своих файлов и организовывать неограниченное число публичных репозиториев.

Для того, чтобы организовать сайт, необходимо создать репозиторий вида <code>username.github.com</code>. В имени создаваемого репозитория, как понятно из примера, используется доменное имя третьего уровня, которое будет задействовано при работе.

Изменить домен на свой очень просто. Достаточно создать в репозитории файл <code>CNAME</code>, в котором будет содержаться только одна строка с именем нужного домена. После чего в панели регистратора доменного имени необходимо создать запись типа <strong>A</strong>, указывающую на ip-адрес <code>207.97.227.245</code>.

По сути уже все готово для размещения любого статического сайта. Но нам ведь интересн не это. Интересно задействовать <code>jekyll</code>! Сделать это так же довольно просто.

В репозитории создается следующая файловая структура:

```text
 .
 |-- _config.yml
 |-- _layouts
 |   |-- default.html
 |   `-- post.html
 |-- _posts
 |   |-- 2007-10-29-why-every-programmer-should-play-nethack.textile
 |   `-- 2009-04-26-barcamp-boston-4-roundup.textile
 |-- _site
 `-- index.html
 ```

Где:
<strong>_config.yml</strong> -- файл, который содержит в себе конфигурацию <code>jekyll</code>, о которой подробно можно прочесть на странице <a href="https://github.com/mojombo/jekyll/wiki/configuration" rel="nofollow">Configuration</a>. Причем в файле обычно прописываются только какие-то изменения. По умолчанию используется стандартная конфигурация.

<strong>_layouts</strong> -- это каталог, содержащий используемые шаблоны страниц. Обычно это два шаблона, использующих язык разметки Liquid.

<strong>_posts</strong> -- это каталог, который будет содержать в себе записи в формате <code>markdown</code> или <code>html</code>. Имя файла должно быть в следующем формате <code>YEAR-MONTH-DATE-title.MARKUP</code>.

<strong>_site</strong> -- это каталог, в котором будут размещаться файлы, готовые для публикации. Рекомендуют данный каталог прописывать в файле <code>.gitignore</code>.

<strong>index.html</strong> -- файл, в формате Liquid-разметки, содержащий в себе описание того, как будет выглядеть главная страница создаваемого сайта.

<code>Jekyll</code> может работать в виде скрипта, который будет запускаться только при изменении файлов. Обычно это желается путем прописывания строки запуска в скрипты <code>git</code>, отрабатываемых после очередного <code>push</code>. При таком режиме работы <code>jekyll</code> работает очень редко, только в тех случаях, когда требуется генерация страниц. Именно этот режим работы используется на сервере <a href="https://github.com" rel="nofollow">Github.com</a>

Другой вариант работы -- это когда <code>jekyll</code> сам начинает выполнять функции web-сервера. Довольно удобно использовать данный режим при отладке, когда генерация сайта производиться на локальной машине. При этом не требуется ставить никаких дополнительных сервисов, типа apache или nginx. И в то же время, можно посмотреть на результат своей работы, перед тем, как загрузить файлы на сервер.

Довольно большая коллекция сайтов, использующих <code>Jekyll</code> представлена на странице <a href="https://github.com/mojombo/jekyll/wiki/sites" rel="nofollow">Sites</a>. Там же есть ссылки на репозитории данных сайтов, можно посмотреть или использовать разметку страниц, которая используется на данных сайтах.

Для добавления комментариев обычно используют внешние системы, типа Disqus или IntenseDebate. Кто-то пытается разработать свою систему комментариев. Динамика сайту добавляется за счет использования внешних javascript-файлов.

На мой взгляд, <code>jekyll</code> и ей подобные системы генерации сайтов намного более эффективны при создании сайтов и порталов, чем динамические движки. Они позволяют снизить и время загрузки страниц сайта и существенно снизить нагрузку на сервер. Как мне кажется, будущее интернета именно за подобными системами, которые будут создавать основу сайта, а динамический контент будет организован за счет внешних сервисов. Что же будет в реальности, посмотрим.
