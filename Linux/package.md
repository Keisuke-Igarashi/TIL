# パッケージ管理

## リポジトリアップデート

```bash  
sudo apt update 
```

* エラーが出る場合は以下を実行して再度リポジトリアップデート  

```bash
wget -q -O - https://archive.kali.org/archive-key.asc | apt-key add 
```

## パッケージインストール

```bash
sudo apt-get update
sudo apt^get install <パッケージ名>
```
