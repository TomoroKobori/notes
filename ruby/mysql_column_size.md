# MySQLのカラムサイズ
## バイトサイズを確認
- “string”.bytesize
```
irb(main):033:0> "あ".bytesize
=> 3
irb(main):034:0> "a".bytesize
=> 1
irb(main):035:0> "ああ".bytesize
=> 6
irb(main):036:0> "𠀋".bytesize
=> 4
irb(main):037:0> "友".bytesize
```

## String(varchar)
- `t.string :name` で `varchar(255)` のカラムが生成される
- バイト数に関係なく255文字まで格納可能

- 成功
```
# 3byte文字
irb(main):010:0> a = "あ"*255
irb(main):016:0> Trick.create(name: a)

# 4byte文字
irb(main):039:0> a = "𠀋" * 255
irb(main):044:0> Trick.create(name: a)
```
- 失敗(MysqlError発生)
```
# 3byte文字
irb(main):019:0> a = "あ"*256
irb(main):020:0> Trick.create(name: a)

# 1byte文字
irb(main):026:0> a = "a"*256
irb(main):031:0> Trick.create(name: a)
```

## TEXT
- `t.text :description` で `text` が生成される
- 65535byteまで
- 文字のバイト数によって最大文字数が変わる
- すべて4byteだと想定して `16383文字以内` に制限しておくのが無難
- 足りなければもっと大きな型を使う

- 成功
```
# 1byte文字
irb(main):047:0> a = "a" * 65336
irb(main):047:0> Trick.create(name: "name", description: a)

# 3byte文字
irb(main):066:0> a = "あ" * 21845
irb(main):066:0> Trick.create(name: "name", description: a)

# 4byte文字
irb(main):090:0> a = "𠀋" * 16383
irb(main):090:0> Trick.create(name: "name", description: a)
```
- 失敗
```
# 1byte文字
irb(main):047:0> a = "a" * 65537
irb(main):047:0> Trick.create(name: "name", description: a)

# 3byte文字
irb(main):066:0> a = "あ" * 21846
irb(main):066:0> Trick.create(name: "name", description: a)

# 4byte文字
irb(main):087:0> a = "𠀋" * 16384
irb(main):087:0> Trick.create(name: "name", description: a)
```

## Integer
- `t.text :tips` でINTが生成される
- 2147483647まで
- 整数のみ

- 成功
```
irb(main):104:0> Trick.create(name: "name", level: 2147483647)
```
- 失敗
```
irb(main):108:0> Trick.create(name: "name", level: 2147483648)
```

## decimal
- 少数も扱える
- 指定した桁数で値が丸められる
- 緯度経度なんかのときに使える
- `t.decimal :weight, precision: 全体の桁数(整数含む), scale: 少数の桁数`

## float
- 少数6桁以内で小数点を丸めずに使う場合
