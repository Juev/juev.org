---
title: "Установка временной зоны при использовании Netlify"
date: "2017-04-01T17:17:00+0300"
tags:
  - web
  - jekyll
  - netlify
---
Про [Netlify](https://www.juev.org/2017/03/12/netlify/ "Netlify как хостинг статических сайтов") я уже писал. Частично затронул тему про его возможности. Наконец сегодня решился полностью мигрировать свой сайт. Так как в статьях больше не использую список похожих, проблем со сборкой непосредственно в Continuous Integration Netlify у меня не возникает. Но столкнулся с тем, что в отличие от Travis, нет возможности управлять переменными среды, как например, нет возможности явно указать временную зону. В связи с чем комит новой статьи не приводил к появлению статьи на сайте, так как считалось, что статья написана в будущем.

Решение нашлось непосредственно в настройках самого Jekyll, достаточно добавить опцию в файл конфигурации `_config.yaml`:

```yaml
timezone: Europe/Samara
```

И во время сборки сайта время, указанное в статьях, переводиться в ту чесовую зону, что указана в конфигурации. При этом проводиться сопоставление с той часовой зоной, что выставлена в системе.
