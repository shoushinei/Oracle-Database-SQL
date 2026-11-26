# 付録 直前チェックリスト

試験直前に見直す「判定基準」の要約です。チェックボックスは自分の確認用に使ってください（ページを再読み込みすると元に戻ります）。
詳しい説明は各リンク先へ。

---

## SELECTの基本（2〜3章）

- [ ] 列別名：`'...'` は不可。記号・空白・予約語・大小保持は `"..."` → [2章](ch02.md#alias)
- [ ] 列別名は ORDER BY だけで使える（WHERE・GROUP BY・HAVINGは不可） → [2章](ch02.md#alias)
- [ ] `SELECT *, 列` は不可、`SELECT 表別名.*, 列` は可 → [2章](ch02.md#select-syntax)
- [ ] DISTINCT は SELECT直後に1回、効果はSELECTリスト全体の組合せ → [2章](ch02.md#distinct)
- [ ] NUMBER(p,s)：整数部 p−s 桁を超えるとエラー、小数部は四捨五入 → [2章](ch02.md#number-ps)
- [ ] DESCRIBE：列名・NULL?・型だけ。索引は見えない。ビューも可、SQL Developerでも可 → [2章](ch02.md#describe)
- [ ] DUAL：SYS所有、1列1行、全員使える → [2章](ch02.md#dual)
- [ ] 算術：`* /` が先、同順位は左から、変えられるのは `( )` だけ、NULLを含むとNULL → [2章](ch02.md#arithmetic)
- [ ] 連結 `||` のNULLは無視される → [2章](ch02.md#literal)
- [ ] `q'[...]'` の区切り文字は対応する括弧か同じ文字 → [2章](ch02.md#q-quote)
- [ ] `= NULL` はエラーにならないが0行、`NOT IS NULL` は構文エラー → [3章](ch03.md#null-condition)
- [ ] LIKE：`%`＝0文字以上、`_`＝ちょうど1文字。「以上」なら最後に `%` → [3章](ch03.md#like)
- [ ] `NOT IN` にNULLがあれば0行。`BETWEEN 大 AND 小` は0行 → [3章](ch03.md#in)
- [ ] 論理演算の優先順位 NOT → AND → OR → [3章](ch03.md#logical)
- [ ] 昇順でNULLは最後、降順で最初 → [3章](ch03.md#nulls-order)
- [ ] `&` は毎回、`&&` は1回（UNDEFINEまで保持）。SET VERIFY ON は置換前後を表示 → [3章](ch03.md#substitution)

## 関数（4〜5章）

- [ ] 単一行関数：引数0〜複数、戻り値の型は引数と同じとは限らない、どこでも使える → [4章](ch04.md#single-row)
- [ ] CONCATは2引数だけ、TRIMの削除文字は1文字だけ・両端のみ → [4章](ch04.md#concat)
- [ ] ROUND/TRUNCはDATEも取れる。負の桁は整数部。MOD(m,0)=m、POWERはべき乗 → [4章](ch04.md#number-functions)
- [ ] 日付±数値＝日付、日付−日付＝数値、日付＋日付・日付×数値は不可 → [4章](ch04.md#date-arithmetic)
- [ ] SYSDATEはDBサーバーOSの日時（時刻も持つ）。1文の中では同じ値 → [4章](ch04.md#date-arithmetic)
- [ ] MONTHS_BETWEEN(新しい日, 古い日) が正 → [4章](ch04.md#date-functions)
- [ ] 文字→日付：MON→MONTH、RR→RRRR は読み替えOK。MONの位置に数字は不可 → [5章](ch05.md#format-match)
- [ ] TO_CHARの固定文字は `"..."`、MIは分、桁あふれは `###` → [5章](ch05.md#to-char)
- [ ] NVL：第2引数は第1引数の型へ変換。NVL2：NULLでなければ第2、NULLなら第3 → [5章](ch05.md#nvl)
- [ ] NULLIF：第1引数にNULLリテラル不可、数値と文字の組合せ不可 → [5章](ch05.md#nullif)
- [ ] COALESCE・CASE：すべて同じ型グループ。単純CASEのWHENに条件は書けない → [5章](ch05.md#coalesce)
- [ ] DECODEはNULL同士が一致、CASEは一致しない → [5章](ch05.md#decode)

## 集計（6章）

- [ ] COUNT(*)は全行、COUNT(列)はNULL以外、COUNT(DISTINCT 列)はNULL以外の種類数 → [6章](ch06.md#count)
- [ ] AVGの分母はNULLでない行数 → [6章](ch06.md#agg-null)
- [ ] SELECTの非集計列はすべてGROUP BYに。GROUP BYに別名不可 → [6章](ch06.md#group-by)
- [ ] WHEREに集計関数は不可（HAVING） → [6章](ch06.md#having)
- [ ] 集計関数のネストは2レベルまで・GROUP BY必須・単独の列と併用不可 → [6章](ch06.md#nesting)
- [ ] GROUP BYなしの集計は常に1行 → [6章](ch06.md#rows)

## 結合・副問合せ・集合演算（7〜9章）

- [ ] USING・NATURALの結合列には修飾子を付けない。表別名を付けたら表名で修飾しない → [7章](ch07.md#using)
- [ ] Oracle構文に完全外部結合はない（(+)を両側に付けられない） → [7章](ch07.md#join-syntax)
- [ ] クロス結合の行数は掛け算。内部結合 ≦ 外部結合 → [7章](ch07.md#join-types)
- [ ] 自己結合は表別名必須、内部・外部・非等価どれでも可 → [7章](ch07.md#self)
- [ ] 単一行副問合せ：2行以上ならエラー、0行なら結果0行 → [8章](ch08.md#single-row)
- [ ] `< ALL`＝最小値より小さい、`> ANY`＝最小値より大きい → [8章](ch08.md#multi-row)
- [ ] 0行でも複数行でもエラーにならない削除は IN か EXISTS → [8章](ch08.md#dml-subquery)
- [ ] 「全体の集計値と個々の行を比べる」ときに副問合せが必要 → [8章](ch08.md#need-subquery)
- [ ] 集合演算：列数一致・型グループ一致・列名は最初のSELECT・ORDER BYは最後に1回 → [9章](ch09.md#rules)
- [ ] UNION ALL以外は重複除去・ソート。集合演算ではNULL同士は同じ → [9章](ch09.md#sort-dup)
- [ ] 「AにあってBにない」は `A MINUS B` → [9章](ch09.md#operators)

## DML・トランザクション（10章）

- [ ] UPDATEのSETに副問合せOK（複数列も可）。0行ならNULLで更新 → [10章](ch10.md#update)
- [ ] マルチテーブルINSERT：挿入先にビュー不可。ALLは該当する全表、FIRSTは最初の1つ、どれにも該当しなければELSE → [10章](ch10.md#multi-insert)
- [ ] MERGE：ON必須、USINGに副問合せ可、MATCHED→UPDATE、NOT MATCHED→INSERT → [10章](ch10.md#merge)
- [ ] コミット：COMMIT・DDL（TRUNCATE含む）・DCL・正常終了。ロールバック：ROLLBACK・異常終了・SHUTDOWN IMMEDIATE → [10章](ch10.md#tx-boundary)
- [ ] ROLLBACK TO：それ以降だけ取消し、トランザクションは継続、指定したセーブポイントは残る → [10章](ch10.md#savepoint)

## オブジェクト（11〜13章）

- [ ] 索引が自動作成されるのはPK・UNIQUEだけ（FKはなし） → [11章](ch11.md#index-auto)
- [ ] INVISIBLEはDMLで更新される、UNUSABLEは更新されない → [11章](ch11.md#unusable-invisible)
- [ ] NEXTVALで発番、CURRVALは発番後のみ。CYCLEで戻るのはMINVALUE。START WITHは変更不可 → [11章](ch11.md#sequence-create)
- [ ] シノニム：セグメントなし、参照先を消しても残る、PUBLICにはCREATE PUBLIC SYNONYM → [11章](ch11.md#synonym)
- [ ] DEFAULTは制約より前、他の列は参照不可 → [12章](ch12.md#default)
- [ ] NOT NULLは列制約だけ、列制約のCHECKは自分の列だけ → [12章](ch12.md#constraint-syntax)
- [ ] 参照されている主キーの無効化はCASCADEで外部キーも無効化 → [12章](ch12.md#constraint-state)
- [ ] データのある表：NOT NULL列追加はDEFAULTが必要、数値の精度縮小は不可、MODIFYのDEFAULTは既存NULLを埋めない → [12章](ch12.md#alter-modify)
- [ ] SET UNUSEDは戻せない・1000列に数える・領域はDROP UNUSED COLUMNSで解放 → [12章](ch12.md#alter-drop)
- [ ] 読取り専用表でもDROP TABLEはできる → [12章](ch12.md#read-only)
- [ ] TRUNCATE：DDL、WHERE不可、ロールバック不可、ごみ箱なし、索引も切り捨て → [12章](ch12.md#drop-truncate)
- [ ] CTAS：データとNOT NULLだけ引き継ぐ。式には別名 → [12章](ch12.md#ctas)
- [ ] GTT：定義は全セッションで共有・永続、データはセッション専用、ロールバック可、TRUNCATE可 → [12章](ch12.md#temp-table)
- [ ] 外部表：SELECTのみ。ORACLE_LOADER（既定）はロードのみ、ORACLE_DATAPUMPはアンロードも可 → [12章](ch12.md#external)
- [ ] ビュー：OR REPLACEで権限は残る、FORCEで実表なしでも作成、ALTER VIEWで定義は変えられない → [13章](ch13.md#create-view)
- [ ] WITH CHECK OPTIONは条件から外れるDMLだけ禁止、READ ONLYは全DML禁止 → [13章](ch13.md#check-option)

## 権限・ディクショナリ・日時（14〜16章）

- [ ] システム権限とオブジェクト権限を1つのGRANTに混在させない → [14章](ch14.md#grant-syntax)
- [ ] ロールにWITH GRANT OPTIONは不可（PUBLICは可）、WITH ADMIN OPTIONはロールにも可 → [14章](ch14.md#grant-syntax)
- [ ] 列指定できるのはINSERT・UPDATE・REFERENCES → [14章](ch14.md#column-priv)
- [ ] 取消しの連鎖：ADMIN OPTIONは連鎖しない、GRANT OPTIONは連鎖する → [14章](ch14.md#revoke)
- [ ] 自分のスキーマ内の操作は（作成以外）権限不要。他人の特定オブジェクト→オブジェクト権限、ANY→システム権限 → [14章](ch14.md#need-which)
- [ ] ロールは誰のスキーマにも属さない。ロールにロール、PUBLICにロールを付与できる → [14章](ch14.md#role)
- [ ] ディクショナリの実表はSYS所有・SYSTEM表領域・自動更新・直接変更禁止 → [15章](ch15.md#dictionary)
- [ ] USER_＝所有、ALL_＝アクセス可能（全員参照可）、DBA_＝全部。一覧はDICTIONARY → [15章](ch15.md#prefix)
- [ ] SYSで始まる関数はサーバー、CURRENT_/LOCALはセッションのタイムゾーン → [16章](ch16.md#tz-functions)
- [ ] TIMESTAMPは小数秒あり、WITH LOCAL TIME ZONEはDBのTZで格納しセッションTZで表示 → [16章](ch16.md#datetime-types)
- [ ] INTERVAL DAY TO SECONDは24時間超もOK、先頭フィールドの既定精度は2 → [16章](ch16.md#interval-types)
