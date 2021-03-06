---
title: "Современные языки программирования"
date: "2019-09-08T15:51:00+0300"
tags:
  - golang
  - rust
  - haskell
---
Сейчас читаю [Mastering Go - Second Edition](https://www.packtpub.com/programming/mastering-go-second-edition), и наткнулся на небольшой код, который используется для проверки быстродействия Go при работе с map-структурами. Вот он:

```go
package main

import (
	"runtime"
)

func main() {
	var N = 40000000
	myMap := make(map[int]int)
	for i := 0; i < N; i++ {
		value := int(i)
		myMap[value] = value
	}
	runtime.GC()
	_ = myMap[0]
}
```

Попробовал запускать на своем макбуке, результат оказался следующим:

```bash
$ time ./compare_go

real	0m11.253s
user	0m9.688s
sys	0m1.310s
```

Я уже писал программы на Rust, мне нравилось. Язык очень интересный, со своей философией. И главное, ориентирован на произодительность. Стало интересно, насколько лучше Rust работает с подобными структурами?? Быстро написал программу:

```rust
fn main() {
    let n: i32 = 40_000_000;
    let mut my_map = Vec::new();
    for i in 0..n {
        let value = i;
        my_map.push(value);
    }
    my_map.pop();
}
```

Запустил:
```bash
$ time ./compare_rs

real	0m2.359s
user	0m2.296s
sys	0m0.054s
```

Подумал, как круто работает Rust! Но затем понял, что реализовал программу с использованием вектора, а не мапа. И сравнивать подобным образом никак нельзя. Нужно использовать HashMap, переписал:

```rust
use std::collections::HashMap;

fn main() {
    let n: i32 = 40_000_000;
    let mut my_map = HashMap::new();
    for i in 0..n {
        let value = i;
        my_map.insert(value, value);
    }
     let _x = my_map[&0];
}
```

Результат был неутешительным:

```bash
$ time ./compare_rs_hashmap

real	0m58.802s
user	0m58.330s
sys	0m0.397s
```

58 секунд против 11, и затем поиск в гугл показал, что да, в Rust есть проблема с производительностью HashMap в связи с тем, что используются криптографические алгоритмы. И что вроде как всех все устраивает.

**update** 09/09/2019  -- Start update --

Получил письмо, в котором указывалось на то, что в Rust произодвительность стоит сравнивать только в релизных сборках и при компиляции нужно использовать оптимизирующие флаги. Что сделал:

```bash
rustc -C opt-level=3 compare_rs_hashmap.rs
```

Размер бинарника уменьшился почти на треть, и повторный запуск теста прошел почти на порядок быстрее:

```bash
$ time ./compare_rs_hashmap

real    0m7.537s
user    0m7.012s
sys     0m0.506s
```

-- End Update --

Чуть позже стало интересно, а что если сравнить с производительностью той же программы на Haskell?? Много читал, но сам почти ничего не писал на этом языке. Решил попробовать. Написал такой вариант:

```haskell
import qualified Data.Map as Map

main = let solution = Map.fromList $ map (\n -> (n, n)) $ take 40000000 [1..]
       in putStrLn ""
```

Но он у меня не заработал в связи с тем, что Хаскел ленивый язык и соответственно так как solution переменная нигде не используется, он просто ее не вычисляет. И вторая проблема была в том, что не мог реализовать программу, которая бы ничего не выводила. Обратился в Twitter, где [JH de Raigniac](https://twitter.com/JHRaigniac) объяснил мне, что нужно делать. И получилась следующая программа:

```haskell
{-# LANGUAGE BangPatterns #-}
import Data.Map (Map)
import qualified Data.Map as Map

myList = Map.fromList $! map makeTuple [1..40000000]
  where makeTuple x = (x, x)

main = let !solution = Map.lookup 1 myList
  in return ( )
```

Результаты ее работы:

```bash
$ time ./compare_hs

real	0m13.312s
user	0m10.247s
sys	0m2.253s
```

Что вполне сопоставимо с результатом Go-программы.

**update** 20.09.2019 -- Start update --

После того, как написал статью успокоился. Но уже вчера в голову пришла мысль сравнить производительность этого же алгоритма, но написанного на языке Lisp. Нашел ряд статей, освещающих работу с HashMap, но реализовать самостоятельно так и не удалось. Обратился за помощью к [Александру Артеменко](https://github.com/svetlyak40wt).

Довольно быстро он предоставил рабочее решение:

```lisp
(defun test ()
  (loop with num-elements = 40000000
        with hash = (make-hash-table :size num-elements)
        for i from 0 below num-elements
        do (setf (gethash i hash)
                 i)
        finally (gethash 0 hash)))
```

Запускал через repl sbcl, при этом нужно было дополнительно задавать размер памяти, в противном случае получали ошибку переполнения:

```bash
rlwrap ros dynamic-space-size=8000 run
```

Определяем функцию и затем запускаем ее:

```lisp
* (time (test))
Evaluation took:
  5.863 seconds of real time
  5.721497 seconds of total run time (4.532341 user, 1.189156 system)
  [ Run times consist of 1.030 seconds GC time, and 4.692 seconds non-GC time. ]
  97.58% CPU
  18,151,994,613 processor cycles
  4,009,778,800 bytes consed
```

Итого, без учета времени инициализации sbcl на выполнение ушло чуть менее 6 секунд. В два раза быстрее решения на Haskell и чуть быстрее решения на Rust.

-- End Update --

Обращаю внимание на то, что данные результаты совершенно не указывают на то, что один язык лучше другого и что их нужно использовать для того, чтобы выбрать язык для реализации следующего вашего проекта. Эти результаты просто констатация интереса, что проснулся при чтении книги. Только и всего.
