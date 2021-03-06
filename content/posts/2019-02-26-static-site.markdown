---
title: "Инфраструктура для статичного сайта"
date: "2019-02-26T02:34:00+0300"
tags:
  - amazon
  - github
  - web
---
## Github

Github позволяет генерировать статичный сайт с использованием генератора [Jekyll](https://jekyllrb.com/). И размещать его с помощью [Github Pages](https://pages.github.com/). Время сборки сайта из примерно 500 страниц составляет менее минуты, включая время публикации. При использовании публичного репозитория размещение сайта бесплатное. Ограничений на число сайтов нет.

Есть возможность использования доменного имени username.github.io, при этом будет использоваться сертификат самого Github. И так же есть возможность использования своего собственного домена. В этом случае автоматически будет сгенерирован сертификат для домена с использованием LetsEncrypt.

## Netlify

Репозиторий с исходным кодом статичного сайта можно по прежнему размещать с помощью Github, а вот собирать и размещать сайт можно с помощью бесплатного [Netlify](https://www.netlify.com/). Как и в случае с Travis, данный сервис позволяет использовать различные генераторы. Но тут есть свои ограничения. И плюс для размещения сайта используется их собственный CDN.

Стоит так же обратить внимание на использование https по умолчанию при использовании Netlify.

## Travis

Если Github позволяет использовать только один определенный генератор, то Travis позволяет включать в сборку любой из существующих генераторов. Единственно нужно учитывать, что к времени сборки сайта будет прибавляться время создания необходимого окружения. В случае с тем же Jekyll время увеличивается примерно до 2 минут. И нужно так же понимать, что Travis лишь система сборки, и необходимо так же озадачиться размещением.

В качестве хостинга можно выбрать различные варианты:
* Github Pages
* Amazon S3
* Отдельный сервер (к примеру на DigitalOcean)

Во всех перечисленных случаях сертификат придется генерировать и размещать самостоятельно. За исключением разве что Github Pages.

В случае же с Amazon S3 можно перейти на использование https за счет использования CloudFlare. Если размещать свои домены у них, появляется возможность проксировать запросы к сайту через их сервера, и размещать на них кастомные сертификаты для домена от CloudFlare сроком на несколько лет.

## Итоги

Таким образом на сегодняшний день у нас есть масса возможностей размещать свои сайты с использованием https либо совершенно бесплатно, либо по минимальной стоимости. А уж какой движок для статичного сайта использовать, или где его размещать, выбирать только нам.
