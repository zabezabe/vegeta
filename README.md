# vegeta

## 負荷ツール「Vegeta」
- golangで作成されており、golangのライブラリとして提供もされている(testケースとして実装可能)

## インストール方法
1. Homebew (mac)
```bash
$ brew update && brew install vegeta
```

2. Source
mac以外でもインストール可能  
事前にgolangをインストール/設定も必要あり
```bash
$ go get -u github.com/tsenart/vegeta
```

また、vegetaのソースコードを`git clone` している場合は以下の方法でも可能
事前にgolangの設定とdepのインストール/設定が必要
```bash
$ mkdir -p $GOPATH/src/github.com/tsenart/
$ cd $GOPATH/src/github.com/tsenart/
$ git clone https://github.com/tsenart/vegeta.git
$ dep ensure
$ go install
```

## 実行方法
条件
- 実行先URL: `http://localhost/{url}`
- リクエスト数: 10rps
- 実行時間: 5s
- 結果ファイル名: result.bin
```bash
$ echo "GET http://localhost/{url}" | vegeta attack -rate=10 -duration=5s | tee result.bin
```

## 結果の確認
### text形式で表示する場合
``` bash
$ vegeta -type=text report result.bin
```
`-type=text`はデフォルト値なので省略可能

### json形式で表示する場合
jsonを扱う場合`jq`があると便利
```bash
$ vegeta -type=json report result.bin | jq .
```

### ヒストグラム形式で表示する場合
```bash
$ cat result.bin | vegeta report -type='hist[0,5ms,10ms,15ms,20ms,100ms]'
```

### グラフをplotしてhtmlを書き出す
```bash
$ cat result.bin | vegeta plot > plot.html
```

複数ファイルを重ねて表示することも可能
```bash
$ vegeta plot resultA.bin resultB.bin resultC.bin > plot.html
```

## おまけ
### jqインストール
1. Homebrew
```bash
$ brew update && brew install jq
```

他インストール方法は[こちら](https://stedolan.github.io/jq/download/)