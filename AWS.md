# AWSの基礎知識
##  AWSには様々な種類がある。
- Ectract Transform Load データエンジニアの領域
- redshift（データウェアハウス）
- lamda&glue(サーバーレスシステム)ETLを構築しDWHに流す
- lamdaは小規模かつイベント処理、glueは大容量かつバッチ処理
- Step Function(サーバーレスサービス)視覚的なワークフローで複数のAWSサービスやAPIを管理

## これらを用いた実務の流れをイメージ
1. ユーザーの行動をlamdaがpythonを用いたイベント処理を行う
- 基礎文法で習ったJSONで整形する？
```python
import json
with open("users.json" "r") as file:
    users = json.load(file)
```

2. S3に生データを保存していく
**疑問**S3とDWHの差異はなんなのだろうか？
- S3に入れただけでは整列していないただのデータ

3. ETL/ELTでパイプライン構築
- Apache Spark(Pyspark)というAPIを用いてデータを抽出

```python
df = df.dropDuplicates()
df = df.filter(df["user_id"].isNotNull())
```
以上のような記述を行うのだろうか。現在の知識で言語化すると一行目でdropduplicate(Pysparkのメソッド)を用い、重複するレコードを除く
二行目pandasのメソッドfileterを用いてuser_idが存在しないデータを省いている

- これら一連の流れを自動化する際にglueが用いられる
- lamdaとの違いを理解していなかったが確かに活用する場所で考えると用途は明白
- lamdaは定期的にユーザーの生のデータを軽度な処理でひとまずS３へ送る
- glueはS3のデータに対してバッチ処理で長時間大容量のデータ加工・抽出を行う

4. Redshiftに格納
- 普通のDBとは違ってデータ分析や集計、BIツー接続を前提とした特徴がある

5. SQLで分析
- 将来的にはこの部分（データアナリスト？）の領分も視野に入れて活躍したい

6. SteP Functionで全体管理
- これまでの処理を管理
lamdaでイベント処理→S3に保存→Glueで抽出・加工→Redshiftへ保存→SQLで集計

7. BI可視化に繋げる


