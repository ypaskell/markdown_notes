
//************************************************* <br>
// Author:         ypprog                           <br>
// E-mail:         pan.yiping.fi@gmail.com          <br>
// Date:           2022-11-24                       <br>
// Description:                                     <br>
// Copyright 2022 by ypprog. All Rights Reserved    <br>
//************************************************* <br>

# [lang] Haskell and Funtional Programming

Haskell Links

* [Haskell reference](http://zvon.org/other/haskell/Outputprelude/index.html)
* [Real World Haskell](https://book.realworldhaskell.org/read/)

## Functional Programming 風格的 C 語言實作

[Jserv](https://hackmd.io/@sysprog/c-functional-programming)

Github Link

* [ypprog/Functional-Programming-in-C](https://github.com/ypprog/Functional-Programming-in-C)

[Why Functional Programming Matters](http://www.cs.kent.ac.uk/people/staff/dat/miranda/whyfp90.pdf) 是篇 1984 年的論文，在 1990 年做小幅度修改，主要凸顯出 FP 的優點與重要特質。

* 中文翻譯：[函數式程式設計為什麼至關重要](https://www.byvoid.com/zhs/blog/why-functional-programming)

Fuctional proramming 特點:

* 運算用的 function 大多只包含條件式及遞迴呼叫;
* [HOF (Higher-order fuctions)](https://en.wikipedia.org/wiki/Higher-order_function)特色
  * function 可當作參數傳入 function 之中
  * function 可以回傳 function
* No implicit [Side Effect](https://en.wikipedia.org/wiki/Side_effect_(computer_science)
* [lazy evaluation](https://en.wikipedia.org/wiki/Lazy_evaluation)

## Haskell 趣味指南

Link

* [Haskell 趣味指南](https://learnyouahaskell.mno2.org/)
* [英文版](http://learnyouahaskell.com/chapters)

### Chapter 2. Statring out

* List
  * the List Monster: head, tail, last and init

    ![list monster](http://s3.amazonaws.com/lyah/listmonster.png)

  * `length` get length
  * `null` check empty
  * `reverse`
  * **`take`**
    * `take 3 [5,4,3,2,1]` --> [5, 4, 3]
  * `drop` drop n items from the front
  * `maximum`, `minimun`
  * `sum`, `product`
  * `elem` takes a thing and a list of things and tells us if that thing is an element of the list

    ```Haskell
    ghci > 4 `elem` [3,4,5,6`
    True
    ```
* Texas ranges

  ```Haskell
  ghci> [1..20]
  [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]
  ghci> ['a'..'z']
  "abcdefghijklmnopqrstuvwxyz"
  ghci> [2,4..20]
  [2,4,6,8,10,12,14,16,18,20]
  ```

* List comprehension

  ```Haskell
  ghci> [ x | x <- [10..20], x /= 13, x /= 15, x /= 19]
  [10,11,12,14,16,17,18,20]
  ```
* Tuples `(8,11)`
  * `fst`, `snd`
  * `zip`

    ```Haskell
    ghci> zip [1,2,3,4,5] [5,5,5,5,5]
    [(1,5),(2,5),(3,5),(4,5),(5,5)]
    ghci> zip [1 .. 5] ["one", "two", "three", "four", "five"]
    [(1,"one"),(2,"two"),(3,"three"),(4,"four"),(5,"five")]
    ```

### Chapter 3. Types and Typeclasses

* Types: Int, Integer, Float, Double, Char, Bool
* Type class: 可以支援不同 Type 的操作
  * `Eq`, `Ord`, `show`, `read` typeclasses
  * Bounded: `minBound`, `maxBound`
  * Num typeclass, Integral typeclass, Float typeclasses

### Chapter 4. Syntax in Functions

* Pattern matching
* Guards
* Where
* let
* Case expression

### Chapter 5. Regression

### Chapter 6. Higher Order Functions

* Curried functions
* Map and filter
  * map :: (a -> b) -> [a] -> [b]

    ```Haskell
    -- returns a list constructed by appling a function (the first argument) to all items in a list passed as the second argument

    Input: map abs [-1,-3,4,-12]
    Output: [1,3,4,12]
    ```

  * filter :: (a -> Bool) -> [a] -> [a]

    ```Haskell
    -- returns a list constructed from members of a list (the second argument) fulfilling a condition given by the first argument

    Input: filter (>5) [1,2,3,4,5,6,7,8]
    Output: [6,7,8]

    ```

* Lambdas (Anonymous function)
  * `(\a -> a + 1) 4` answer is 5
* `fold`
  * fold (+) [1,2,3,4,5] --> 15
  * Wiki haskell org [source code](https://wiki.haskell.org/Anonymous_function)

    ```Haskell
    -- if the list is empty, the result is the initial value z; else
    -- apply f to the first element and the result of folding the rest
    foldr f z []     = z
    foldr f z (x:xs) = f x (foldr f z xs)

    -- if the list is empty, the result is the initial value; else
    -- we recurse immediately, making the new initial value the result
    -- of combining the old initial value with the first element.
    foldl f z []     = z
    foldl f z (x:xs) = foldl f (f z x) xs
    ```

* `$` function (function application)

  ```Haskell
  ($) :: (a -> b) -> a -> b
  f $ x = f x
  ```

  * `$` 的優先順序則最低
  * `$` 則是右結合的 (用空格的函數呼叫符是左結合的，如 `f a b c` 與 `((f a) b) c` 等價)
  * 減少我們程式碼中括號的數目

* `.` Function composistion

  ``` Haskell
  (.) :: (b -> c) -> (a -> b) -> a -> c
  f . g = \x -> f (g x)
  ```

  * 函數組合的用處之一就是生成新函數，並傳遞給其它函數。

### Chapter 8. Create own type and typeclass

* Algebraic data types intro

  ```Haskell
  data Shape = Circle Float Float Float | Rectangle Float Float Float Float
  ```

  ```Haskell
  ghci> :t Circle
  Circle :: Float -> Float -> Float -> Shape
  ```

  * Circle is a constructor with three fields
  * value constructor are functions

  ```Haskell
  surface :: Shape -> Float
  surface (Circle _ _ r) = pi * r ^ 2
  surface (Rectangle x1 y1 x2 y2) = (abs $ x2 - x1) * (abs $ y2 - y1)
  ```

  ```Haskell
  ghci> surface $ Circle 10 20 10
  314.15927
  ghci> surface $ Rectangle 0 0 100 100
  10000.0
  ```

* Record syntax

  ```Haskell
  data Person = Person { firstName :: String
                       , lastName :: String
                       , age :: Int
                       , height :: Float
                       , phoneNumber :: String
                       , flavor :: String
                       } deriving (Show, Eq, Read)
  ```

  Auto generated constructor

  ```Haskell
  ghci> :t firstName
  firstName :: Person -> String
  ```

* Type synonym
  * `[Char]` 和 `String` 等價

  ```Haskell
  type PhoneBook = [(String, String)]

  -- Equal to
  -- 目的: 更加易讀
  type PhoneNumber = String
  type Name = String
  type PhoneBook = [(Name,PhoneNumber)]
  ```

  * 型別別名也是可以有參數的，如果你想搞個型別來表示關聯 List

  ```Haskell
  type AssocList k v = [(k,v)]
  ```

* Recursive data structures (遞迴地定義資料結構)

  ```Haskell
  data Tree a = EmptyTree | Node a (Tree a) (Tree a) deriving (Show, Read, Eq)
  ```

* Typeclasses 102
* A yes-no typeclass
* The Functor typeclass
* Kinds and some type-foo
