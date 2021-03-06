---
title: "Cloudflare"
date: "2017-02-19T15:29:00+0300"
tags:
  - vps
  - services
image: https://static.juev.org/2017/02/rejected_cloudflare_orange.png
---
После того, как столкнулся с проблемами на DigitalOcean, провел миграцию на [Scaleway](https://www.juev.org/2017/02/13/scaleway/ "ScaleWay: впечатления от работы"), и затем после появления нового тарифа у [Linode](https://www.linode.com/?r=ac993402fdf5112eb9afdfd2cccf10f798140ea7 "Linode") перенес сайт на их сервера.

Так как в последнее время довольно часто менял хостинг, написал [скрипт по миграции](https://raw.githubusercontent.com/Juev/juev.org/492f223518d49769fad317d7de550adac2528b2d/config/migrate.sh "migrate.sh"). Который позволяет провести перенос сайта и настроить VPN буквально в одну команду.

Заодно провел миграцию DNS с амазоновского Route53 на [Cloudflare](https://www.cloudflare.com "Cloudflare"). Как оказалось, Cloudflare столь же эффективно позволяет управлять доменной зоной, вплоть до указания CNAME для корневого домена. И обеспечивает целый ряд дополнительных сервисов, типа CDN, редиректов и безопасности.

Есть несколько тарифов, из которых я  использую бесплатный вариант, который позволяет мне управлять доменной зоной, задавать три правила по редиректу и использовать их CDN.

Скорость сайта незначительно, но выросла. При этом провел дополнительную настройку DNSSEC для своих доменов. Так же Cloudflare позволяет организовать раздачу контента через SSL, с использованием их сертификатов. И таким образом теперь можно довольно быстро организовать вебсайт на том же Amazon S3, но раздавать его уже с использованием https-протокола.

В итоге получил экономию в размере $1 в месяц за счет отказа от Amazon Route53, но дополнительно получил целый ряд преимуществ.
