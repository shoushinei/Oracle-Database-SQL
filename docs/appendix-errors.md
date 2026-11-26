# 付録 ORAエラー逆引き

問題の解説や自分で試したときに出たエラー番号から、原因の論点を引くための表です。
（試験でエラー番号そのものを問われることはほぼありませんが、「なぜエラーになるか」の理解に役立ちます）

---

## SELECT・式・関数

| エラー | 意味 | よくある原因 | 論点 |
|---|---|---|---|
| ORA-00904 | 無効な識別子 | WHEREで列別名を使った／表別名を付けたのに表名で修飾／`"文字"` を文字列のつもりで使った | [2章 列別名](ch02.md#alias) / [7章 表接頭辞](ch07.md#prefix) |
| ORA-00918 | 列の定義があいまい | 結合で両方の表にある列を修飾していない | [7章 表接頭辞](ch07.md#prefix) |
| ORA-00923 | FROMキーワードが指定の位置にない | 列別名に `'...'`・`-`・空白・予約語、`SELECT *, 列` | [2章 列別名](ch02.md#alias) |
| ORA-00936 | 式がない | `SELECT a, DISTINCT b` など | [2章 DISTINCT](ch02.md#distinct) |
| ORA-00909 | 引数の個数が無効 | `CONCAT` に3つの引数 | [4章 CONCAT](ch04.md#concat) |
| ORA-30001 | トリム・セットは1文字のみ | `TRIM('ab' FROM ...)` | [4章 TRIM](ch04.md#trim-replace) |
| ORA-00932 | データ型が一致しない | `NULLIF(数値, 文字)`、`NULLIF(NULL, ...)`、`COALESCE`・`CASE` の型混在、`日付 * 数値` | [5章 NULLIF](ch05.md#nullif) / [5章 CASE](ch05.md#case) |
| ORA-00975 | 日付＋日付は使用できない | `hire_date + SYSDATE` | [4章 日付演算](ch04.md#date-arithmetic) |
| ORA-01476 | 除数がゼロ | `x / 0` | [2章 算術演算子](ch02.md#arithmetic) |
| ORA-01722 | 数値が無効 | 数値にできない文字の暗黙変換（`NVL(数値列, 'なし')` など） | [5章 暗黙の変換](ch05.md#implicit) / [5章 NVL](ch05.md#nvl) |
| ORA-01843 | 無効な月 | `DD-MON-RR` に `'31-03-2005'` | [5章 一致ルール](ch05.md#format-match) |
| ORA-01861 | リテラルが書式文字列と一致しない | TO_DATEの文字列と書式が合わない | [5章 TO_DATE](ch05.md#to-date) |
| ORA-01785 | ORDER BYの項目はSELECTリストの番号でなければならない | 列数を超える位置番号 | [3章 ORDER BY](ch03.md#order-by) |
| ORA-01791 | SELECT式ではない | DISTINCT使用時にSELECTにない列でORDER BY | [2章 DISTINCT](ch02.md#distinct) |

## 集計・GROUP BY

| エラー | 意味 | よくある原因 | 論点 |
|---|---|---|---|
| ORA-00934 | グループ関数は使用できない | WHEREに集計関数 | [6章 WHEREとHAVING](ch06.md#having) |
| ORA-00937 | 単一グループのグループ関数ではない | GROUP BYなしで集計関数と列を並べた／ネストした集計関数と列を並べた | [6章 GROUP BY](ch06.md#group-by) / [6章 ネスト](ch06.md#nesting) |
| ORA-00979 | GROUP BYの式ではない | SELECTの非集計列がGROUP BYにない | [6章 GROUP BY](ch06.md#group-by) |
| ORA-00978 | GROUP BYなしのネストしたグループ関数 | `MAX(AVG(...))` にGROUP BYがない | [6章 ネスト](ch06.md#nesting) |

## 結合・副問合せ・集合演算

| エラー | 意味 | よくある原因 | 論点 |
|---|---|---|---|
| ORA-25154 | USING句の列には修飾子を付けられない | `USING (department_id)` の列を `e.department_id` と書いた | [7章 USING](ch07.md#using) |
| ORA-25155 | NATURAL結合の列には修飾子を付けられない | NATURAL JOINの共通列を修飾 | [7章 NATURAL](ch07.md#natural) |
| ORA-01468 | 外部結合できる表は1つのみ | `(+)` を両側に付けた | [7章 (+)](ch07.md#oracle-outer) |
| ORA-01719 | (+)はORやINのオペランドで使用できない | `(+)` を OR で結んだ | [7章 (+)](ch07.md#oracle-outer) |
| ORA-01427 | 単一行副問合せで複数行が返された | `=` などに複数行の副問合せ | [8章 単一行副問合せ](ch08.md#single-row) |
| ORA-01789 | 結果列の数が正しくない | 集合演算の列数が違う | [9章 列数とデータ型](ch09.md#rules) |
| ORA-01790 | 対応する式と同じデータ型にする必要がある | 集合演算で数値と文字が対応 | [9章 列数とデータ型](ch09.md#rules) |
| ORA-00933 / ORA-00907 | SQLコマンドが正しく終了していない／右括弧がない | 集合演算の途中にORDER BY、WHERE句の副問合せにORDER BY | [9章 ORDER BY](ch09.md#order-by) / [8章](ch08.md#where-used) |

## DML・トランザクション

| エラー | 意味 | よくある原因 | 論点 |
|---|---|---|---|
| ORA-00913 / ORA-00947 | 値が多すぎる／不足している | INSERTの列数と値の数が違う | [10章 INSERT](ch10.md#insert) |
| ORA-01400 | NULLは挿入できない | NOT NULL列を省略（DEFAULTなし） | [10章 INSERT](ch10.md#insert) |
| ORA-01438 | 指定精度を超える値 | NUMBER(p,s)の整数部があふれた | [2章 NUMBER(p,s)](ch02.md#number-ps) |
| ORA-02291 | 親キーがない | 子に親にない値を挿入 | [12章 FK](ch12.md#fk) |
| ORA-02292 | 子レコードがある | 参照されている親の行を削除 | [12章 FK](ch12.md#fk) |
| ORA-38104 | ON句で参照される列は更新できない | MERGEでON句の列をUPDATE | [10章 MERGE](ch10.md#merge) |
| ORA-01086 | セーブポイントが設定されていない | 存在しない（またはコミットで消えた）セーブポイントへROLLBACK TO | [10章 SAVEPOINT](ch10.md#savepoint) |
| ORA-01402 | ビューのWITH CHECK OPTIONのWHERE句違反 | ビューの条件から外れる行を作るDML | [13章 WITH CHECK OPTION](ch13.md#check-option) |

## DDL・制約・オブジェクト

| エラー | 意味 | よくある原因 | 論点 |
|---|---|---|---|
| ORA-00984 | ここでは列は使用できない | DEFAULTで他の列を参照 | [12章 DEFAULT](ch12.md#default) |
| ORA-00998 | この式は列の別名で指定する必要がある | CTAS・ビューの式に別名がない | [12章 CTAS](ch12.md#ctas) / [13章](ch13.md#create-view) |
| ORA-02438 | 列チェック制約は他の列を参照できない | 列制約のCHECKで他の列 | [12章 CHECK](ch12.md#check) |
| ORA-02436 | CHECK制約の日付またはシステム変数の指定が不正 | CHECKでSYSDATE、不完全な日付 | [12章 CHECK](ch12.md#check) |
| ORA-01758 | 必須列を追加するには表が空である必要がある | データのある表にNOT NULL列（DEFAULTなし）を追加 | [12章 ADD](ch12.md#alter-add) |
| ORA-02296 | 使用可能にできない - NULL値が見つかった | NULLのある列にNOT NULLを付けた | [12章 MODIFY](ch12.md#alter-modify) |
| ORA-01440 | 精度・位取りを減らすには列が空である必要がある | データのある数値列の精度を縮小 | [12章 MODIFY](ch12.md#alter-modify) |
| ORA-01441 | 値が大きすぎるため列の長さを減らせない | 既存の値より短くした | [12章 MODIFY](ch12.md#alter-modify) |
| ORA-02297 | 制約を使用禁止にできない - 依存性が存在 | 参照されている主キーをCASCADEなしで無効化 | [12章 制約の状態](ch12.md#constraint-state) |
| ORA-02266 | 有効な外部キーで参照される一意／主キーが表にある | 親表をTRUNCATE | [12章 DROP/TRUNCATE](ch12.md#drop-truncate) |
| ORA-02449 | 外部キーにより参照される一意／主キーが表にある | 親表をDROP（CASCADE CONSTRAINTSなし） | [12章 FK](ch12.md#fk) |
| ORA-02429 | 一意／主キーの施行に使用する索引は削除できない | 制約が使っている索引をDROP INDEX | [11章 索引の維持](ch11.md#index-maintain) |
| ORA-01702 | ビューはここでは適切ではない | ビューに索引を作成 | [13章](ch13.md#alter-drop-view) |
| ORA-00997 | LONGデータ型は使用できない | CTASでLONG列をコピー | [2章 データ型](ch02.md#datatypes) |
| ORA-08002 | 順序のCURRVALはこのセッションではまだ定義されていない | NEXTVALより先にCURRVAL | [11章 NEXTVAL/CURRVAL](ch11.md#nextval-currval) |
| ORA-02287 | 順序番号はここでは使用できない | DISTINCT・副問合せ・集合演算などでNEXTVAL | [11章 使える場所](ch11.md#nextval-where) |
| ORA-01873 | 期間の先行精度が小さすぎる | `INTERVAL '100' DAY` | [16章 期間リテラル](ch16.md#interval-literal) |
| ORA-01839 | 指定した月に対して日付が無効 | 1/31 + INTERVAL '1' MONTH | [16章 加減算](ch16.md#interval-arith) |

## 権限

| エラー | 意味 | よくある原因 | 論点 |
|---|---|---|---|
| ORA-00990 | 権限が不足しているか無効 | システム権限とオブジェクト権限の混在、存在しない権限名 | [14章 GRANT構文](ch14.md#grant-syntax) |
| ORA-01926 | WITH GRANT OPTIONでロールにGRANTできない | ロールにGRANT OPTION | [14章 GRANT構文](ch14.md#grant-syntax) |
| ORA-01927 | 付与していない権限はREVOKEできない | PUBLICに付与した権限を個人からREVOKE | [14章 PUBLIC](ch14.md#public) |
| ORA-00942 | 表またはビューが存在しない | 名前の誤り、または **権限がない**（存在しても見えない） | [14章](ch14.md#need-which) |
| ORA-01031 | 権限が不足している | 必要なシステム権限がない | [14章](ch14.md#need-which) |
| ORA-01950 | 表領域に対する権限がない | QUOTAが割り当てられていない | [14章 ユーザー](ch14.md#user) |
