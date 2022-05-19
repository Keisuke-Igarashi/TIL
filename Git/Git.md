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
git config --global user.name "XXX"
git config --global user.email "xxxx@hogehoge.com"
```

# .gitignoreファイル
- .gitgnoreというファイル名で、リポジトリの直下に配置する(.gitファイルがあるディレクトリ)

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

