# Kaggle用環境設定

## githubアカウント作成
github.comへアクセスしてアカウントを新規作成

## sshキーを設定
```
$ ls ~/.ssh    -> 鍵が未作成であることを確認する
$ ssh-keygen -t rsa    -> すべてEnterを押してスキップしてもよい
$ cat ~/.ssh/id_rsa.pub
```
表示された公開鍵をgithub管理画面 Settings > SSH and GPG keys > New SSH key へ登録

## github
1. github管理画面で新規リポジトリを作成i(今回はkaggleという名称で作成)
1. ローカル環境で上記をclone（URLは管理画面で「SSH」を選択する）
   ```
   $ git clone git@github.com:XXXXXX/kaggle.git
   ```

## README.mdを作成
```
$ cd kaggle
$ echo '# kaggle用環境構築テスト' > README.md
```

```
$ git add README.md
$ git commit -m 'first commit'
$ git push origin master
```

## kaggleコンペ別のブランチ作成
git checkout -b で一発でできるけどあえて分けて実行
```
$ git branch [kaggleコンペ名など任意の名称]
$ git branch    -> ブランチが作成されていることを確認
$ git checkout [作成したブランチ名]
$ git branch    -> ブランチが移動できたことを確認
```

### ブランチ別にvenvの仮想環境で構築

venvによる仮想環境を構築して環境を適用する
```
$ python3 -m venv .venv
$ . .venv/bin/activate
```

必要なライブラリをインストール
```
$ pip install --upgrade pip
$ pip install jupyterlab
$ pip install numpy
$ pip install scipy
$ pip install pandas
$ pip install matplotlib
$ pip install seaborn
$ pip install japanize-matplotlib
$ pip install scikit-learn
$ pip install kaggle
```

pipによる環境設定を requirements.txtに書き出す
```
$ pip freeze > requirements.txt
```
※次回以降は下記コマンドで一括インストール可能
```
$ pip install -r requirements.txt
```

githubに上げる必要のないファイルを .gitignoreに記載
```
$ echo '.venv' > .gitignore
$ echo 'data/input/*' >> .gitignore
```

必要なディレクトリを作成
```
$ mkdir -p data/input data/output
$ mkdir notebook utils
```
こちらを参考に今後拡張予定 -> https://upura.hatenablog.com/entry/2018/12/28/225234


Jupyter Labを開けるか確認
```
$ jupyter lab
```
※画面が完全に止まったら「ctrl+c」を押して、表示されるURLをコピーしてブラウザで表示<br>


すべてgithubに上げる<br>
※空のディレクトリはgithubに上げれないので必要に応じて .gitkeepファイルを作成する
```
$ git status
$ git add *
$ git commit -m 'first commit'
$ git push origin [ブランチ名]
```

### masterとコンペ用ブランチを切り離す
ここまでの設定のまま git checkout master とかすると、masterブランチに不要なファイルを持ち込んでしまうため下記を実行する。

（もっと効率的なやり方はないだろうか・・・）
```
$ deactivate
$ cd ~
$ git clone -b [ブランチ名] [Git URL] [名称 ※任意]
  ->例) git clone -b master git@github.com:XXXXX/kaggle.git kaggle-master
```
以後、masterの変更はこのディレクトリで作業をする。


### 補足）kaggleコマンドの使用
kaggleコマンド自体は既にpip install kaggleでインストール済み

kaggleの自分のアカウントの画面で Create New API Token を押下

```
$ mkdir ~/.kaggle
```
上記に取得したjsonファイルを配置して、下記を実行してコンペ一覧が表示されたらOK

```
$ kaggle competitions list
```

詳細はこちらを参照 -> https://www.currypurin.com/entry/2018/kaggle-api


### ブランチ別にdocker環境を構築

今後追記予定

https://www.fabrica-com.co.jp/techblog/technology/994/

https://amalog.hateblo.jp/entry/data-analysis-docker





