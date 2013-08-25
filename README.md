このスタイルガイドは、[Bozhidar Batsov](https://twitter.com/bbatsov)氏による[bbatsov/clojure-style-guide](https://github.com/bbatsov/clojure-style-guide)の日本語訳フォークです。

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)本翻訳版のライセンスは[クリエイティブ・コモンズ 表示 3.0 非移植 ライセンス](http://creativecommons.org/licenses/by/3.0/deed.ja_JP)とします。原著作者はBozhidar Batsov氏です。原文のライセンスについてもご注意ください。

意訳していて、原文の意味を損なわない程度に言葉を加えたり省略している部分があります。また、訳が間違っている可能性があります。逐次修正は行いますが、原文を優先するようにしてください。用語の翻訳には、[プログラミングClojure第2版](http://ssl.ohmsha.co.jp/cgi-bin/menu.cgi?ISBN=978-4-274-06913-0)を参考にしています。翻訳に対する意見や修正は、IssueやPull Requestを作成していただけると助かります。なお、規約自体に対する意見は原文リポジトリのほうにお願いします。

----

# Clojureスタイルガイド

> 良い手本は大切だ。<br/>
> -- Officer Alex J. Murphy / RoboCop

このClojureスタイルガイドは、実際のClojureプログラマーが他のClojureプログラマーに保守してもらえるコードを書くための、ベスト・プラクティスを勧めるものだ。
実際の使われ方を反映したスタイルガイドは利用されるが、理想的であっても人々に受け入れられないような規約を持つスタイルガイドは全く利用されない危険がある&mdash;&mdash;たとえそれがどれほど良いものであっても。

ガイドは関連する規約ごとにいくつかのセクションに分かれて構成されている。規約には理論的根拠を付けるようにした（ただし、根拠が明らかな場合は省略している）。

これらの規約は、出し抜けに考えだされたものではない。これらの多くは、私のプロソフトウェアエンジニアとしての幅広い仕事や、Clojureコミュニティメンバーからのフィードバックと意見、そして、["Clojure Programming"](http://www.clojurebook.com/)や["The Joy of Clojure"](http://joyofclojure.com/)のような高い評価を受けている様々なリソースに基づいている。

このスタイルガイドはまだ作成途中だ。そのため、いくつかのセクションが欠けていたり、不完全であったり、いくつかの規約には例がなかったり、それが明快でなかったりする。これらの問題が解決されるまで、それを念頭に置いてほしい。

[Transmuter](https://github.com/TechnoGate/transmuter)を利用することで、このスタイルガイドのPDF版やHTML版を生成することができる。

## 目次

* [ソースコードのレイアウトと構造](#ソースコードのレイアウトと構造)
* [構文](#構文)
* [命名規約](#命名規約)
* [コレクション](#コレクション)
* [状態](#状態)
* [文字列](#文字列)
* [例外](#例外)
* [マクロ](#マクロ)
* [コメント](#コメント)
    * [コメントアノテーション](#コメントアノテーション)
* [実際のコードでは](#実際のコードでは)
* [ツール](#ツール)

## ソースコードのレイアウトと構造

> ほとんど全ての人は、自分のスタイル以外のあらゆるスタイルは汚くて読みにくい、と思っている。「自分のスタイル以外の」を削れば、それはおそらく正しい…<br/>
> -- Jerry Coffin (インデントについて)

* 各インデントには2つの **スペース** を使う。タブは使わない。

    ```Clojure
    ;; good
    (when something
      (something-else))

    ;; bad - four spaces
    (when something
        (something-else))
    ```

* 関数の引数は左揃えにする。

    ```Clojure
    ;; good
    (filter even?
            (range 1 10))

    ;; bad
    (filter even?
      (range 1 10))
    ```

* `let`の束縛とマップのキーワードを揃える。

    ```Clojure
    ;; good
    (let [thing1 "some stuff"
          thing2 "other stuff"]
      {:thing1 thing1
       :thing2 thing2})

    ;; bad
    (let [thing1 "some stuff"
      thing2 "other stuff"]
      {:thing1 thing1
      :thing2 thing2})
    ```

* `defn`において、ドキュメント文字列を持たない場合は、関数名と引数ベクタの間の改行を省略しても良い。

    ```Clojure
    ;; good
    (defn foo
      [x]
      (bar x))

    ;; good
    (defn foo [x]
      (bar x))

    ;; bad
    (defn foo
      [x] (bar x))
    ```

* 関数本体が短い場合、引数ベクタと関数本体の間の改行は省略しても良い。

    ```Clojure
    ;; good
    (defn foo [x]
      (bar x))

    ;; good for a small function body
    (defn foo [x] (bar x))

    ;; good for multi-arity functions
    (defn foo
      ([x] (bar x))
      ([x y]
        (if (predicate? x)
          (bar x)
          (baz x))))

    ;; bad
    (defn foo
      [x] (if (predicate? x)
            (bar x)
            (baz x)))
    ```

* 複数行に渡るドキュメント文字列はインデントする。

    ```Clojure
    ;; good
    (defn foo
      "Hello there. This is
      a multi-line docstring."
      []
      (bar))

    ;; bad
    (defn foo
      "Hello there. This is
    a multi-line docstring."
      []
      (bar))
    ```

* Unixスタイルの行エンコーディングを使用する。（*BSD/Solaris/Linux/OSXユーザはデフォルトで問題ないが、Windowsユーザは特に注意すること。）
    * Gitを使っているなら、次の設定を追加して、Windowsの行末コードを防ぐのもいい。

        ```bash
        $ git config --global core.autocrlf true
        ```

* 開き括弧（`(`, `{`, `[`）の前の文字と、閉じ括弧（`)`, `}`, `]`）の後の文字は、括弧との間にスペースを設ける。
逆に、開き括弧とそれに続く文字、閉じ括弧と直前の文字の間にはスペースを入れない。

    ```Clojure
    ;; good
    (foo (bar baz) quux)

    ;; bad
    (foo(bar baz)quux)
    (foo ( bar baz ) quux)
    ```

* シーケンシャルコレクションのリテラルの要素の間にコンマを使わない。

    ```Clojure
    ;; good
    [1 2 3]
    (1 2 3)

    ;; bad
    [1, 2, 3]
    (1, 2, 3)
    ```

* コンマや改行を使い、マップリテラルの可読性を向上させることを検討する。

    ```Clojure
    ;; good
    {:name "Bruce Wayne" :alter-ego "Batman"}

    ;; good and arguably a bit more readable
    {:name "Bruce Wayne"
     :alter-ego "Batman"}

    ;; good and arguably more compact
    {:name "Bruce Wayne", :alter-ego "Batman"}
    ```

* 後ろ側に連続する括弧は同じ行に含める。

    ```Clojure
    ;; good
    (when something
      (something-else))

    ;; bad
    (when something
      (something-else)
    )
    ```

* トップレベルのフォームの間には空白行を挟む。

    ```Clojure
    ;; good
    (def x ...)

    (defn foo ...)

    ;; bad
    (def x ...)
    (defn foo ...)
    ```

    例外として、関連する`def`はまとめてしまっても良い。

    ```Clojure
    ;; good
    (def min-rows 10)
    (def max-rows 20)
    (def min-cols 15)
    (def max-cols 30)
    ```

* 関数やマクロ定義の中には空白行を入れない。ただし、`let`や`cond`などの中においてペアをグループ分けするために入れるのは良い。

* 1行が80文字を超えないようにする。
* 行末の空白を避ける。
* 名前空間ごとにファイルを分ける。
* 全ての名前空間は、複数の`import`, `require`, `refer`, `use`からなる`ns`フォームで始める。

    ```Clojure
    (ns examples.ns
      (:refer-clojure :exclude [next replace remove])
      (:require [clojure.string :as s :refer [blank?]]
                [clojure.set :as set]
                [clojure.java.shell :as sh])
      (:use [clojure xml zip])
      (:import java.util.Date
               java.text.SimpleDateFormat
               [java.util.concurrent Executors
                                     LinkedBlockingQueue]))
    ```

* nsマクロでは、`:use`よりも`:require :refer :all`を用いるほうが良い。

    ```Clojure
    ;; good
    (ns examlpes.ns
      (:require [clojure.zip :refer :all]))

    ;; bad
    (ns examlpes.ns
      (:use [clojure.zip]))
    ```

* 単一セグメントの名前空間を使わない。

    ```Clojure
    ;; good
    (ns example.ns)

    ;; bad
    (ns example)
    ```

* 無駄に長い名前空間を使わない（例えば、5セグメントを超えるような）。

* 関数は10行を超えないようにする。理想的には、ほとんどの関数は5行より短くしたほうが良い。

* 3つか4つを超えるパラメータを持つパラメータリストの使用を避ける。

## 構文

* `require`や`refer`のような名前空間を扱う関数の使用を避ける。これらはREPL環境以外では必要ないものだ。
* 前方参照を可能にするには`declare`を使う。
* `loop/recur`よりも`map`のように、より高階な関数のほうが好ましい。

* 関数本体内では、コンディションマップによる入力値、出力値のチェックがより良い。

    ```Clojure
    ;; good
    (defn foo [x]
      {:pre [(pos? x)]}
      (bar x))

    ;; bad
    (defn foo [x]
      (if (pos? x)
        (bar x)
        (throw (IllegalArgumentException "x must be a positive number!")))
    ```

* 関数内でvarを定義しない。

    ```Clojure
    ;; very bad
    (defn foo []
      (def x 5)
      ...)
    ```

* ローカル束縛によって`clojure.core`の名前を隠さない。

    ```Clojure
    ;; bad - you're forced to used clojure.core/map fully qualified inside
    (defn foo [map]
      ...)
    ```

* シーケンスが空かどうかをチェックするには`seq`を使う（このテクニックはしばしば *nil punning* と呼ばれる）。

    ```Clojure
    ;; good
    (defn print-seq [s]
      (when (seq s)
        (prn (first s))
        (recur (rest s))))

    ;; bad
    (defn print-seq [s]
      (when-not (empty? s)
        (prn (first s))
        (recur (rest s))))
    ```

* `(if ... (do ...)`の代わりに`when`を使う。

    ```Clojure
    ;; good
    (when pred
      (foo)
      (bar))

    ;; bad
    (if pred
      (do
        (foo)
        (bar)))
    ```
* `let` + `if`の代わりに`if-let`を使う。

    ```Clojure
    ;; good
    (if-let [result (foo x)]
      (something-with result)
      (something-else))

    ;; bad
    (let [result (foo x)]
      (if result
        (something-with result)
        (something-else)))
    ```

* `let` + `when`の代わりに`when-let`を使う。

    ```Clojure
    ;; good
    (when-let [result (foo x)]
      (do-something-with result)
      (do-something-more-with result))

    ;; bad
    (let [result (foo x)]
      (when result
        (do-something-with result)
        (do-something-more-with result)))
    ```

* `(if (not ...) ...)`の代わりに`if-not`を使う。

    ```Clojure
    ;; good
    (if-not (pred)
      (foo))

    ;; bad
    (if (not pred)
      (foo))
    ```

* `(when (not ...) ...)`の代わりに`when-not`を使う。

    ```Clojure
    ;; good
    (when-not pred
      (foo)
      (bar))

    ;; bad
    (when (not pred)
      (foo)
      (bar))
    ```

* `(if-not ... (do ...)`の代わりに`when-not`を使う。

    ```Clojure
    ;; good
    (when-not pred
      (foo)
      (bar))

    ;; bad
    (if-not pred
      (do
        (foo)
        (bar)))
    ```

* `(not (= ...))`の代わりに`not=`を使う。

    ```Clojure
    ;; good
    (not= foo bar)

    ;; bad
    (not (= foo bar))
    ```

* 比較を行うときは、Clojure関数の`<`や`>`などは可変長引数を許していることを覚えておこう。

    ```Clojure
    ;; good
    (< 5 x 10)

    ;; bad
    (and (> x 5) (< x 10))
    ```

* ただ1つのパラメータを持つ関数リテラルでは、`%1`よりも`%`のほうが好ましい。

    ```Clojure
    ;; good
    #(Math/round %)

    ;; bad
    #(Math/round %1)
    ```

* 複数のパラメータを持つ関数リテラルでは、`%`よりも`%1`のほうが好ましい。

    ```Clojure
    ;; good
    #(Math/pow %1 %2)

    ;; bad
    #(Math/pow % %2)
    ```

* 必要ないなら無名関数でラップしない。

    ```Clojure
    ;; good
    (filter even? (range 1 10))

    ;; bad
    (filter #(even? %) (range 1 10))
    ```

* 関数本体が2つ以上のフォームを含む場合は、関数リテラルを使用しない。

    ```Clojure
    ;; good
    (fn [x]
      (println x)
      (* x 2))

    ;; bad (you need an explicit do form)
    #(do (println %)
         (* % 2))
    ```

* 無名関数よりも`complement`を用いたほうが良い。

    ```Clojure
    ;; good
    (filter (complement some-pred?) coll)

    ;; bad
    (filter #(not (some-pred? %)) coll)
    ```

    この規約は、反対の述語が別の関数としてある場合は無視するべきだ。（例：`even?`と`odd?`）

* コードをシンプルにするために`comp`の使用を考える。

    ```Clojure
    ;; Assuming `(:require [clojure.string :as str])`...

    ;; good
    (map #(str/capitalize (str/trim %)) ["top " " test "])

    ;; better
    (map (comp str/capitalize str/trim) ["top " " test "])
    ```

* コードをシンプルにするために`partial`の使用を考える。

    ```Clojure
    ;; good
    (map #(+ 5 %) (range 1 10))

    ;; (arguably) better
    (map (partial + 5) (range 1 10))
    ```

* 深いネストよりもスレッディングマクロ`->` (thread-first)と`->>` (thread-last)の使用が好ましい。

    ```Clojure
    ;; good
    (-> [1 2 3]
        reverse
        (conj 4)
        prn)

    ;; not as good
    (prn (conj (reverse [1 2 3])
               4))

    ;; good
    (->> (range 1 10)
         (filter even?)
         (map (partial * 2)))

    ;; not as good
    (map (partial * 2)
         (filter even? (range 1 10)))
    ```

* Java呼び出しの際のチェーンメソッドコールには、`->`よりも`..`が好ましい。

    ```Clojure
    ;; good
    (-> (System/getProperties) (.get "os.name"))

    ;; better
    (.. System getProperties (get "os.name"))
    ```

* `cond`や`condp`で残り全ての条件をキャッチするときは`:else`を使う。

    ```Clojure
    ;; good
    (cond
      (< n 0) "negative"
      (> n 0) "positive"
      :else "zero"))

    ;; bad
    (cond
      (< n 0) "negative"
      (> n 0) "positive"
      true "zero"))
    ```

* 述語と式が変わらない場合、`cond`よりも`condp`のほうが良い。

    ```Clojure
    ;; good
    (cond
      (= x 10) :ten
      (= x 20) :twenty
      (= x 30) :forty
      :else :dunno)

    ;; much better
    (condp = x
      10 :ten
      20 :twenty
      30 :forty
      :dunno)
    ```

* テスト式がコンパイル時に固定の場合、`cond`や`condp`の代わりに`case`を使うのが良い。

    ```Clojure
    ;; good
    (cond
      (= x 10) :ten
      (= x 20) :twenty
      (= x 30) :forty
      :else :dunno)

    ;; better
    (condp = x
      10 :ten
      20 :twenty
      30 :forty
      :dunno)

    ;; best
    (case x
      10 :ten
      20 :twenty
      30 :forty
      :dunno)
    ```

* `cond`などの中では短いフォームを用いる。それが無理なら、コメントや空白行を使用して、ペアグループを見えやすくする。

    ```Clojure
    ;; good
	(cond
	  (test1) (action1)
	  (test2) (action2)
	  :else   (default-action))

	;; ok-ish
	(cond
	;; test case 1
	(test1)
	(long-function-name-which-requires-a-new-line
       (complicated-sub-form
          (-> 'which-spans
              multiple-lines)))

	(test2)
	(another-very-long-function-name
       (yet-another-sub-form
          (-> 'which-spans
              multiple-lines)))

    :else
    (the-fall-through-default-case
      (which-also-spans 'multiple
                        'lines)))
    ```

* `set`を述語として使うことができる。

    ```Clojure
    ;; good
    (remove #{0} [0 1 2 3 4 5])

    ;; bad
    (remove #(= % 0) [0 1 2 3 4 5])

    ;; good
    (count (filter #{\a \e \i \o \u} "mary had a little lamb"))

    ;; bad
    (count (filter #(or (= % \a)
                        (= % \e)
                        (= % \i)
                        (= % \o)
                        (= % \u))
                   "mary had a little lamb"))
    ```

* `(+ x 1)`や`(- x 1)`の代わりに`(inc x)`や`(dec x)`を使う。

* `(> x 0)`, `(< x 0)`, `(= x 0)`の代わりに`(pos? x)`, `(neg? x)`, `(zero? x)`を使う。

* 糖衣されたJava呼び出しフォームを用いる。

    ```Clojure
    ;;; object creation
    ;; good
    (java.util.ArrayList. 100)

    ;; bad
    (new java.util.ArrayList 100)

    ;;; static method invocation
    ;; good
    (Math/pow 2 10)

    ;; bad
    (. Math pow 2 10)

    ;;; instance method invocation
    ;; good
    (.substring "hello" 1 3)

    ;; bad
    (. "hello" substring 1 3)

    ;;; static field access
    ;; good
    Integer/MAX_VALUE

    ;; bad
    (. Integer MAX_VALUE)

    ;;; instance field access
    ;; good
    (.someField some-object)

    ;; bad
    (. some-object someField)
    ```

* キーがキーワード、値がブール値`true`のスロットしか持たないメタデータには、簡易メタデータ表記を使う。

    ```Clojure
    ;; good
    (def ^:private a 5)

    ;; bad
    (def ^{:private true} a 5)
    ```

* コード中のプライベート部分には印を付ける。

    ```Clojure
    ;; good
    (defn- private-fun [] ...)

    (def ^:private private-var ...)

    ;; bad
    (defn private-fun [] ...) ; not private at all

    (defn ^:private private-fun [] ...) ; overly verbose

    (def private-var ...) ; not private at all
    ```

* メタデータを何に付加するかについては、よく注意したほうが良い。

    ```Clojure
    ;; we attach the metadata to the var referenced by `a`
    (def ^:private a {})
    (meta a) ;=> nil
    (meta #'a) ;=> {:private true}

    ;; we attach the metadata to the empty hash-map value
    (def a ^:private {})
    (meta a) ;=> {:private true}
    (meta #'a) ;=> nil
    ```

## 命名規約

> プログラミングで本当に難しいのは、キャッシュの無効化と命名の仕方だけだ。<br/>
> -- Phil Karlton

* 名前空間は次の2つの名づけ方が好ましい。
    * `project.module`
    * `organization.project.module`
* 複数単語からなる名前空間セグメントには`lisp-case`を使う（例：`bruce.project-euler`）
* 関数名や変数名には`lisp-case`を使う。

    ```Clojure
    ;; good
    (def some-var ...)
    (defn some-fun ...)

    ;; bad
    (def someVar ...)
    (defn somefun ...)
    (def some_fun ...)
    ```

* プロトコル、レコード、構造体、型には`CamelCase`を用いる。（HTTP, RFC, XMLのような頭字語は大文字を保持する。）
* 述語（ブール値を返す関数）の名前はクエスチョンマーク（?）で終わるべきだ。（例：`even?`）

    ```Clojure
    ;; good
    (defn palindrome? ...)

    ;; bad
    (defn palindrome-p ...) ; Common Lisp style
    (defn is-palindrome ...) ; Java style
    ```

* STMトランザクションの中で安全でない関数・マクロの名前はエクスクラメーションマーク（!）で終わるべきだ。（例：`reset!`）
* 変換のための関数名には`to`ではなく`->`を用いる。

    ```Clojure
    ;; good
    (defn f->c ...)

    ;; not so good
    (defn f-to-c ...)
    ```

* 再束縛を想定しているものには`*earmuffs*`を使う（つまりdynamicなものだ）。

    ```Clojure
    ;; good
    (def ^:dynamic *a* 10)

    ;; bad
    (def ^:dynamic a 10)
    ```

* 定数のために特別な表記をしない。特定のものを除いて、全ては定数である。
* 分配束縛しても直後のコードで使われない変数名には`_`を使う。

    ```Clojure
    ;; good
    (let [[a b _ c] [1 2 3 4]]
      (println a b c))

    (dotimes [_ 3]
      (println "Hello!"))

    ;; bad
    (let [[a b c d] [1 2 3 4]]
      (println a b d))

    (dotimes [i 3]
      (println "Hello!"))
    ```

* `pred`や`coll`のような慣用名には`clojure.core`の例が参考になる。
    * 関数内では、
        * `f`, `g`, `h` - 関数入力
        * `n` - サイズを示す整数値
        * `index` - 整数のインデックス
        * `x`, `y` - 数値
        * `s` - 文字列入力
        * `coll` - コレクション
        * `pred` - 述語クロージャ
        * `& more` - 可変長引数
    * マクロ内では、
        * `expr` - 式
        * `body` - マクロ本体
        * `binding` - マクロの束縛ベクタ

## コレクション

> 10種のデータ構造を処理できる機能を10個用意するより、1種のデータ構造を処理できる機能を100個用意した方が良い。<br/>
> -- Alan J. Perlis

* 汎用的なデータ置き場としてリストを使うことを避ける（リストが本当に必要な場合を除く）。
* マップのキーにはキーワードを用いたほうが良い。

    ```Clojure
    ;; good
    {:name "Bruce" :age 30}

    ;; bad
    {"name" "Bruce" "age" 30}
    ```

* 可能なら、コレクションのリテラル構文を用いたほうが良い。ただし、セットを定義するときは、コンパイル時に定数である値についてのみリテラル構文を使用する。

    ```Clojure
    ;; good
    [1 2 3]
    #{1 2 3}
    (hash-set (func1) (func2)) ; values determined at runtime

    ;; bad
    (vector 1 2 3)
    (hash-set 1 2 3)
    #{(func1) (func2)} ; will throw runtime exception if (func1) = (func2)
    ```

* 可能なら、コレクションの要素にインデックスでアクセスすることを避ける。

* 可能なら、マップから値を取得する関数としてキーワードを用いるのが良い。

    ```Clojure
    (def m {:name "Bruce" :age 30})

    ;; good
    (:name m)

    ;; more verbose than necessary
    (get m :name)

    ;; bad - susceptible to NullPointerException
    (m :name)
    ```

* ほとんどのコレクションはその要素の関数であることを活用する。

    ```Clojure
    ;; good
    (filter #{\a \e \o \i \u} "this is a test")

    ;; bad - too ugly to share
    ```

* キーワードはコレクションの関数として使えることを活用する。

    ```Clojure
    ((juxt :a :b) {:a "ala" :b "bala"})
    ```

* パフォーマンス問題がクリティカルとなる部分を除いて、一時的コレクションの使用を避ける。

* Javaのコレクションの使用を避ける。

* Java呼び出しや、プリミティブ型を多用するパフォーマンスクリティカルなコードを除いて、Javaの配列の使用を避ける。

## 状態

### ref

* トランザクションの中で思いがけずI/Oコールを呼んでしまったときの問題を回避するため、全てのI/Oコールを`io!`マクロでラップすることを考える。
* 出来る限り`ref-set`は使用しない。

    ```Clojure
    (def r (ref 0))

    ;; good
    (dosync (alter r + 5))

    ;; bad
    (dosync (ref-set r 5))
    ```

* トランザクションのサイズ（包んでいる処理の量）を出来る限り小さく保つようにする。
* 同一のrefとやり取りを行う、短期のトランザクションと長期のトランザクションを両方持つことを避ける。

### エージェント

* それがCPUバウンドで、かつI/Oや他スレッドをブロックしない処理のときだけ`send`を用いる。
* それがスレッドをブロック、スリープさせたり、そうでなくても停滞させるかもしれない処理には`send-off`を用いる。

### アトム

* STMトランザクションの中でアトムを更新することを避ける。
* 可能なら、`reset!`よりも`swap!`を使うようにする。

    ```Clojure
    (def a (atom 0))

    ;; good
    (swap! a + 5)

    ;; not as good
    (reset! a 5)
    ```

## 文字列

* 文字列処理は、Java呼び出しや独自実装よりも、`clojure.string`の関数を使うほうが好ましい。

    ```Clojure
    ;; good
    (clojure.string/upper-case "bruce")

    ;; bad
    (.toUpperCase "bruce")
    ```

## 例外

* 既存の例外型を再利用する。慣用的なClojureコードでは、例外を投げるとき、基本的な例外型を用いている。
  （例： `java.lang.IllegalArgumentException`,
  `java.lang.UnsupportedOperationException`,
  `java.lang.IllegalStateException`, `java.io.IOException`）。
* `finally`よりも`with-open`のほうが好ましい。

## マクロ

* その処理が関数でできるならマクロを書かない。
* まずマクロの使用例を作成し、その後でマクロを作る。
* 可能なら、複雑なマクロはより小さい機能に分割する。
* マクロは通常、構文糖衣を提供するものであるべきで、そのコアは単純な機能であるべきだ。そうすることでより構造化されるだろう。
* 自分でリストを組み立てるよりも、構文クオートを使用するほうが好ましい。

## コメント

> 良いコードとは、それ自体が最良のドキュメントになっているものだ。コメントを付けようとしたとき、自分の胸に聞いてみるといい、「どうやってコードを改良して、このコメントを不要にできるだろうか？」ってね。より美しくするために、コードを改良してからドキュメント化するんだ。<br/>
> -- Steve McConnell

* 出来る限り、コードを見れば何をしているのか分かるように努める。

* ヘッダーコメントには最低4つのセミコロンを用いる。

* トップレベルのコメントには3つのセミコロンを用いる。

* 特定のコード部分の直前にコメントを書くときは、コード部分とインデントを揃え、2つのセミコロンを用いる。

* 行末コメントには1つのセミコロンを用いる。

* セミコロンとテキストの間には最低1つのスペースを入れる。

    ```Clojure
    ;;;; Frob Grovel

    ;;; This section of code has some important implications:
    ;;;   1. Foo.
    ;;;   2. Bar.
    ;;;   3. Baz.

    (defn fnord [zarquon]
      ;; If zob, then veeblefitz.
      (quux zot
            mumble             ; Zibblefrotz.
            frotz))
    ```

* 2単語以上のコメントは大文字で始め、句読点を用いる。各文は[1つのスペース](http://en.wikipedia.org/wiki/Sentence_spacing)で分ける。
* 無意味なコメントを避ける。

    ```Clojure
    ;; bad
    (inc counter) ; increments counter by one
    ```

* コメントは常に更新していなければならない。古いコメントは、コメントがないことよりも害悪だ。
* 特定のフォームをコメントアウトする必要があるときは、通常のコメントではなく`#_`リーダマクロを用いたほうが良い。

    ```Clojure
    ;; good
    (+ foo #_(bar x) delta)

    ;; bad
    (+ foo
       ;; (bar x)
       delta)
    ```

> 良いコードというのは面白いジョークのようなものだ。説明する必要がない。<br/>
> -- Russ Olsen

* 悪いコードを説明するためにコメントを書くことを避ける。コードをリファクタリングして、コメントが不要なようにするべきだ。（「やるか、やらないだ。やってみるではない」--Yoda）

### コメントアノテーション

* アノテーションは通常、当該コードの直前に書かれるべきだ。
* アノテーションキーワードの後にはコロンとスペースを入れ、その後で詳細を書く。
* 詳細が複数行に渡る場合、2行目以降は1行目に合わせてインデントするべきだ。
* アノテーションには記述者のイニシャルと日付を入れる。そうすればその妥当性を容易に示せる。

    ```Clojure
    (defn some-fun
      []
      ;; FIXME: This has crashed occasionally since v1.2.3. It may
      ;;        be related to the BarBazUtil upgrade. (xz 13-1-31)
      (baz))
    ```

* ドキュメント化が不必要なほどに問題が明らかな箇所では、当該行の末尾に説明なしでアノテーションを付けても良い。この使用法は例外的であるべきで、規約ではない。

    ```Clojure
    (defn bar
      []
      (sleep 100)) ; OPTIMIZE
    ```

* 後日追加されるべき機能には`TODO`を使う。
* コードが壊れていて、修正の必要がある箇所には`FIXME`を使う。
* パフォーマンス問題の原因となりうる、遅かったり非効率なコードには`OPTIMIZE`を使う。
* 疑わしいコーディングの仕方がされており、リファクタリングすべき「コード・スメル」には`HACK`を用いる。
* 意図するように動くかどうか確認すべき箇所には`REVIEW`を使う。例：`REVIEW: Are we sure this is how the client does X currently?`
* そのほうが適切だと思えば、その他独自のアノテーションキーワードを用いる。ただし、プロジェクトの`README`などに忘れずにドキュメント化しておく。

## 実際のコードでは

* 関数型的にコードを書き、状態を持つことを避けるのが理にかなっている。
* 一貫させる。理想的には、このガイドの通りにする。
* 常識的に考える。

## ツール

慣用的なClojureコードを書くのを助けてくれるツールがClojureコミュニティによって作られている。

* [Slamhound](https://github.com/technomancy/slamhound)は既存のコードから適切な`ns`定義を自動的に生成してくれる。
* [kibit](https://github.com/jonase/kibit)はClojure向けの静的コード解析ツールだ。より慣用的な関数やマクロの探索には[core.logic](https://github.com/clojure/core.logic)を用いている。

# 広めてください

コミュニティドリブンのスタイルガイドは、その存在を知らないコミュニティではあまり役に立ちません。どうか、このガイドについてツイートをして、あなたの友達や同僚と共有してください。頂いたあらゆるコメントや提案、意見がほんの少しずつ、このガイドを形作っていくのです。みんなで最高のスタイルガイドを作りましょう。

Cheers,<br/>
[Bozhidar](https://twitter.com/bbatsov)
