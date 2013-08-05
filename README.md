このスタイルガイドは、[Bozhidar Batsov](https://twitter.com/bbatsov)氏による[bbatsov/clojure-style-guide](https://github.com/bbatsov/clojure-style-guide)の日本語訳フォークです。

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)本翻訳版のライセンスは[クリエイティブ・コモンズ 表示 3.0 非移植 ライセンス](http://creativecommons.org/licenses/by/3.0/deed.ja_JP)とします。原著作者はBozhidar Batsov氏です。原文のライセンスについてもご注意ください。

意訳していて、原文の意味を損なわない程度に言葉を加えたり省略している部分があります。また、訳が間違っている可能性があります。暫時修正は行いますが、原文を優先するようにしてください。用語の翻訳には、[プログラミングClojure第2版](http://ssl.ohmsha.co.jp/cgi-bin/menu.cgi?ISBN=978-4-274-06913-0)を参考にしています。翻訳に対する意見や修正は、IssueやPull Requestを作成していただけると助かります。なお、規約自体に対する意見は原文リポジトリのほうにお願いします。

----

# Clojureスタイルガイド

<!--
# The Clojure Style Guide
-->

> 良い手本は大切だ。<br/>
> -- Officer Alex J. Murphy / RoboCop

<!--
> Role models are important. <br/>
> -- Officer Alex J. Murphy / RoboCop
-->

このClojureスタイルガイドは、実際のClojureプログラマーが他のClojureプログラマーに保守してもらえるコードを書くための、ベスト・プラクティスを勧めるものだ。
実際の使われ方を反映したスタイルガイドは利用されるが、理想的であっても人々に受け入れられないような規約を持つスタイルガイドは全く利用されない危険がある&mdash;&mdash;たとえそれがどれほど良いものであっても。

ガイドは関連する規約ごとにいくつかのセクションに分かれて構成されている。規約には理論的根拠を付けるようにした（ただし、根拠が明らかな場合は省略している）。

これらの規約は、出し抜けに考えだされたものではない。これらの多くは、私のプロソフトウェアエンジニアとしての幅広い仕事や、Clojureコミュニティメンバーからのフィードバックと意見、そして、["Clojure Programming"](http://www.clojurebook.com/)や["The Joy of Clojure"](http://joyofclojure.com/)のような高い評価を受けている様々なリソースに基づいている。

このスタイルガイドはまだ作成途中だ。そのため、いくつかのセクションが欠けていたり、不完全であったり、いくつかの規約には例がなかったり、それが明快でなかったりする。これらの問題が解決されるまで、それを念頭に置いてほしい。

[Transmuter](https://github.com/TechnoGate/transmuter)を利用することで、このスタイルガイドのPDF版やHTML版を生成することができる。

<!--
This Clojure style guide recommends best practices so that real-world Clojure
programmers can write code that can be maintained by other real-world Clojure
programmers. A style guide that reflects real-world usage gets used, and a
style guide that holds to an ideal that has been rejected by the people it is
supposed to help risks not getting used at all &ndash; no matter how good it is.

The guide is separated into several sections of related rules. I've
tried to add the rationale behind the rules (if it's omitted I've
assumed that is pretty obvious).

I didn't come up with all the rules out of nowhere - they are mostly
based on my extensive career as a professional software engineer,
feedback and suggestions from members of the Clojure community, and
various highly regarded Clojure programming resources, such as
["Clojure Programming"](http://www.clojurebook.com/)
and ["The Joy of Clojure"](http://joyofclojure.com/).

The guide is still a work in progress - some sections are missing,
others are incomplete, some rules are lacking examples, some rules
don't have examples that illustrate them clearly enough. In due time
these issues will be addressed - just keep them in mind for now.

You can generate a PDF or an HTML copy of this guide using
[Transmuter](https://github.com/TechnoGate/transmuter).
-->

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

<!--
## Table of Contents

* [Source Code Layout & Organization](#source-code-layout--organization)
* [Syntax](#syntax)
* [Naming](#naming)
* [Collections](#collections)
* [Mutation](#mutation)
* [Strings](#strings)
* [Exceptions](#exceptions)
* [Macros](#macros)
* [Comments](#comments)
    * [Comment Annotations](#comment-annotations)
* [Existential](#existential)
* [Tooling](#tooling)
-->

## ソースコードのレイアウトと構造

<!--
## Source Code Layout & Organization
-->

> ほとんど全ての人は、自分のスタイル以外のあらゆるスタイルは汚くて読みにくい、と思っている。「自分のスタイル以外の」を削れば、それはおそらく正しい…<br/>
> -- Jerry Coffin (インデントについて)

<!--
> Nearly everybody is convinced that every style but their own is
> ugly and unreadable. Leave out the "but their own" and they're
> probably right... <br/>
> -- Jerry Coffin (on indentation)
-->

* 各インデントには2つの **スペース** を使う。タブは使わない。
<!-- * Use two **spaces** per indentation level. No hard tabs. -->

    ```Clojure
    ;; good
    (when something
      (something-else))

    ;; bad - four spaces
    (when something
        (something-else))
    ```

* 関数の引数は左揃えにする。
<!-- * Align vertically function arguments. -->

    ```Clojure
    ;; good
    (filter even?
            (range 1 10))

    ;; bad
    (filter even?
      (range 1 10))
    ```

* `let`の束縛とマップのキーワードを揃える。
<!-- * Align `let` bindings and map keywords. -->

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
<!-- * Optionally omit the new line between the function name and argument -->
<!--   vector for `defn` when there is no docstring. -->

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
<!-- * Optionally omit the new line between the argument vector and a short -->
<!--   function body. -->

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
<!-- * Indent each line of multi-line docstrings. -->

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
<!-- * Use Unix-style line endings. (*BSD/Solaris/Linux/OSX users are -->
<!--   covered by default, Windows users have to be extra careful.) -->
<!--     * If you're using Git you might want to add the following -->
<!--     configuration setting to protect your project from Windows line -->
<!--     endings creeping in: -->

        ```bash
        $ git config --global core.autocrlf true
        ```

* 開き括弧（`(`, `{`, `[`）の前の文字と、閉じ括弧（`)`, `}`, `]`）の後の文字は、括弧との間にスペースを設ける。
逆に、開き括弧とそれに続く文字、閉じ括弧と直前の文字の間にはスペースを入れない。
<!-- * If any text precedes an opening bracket(`(`, `{` and -->
<!-- `[`) or follows a closing bracket(`)`, `}` and `]`), separate that -->
<!-- text from that bracket with a space. Conversely, leave no space after -->
<!-- an opening bracket and before following text, or after preceding text -->
<!-- and before a closing bracket. -->

    ```Clojure
    ;; good
    (foo (bar baz) quux)

    ;; bad
    (foo(bar baz)quux)
    (foo ( bar baz ) quux)
    ```

* シーケンシャルコレクションのリテラルの要素の間にコンマを使わない。
<!-- * Don't use commas between the elements of sequential collection literals. -->

    ```Clojure
    ;; good
    [1 2 3]
    (1 2 3)

    ;; bad
    [1, 2, 3]
    (1, 2, 3)
    ```

* コンマや改行を使い、マップリテラルの可読性を向上させることを検討する。
<!-- * Consider enhancing the readability of map literals via judicious use -->
<!-- of commas and line breaks. -->

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
<!-- * Place all trailing parentheses on a single line. -->

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
<!-- * Use empty lines between top-level forms. -->

    ```Clojure
    ;; good
    (def x ...)

    (defn foo ...)

    ;; bad
    (def x ...)
    (defn foo ...)
    ```

    例外として、関連する`def`はまとめてしまっても良い。
    <!-- An exception to the rule is the grouping of related `def`s together. -->

    ```Clojure
    ;; good
    (def min-rows 10)
    (def max-rows 20)
    (def min-cols 15)
    (def max-cols 30)
    ```

* 関数やマクロ定義の中には空白行を入れない。ただし、`let`や`cond`などの中においてペアをグループ分けするために入れるのは良い。
<!-- * Do not place blank lines in the middle of a function or -->
<!-- macro definition.  An exception can be made to indicate grouping of -->
<!-- pairwise constructs as found in e.g. `let` and `cond`. -->

* 1行が80文字を超えないようにする。
* 行末の空白を避ける。
* 名前空間ごとにファイルを分ける。
* 全ての名前空間は、複数の`import`、複数の`require`、複数の`use`からなる`ns`フォームで始める。
<!-- * Where feasible, avoid making lines longer than 80 characters. -->
<!-- * Avoid trailing whitespace. -->
<!-- * Use one file per namespace. -->
<!-- * Start every namespace with a comprehensive `ns` form, comprised of -->
<!--   `import`s, `require`s, `refer`s and `use`s. -->

    ```Clojure
    (ns examples.ns
      (:refer-clojure :exclude [next replace remove])
      (:require (clojure [string :as string]
                         [set :as set])
                [clojure.java.shell :as sh])
      (:use (clojure zip xml))
      (:import java.util.Date
               java.text.SimpleDateFormat
               (java.util.concurrent Executors
                                     LinkedBlockingQueue)))
    ```

* 単一セグメントの名前空間を使わない。
<!-- * Avoid single-segment namespaces. -->

    ```Clojure
    ;; good
    (ns example.ns)

    ;; bad
    (ns example)
    ```

* 無駄に長い名前空間を使わない（例えば、5セグメントを超えるような）。
<!-- * Avoid the use of overly long namespaces(i.e. with more than 5 segments). -->

* 関数は10行を超えないようにする。理想的には、ほとんどの関数は5行より短くしたほうが良い。
<!-- * Avoid functions longer than 10 LOC (lines of code). Ideally, most -->
<!--   functions will be shorter than 5 LOC. -->

* 3つか4つを超えるパラメータを持つパラメータリストの使用を避ける。
<!-- * Avoid parameter lists with more than three or four positional parameters. -->

## 構文

<!--
## Syntax
-->

* `require`や`refer`のような名前空間を扱う関数の使用を避ける。これらはREPL環境以外では必要ないものだ。
* 前方参照を可能にするには`declare`を使う。
* `loop/recur`よりも`map`のように、より高階な関数のほうが好ましい。
<!-- * Avoid the use of namespace-manipulating functions like `require` and -->
<!--   `refer`. They are entirely unnecessary outside of a REPL -->
<!--   environment. -->
<!-- * Use `declare` to enable forward references. -->
<!-- * Prefer higher-order functions like `map` to `loop/recur`. -->

* 関数本体内では、コンディションマップによる入力値、出力値のチェックがより良い。
<!-- * Prefer function pre and post conditions to checks inside a function's body. -->

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
<!-- * Don't define vars inside functions. -->

    ```Clojure
    ;; very bad
    (defn foo []
      (def x 5)
      ...)
    ```

* ローカル束縛によって`clojure.core`の名前を隠さない。
<!-- * Don't shadow `clojure.core` names with local bindings. -->

    ```Clojure
    ;; bad - you're forced to used clojure.core/map fully qualified inside
    (defn foo [map]
      ...)
    ```

* シーケンスが空かどうかをチェックするには`seq`を使う（このテクニックはしばしば *nil punning* と呼ばれる）。
<!-- * Use `seq` as a terminating condition to test whether a sequence is -->
<!--   empty (this technique is sometimes called *nil punning*). -->

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
<!-- * Use `when` instead of `(if ... (do ...)`. -->

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
<!-- * Use `if-let` instead of `let` + `if`. -->

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
<!-- * Use `when-let` instead of `let` + `when`. -->

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
<!-- * Use `if-not` instead of `(if (not ...) ...)`. -->

    ```Clojure
    ;; good
    (if-not (pred)
      (foo))

    ;; bad
    (if (not pred)
      (foo))
    ```

* `(when (not ...) ...)`の代わりに`when-not`を使う。
<!-- * Use `when-not` instead of `(when (not ...) ...)`. -->

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
<!-- * Use `when-not` instead of `(if-not ... (do ...)`. -->

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
<!-- * Use `not=` instead of `(not (= ...))`. -->

    ```Clojure
    ;; good
    (not= foo bar)

    ;; bad
    (not (= foo bar))
    ```

* 比較を行うときは、Clojure関数の`<`や`>`などは可変引数を許していることを覚えておこう。
<!-- * When doing comparisons keep in mind that Clojure's functions `<`, -->
<!--   `>`, etc accept variable number of arguments. -->

    ```Clojure
    ;; good
    (< 5 x 10)

    ;; bad
    (and (> x 5) (< x 10))
    ```

* ただ1つのパラメータを持つ関数リテラルでは、`%1`よりも`%`のほうが好ましい。
<!-- * Prefer `%` over `%1` in function literals with only one parameter. -->

    ```Clojure
    ;; good
    #(Math/round %)

    ;; bad
    #(Math/round %1)
    ```

* ふたつ以上のパラメータを持つ関数リテラルでは、`%`よりも`%1`のほうが好ましい。
<!-- * Prefer `%1` over `%` in function literals with more than one parameter. -->

    ```Clojure
    ;; good
    #(Math/pow %1 %2)

    ;; bad
    #(Math/pow % %2)
    ```

* 必要ないなら無名関数をラップしない。
<!-- * Don't wrap functions in anonymous functions when you don't need to. -->

    ```Clojure
    ;; good
    (filter even? (range 1 10))

    ;; bad
    (filter #(even? %) (range 1 10))
    ```

* 関数本体が2つ以上のフォームを含む場合は、関数リテラルを使用しない。
<!-- * Don't use function literals if the function body will consist of -->
<!--   more than one form. -->

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
<!-- * Favor the use of `complement` versus the use of an anonymous function. -->

    ```Clojure
    ;; good
    (filter (complement some-pred?) coll)

    ;; bad
    (filter #(not (some-pred? %)) coll)
    ```

    この規約は、反対の述語が別の関数としてある場合は無視するべきだ。（例：`even?`と`odd?`）

<!--
    This rule should obviously be ignored if the complementing predicate
    exists in the form of a separate function (e.g. `even?` and `odd?`).
-->

* コードをシンプルにするために`comp`の使用を考える。
<!-- * Leverage `comp` when it would yield simpler code. -->

    ```Clojure
    ;; Assuming `(:require [clojure.string :as str])`...

    ;; good
    (map #(str/capitalize (str/trim %)) ["top " " test "])

    ;; better
    (map (comp str/capitalize str/trim) ["top " " test "])
    ```

* コードをシンプルにするために`partial`の使用を考える。
<!-- * Leverage `partial` when it would yield simpler code. -->

    ```Clojure
    ;; good
    (map #(+ 5 %) (range 1 10))

    ;; (arguably) better
    (map (partial + 5) (range 1 10))
    ```

* 深いネストよりもスレッディングマクロ`->` (thread-first)と`->>` (thread-last)の使用が好ましい。
<!-- * Prefer the use of the threading macros `->` (thread-first) and `->>` -->
<!-- (thread-last) to heavy form nesting. -->

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
<!-- * Prefer `..` to `->` when chaining method calls in Java interop. -->

    ```Clojure
    ;; good
    (-> (System/getProperties) (.get "os.name"))

    ;; better
    (.. System getProperties (get "os.name"))
    ```

* `cond`や`condp`で残り全ての条件をキャッチするときは`:else`を使う。
<!-- * Use `:else` as the catch-all test expression in `cond` and `condp`. -->

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
<!-- * Prefer `condp` instead of `cond` when the predicate & expression don't -->
<!--   change. -->

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
<!-- * Prefer `case` instead of `cond` or `condp` when test expressions are -->
<!-- compile-time constants. -->

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
<!-- * Use short forms in `cond` and related.  If not possible give visual -->
<!-- hints for the pairwise grouping with comments or empty lines. -->

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
<!-- * Use a `set` as a predicate when appropriate. -->

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
<!-- * Use `(inc x)` & `(dec x)` instead of `(+ x 1)` and `(- x 1)`. -->

* `(> x 0)`, `(< x 0)`, `(= x 0)`の代わりに`(pos? x)`, `(neg? x)`, `(zero? x)`を使う。
<!-- * Use `(pos? x)`, `(neg? x)` & `(zero? x)` instead of `(> x 0)`, -->
<!-- `(< x 0)` & `(= x 0)`. -->

* 糖衣されたJava呼び出しフォームを用いる。
<!-- * Use the sugared Java interop forms. -->

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
    (. some-object some-field)
    ```

* キーがキーワード、値がブール値`true`のスロットしか持たないメタデータには、簡易メタデータ表記を使う。
<!-- * Use the compact metadata notation for metadata that contains only -->
<!--   slots whose keys are keywords and whose value is boolean `true`. -->

    ```Clojure
    ;; good
    (def ^:private a 5)

    ;; bad
    (def ^{:private true} a 5)
    ```

* コード中のプライベート部分には印を付ける。
<!-- * Denote private parts of your code. -->

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
<!-- * Be careful regarding what exactly do you attach metadata to. -->

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

<!--
## Naming
-->

> プログラミングで本当に難しいのは、キャッシュの無効化と命名の仕方だけだ。<br/>
> -- Phil Karlton

<!--
> The only real difficulties in programming are cache invalidation and
> naming things. <br/>
> -- Phil Karlton
-->

* 名前空間は次の2つの名づけ方が好ましい。
    * `project.module`
    * `organization.project.module`
* 複数単語からなる名前空間セグメントには`lisp-case`を使う（例：`bruce.project-euler`）
* 関数名や変数名には`lisp-case`を使う。
<!-- * When naming namespaces favor the following two schemas: -->
<!--     * `project.module` -->
<!--     * `organization.project.module` -->
<!-- * Use `lisp-case` in composite namespace segments(e.g. `bruce.project-euler`) -->
<!-- * Use `lisp-case` for function and variable names. -->

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
<!-- * Use `CamelCase` for protocols, records, structs, and types. (Keep -->
<!--   acronyms like HTTP, RFC, XML uppercase.) -->
* 述語（ブール値を返す関数）の名前はクエスチョンマーク（?）で終わるべきだ。（例：`even?`）
<!-- * The names of predicate methods (methods that return a boolean value) -->
<!--   should end in a question mark. -->
<!--   (i.e. `even?`). -->

    ```Clojure
    ;; good
    (defn palindrome? ...)

    ;; bad
    (defn palindrome-p ...) ; Common Lisp style
    (defn is-palindrome ...) ; Java style
    ```

* STMトランザクションの中で安全でない関数・マクロの名前はエクスクラメーションマーク（!）で終わるべきだ。（例：`reset!`）
<!-- * The names of functions/macros that are not safe in STM transactions -->
<!--   should end with an exclamation mark. (i.e. `reset!`) -->
* 変換のための関数名には`to`ではなく`->`を用いる。
<!-- * Use `->` instead of `to` in the names of conversion functions. -->

    ```Clojure
    ;; good
    (defn f->c ...)

    ;; not so good
    (defn f-to-c ...)
    ```

* 再束縛を想定しているものには`*earmuffs*`を使う（dynamicのような）。
<!-- * Use `*earmuffs*` for things intended for rebinding (ie. are dynamic). -->

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

<!--
* Don't use a special notation for constants; everything is assumed a constant
  unless specified otherwise.
* Use `_` for destructuring targets and formal arguments names whose
  value will be ignored by the code at hand.

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
-->

* `pred`や`coll`のような慣習名には`clojure.core`の例が参考になる。
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

<!--
* Follow `clojure.core`'s example for idiomatic names like `pred` and `coll`.
    * in functions:
        * `f`, `g`, `h` - function input
        * `n` - integer input usually a size
        * `index` - integer index
        * `x`, `y` - numbers
        * `s` - string input
        * `coll` - a collection
        * `pred` - a predicate closure
        * `& more` - variadic input
    * in macros:
        * `expr` - an expression
        * `body` - a macro body
        * `binding` - a macro binding vector
-->

## コレクション

<!--
## Collections
-->

> 10種のデータ構造を処理できる機能を10個用意するより、1種のデータ構造を処理できる機能を100個用意した方がよい。<br/>
> -- Alan J. Perlis

<!--
> It is better to have 100 functions operate on one data structure
> than to have 10 functions operate on 10 data structures. <br/>
> -- Alan J. Perlis
-->

* 汎用的なデータ置き場としてリストを使うことを避ける（リストが本当に必要な場合を除く）。
* マップのキーにはキーワードを用いたほうが良い。

    ```Clojure
    ;; good
    {:name "Bruce" :age 30}

    ;; bad
    {"name" "Bruce" "age" 30}
    ```

<!--
* Avoid the use of lists for generic data storage (unless a list is
  exactly what you need).
* Prefer the use of keywords for hash keys.

    ```Clojure
    ;; good
    {:name "Bruce" :age 30}

    ;; bad
    {"name" "Bruce" "age" 30}
    ```
-->

* 可能ならコレクションのリテラル構文を用いたほうが良い。ただし、セットを定義するときは、コンパイル時に定数である値についてのみリテラル構文を使用する。
<!-- * Prefer the use of the literal collection syntax where -->
<!--   applicable. However, when defining sets, only use literal syntax -->
<!--   when the values are compile-time constants. -->

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

* 可能ならコレクションの要素にインデックスでアクセスすることを避ける。
<!-- * Avoid accessing collection members by index whenever possible. -->

* 可能なら、マップの値を取得するにはキーワードを関数として用いるのが良い。
<!-- * Prefer the use of keywords as functions for retrieving values from -->
<!--   maps, where applicable. -->

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

<!--
* Leverage the fact that most collections are functions of their elements.

    ```Clojure
    ;; good
    (filter #{\a \e \o \i \u} "this is a test")

    ;; bad - too ugly to share
    ```
-->

* キーワードはコレクションの関数として使えることを活用する。

    ```Clojure
    ((juxt :a :b) {:a "ala" :b "bala"})
    ```

<!--
* Leverage the fact that keywords can be used as functions of a collection.

    ```Clojure
    ((juxt :a :b) {:a "ala" :b "bala"})
    ```
-->

* パフォーマンス問題がクリティカルとなる部分を除いて、一時的なコレクションの使用を避ける。

* Javaのコレクションの使用を避ける。

* Java呼び出しや、プリミティブ型を多く使うパフォーマンスクリティカルな部分を除いて、Javaの配列の使用を避ける。

<!--
* Avoid the use of transient collections, except for
performance-critical portions of the code.

* Avoid the use of Java collections.

* Avoid the use of Java arrays, except for interop scenarios and
performance-critical code dealing heavily with primitive types.
-->

## 状態

<!--
## Mutation
-->

### ref

<!--
### Refs
-->

* トランザクションの中で思いがけずI/Oコールを呼んでしまったときの問題を回避するため、全てのI/Oコールを`io!`マクロでラップすることを考える。
* 出来る限り`ref-set`は使用しない。

    ```Clojure
    (def r (ref 0))

    ;; good
    (dosync (alter r + 5))

    ;; bad
    (dosync (ref-set r 5))
    ```

<!--
* Consider wrapping all I/O calls with the `io!` macro to avoid nasty
surprises if you accidentally end up calling such code in a
transaction.
* Avoid the use of `ref-set` whenever possible.

    ```Clojure
    (def r (ref 0))

    ;; good
    (dosync (alter r + 5))

    ;; bad
    (dosync (ref-set r 5))
    ```
-->

* トランザクションのサイズ（包んでいる処理の量）を出来る限り小さく保つようにする。
* 同一のrefとやり取りを行う、短期のトランザクションと長期のトランザクションを持つことを避ける。

<!--
* Try to keep the size of transactions (the amount of work encapsulated in them)
as small as possible.
* Avoid having both short- and long-running transactions interacting
  with the same Ref.
-->

### エージェント

* それがCPUバウンドで、かつI/Oや他スレッドをブロックしない処理のときだけ`send`を用いる。
* それがスレッドをブロック、スリープさせたり、そうでなくても停滞させるかもしれない処理には`send-off`を用いる。

<!--
### Agents

* Use `send` only for actions that are CPU bound and don't block on I/O
  or other threads.
* Use `send-off` for actions that might block, sleep, or otherwise tie
  up the thread.
-->

### アトム

<!--
### Atoms
-->

* STMトランザクションの中でアトムを更新することを避ける。
* 可能なら、`reset!`よりも`swap!`を使うようにする。

    ```Clojure
    (def a (atom 0))

    ;; good
    (swap! a + 5)

    ;; not as good
    (reset! a 5)
    ```

<!--
* Avoid atom updates inside STM transactions.
* Try to use `swap!` rather than `reset!`, where possible.

    ```Clojure
    (def a (atom 0))

    ;; good
    (swap! a + 5)

    ;; not as good
    (reset! a 5)
    ```
-->

## 文字列

* 文字列処理は、Java呼び出しや自分で書くよりも、`clojure.string`の関数を使うほうが好ましい。

    ```Clojure
    ;; good
    (clojure.string/upper-case "bruce")

    ;; bad
    (.toUpperCase "bruce")
    ```

<!--
* Prefer string manipulation functions from `clojure.string` over Java interop or rolling your own.

    ```Clojure
    ;; good
    (clojure.string/upper-case "bruce")

    ;; bad
    (.toUpperCase "bruce")
    ```
-->

## 例外

* 既存の例外型を再利用する。慣用的なClojureコードでは、例外を投げるとき、基本的な例外型を用いている。
  （例： `java.lang.IllegalArgumentException`,
  `java.lang.UnsupportedOperationException`,
  `java.lang.IllegalStateException`, `java.io.IOException`）。
* `finally`よりも`with-open`のほうが好ましい。

<!--
* Reuse existing exception types. Idiomatic Clojure code, when it does
  throw an exception, throws an exception of a standard type
  (e.g. `java.lang.IllegalArgumentException`,
  `java.lang.UnsupportedOperationException`,
  `java.lang.IllegalStateException`, `java.io.IOException`).
* Favor `with-open` over `finally`.
-->

## マクロ

<!--
## Macros
-->

* その処理が関数でできるならマクロを書かない。
* まずマクロの使用例を作成し、その後でマクロを作る。
* 複雑なマクロは、可能なら小さい機能に分割する。
* マクロは通常、構文糖衣を提供するものであるべきで、そのコアは単純な機能であるべきだ。そうすることでより構造化されるだろう。
* 自分でリストを組み立てるよりも構文クオートを使用するほうが好ましい。

<!--
* Don't write a macro if a function will do.
* Create an example of a macro usage first and the macro afterwards.
* Break complicated macros into smaller functions whenever possible.
* A macro should usually just provide syntactic sugar and the core of
  the macro should be a plain function. Doing so will improve
  composability.
* Prefer syntax-quoted forms over building lists manually.
-->

## コメント

<!--
## Comments
-->

> 良いコードとは、それ自体が最良のドキュメントになっているものだ。コメントを付けようとしたとき、自分の胸に聞いてみるといい、「どうやってコードを改良して、このコメントを不要にできるだろうか？」ってね。より美しくするために、コードを改良してからドキュメント化するんだ。<br/>
> -- Steve McConnell

<!--
> Good code is its own best documentation. As you're about to add a
> comment, ask yourself, "How can I improve the code so that this
> comment isn't needed?" Improve the code and then document it to make
> it even clearer. <br/>
> -- Steve McConnell
-->

* 出来る限り、コードを見れば何をしているのか分かるように努める。
<!-- * Endeavor to make your code as self-documenting as possible. -->

* ヘッダーコメントには最低4つのセミコロンを用いる。
<!-- * Write heading comments with at least four semicolons. -->

* トップレベルのコメントには3つのセミコロンを用いる。
<!-- * Write top-level comments with three semicolons. -->

* 特定のコード部分の直前にコメントを書くときは、コード部分とインデントを揃え、2つのセミコロンを用いる。
<!-- * Write comments on a particular fragment of code before that fragment -->
<!-- and aligned with it, using two semicolons. -->

* 行末コメントには1つのセミコロンを用いる。
<!-- * Write margin comments with one semicolon. -->

* セミコロンとテキストの間には最低1つのスペースを入れる。
<!-- * Always have at least one space between the semicolon -->
<!-- and the text that follows it. -->

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
<!-- * Comments longer than a word begin with a capital letter and use -->
<!--   punctuation. Separate sentences with -->
<!--   [one space](http://en.wikipedia.org/wiki/Sentence_spacing). -->
* 無意味なコメントを避ける。
<!-- * Avoid superfluous comments. -->

    ```Clojure
    ;; bad
    (inc counter) ; increments counter by one
    ```

* コメントは常に更新していなければならない。古いコメントは、コメントがないことよりも害悪だ。
<!-- * Keep existing comments up-to-date. An outdated comment is worse than no comment -->
<!-- at all. -->
* 特定のフォームをコメントアウトする必要があるときは、通常のコメントではなく`#_`リーダマクロを用いたほうが良い。
<!-- * Prefer the use of the `#_` reader macro over a regular comment when -->
<!-- you need to comment out a particular form. -->

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

<!--
> Good code is like a good joke - it needs no explanation. <br/>
> -- Russ Olsen
-->

* 悪いコードを説明するためにコメントを書くことを避ける。コードをリファクタリングして、コメントが不要なようにするべきだ。（「やるか、やらないだ。やってみるではない」--Yoda）

<!--
* Avoid writing comments to explain bad code. Refactor the code to
  make it self-explanatory. ("Do, or do not. There is no try." --Yoda)
-->

### コメントアノテーション

<!--
### Comment Annotations
-->

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

<!--
* Annotations should usually be written on the line immediately above
  the relevant code.
* The annotation keyword is followed by a colon and a space, then a note
  describing the problem.
* If multiple lines are required to describe the problem, subsequent
  lines should be indented as much as the first one.
* Tag the annotation with your initials and a date so its relevance can
  be easily verified.

    ```Clojure
    (defn some-fun
      []
      ;; FIXME: This has crashed occasionally since v1.2.3. It may
      ;;        be related to the BarBazUtil upgrade. (xz 13-1-31)
      (baz))
    ```
-->

* ドキュメント化が不必要なほどに問題が明らかな箇所では、当該行の末尾に説明なしでアノテーションを付けても良い。この使用法は例外的であるべきで、規約ではない。

    ```Clojure
    (defn bar
      []
      (sleep 100)) ; OPTIMIZE
    ```

<!--
* In cases where the problem is so obvious that any documentation would
  be redundant, annotations may be left at the end of the offending line
  with no note. This usage should be the exception and not the rule.

    ```Clojure
    (defn bar
      []
      (sleep 100)) ; OPTIMIZE
    ```
-->

* 後日追加されるべき機能には`TODO`を使う。
* コードが壊れていて、修正の必要がある箇所には`FIXME`を使う。
* パフォーマンス問題の原因となりうる、遅かったり非効率なコードには`OPTIMIZE`を使う。
* 疑わしいコーディングの仕方がされており、リファクタリングすべき「コード・スメル」には`HACK`を用いる。
* 意図するように動くかどうか確認すべき箇所には`REVIEW`を使う。例：`REVIEW: Are we sure this is how the client does X currently?`
* そのほうが適切だと思えば、その他独自のアノテーションキーワードを用いる。ただし、プロジェクトの`README`などに忘れずにドキュメント化しておく。

<!--
* Use `TODO` to note missing features or functionality that should be
  added at a later date.
* Use `FIXME` to note broken code that needs to be fixed.
* Use `OPTIMIZE` to note slow or inefficient code that may cause
  performance problems.
* Use `HACK` to note "code smells" where questionable coding practices
  were used and should be refactored away.
* Use `REVIEW` to note anything that should be looked at to confirm it
  is working as intended. For example: `REVIEW: Are we sure this is how the
  client does X currently?`
* Use other custom annotation keywords if it feels appropriate, but be
  sure to document them in your project's `README` or similar.
-->

## 実際のコードでは

* 関数型的にコードを書き、状態を持つことを避けるのが理にかなっている。
* 一貫させる。理想的には、このガイドの通りにする。
* 常識的に考える。

<!--
* Code in a functional way, avoiding mutation when that makes sense.
* Be consistent. In an ideal world, be consistent with these guidelines.
* Use common sense.
-->

## ツール

<!--
## Tooling
-->

慣用的なClojureコードを書くのを助けてくれるツールがClojureコミュニティによって作られている。

<!--
There are some tools created by the Clojure community that might aid you
in your endeavor to write idiomatic Clojure code.
-->

* [Slamhound](https://github.com/technomancy/slamhound)は既存のコードから適切な`ns`定義を自動的に生成してくれる。
* [kibit](https://github.com/jonase/kibit)はClojure向けの静的コード解析ツールだ。より慣用的な関数やマクロの探索には[core.logic](https://github.com/clojure/core.logic)を用いている。

<!--
* [Slamhound](https://github.com/technomancy/slamhound) is a tool that
will automatically generate proper `ns` declarations from your
existing code.
* [kibit](https://github.com/jonase/kibit) is a static code analyzer
  for Clojure which uses
  [core.logic](https://github.com/clojure/core.logic) to search for
  patterns of code for which there might exist a more idiomatic
  function or macro.
-->

<!--
# Contributing

Nothing written in this guide is set in stone. It's my desire to work
together with everyone interested in Clojure coding style, so that we could
ultimately create a resource that will be beneficial to the entire Clojure
community.

Feel free to open tickets or send pull requests with improvements. Thanks in
advance for your help!

# License

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
This work is licensed under a
[Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/deed.en_US)
-->

# 広めてください

コミュニティドリブンのスタイルガイドは、その存在を知らないコミュニティではあまり役に立ちません。どうか、このガイドについてツイートをして、あなたの友達や同僚と共有してください。頂いたあらゆるコメントや提案、意見がほんの少しずつ、このガイドを形作っていくのです。みんなで最高のスタイルガイドを作りましょう。


<!--
A community-driven style guide is of little use to a community that
doesn't know about its existence. Tweet about the guide, share it with
your friends and colleagues. Every comment, suggestion or opinion we
get makes the guide just a little bit better. And we want to have the
best possible guide, don't we?
-->

Cheers,<br/>
[Bozhidar](https://twitter.com/bbatsov)
