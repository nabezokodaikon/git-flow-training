# git-flow training
 
## 参考
* [いまさら聞けない、成功するブランチモデルとgit-flowの基礎知識](http://www.atmarkit.co.jp/ait/articles/1311/18/news017.html)
* [git-flow cheatsheet](http://danielkummer.github.io/git-flow-cheatsheet/index.ja_JP.html)

## ブランチ
### メインブランチ
1. master
    * 常にリリース可能な状態を保持するブランチ。
    * リリース時にはタグを付ける。
1. develop
    * 常に最新の開発結果を保持するブランチ。

### サポートブランチ
1. feature
    * 新しい機能を開発するブランチ。
    * `develop`から分岐し、`develop`へマージする。
    * 機能が確定するまで`develop`を変更しないことが重要。
1. release
    * リリース準備を行うブランチ。
    * `develop`から分岐し、`master`と`develop`にマージする。
    * 機能の開発が確定したところで`develop`から分岐することにより、`develop`は次のリリースに向けた開発を進めることができる。
1. hotfix
    * リリース後に発生した不具合を修正するブランチ。
    * `master`から分岐し、`master`と`develop`にマージする。
    * `master`と`develop`の時差を考慮した上で即座に不具合に対応できることが重要。
    * `master`から分岐するため、`develop`で開発を進めていてもリリース環境と同様の環境で対応可能。
 
## コマンド
### 初期化
ブランチ構成の作成。`develop`が作成される。
```
$ git flow init -d
```
> `-d`オプションを外すと、git-flow で利用するブランチ名を自分で付けることができる。 
 
### サポートブランチ
#### サポートブランチの開始操作。
指定したサポートブランチが作成される。
```
$ git flow [feature|release|hotfix] start [ブランチ名]
```
 
#### サポートブランチの終了操作。
指定したサポートブランチがマージされ、削除される。
```
$ git flow [feature|release|hotfix] finish [ブランチ名]
```

### push
ブランチをまとめて`push`する。
```
$ git push --all
```

### 開発フロー
#### feature
##### 開発開始
```
$ git flow feature start [ブランチ名]
```

##### 開発をリモートへ
複数人と同じブランチで作業する場合は、リモートへプッシュする。
```
$ git flow feature publish [ブランチ名]
```

##### 修正分を取り込む
他の人の修正分をローカルにプルする。
```
$ git flow feature pull [ブランチ名]
```

##### 開発終了
`develop`にマージする。
`feature`が削除される。
```
$ git flow feature finish [ブランチ名]
```

#### release
##### リリース準備開始
`develop`から`release`を作成する。
```
$ git flow release start [バージョン] [develpのコミット]
```
> develpのコミットを省略すると`HEAD`が使用される。

##### リリース準備
* `release`で行った修正は、`feature`と同じようにプッシュできる。
```
$ git flow release publish [バージョン]
```

`release`リポジトリの修正をトラッキングすることもできる。
```
$ git flow release track [バージョン]
```

##### リリース準備完了
* `master`に`release`をマージする。
* `master`にリリース用のタグを付ける。
* `develop`に`release`をマージする。
* `release`が削除される。
```
$ git flow release finish [バージョン]
```

#### hotfix
##### 緊急対応の開始
* `master`から`hotfix`を作成する。
```
git flow hotfix start [ホットフィックス]
```

##### 緊急対応の終了
* `master`に`hotfix`をマージする。
* `master`にホットフィックスのタグを付ける。
* `develop`に`hotfix`をマージする。
```
$ git flow hotfix finish [ホットフィックス]
```
