# PYthon_studyでの気づき
## 環境構築
- python3 -m venv venv （venvモジュールでvenv仮想環境を作る）-mはモジュールのm
- source venv/bin/activateで仮想環境接続

## pandas
- upperメソッドで大文字
- enumerate()メソッドでindex付きでloopを回す。each_with_indexに近いか

- listはindexでdictはkey名で取り出す
- df[条件式]で最初Boolean値（ブール値）で真贋判定
- forやifで回していた抽出がpandasだと一行で済む
- df["name"]行だけ取り出した時それは一次元ベクトルのpandas.Seriesデータ
- dfは表、二次元 Seriesは一次元一本の列？データ
- df["name"]はname列そのもの
- df[["name"]]列名のリスト。箱
- .shapeメソッドやtype()で型を確認

 ## 列追加
 - 代入された値がそのまま列に入る。比較式、条件式の場合boolean値になる
- 確認する際はDataframeの列確認df.dtypesを用いる type()との違いに注意
- 後者はpythonの標準でdfオブジェクト自体の型を見ている。変数の正体
- 前者はpandasの標準で各列の型を見ている

## sort
- 基本は昇順
- ascending=Falseで降順descending
- 変数に保存する必要がある
 第一基準、第二基準について
- sort_values(["score", "name"])複数列を渡す際はlist化
- scoreで整理した後、scoreが同じ人がいたらあかさたな順に整理
- indexを振り直すにはreset_index()メソッド
- (drop=True)で古いindex列を捨てる

## enumerate()
- for index, name in enumerate(names):
- indexとnameを同時に代入しているイメージ
- a, b = [10, 20]と同様
- for index, name in enumerate(names, start=1 )１からindexスタートすることもできる
- print(index, name)とすると表のよう見える

## groupby()
- df.groupby("team")["score"].mean()
- 何で集めるか、どの列か、どの処理か
- sum(),max(),count()等
- df.group(["", ""]).mean()listを渡して複数列指定
- reset_index()で可読性を上げる

## agg()
- aggregation(集計)
- メソッドチェーンで繋いで行き複数処理を同時集計できる
- agg(["mean", "max", "min", "count", "sum"])
- 複数はlist化を覚えよう
- df.groupby("team")["score"].agg(["mean", "sum", "count", "max"]).sort_values(["mean", "count"]).reset_index()全部盛り

## merge
- キーとなるカラムをonに記述
- pd.merge(users, scores, on="")
- how = " inner, left rigth outer" 共通部分だけ、左準拠、　右準拠、　全残し

## 復習
- csv読み込み直後df.head()で先頭五行を確認する
- groupby("department"), groupby("product"), groupby("date")
- SQl版
SELECT department, 
AVG(salary) 
FROM employees
GROUP BY department
pandas変換
df.groupby("department")["salary"].mean()
- SyntaxError	文法が間違っている
- IndentationError	インデントが正しくない
- NameError	定義されていない変数・関数を参照した
- TypeError	型が合わない操作をした・引数の数が違う
- AttributeError	オブジェクトに存在しない属性・メソッドを呼んだ
- ValueError	型は正しいが値が不正
- df = pd.read_csv("")で外部csv参照
- Seriesは一列なので計算が速い前要素が同じ型のため
- DataFrameは複数のSeriesが並んだ表全体csvやexcelをそのまま取り込んだとき
- .plot()やfillna()を使うときはSeriesがそのまま使える