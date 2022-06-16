# **Ansible**

* 特徴：冪等性（べきとうせい）

![ansible_sumary](/Ansible/img/ansible_sumaly.png)

## インストール
```bash
sudo apt update
sudo apt install ansible
ansible --version
```

## インベントリの記載方法（例）
```
[web_serber]
xxx.xxx.xxx.xxx

[allhosts]
localhost ansible_connection=local
```

## playbookの記載方法（例）**タブは使わず半角空白でインデントすること**
```
- hosts: webserver
  remote_user: root
  tasks:
  - name: install httpd
    yum;
      name: httpd
      state: latest

  - name: run httpd
    systemd:
      name: httpd
      state: started
      enabled: yes
  
  - name: permit http access
    firewalld:
      service: http
      permanent: yes
      state: enabled
      immediate: yes
```

https://docs.ansible.com/ansible/2.9_ja/modules/firewalld_module.html
上記で検索しながら調べていく

## playbookの実行
  ```bash
  ansible-playbook -i inventory xxxxx.yml
```

  * inventory：ホストやIPアドレスなど接続先の情報を記載
  * xxxx.yml：playbook.設定内容を記載する。yml形式以外にini形式がある


## playbookのオプション
### -iオプション
インベントリを指定し実行する  
```
ansible-playbook -i <インベントリファイル名>　<playbook名>
```

### --checkオプション  
changedとなる可能性がある箇所を予測し、差分確認のみを行う
```
ansible-playbook -i <インベントリファイル名>　<playbook名> --check
```

### -v, -vv, -vvvオプション
playbook実行時にログを表示vが多いほど、より詳細にログが表示される  
```
ansible-playbook -i <インベントリファイル名>　<playbook名> -v
```

### --tagsオプション
指定したタグのplayとtaskのみ実行する  
```
ansible-playbook -i <インベントリファイル名>　<playbook名> --tags <タグ名>, <タグ名>
```

### --skip-tagsオプション
指定したタグについては
**実行しない**
```
ansible-playbook -i <インベントリファイル名>　<playbook名> --skip-tags <タグ名>, <タグ名>
```

### -k,-Kオプション
対象サーバーのユーザのパスワードを要求する。  
-kは一般ユーザ、-Kは権限昇格時(sudo時)のパスワード  
```
ansible-playbook -i <インベントリファイル名>　<playbook名> -k
```

### --stepオプション
- playbookのステップ実行で1つずつタスクを実行する
- yで次のステップに進む、nでスキップ、cは残りすべて実行する
```
ansible-playbook -i <インベントリファイル名>　<playbook名> --step
```

### -lオプション
playbook実行時に実行対象のホストを指定する
```
ansible-playbook -i <インベントリファイル名>　<playbook名> -l <実行対象のホスト名>
```

### --start-at-taskオプション
指定されたタスク名からPlaybookを実行する  
```
ansible-playbook -i <インベントリファイル名>　<playbook名> --start-at-task <開始タスク名>
```

### --syntax-checkオプション
playbookファイルのYAML構文チェック  
```
ansible-playbook -i <インベントリファイル名>　<playbook名> --syntax-check
```

## debugの実行
事前にplaybookに以下設定を入れておく
```
debugger: on_failed
```

### debuggerキーワード
|値|動作|
----|----
|always|結果にかかわらず常にデバッガを起動する|
|never|デバッガを起動しない|
|on_failed|タスクが失敗した時にデバッガを起動する|
|on_unreachable|ホストに到達しなかった場合にデバッガを起動する|
|on_skipped|タスクがスキップされた時にデバッガを起動する|

### デバッグモード時の操作

|コマンド(丸かっこは省略前のコマンド）|動作|
----|----
|p 値|モジュール実行に必要な値を表示する。表示できるのは以下5種類task/task.args/task_vars/host/result|
|task.args[key] = value|モジュールの変数を更新する|
|task_vars[key] = value|タスクの値を更新する  更新後は必ずupdate_taskをすること|
|u(update_task)|編集した内容でtaskを更新する|
|r(redo)|taskを再実行する|
|c(continue)|playの実行継続（次のtaskから実行)|
|q(quit)|playのデバック終了(playbookの実行も中断)|

