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

また、Clojure開発コミュニティが[ライブラリのコーディング規約](http://dev.clojure.org/display/community/Library+Coding+Standards)の一覧をまとめていることも覚えておいてほしい。

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

* <a name="spaces"></a>
  各インデントには **スペース** を使う。タブは使わない。
<sup>[[リンク](#spaces)]</sup>

* <a name="body-indentation"></a>
ボディパラメータをもつフォームのボディには2つのスペースを使う。これには全ての`def`フォーム、ローカル束縛をもつスペシャルフォームおよびマクロ（例：`loop`, `let`, `when-let`）、そして`when`, `cond`, `as->`, `case`,
`with-*`などの多くのマクロが含まれる。
<sup>[[リンク](#body-indentation)]</sup>


    ```Clojure
    ;; 良い
    (when something
      (something-else))

    (with-out-str
      (println "Hello, ")
      (println "world!"))

    ;; 悪い - 4つのスペース
    (when something
        (something-else))

    ;; 悪い - 1つのスペース
    (with-out-str
     (println "Hello, ")
     (println "world!"))
    ```

* <a name="vertically-align-fn-args"></a>
  複数行にわたる関数（マクロ）の引数は左揃えにする。
<sup>[[リンク](#vertically-align-fn-args)]</sup>

    ```Clojure
    ;; 良い
    (filter even?
            (range 1 10))

    ;; 悪い
    (filter even?
      (range 1 10))
    ```

* <a name="one-space-indent"></a>
関数（マクロ）名と同じ行に引数をもたない関数（マクロ）では、インデントには1つのスペースを用いる。
<sup>[[リンク](#one-space-indent)]</sup>

    ```Clojure
    ;; 良い
    (filter
     even?
     (range 1 10))

    (or
     ala
     bala
     portokala)

    ;; 悪い - 2つのスペースによるインデント
    (filter
      even?
      (range 1 10))

    (or
      ala
      bala
      portokala)
    ```


* <a name="vertically-align-let-and-map"></a>
  `let`の束縛とマップのキーワードを左揃えにする。
<sup>[[リンク](#vertically-align-let-and-map)]</sup>

    ```Clojure
    ;; 良い
    (let [thing1 "some stuff"
          thing2 "other stuff"]
      {:thing1 thing1
       :thing2 thing2})

    ;; 悪い
    (let [thing1 "some stuff"
      thing2 "other stuff"]
      {:thing1 thing1
      :thing2 thing2})
    ```

* <a name="optional-new-line-after-fn-name"></a>
  `defn`において、ドキュメント文字列を持たない場合は、関数名と引数ベクタの間の改行を省略しても良い。
<sup>[[リンク](#optional-new-line-after-fn-name)]</sup>

    ```Clojure
    ;; 良い
    (defn foo
      [x]
      (bar x))

    ;; 良い
    (defn foo [x]
      (bar x))

    ;; 悪い
    (defn foo
      [x] (bar x))
    ```

* <a name="oneline-short-fn"></a>
  関数本体が短い場合、引数ベクタと関数本体の間の改行は省略しても良い。
<sup>[[リンク](#oneline-short-fn)]</sup>

    ```Clojure
    ;; 良い
    (defn foo [x]
      (bar x))

    ;; 関数本体が短い場合は良い
    (defn foo [x] (bar x))

    ;; マルチアリティ関数には良い
    (defn foo
      ([x] (bar x))
      ([x y]
       (if (predicate? x)
         (bar x)
         (baz x))))

    ;; 悪い
    (defn foo
      [x] (if (predicate? x)
            (bar x)
            (baz x)))
    ```

* <a name="multiple-arity-indentation"></a>
  関数定義における各アリティのフォームのインデントは、そのパラメータと左揃えにする。
<sup>[[リンク](#multiple-arity-indentation)]</sup>

```Clojure
;; 良い
(defn foo
  "I have two arities."
  ([x]
   (foo x 1))
  ([x y]
   (+ x y)))

;; 悪い - 過剰なインデント
(defn foo
  "I have two arities."
  ([x]
    (foo x 1))
  ([x y]
    (+ x y)))
```

* <a name="align-docstring-lines"></a>
  複数行に渡るドキュメント文字列はインデントする。
<sup>[[リンク](#align-docstring-lines)]</sup>

    ```Clojure
    ;; 良い
    (defn foo
      "Hello there. This is
      a multi-line docstring."
      []
      (bar))

    ;; 悪い
    (defn foo
      "Hello there. This is
    a multi-line docstring."
      []
      (bar))
    ```

* <a name="crlf"></a>
  Unixスタイルの改行コードを使用する。（*BSD/Solaris/Linux/OSXユーザはデフォルトで問題ないが、Windowsユーザは特に注意すること。）
<sup>[[リンク](#crlf)]</sup>

    * Gitを使っているなら、次の設定を追加して、Windowsの改行コードを防ぐのもいい。

    ```
    bash$ git config --global core.autocrlf true
    ```

* <a name="bracket-spacing"></a>
  開き括弧（`(`, `{`, `[`）の前の文字と、閉じ括弧（`)`, `}`, `]`）の後の文字は、括弧との間にスペースを設ける。
逆に、開き括弧とそれに続く文字、閉じ括弧と直前の文字の間にはスペースを入れない。
<sup>[[リンク](#bracket-spacing)]</sup>

    ```Clojure
    ;; 良い
    (foo (bar baz) quux)

    ;; 悪い
    (foo(bar baz)quux)
    (foo ( bar baz ) quux)
    ```

> 構文糖衣はセミコロンのガンを引き起こす。 <br/>
> -- Alan Perlis

* <a name="no-commas-for-seq-literals"></a>
  シーケンシャルコレクションのリテラルの要素の間にコンマを使わない。
<sup>[[リンク](#no-commas-for-seq-literals)]</sup>

    ```Clojure
    ;; 良い
    [1 2 3]
    (1 2 3)

    ;; 悪い
    [1, 2, 3]
    (1, 2, 3)
    ```

* <a name="opt-commas-in-map-literals"></a>
  コンマや改行を使い、マップリテラルの可読性を向上させることを検討する。
<sup>[[リンク](#opt-commas-in-map-literals)]</sup>

    ```Clojure
    ;; 良い
    {:name "Bruce Wayne" :alter-ego "Batman"}

    ;; 良い、より読みやすい
    {:name "Bruce Wayne"
     :alter-ego "Batman"}

    ;; 良い、よりコンパクト
    {:name "Bruce Wayne", :alter-ego "Batman"}
    ```

* <a name="gather-trailing-parens"></a>
  後ろ側に連続する括弧は、別々の行にせず、同じ行に含める。
<sup>[[リンク](#gather-trailing-parens)]</sup>

    ```Clojure
    ;; 良い。同じ行になっている。
    (when something
      (something-else))

    ;; 悪い。別の行になっている。
    (when something
      (something-else)
    )
    ```

* <a name="empty-lines-between-top-level-forms"></a>
  トップレベルのフォームの間には空白行を挟む。
<sup>[[リンク](#empty-lines-between-top-level-forms)]</sup>

    ```Clojure
    ;; 良い
    (def x ...)

    (defn foo ...)

    ;; 悪い
    (def x ...)
    (defn foo ...)
    ```

    例外として、関連する`def`はまとめてしまっても良い。

    ```Clojure
    ;; 良い
    (def min-rows 10)
    (def max-rows 20)
    (def min-cols 15)
    (def max-cols 30)
    ```

* <a name="no-blank-lines-within-def-forms"></a>
  関数やマクロ定義の中には空白行を入れない。ただし、`let`や`cond`などの中においてペアをグループ分けするために入れるのは良い。
<sup>[[リンク](#no-blank-lines-within-def-forms)]</sup>

* <a name="80-character-limits"></a>
  1行が80文字を超えないようにする。
<sup>[[リンク](#80-character-limits)]</sup>

* <a name="no-trailing-whitespace"></a>
  行末の空白を避ける。
<sup>[[リンク](#no-trailing-whitespace)]</sup>

* <a name="one-file-per-namespace"></a>
  名前空間ごとにファイルを分ける。
<sup>[[リンク](#one-file-per-namespace)]</sup>

* <a name="comprehensive-ns-declaration"></a>
  全ての名前空間は、複数の`import`, `require`, `refer`, `use`からなる`ns`フォームで始める。
<sup>[[リンク](#comprehensive-ns-declaration)]</sup>

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

* <a name="prefer-require-over-use"></a>
  nsマクロでは、`:use`よりも`:require :refer :all`を用いるほうが良い。
<sup>[[リンク](#prefer-require-over-use)]</sup>

    ```Clojure
    ;; 良い
    (ns examples.ns
      (:require [clojure.zip :refer :all]))

    ;; 悪い
    (ns examples.ns
      (:use clojure.zip))
    ```

* <a name="no-single-segment-namespaces"></a>
  単一セグメントの名前空間を使わない。
<sup>[[リンク](#no-single-segment-namespaces)]</sup>

    ```Clojure
    ;; 良い
    (ns example.ns)

    ;; 悪い
    (ns example)
    ```

* <a name="namespaces-with-5-segments-max"></a>
  無駄に長い名前空間を使わない（例えば、5セグメントを超えるような）。
<sup>[[リンク](#namespaces-with-5-segments-max)]</sup>

* <a name="10-loc-per-fn-limit"></a>
  関数は10行を超えないようにする。理想的には、ほとんどの関数は5行より短くしたほうが良い。
<sup>[[リンク](#10-loc-per-fn-limit)]</sup>

* <a name="4-positional-fn-params-limit"></a>
  3つか4つを超えるパラメータを持つパラメータリストの使用を避ける。
<sup>[[リンク](#4-positional-fn-params-limit)]</sup>

## 構文

* <a name="ns-fns-only-in-repl"></a>
  `require`や`refer`のような名前空間を扱う関数の使用を避ける。これらはREPL環境以外では必要ないものだ。
<sup>[[リンク](#ns-fns-only-in-repl)]</sup>

* <a name="declare"></a>
  前方参照を可能にするには`declare`を使う。
<sup>[[リンク](#declare)]</sup>

* <a name="higher-order-fns"></a>
  `loop/recur`よりも`map`のように、より高階な関数のほうが好ましい。
<sup>[[リンク](#higher-order-fns)]</sup>

* <a name="pre-post-conditions"></a>
  関数本体内では、コンディションマップによる入力値、出力値のチェックがより良い。
<sup>[[リンク](#pre-post-conditions)]</sup>

    ```Clojure
    ;; 良い
    (defn foo [x]
      {:pre [(pos? x)]}
      (bar x))

    ;; 悪い
    (defn foo [x]
      (if (pos? x)
        (bar x)
        (throw (IllegalArgumentException. "x must be a positive number!")))
    ```

* <a name="dont-def-vars-inside-fns"></a>
  関数内でvarを定義しない。
<sup>[[リンク](#dont-def-vars-inside-fns)]</sup>

    ```Clojure
    ;; 非常に悪い
    (defn foo []
      (def x 5)
      ...)
    ```

* <a name="dont-shadow-clojure-core"></a>
  ローカル束縛によって`clojure.core`の名前を隠さない。
<sup>[[リンク](#dont-shadow-clojure-core)]</sup>

    ```Clojure
    ;; 悪い - 関数内では完全修飾したclojure.core/mapを使わなければいけなくなる
    (defn foo [map]
      ...)
    ```

* <a name="nil-punning"></a>
  シーケンスが空かどうかをチェックするには`seq`を使う（このテクニックはしばしば *nil punning* と呼ばれる）。
<sup>[[リンク](#nil-punning)]</sup>

    ```Clojure
    ;; 良い
    (defn print-seq [s]
      (when (seq s)
        (prn (first s))
        (recur (rest s))))

    ;; 悪い
    (defn print-seq [s]
      (when-not (empty? s)
        (prn (first s))
        (recur (rest s))))
    ```

* <a name="to-vector"></a>
  シーケンスをベクタに変換する必要があるときは、`into`よりも`vec`を用いたほうが良い。
<sup>[[リンク](#to-vector)]</sup>

    ```Clojure
    ;; 良い
    (vec some-seq)

    ;; 悪い
    (into [] some-seq)
    ```

* <a name="when-instead-of-single-branch-if"></a>
  `(if ... (do ...)`の代わりに`when`を使う。
<sup>[[リンク](#when-instead-of-single-branch-if)]</sup>

    ```Clojure
    ;; 良い
    (when pred
      (foo)
      (bar))

    ;; 悪い
    (if pred
      (do
        (foo)
        (bar)))
    ```
* <a name="if-let"></a>
  `let` + `if`の代わりに`if-let`を使う。
<sup>[[リンク](#if-let)]</sup>

    ```Clojure
    ;; 良い
    (if-let [result (foo x)]
      (something-with result)
      (something-else))

    ;; 悪い
    (let [result (foo x)]
      (if result
        (something-with result)
        (something-else)))
    ```

* <a name="when-let"></a>
  `let` + `when`の代わりに`when-let`を使う。
<sup>[[リンク](#when-let)]</sup>

    ```Clojure
    ;; 良い
    (when-let [result (foo x)]
      (do-something-with result)
      (do-something-more-with result))

    ;; 悪い
    (let [result (foo x)]
      (when result
        (do-something-with result)
        (do-something-more-with result)))
    ```

* <a name="if-not"></a>
  `(if (not ...) ...)`の代わりに`if-not`を使う。
<sup>[[リンク](#if-not)]</sup>

    ```Clojure
    ;; 良い
    (if-not pred
      (foo))

    ;; 悪い
    (if (not pred)
      (foo))
    ```

* <a name="when-not"></a>
  `(when (not ...) ...)`の代わりに`when-not`を使う。
<sup>[[リンク](#when-not)]</sup>

    ```Clojure
    ;; 良い
    (when-not pred
      (foo)
      (bar))

    ;; 悪い
    (when (not pred)
      (foo)
      (bar))
    ```

* <a name="when-not-instead-of-single-branch-if-not"></a>
  `(if-not ... (do ...)`の代わりに`when-not`を使う。
<sup>[[リンク](#when-not-instead-of-single-branch-if-not)]</sup>

    ```Clojure
    ;; 良い
    (when-not pred
      (foo)
      (bar))

    ;; 悪い
    (if-not pred
      (do
        (foo)
        (bar)))
    ```

* <a name="not-equal"></a>
  `(not (= ...))`の代わりに`not=`を使う。
<sup>[[リンク](#not-equal)]</sup>

    ```Clojure
    ;; 良い
    (not= foo bar)

    ;; 悪い
    (not (= foo bar))
    ```

* <a name="multiple-arity-of-gt-and-ls-fns"></a>
  比較を行うときは、Clojure関数の`<`や`>`などは可変長引数を許していることを覚えておこう。
<sup>[[リンク](#multiple-arity-of-gt-and-ls-fns)]</sup>

    ```Clojure
    ;; 良い
    (< 5 x 10)

    ;; 悪い
    (and (> x 5) (< x 10))
    ```

* <a name="single-param-fn-literal"></a>
  ただ1つのパラメータを持つ関数リテラルでは、`%1`よりも`%`のほうが好ましい。
<sup>[[リンク](#single-param-fn-literal)]</sup>

    ```Clojure
    ;; 良い
    #(Math/round %)

    ;; 悪い
    #(Math/round %1)
    ```

* <a name="multiple-params-fn-literal"></a>
  複数のパラメータを持つ関数リテラルでは、`%`よりも`%1`のほうが好ましい。
<sup>[[リンク](#multiple-params-fn-literal)]</sup>

    ```Clojure
    ;; 良い
    #(Math/pow %1 %2)

    ;; 悪い
    #(Math/pow % %2)
    ```

* <a name="no-useless-anonymous-fns"></a>
  必要ないなら無名関数でラップしない。
<sup>[[リンク](#no-useless-anonymous-fns)]</sup>

    ```Clojure
    ;; 良い
    (filter even? (range 1 10))

    ;; 悪い
    (filter #(even? %) (range 1 10))
    ```

* <a name="no-multiple-forms-fn-literals"></a>
  関数本体が2つ以上のフォームを含む場合は、関数リテラルを使用しない。
<sup>[[リンク](#no-multiple-forms-fn-literals)]</sup>

    ```Clojure
    ;; 良い
    (fn [x]
      (println x)
      (* x 2))

    ;; 悪い (doフォームを明示的に使わなければならない)
    #(do (println %)
         (* % 2))
    ```

* <a name="complement"></a>
  無名関数よりも`complement`を用いたほうが良い。
<sup>[[リンク](#complement)]</sup>

    ```Clojure
    ;; 良い
    (filter (complement some-pred?) coll)

    ;; 悪い
    (filter #(not (some-pred? %)) coll)
    ```

    この規約は、反対の述語が別の関数としてある場合は無視するべきだ。（例：`even?`と`odd?`）

* <a name="comp"></a>
  コードをシンプルにするために`comp`の使用を考える。
<sup>[[リンク](#comp)]</sup>

    ```Clojure
    ;; `(:require [clojure.string :as str])`を仮定して...

    ;; 良い
    (map #(str/capitalize (str/trim %)) ["top " " test "])

    ;; より良い
    (map (comp str/capitalize str/trim) ["top " " test "])
    ```

* <a name="partial"></a>
  コードをシンプルにするために`partial`の使用を考える。
<sup>[[リンク](#partial)]</sup>

    ```Clojure
    ;; 良い
    (map #(+ 5 %) (range 1 10))

    ;; (きっと) より良い
    (map (partial + 5) (range 1 10))
    ```

* <a name="threading-macros"></a>
  深いネストよりもスレッディングマクロ`->` (thread-first)と`->>` (thread-last)の使用が好ましい。
<sup>[[リンク](#threading-macros)]</sup>

    ```Clojure
    ;; 良い
    (-> [1 2 3]
        reverse
        (conj 4)
        prn)

    ;; あまり良くない
    (prn (conj (reverse [1 2 3])
               4))

    ;; 良い
    (->> (range 1 10)
         (filter even?)
         (map (partial * 2)))

    ;; あまり良くない
    (map (partial * 2)
         (filter even? (range 1 10)))
    ```

* <a name="dot-dot-macro"></a>
  Java呼び出しの際のチェーンメソッドコールには、`->`よりも`..`が好ましい。
<sup>[[リンク](#dot-dot-macro)]</sup>

    ```Clojure
    ;; 良い
    (-> (System/getProperties) (.get "os.name"))

    ;; より良い
    (.. System getProperties (get "os.name"))
    ```

* <a name="else-keyword-in-cond"></a>
  `cond`で残り全ての条件をキャッチするときは`:else`を使う。
<sup>[[リンク](#else-keyword-in-cond)]</sup>

    ```Clojure
    ;; 良い
    (cond
      (< n 0) "negative"
      (> n 0) "positive"
      :else "zero"))

    ;; 悪い
    (cond
      (< n 0) "negative"
      (> n 0) "positive"
      true "zero"))
    ```

* <a name="condp"></a>
  述語と式が変わらない場合、`cond`よりも`condp`のほうが良い。
<sup>[[リンク](#condp)]</sup>

    ```Clojure
    ;; 良い
    (cond
      (= x 10) :ten
      (= x 20) :twenty
      (= x 30) :forty
      :else :dunno)

    ;; より良い
    (condp = x
      10 :ten
      20 :twenty
      30 :forty
      :dunno)
    ```

* <a name="case"></a>
  テスト式がコンパイル時に固定の場合、`cond`や`condp`の代わりに`case`を使うのが良い。
<sup>[[リンク](#case)]</sup>

    ```Clojure
    ;; 良い
    (cond
      (= x 10) :ten
      (= x 20) :twenty
      (= x 30) :forty
      :else :dunno)

    ;; より良い
    (condp = x
      10 :ten
      20 :twenty
      30 :forty
      :dunno)

    ;; 最も良い
    (case x
      10 :ten
      20 :twenty
      30 :forty
      :dunno)
    ```

* <a name="shor-forms-in-cond"></a>
  `cond`などの中では短いフォームを用いる。それが無理なら、コメントや空白行を使用して、ペアグループを見えやすくする。
<sup>[[リンク](#shor-forms-in-cond)]</sup>

    ```Clojure
    ;; 良い
	(cond
	  (test1) (action1)
	  (test2) (action2)
	  :else   (default-action))

	;; まあ良い
	(cond
	;; test case 1
	(test1)
	(long-function-name-which-requires-a-new-line
       (complicated-sub-form
          (-> 'which-spans multiple-lines)))

	(test2)
	(another-very-long-function-name
       (yet-another-sub-form
          (-> 'which-spans multiple-lines)))

    :else
    (the-fall-through-default-case
      (which-also-spans 'multiple
                        'lines)))
    ```

* <a name="set-as-predicate"></a>
  `set`を述語として使うことができる。
<sup>[[リンク](#set-as-predicate)]</sup>

    ```Clojure
    ;; 良い
    (remove #{0} [0 1 2 3 4 5])

    ;; 悪い
    (remove #(= % 0) [0 1 2 3 4 5])

    ;; 良い
    (count (filter #{\a \e \i \o \u} "mary had a little lamb"))

    ;; 悪い
    (count (filter #(or (= % \a)
                        (= % \e)
                        (= % \i)
                        (= % \o)
                        (= % \u))
                   "mary had a little lamb"))
    ```

* <a name="inc-and-dec"></a>
  `(+ x 1)`や`(- x 1)`の代わりに`(inc x)`や`(dec x)`を使う。
<sup>[[リンク](#inc-and-dec)]</sup>

* <a name="pos-and-neg"></a>
  `(> x 0)`, `(< x 0)`, `(= x 0)`の代わりに`(pos? x)`, `(neg? x)`, `(zero? x)`を使う。
<sup>[[リンク](#pos-and-neg)]</sup>

* <a name="list-star-instead-of-nested-cons"></a>
  ネストされた`cons`を呼び出す代わりに`list*`を使う。
<sup>[[リンク](#list-star-instead-of-nested-cons)]</sup>

    ```Clojure
    ;; 良い
    (list* 1 2 3 [4 5])

    ;; 悪い
    (cons 1 (cons 2 (cons 3 [4 5])))
    ```

* <a name="sugared-java-interop"></a>
  糖衣されたJava呼び出しフォームを用いる。
<sup>[[リンク](#sugared-java-interop)]</sup>

    ```Clojure
    ;;; オブジェクト生成
    ;; 良い
    (java.util.ArrayList. 100)

    ;; 悪い
    (new java.util.ArrayList 100)

    ;;; 静的メソッドの呼び出し
    ;; 良い
    (Math/pow 2 10)

    ;; 悪い
    (. Math pow 2 10)

    ;;; インスタンスメソッドの呼び出し
    ;; 良い
    (.substring "hello" 1 3)

    ;; 悪い
    (. "hello" substring 1 3)

    ;;; 静的フィールドへのアクセス
    ;; 良い
    Integer/MAX_VALUE

    ;; 悪い
    (. Integer MAX_VALUE)

    ;;; インスタンスフィールドへのアクセス
    ;; 良い
    (.someField some-object)

    ;; 悪い
    (. some-object someField)
    ```

* <a name="compact-metadata-notation-for-true-flags"></a>
  キーがキーワード、値がブール値`true`のスロットしか持たないメタデータには、簡易メタデータ表記を使う。
<sup>[[リンク](#compact-metadata-notation-for-true-flags)]</sup>

    ```Clojure
    ;; 良い
    (def ^:private a 5)

    ;; 悪い
    (def ^{:private true} a 5)
    ```

* <a name="private"></a>
  コード中のプライベート部分には印を付ける。
<sup>[[リンク](#private)]</sup>

    ```Clojure
    ;; 良い
    (defn- private-fun [] ...)

    (def ^:private private-var ...)

    ;; 悪い
    (defn private-fun [] ...) ; 全くプライベートでない

    (defn ^:private private-fun [] ...) ; 冗長な記述だ

    (def private-var ...) ; 全くプライベートでない
    ```

* <a name="access-private-var"></a>
  （例えばテストのために）プライベートなvarにアクセスするには、`@#'some.ns/var`フォームを使う。
<sup>[[リンク](#access-private-var)]</sup>

* <a name="attach-metadata-carefully"></a>
  メタデータを何に付加するかについては、よく注意したほうが良い。
<sup>[[リンク](#attach-metadata-carefully)]</sup>

    ```Clojure
    ;; `a`で参照されるvarにメタデータを付加している
    (def ^:private a {})
    (meta a) ;=> nil
    (meta #'a) ;=> {:private true}

    ;; 空のハッシュマップ値にメタデータを付加している
    (def a ^:private {})
    (meta a) ;=> {:private true}
    (meta #'a) ;=> nil
    ```

## 命名規約

> プログラミングで本当に難しいのは、キャッシュの無効化と命名の仕方だけだ。<br/>
> -- Phil Karlton

* <a name="ns-naming-schemas"></a>
  名前空間は次の2つの名づけ方が好ましい。
<sup>[[リンク](#ns-naming-schemas)]</sup>

    * `project.module`
    * `organization.project.module`

* <a name="lisp-case-ns"></a>
  複数単語からなる名前空間セグメントには`lisp-case`を使う（例：`bruce.project-euler`）
<sup>[[リンク](#lisp-case-ns)]</sup>

* <a name="lisp-case"></a>
  関数名や変数名には`lisp-case`を使う。
<sup>[[リンク](#lisp-case)]</sup>

    ```Clojure
    ;; 良い
    (def some-var ...)
    (defn some-fun ...)

    ;; 悪い
    (def someVar ...)
    (defn somefun ...)
    (def some_fun ...)
    ```

* <a name="CamelCase-for-protocols-records-structs-and-types"></a>
  プロトコル、レコード、構造体、型には`CamelCase`を用いる。（HTTP, RFC, XMLのような頭字語は大文字を保持する。）
<sup>[[リンク](#CamelCase-for-protocols-records-structs-and-types)]</sup>

* <a name="pred-with-question-mark"></a>
  述語（ブール値を返す関数）の名前はクエスチョンマーク（?）で終わるべきだ。（例：`even?`）
<sup>[[リンク](#pred-with-question-mark)]</sup>

    ```Clojure
    ;; 良い
    (defn palindrome? ...)

    ;; 悪い
    (defn palindrome-p ...) ; Common Lispスタイル
    (defn is-palindrome ...) ; Javaスタイル
    ```

* <a name="changing-state-fns-with-exclamation-mark"></a>
  STMトランザクションの中で安全でない関数・マクロの名前はエクスクラメーションマーク（!）で終わるべきだ。（例：`reset!`）
<sup>[[リンク](#changing-state-fns-with-exclamation-mark)]</sup>

* <a name="arrow-instead-of-to"></a>
  変換のための関数名には`to`ではなく`->`を用いる。
<sup>[[リンク](#arrow-instead-of-to)]</sup>

    ```Clojure
    ;; 良い
    (defn f->c ...)

    ;; あまり良くない
    (defn f-to-c ...)
    ```

* <a name="earmuffs-for-dynamic-vars"></a>
  再束縛を想定しているものには`*earmuffs*`を使う（つまりdynamicなものだ）。
<sup>[[リンク](#earmuffs-for-dynamic-vars)]</sup>

    ```Clojure
    ;; 良い
    (def ^:dynamic *a* 10)

    ;; 悪い
    (def ^:dynamic a 10)
    ```

* <a name="dont-flag-constants"></a>
  定数のために特別な表記をしない。特定のものを除いて、全ては定数である。
<sup>[[リンク](#dont-flag-constants)]</sup>

* <a name="underscore-for-unused-bindings"></a>
  分配束縛しても直後のコードで使われない変数名には`_`を使う。
<sup>[[リンク](#underscore-for-unused-bindings)]</sup>

    ```Clojure
    ;; 良い
    (let [[a b _ c] [1 2 3 4]]
      (println a b c))

    (dotimes [_ 3]
      (println "Hello!"))

    ;; 悪い
    (let [[a b c d] [1 2 3 4]]
      (println a b d))

    (dotimes [i 3]
      (println "Hello!"))
    ```

* <a name="idiomatic-names"></a>
  `pred`や`coll`のような慣用名には`clojure.core`の例が参考になる。
<sup>[[リンク](#idiomatic-names)]</sup>

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

* <a name="avoid-lists"></a>
  汎用的なデータ置き場としてリストを使うことを避ける（リストが本当に必要な場合を除く）。
<sup>[[リンク](#avoid-lists)]</sup>

* <a name="keywords-for-hash-keys"></a>
  マップのキーにはキーワードを用いたほうが良い。
<sup>[[リンク](#keywords-for-hash-keys)]</sup>

    ```Clojure
    ;; 良い
    {:name "Bruce" :age 30}

    ;; 悪い
    {"name" "Bruce" "age" 30}
    ```

* <a name="literal-col-syntax"></a>
  可能なら、コレクションのリテラル構文を用いたほうが良い。ただしセットを定義するときは、コンパイル時に定数である値についてのみリテラル構文を使用する。
<sup>[[リンク](#literal-col-syntax)]</sup>

    ```Clojure
    ;; 良い
    [1 2 3]
    #{1 2 3}
    (hash-set (func1) (func2)) ; 実行時に決定する値

    ;; 悪い
    (vector 1 2 3)
    (hash-set 1 2 3)
    #{(func1) (func2)} ; もし (func1) = (func2) だったら実行時例外が投げられる
    ```

* <a name="avoid-index-based-coll-access"></a>
  可能なら、コレクションの要素にインデックスでアクセスすることを避ける。
<sup>[[リンク](#avoid-index-based-coll-access)]</sup>

* <a name="keywords-as-fn-to-get-map-values"></a>
  可能なら、マップから値を取得する関数としてキーワードを用いるのが良い。
<sup>[[リンク](#keywords-as-fn-to-get-map-values)]</sup>

    ```Clojure
    (def m {:name "Bruce" :age 30})

    ;; 良い
    (:name m)

    ;; 必要以上の記述だ
    (get m :name)

    ;; 悪い - NullPointerExceptionが発生する可能性が高い
    (m :name)
    ```

* <a name="colls-as-fns"></a>
  ほとんどのコレクションはその要素の関数であることを活用する。
<sup>[[リンク](#colls-as-fns)]</sup>

    ```Clojure
    ;; 良い
    (filter #{\a \e \o \i \u} "this is a test")

    ;; 悪い - 汚すぎて書けない
    ```

* <a name="keywords-as-fns"></a>
  キーワードはコレクションの関数として使えることを活用する。
<sup>[[リンク](#keywords-as-fns)]</sup>

    ```Clojure
    ((juxt :a :b) {:a "ala" :b "bala"})
    ```

* <a name="avoid-transient-colls"></a>
  パフォーマンス問題がクリティカルとなる部分を除いて、一時的（transient）コレクションの使用を避ける。
<sup>[[リンク](#avoid-transient-colls)]</sup>

* <a name="avoid-java-colls"></a>
  Javaのコレクションの使用を避ける。
<sup>[[リンク](#avoid-java-colls)]</sup>

* <a name="avoid-java-arrays"></a>
  Java呼び出しや、プリミティブ型を多用するパフォーマンスクリティカルなコードを除いて、Javaの配列の使用を避ける。
<sup>[[リンク](#avoid-java-arrays)]</sup>

## 状態

### ref

* <a name="refs-io-macro"></a>
  トランザクションの中で思いがけずI/Oコールを呼んでしまったときの問題を回避するため、全てのI/Oコールを`io!`マクロでラップすることを考える。
<sup>[[リンク](#refs-io-macro)]</sup>

* <a name="refs-avoid-ref-set"></a>
  出来る限り`ref-set`は使用しない。
<sup>[[リンク](#refs-avoid-ref-set)]</sup>

    ```Clojure
    (def r (ref 0))

    ;; 良い
    (dosync (alter r + 5))

    ;; 悪い
    (dosync (ref-set r 5))
    ```

* <a name="refs-small-transactions"></a>
  トランザクションのサイズ（包んでいる処理の量）を出来る限り小さく保つようにする。
<sup>[[リンク](#refs-small-transactions)]</sup>

* <a name="refs-avoid-short-long-transactions-with-same-ref"></a>
  同一のrefとやり取りを行う、短期のトランザクションと長期のトランザクションを両方持つことを避ける。
<sup>[[リンク](#refs-avoid-short-long-transactions-with-same-ref)]</sup>

### エージェント

* <a name="agents-send"></a>
  それがCPUバウンドで、かつI/Oや他スレッドをブロックしない処理のときだけ`send`を用いる。
<sup>[[リンク](#agents-send)]</sup>

* <a name="agents-send-off"></a>
  それがスレッドをブロック、スリープさせたり、そうでなくても停滞させるかもしれない処理には`send-off`を用いる。
<sup>[[リンク](#agents-send-off)]</sup>

### アトム

* <a name="atoms-no-update-within-transactions"></a>
  STMトランザクションの中でアトムを更新することを避ける。
<sup>[[リンク](#atoms-no-update-within-transactions)]</sup>

* <a name="atoms-prefer-swap-over-reset"></a>
  可能なら、`reset!`よりも`swap!`を使うようにする。
<sup>[[リンク](#atoms-prefer-swap-over-reset)]</sup>

    ```Clojure
    (def a (atom 0))

    ;; 良い
    (swap! a + 5)

    ;; あまり良くない
    (reset! a 5)
    ```

## 文字列

* <a name="prefer-clojure-string-over-interop"></a>
  文字列処理は、Java呼び出しや独自実装よりも、`clojure.string`の関数を使うほうが好ましい。
<sup>[[リンク](#prefer-clojure-string-over-interop)]</sup>

    ```Clojure
    ;; 良い
    (clojure.string/upper-case "bruce")

    ;; 悪い
    (.toUpperCase "bruce")
    ```

## 例外

* <a name="reuse-existing-exception-types"></a>
  既存の例外型を再利用する。慣用的なClojureコードでは、例外を投げるとき、基本的な例外型を用いている。
  （例： `java.lang.IllegalArgumentException`,
  `java.lang.UnsupportedOperationException`,
  `java.lang.IllegalStateException`, `java.io.IOException`）。
<sup>[[リンク](#reuse-existing-exception-types)]</sup>

* <a name="prefer-with-open-over-finally"></a>
  `finally`よりも`with-open`のほうが好ましい。
<sup>[[リンク](#prefer-with-open-over-finally)]</sup>

## マクロ

* <a name="dont-write-macro-if-fn-will-do"></a>
  その処理が関数でできるならマクロを書かない。
<sup>[[リンク](#dont-write-macro-if-fn-will-do)]</sup>

* <a name="write-macro-usage-before-writing-the-macro"></a>
  まずマクロの使用例を作成し、その後でマクロを作る。
<sup>[[リンク](#write-macro-usage-before-writing-the-macro)]</sup>

* <a name="break-complicated-macros"></a>
  可能なら、複雑なマクロはより小さい機能に分割する。
<sup>[[リンク](#break-complicated-macros)]</sup>

* <a name="macros-as-syntactic-sugar"></a>
  マクロは通常、構文糖衣を提供するものであるべきで、そのコアは単純な機能であるべきだ。そうすることでより構造化されるだろう。
<sup>[[リンク](#macros-as-syntactic-sugar)]</sup>

* <a name="syntax-quoted-forms"></a>
  自分でリストを組み立てるよりも、構文クオートを使用するほうが好ましい。
<sup>[[リンク](#syntax-quoted-forms)]</sup>

## コメント

> 良いコードとは、それ自体が最良のドキュメントになっているものだ。コメントを付けようとしたとき、自分の胸に聞いてみるといい、「どうやってコードを改良して、このコメントを不要にできるだろうか？」ってね。より美しくするために、コードを改良してからドキュメント化するんだ。<br/>
> -- Steve McConnell

* <a name="self-documenting-code"></a>
  出来る限り、コードを見れば何をしているのか分かるように努める。
<sup>[[リンク](#self-documenting-code)]</sup>

* <a name="four-semicolons-for-heading-comments"></a>
  ヘッダーコメントには最低4つのセミコロンを用いる。
<sup>[[リンク](#four-semicolons-for-heading-comments)]</sup>

* <a name="three-semicolons-for-top-level-comments"></a>
  トップレベルのコメントには3つのセミコロンを用いる。
<sup>[[リンク](#three-semicolons-for-top-level-comments)]</sup>

* <a name="two-semicolons-for-code-fragment"></a>
  特定のコード部分の直前にコメントを書くときは、コード部分とインデントを揃え、2つのセミコロンを用いる。
<sup>[[リンク](#two-semicolons-for-code-fragment)]</sup>

* <a name="one-semicolon-for-margin-comments"></a>
  行末コメントには1つのセミコロンを用いる。
<sup>[[リンク](#one-semicolon-for-margin-comments)]</sup>

* <a name="semicolon-space"></a>
  セミコロンとテキストの間には最低1つのスペースを入れる。
<sup>[[リンク](#semicolon-space)]</sup>

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

* <a name="english-syntax"></a>
  2単語以上のコメントは大文字で始め、句読点を用いる。各文は[1つのスペース](http://en.wikipedia.org/wiki/Sentence_spacing)で分ける。
<sup>[[リンク](#english-syntax)]</sup>

* <a name="no-superfluous-comments"></a>
  無意味なコメントを避ける。
<sup>[[リンク](#no-superfluous-comments)]</sup>

    ```Clojure
    ;; 悪い
    (inc counter) ; increments counter by one
    ```

* <a name="comment-upkeep"></a>
  コメントは常に更新していなければならない。古いコメントは、コメントがないことよりも害悪だ。
<sup>[[リンク](#comment-upkeep)]</sup>

* <a name="dash-underscore-reader-macro"></a>
  特定のフォームをコメントアウトする必要があるときは、通常のコメントではなく`#_`リーダマクロを用いたほうが良い。
<sup>[[リンク](#dash-underscore-reader-macro)]</sup>

    ```Clojure
    ;; 良い
    (+ foo #_(bar x) delta)

    ;; 悪い
    (+ foo
       ;; (bar x)
       delta)
    ```

> 良いコードというのは面白いジョークのようなものだ。説明する必要がない。<br/>
> -- Russ Olsen

* <a name="refactor-dont-comment"></a>
  悪いコードを説明するためにコメントを書くことを避ける。コードをリファクタリングして、コメントが不要なようにするべきだ。（「やるか、やらないだ。やってみるではない」--Yoda）
<sup>[[リンク](#refactor-dont-comment)]</sup>

### コメントアノテーション

* <a name="annotate-above"></a>
  アノテーションは通常、当該コードの直前に書かれるべきだ。
<sup>[[リンク](#annotate-above)]</sup>

* <a name="annotate-keywords"></a>
  アノテーションキーワードの後にはコロンとスペースを入れ、その後で詳細を書く。
<sup>[[リンク](#annotate-keywords)]</sup>

* <a name="indent-annotations"></a>
  詳細が複数行に渡る場合、2行目以降は1行目に合わせてインデントするべきだ。
<sup>[[リンク](#indent-annotations)]</sup>

* <a name="sing-and-date-annotations"></a>
  アノテーションには記述者のイニシャルと日付を入れる。そうすればその妥当性を容易に示せる。
<sup>[[リンク](#sing-and-date-annotations)]</sup>

    ```Clojure
    (defn some-fun
      []
      ;; FIXME: This has crashed occasionally since v1.2.3. It may
      ;;        be related to the BarBazUtil upgrade. (xz 13-1-31)
      (baz))
    ```

* <a name="rare-eol-annotations"></a>
  ドキュメント化が不必要なほどに問題が明らかな箇所では、当該行の末尾に説明なしでアノテーションを付けても良い。この使用法は例外的であるべきで、規約ではない。
<sup>[[リンク](#rare-eol-annotations)]</sup>

    ```Clojure
    (defn bar
      []
      (sleep 100)) ; OPTIMIZE
    ```

* <a name="todo"></a>
  後日追加されるべき機能には`TODO`を使う。
<sup>[[リンク](#todo)]</sup>

* <a name="fixme"></a>
  コードが壊れていて、修正の必要がある箇所には`FIXME`を使う。
<sup>[[リンク](#fixme)]</sup>

* <a name="optimize"></a>
  パフォーマンス問題の原因となりうる、遅かったり非効率なコードには`OPTIMIZE`を使う。
<sup>[[リンク](#optimize)]</sup>

* <a name="hack"></a>
  疑わしいコーディングの仕方がされており、リファクタリングすべき「コード・スメル」には`HACK`を用いる。
<sup>[[リンク](#hack)]</sup>

* <a name="review"></a>
  意図するように動くかどうか確認すべき箇所には`REVIEW`を使う。例：`REVIEW: Are we sure this is how the client does X currently?`
<sup>[[リンク](#review)]</sup>

* <a name="document-annotations"></a>
  そのほうが適切だと思えば、その他独自のアノテーションキーワードを用いる。ただし、プロジェクトの`README`などに忘れずにドキュメント化しておく。
<sup>[[リンク](#document-annotations)]</sup>

## 実際のコードでは

* <a name="be-functional"></a>
  関数型的にコードを書き、そのほうが適切なときのみミュータブルにする。
<sup>[[リンク](#be-functional)]</sup>

* <a name="be-consistent"></a>
  一貫させる。理想的には、このガイドの通りにする。
<sup>[[リンク](#be-consistent)]</sup>

* <a name="common-sense"></a>
  常識的に考える。
<sup>[[リンク](#common-sense)]</sup>

## ツール

慣用的なClojureコードを書くのを助けてくれるツールがClojureコミュニティによって作られている。

* [Slamhound](https://github.com/technomancy/slamhound)は既存のコードから適切な`ns`定義を自動的に生成してくれる。
* [kibit](https://github.com/jonase/kibit)はClojure向けの静的コード解析ツールだ。より慣用的な関数やマクロの探索には[core.logic](https://github.com/clojure/core.logic)を用いている。

# 広めてください

コミュニティドリブンのスタイルガイドは、その存在を知らないコミュニティではあまり役に立ちません。どうか、このガイドについてツイートをして、あなたの友達や同僚と共有してください。頂いたあらゆるコメントや提案、意見がほんの少しずつ、このガイドを形作っていくのです。みんなで最高のスタイルガイドを作りましょう。

Cheers,<br/>
[Bozhidar](https://twitter.com/bbatsov)
