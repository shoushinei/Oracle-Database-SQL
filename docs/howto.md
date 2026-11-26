# 問題タイプ別の解き方（逆引き）

問題文の「聞き方」から、どの判定基準を使えばよいかを引くページです。
間違えた問題の聞き方に近いものを探し、リンク先の論点を読んでください。

---

## 共通の進め方

!!! abstract "1問の処理手順"
    1. **問題文の条件を確認する**（NLS_DATE_FORMAT、NOT NULL制約、表のデータ、「2つ選択」など）
    2. **選択肢を1つずつ、判定基準に照らす**（「なんとなく正しそう」で選ばない）
    3. **「選択数」を使う** … 2つ選択なら、確実な1つ＋残りの比較で決める
    4. **最後に問題文に戻る** … 「エラーにならない」ものを聞いているのか、「目的の結果を返す」ものを聞いているのかを再確認

!!! warning "「エラーにならない」と「目的どおりの結果になる」は別物"
    `WHERE commission_pct = NULL` は **エラーにはならない** が、1行も返らない。
    「正常に実行される問合せ」を聞かれたら ✅、「NULLの行を表示する問合せ」を聞かれたら ❌ です。
    このテキストでは、このタイプを ⚠️ で示しています。

!!! tip "知識問題の断定語に注意"
    「**必ず**」「**常に**」「**すべて**」「**〜のみ**」「**絶対に〜できない**」を含む選択肢は、例外が1つあれば誤り。
    例外を知っているかが問われています。各章末の **○×クイック判定** で例外を確認できます。

---

## 「正常に実行される」「構文エラーにならない」SQLはどれか { #syntax }

選択肢のSQLを、文の種類ごとに次の順でチェックします。上から順に「よくある誤り」です。

### SELECT文

| # | チェック項目 | 判定基準へ |
|---|---|---|
| 1 | 句の順序は `SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY` か | [2章 句の順序](ch02.md#select-syntax) |
| 2 | 列別名は正しいか（単一引用符は不可、記号・空白・予約語は二重引用符） | [2章 列別名](ch02.md#alias) |
| 3 | `DISTINCT` は `SELECT` の直後に1回だけか | [2章 DISTINCT](ch02.md#distinct) |
| 4 | `IS NOT NULL` の語順は正しいか（`NOT IS NULL` は不可） | [3章 NULL条件](ch03.md#null-condition) |
| 5 | 関数の引数の数・型は正しいか（`CONCAT`は2引数、`NULLIF`の第1引数にNULLリテラル不可…） | [4章](ch04.md#concat) / [5章 NULLIF](ch05.md#nullif) |
| 6 | 日付の演算は許される形か（日付＋日付、日付×数値は不可） | [4章 日付演算](ch04.md#date-arithmetic) |
| 7 | 集計関数を `WHERE` に書いていないか | [6章 WHEREとHAVING](ch06.md#having) |
| 8 | `GROUP BY` に含まれない列を `SELECT` に書いていないか | [6章 GROUP BYのルール](ch06.md#group-by) |
| 9 | 集計関数のネストに `GROUP BY` があり、`SELECT` に単独の列がないか | [6章 集計関数のネスト](ch06.md#nesting) |
| 10 | `USING`／`NATURAL JOIN` の結合列に表接頭辞を付けていないか | [7章 USING](ch07.md#using) / [7章 NATURAL](ch07.md#natural) |
| 11 | 表別名を付けたのに表名で修飾していないか／列があいまいでないか | [7章 表接頭辞](ch07.md#prefix) |
| 12 | 単一行演算子（`=` など）に複数行を返す副問合せを使っていないか | [8章 単一行副問合せ](ch08.md#single-row) |
| 13 | 集合演算で列数・データ型グループが一致し、`ORDER BY` は最後に1回か | [9章 列数とデータ型](ch09.md#rules) / [9章 ORDER BY](ch09.md#order-by) |

### CREATE TABLE / ALTER TABLE

| # | チェック項目 | 判定基準へ |
|---|---|---|
| 1 | 表名・列名は命名規則に合うか | [12章 命名規則](ch12.md#naming) |
| 2 | `VARCHAR2` にサイズがあるか、`NUMBER(p,s)` の範囲は正しいか | [2章 データ型](ch02.md#datatypes) |
| 3 | `DEFAULT` は制約より前か、他の列を参照していないか | [12章 DEFAULT](ch12.md#default) |
| 4 | `NOT NULL` を表制約として書いていないか | [12章 NOT NULL](ch12.md#not-null) |
| 5 | 列制約の `CHECK` が他の列を参照していないか | [12章 CHECK](ch12.md#check) |
| 6 | 既存データと矛盾する `ALTER TABLE` ではないか（NOT NULL追加、精度の縮小） | [12章 MODIFY](ch12.md#alter-modify) / [12章 ADD](ch12.md#alter-add) |
| 7 | `CREATE TABLE AS SELECT` の式に別名があるか | [12章 CTAS](ch12.md#ctas) |

### DML / MERGE / シーケンス

| # | チェック項目 | 判定基準へ |
|---|---|---|
| 1 | `INSERT` の列数と値の数は一致するか | [10章 INSERT](ch10.md#insert) |
| 2 | `UPDATE ... SET (列, 列) = (副問合せ)` の副問合せは1行か | [10章 UPDATE](ch10.md#update) |
| 3 | マルチテーブルINSERTの挿入先がビューではないか | [10章 マルチテーブルINSERT](ch10.md#multi-insert) |
| 4 | `MERGE` に `ON (...)` があり、`ON` の列を `UPDATE` していないか | [10章 MERGE](ch10.md#merge) |
| 5 | `NEXTVAL` を使えない場所（DISTINCT、GROUP BY、集合演算、副問合せ…）で使っていないか | [11章 NEXTVALが使える場所](ch11.md#nextval-where) |

### GRANT / REVOKE

| # | チェック項目 | 判定基準へ |
|---|---|---|
| 1 | システム権限とオブジェクト権限を1つのGRANTに混在させていないか | [14章 GRANT構文](ch14.md#grant-syntax) |
| 2 | `WITH GRANT OPTION` の付与先がロールではないか | [14章 GRANT構文](ch14.md#grant-syntax) |
| 3 | 列指定できない権限（SELECTなど）で列を指定していないか | [14章 列レベル権限](ch14.md#column-priv) |
| 4 | そのオブジェクトに存在しない権限を付与していないか（シーケンスにREFERENCES など） | [14章 権限の種類](ch14.md#sys-obj-priv) |

---

## 「何行返されますか」「どの結果が返されますか」 { #result }

| 何が絡むか | 押さえる判定基準 |
|---|---|
| `COUNT(*)` / `COUNT(列)` / `COUNT(DISTINCT 列)` | [6章 COUNTの3形態](ch06.md#count) |
| 集計とNULL（AVGの分母など） | [6章 NULLの扱い](ch06.md#agg-null) |
| GROUP BYの有無と行数 | [6章 返る行数](ch06.md#rows) |
| クロス結合・内部結合・外部結合の行数 | [7章 結合の種類と行数](ch07.md#join-types) |
| UNION / UNION ALL / INTERSECT / MINUS の行数 | [9章 行数の数え方](ch09.md#count-rows) |
| 文字関数の結果（TRIM、REPLACE、SUBSTR、INSTR） | [4章 SUBSTR/INSTR](ch04.md#substr-instr) / [4章 TRIM/REPLACE](ch04.md#trim-replace) |
| 数値の丸め（ROUND/TRUNC、NUMBER(p,s)への格納） | [4章 数値関数](ch04.md#number-functions) / [2章 NUMBER(p,s)](ch02.md#number-ps) |
| 関数のネスト | [4章 ネストの評価順](ch04.md#nesting) |
| NULLのソート位置 | [3章 NULLのソート位置](ch03.md#nulls-order) |
| `ALL` / `ANY` 副問合せの意味 | [8章 ANY/ALL](ch08.md#multi-row) |
| 現在日時関数の結果と表示形式 | [16章 現在日時関数](ch16.md#tz-functions) |
| 置換変数の入力回数 | [3章 置換変数](ch03.md#substitution) |

---

## 「〜したい。どの問合せを使用しますか」 { #purpose }

「エラーにならない」だけでは足りず、**目的どおりの結果** が必要です。

| 目的 | 押さえる判定基準 |
|---|---|
| NULLの行／NULLでない行を探す | [3章 NULL条件](ch03.md#null-condition) |
| 「n文字目が〜」「〜を含む」 | [3章 LIKE](ch03.md#like) |
| 大文字小文字を問わず検索 | [4章 文字関数](ch04.md#char-functions) |
| 「過去nか月以内」「日数を計算」 | [4章 日付関数](ch04.md#date-functions) / [5章 TO_DATE](ch05.md#to-date) |
| 「全体の平均より高い人」「一番古い人」 | [8章 副問合せが必要なとき](ch08.md#need-subquery) |
| 部門ごとに重複しない組み合わせ | [2章 DISTINCT](ch02.md#distinct) |
| 「Aにあって、Bにない」 | [9章 集合演算子の種類](ch09.md#operators) |
| 0行でも複数行でもエラーにならない削除・更新 | [8章 DMLでの副問合せ](ch08.md#dml-subquery) |

---

## 「〜について正しい記述はどれですか」 { #knowledge }

各章末の **○×クイック判定** が対応します。特に出題が多いのは次のテーマです。

- SQL・E-Rモデル・キー → [1章](ch01.md#quiz)
- 列別名、DESCRIBE、DUAL、演算子 → [2章](ch02.md#quiz)
- 単一行関数の性質 → [4章](ch04.md#quiz)
- 結合の種類 → [7章](ch07.md#quiz)
- 集合演算の性質 → [9章](ch09.md#quiz)
- 索引・シーケンス・シノニム → [11章](ch11.md#quiz)
- 制約、列の削除・未使用化、TRUNCATE、一時表、外部表 → [12章](ch12.md#quiz)
- ビュー → [13章](ch13.md#quiz)
- 権限・ロール → [14章](ch14.md#quiz)
- データディクショナリ → [15章](ch15.md#quiz)
- 日時・期間データ型 → [16章](ch16.md#quiz)

---

## 「実行後の状態はどれですか」（トランザクション） { #transaction }

1. **DDL（CREATE / ALTER / DROP / TRUNCATE / GRANT…）が出てきたら、その直前でコミット** されたとみなす
2. `ROLLBACK TO セーブポイント` は **そのセーブポイント以降だけ** を取り消す（トランザクションは続く）
3. 最後に `COMMIT` / `ROLLBACK` / 終了のどれで終わったかを見る

→ [10章 トランザクションの開始と終了](ch10.md#tx-boundary) / [10章 SAVEPOINT](ch10.md#savepoint)

---

## 「権限が必要な操作はどれですか」 { #privilege }

1. **自分のスキーマの中で完結する操作か？** → 多くは権限不要（ただし「作成」にはCREATE系システム権限が必要）
2. **他人の特定のオブジェクトを使う操作か？** → オブジェクト権限
3. **他人のスキーマ全体・データベース全体に対する操作か？**（`ANY`） → システム権限

→ [14章 どの権限が必要か](ch14.md#need-which) / [14章 権限不要の操作](ch14.md#no-priv)
