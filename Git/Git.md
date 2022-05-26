# リモートリポジトリを取得
```bash
git clone <リモートリポジトリのURL>
```

# 間違えて取得した場合の削除
```bash
rd /s <間違えてcloneしてきたディレクトリ>
```

# github反映までの流れ
```bash
git add ファイル名
git add .
git commit -m "コミットメッセージ"
git push origin main
```

# 状態確認
```bash
git status
git branch
```

# Gitのコンフィグ設定
```bash
git config --global user.name "Keisuke-Igarashi"
git config --global user.email "igarashi4948@gmial.com"
git config --global user.email "ke.igarashi@tech.nflabs.jp"
```

# .gitignoreファイル
- .gitgnoreというファイル名で、リポジトリの直下に配置する(.gitファイルがあるディレクトリ)

言語やフレームワークごとにテンプレあるので使う
'https://github.com/github/gitignore'
'https://www.toptal.com/developers/gitignore'

# .gitignoreの編集内容（venv)
```
# Environments
.venv
venv/
```


# ファイルのステージングの削除
```bash
git rm --cached <ファイル名>
git rm --cached ./venv/
```


# ローカルリポジトリからGitHubにリポジトリ作成するまでの手順

1. GitHub上でリポジトリを空作成する。
2. ローカルリポジトリ側でリモートを追加する。

```
git remote add origin <GitHubリポジトリのURL>
git remote show
```
これでoriginが追加されるのであとは他と一緒である。

# git log
* ログ履歴
* SHA1 IDで一意管理
```
git log
git log --oneline
git log --patch-with-stat #具体的に何が変更になったかまでみる
git log --oneline math.sh
```

# gitk
* GUIで確認
```
gitk
```

# トラブルシュート

## git pull / git push できなくなったとき
```
(venv) PS C:\Users\nflabs-03\Documents\git\kentikuApp> git pull origin master
From https://github.com/Keisuke-Igarashi/kentikuApp
 * branch            master     -> FETCH_HEAD
fatal: refusing to merge unrelated histories

(venv) PS C:\Users\nflabs-03\Documents\git\kentikuApp> ｀ 
```
