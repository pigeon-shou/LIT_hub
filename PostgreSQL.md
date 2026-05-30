# 環境構築

- RDBSM,MySQLは読み取り管理に向いているPostgresSQLは読み書きかつ膨大なデータセット複雑なクエリ操作に向いている
- homebrew経由でスクールで導入していたのでインストールは省く
- 現在のpsql --versionは14.22
- brew services start pstgresql@14 起動
- brew services stop @14　停止
- psql postgres 接続
- CLI psqlで操作していく
# psql操作
- postgreSQLに今どんなserverが存在しているか"\l"
- CREATE DATABASE 名前;で新しく作成
- 新しいDBに移動 \c study_db
- 最後の;が入力されるまでpsqlは待ってくれるEnterで続けてok
- \r で入力リセット
- CREATE TABLE users ( userテーブルを作れ
    id SERIAL PRIMARY KEY, id列　連番　主キー（一意性）
    name VARCHAR(50), VARCHAR可変文字列（５０）
    age INTEGER　整数型
);
- \d でデータベースを見る
study_db=# \d
            List of relations
 Schema |     Name     |   Type   | Owner 
--------+--------------+----------+-------
 public | users        | table    | 
 public | users_id_seq | sequence | 
(2 rows)
- \dtでテーブルだけ見る
- SELECT * FROM users 選ぶ　*=全部の列　usersから
- INSERT INTO users (列名, 列名) VALUES ('', )
- INSERT 0 1 一行追加したという反応が返ってくる
study_db=# INSERT INTO users (name, age)
study_db-# VALUES('田中', 30);
INSERT 0 1
study_db=# INSERT INTO users (name, age) 
study_db-# VALUES('鈴木', 22)
study_db-# ;
INSERT 0 1
study_db=# SELECT * FROM users;
 id | name | age 
----+------+-----
  1 | 佐藤 |  25
  2 | 田中 |  30
  3 | 鈴木 |  22
(3 rows)

# DB操作
- WHERE 条件式で合致するデータだけ引っ張る
- SELECT * FROMはまれ　実務ではむしろSERECT name, ageと欲しい列だけ取り出す
- ORDER BY age DESC; ORDER BY で基準に並べ DESC;　で降順
- SELECT age, COUNT(*) FROM users GROUP BY age;
ageを表示して　COUNT(*)行数を数えろ GROUP BY ageでまとめて
- GROUPBYはグループ化　SELECTは表示用
- SELECT * FROM users LEFT JOIN scores ON users.id = scores.user_id;
userテーブルを基準として　scoresテーブルを結合し user_id同士で結語させろ
- ON users.id = scores.user_id; 左と右で何と何を関連づけてるか要注意 id SERIAL PRIMARY KEYがuser側なのでusers.id、スコアテーブルはscores.user_id
- t.references : 
-  UPDATE users SET age = 31 WHERE name = '田中';
UPDATE テーブルを選んで　SET で変更を書く WHERE name ='田中'指定する列
- WHERE がないと全部変わるので注意
- DELETE FROM users WHERE name = '高橋';
DELETEテーブル選んで WHEREで指定
- idはnextval(users_id_seq)で連続的に発行しているのでid=5で発行しても新しく発行されるidは6
- SQLの処理順は
FROM
↓
WHERE
↓
GROUP BY
↓
HAVING
↓
SELECT
- HAVINGでグループを絞る WHEREは行を絞る
- SELECT age, COUNT(*) FROM users GROUP BY age HAVING COUNT(*) >=2;
- サブクエリが先に実行される
- 実務ではSELECT name  WHERE 条件式（サブクエリ）という構造が多い
SELECT *
FROM users
WHERE age > (
    SELECT AVG(age)
    FROM users
);