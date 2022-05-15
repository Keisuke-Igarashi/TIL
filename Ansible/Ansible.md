- インストール
```bash
sudo apt update
sudo apt install ansible
ansible --version
```

- インベントリの記載方法（例）
```
[web_serber]
xxx.xxx.xxx.xxx

[allhosts]
localhost ansible_connection=local
```

- playbookの記載方法（例）**タブは使わず半角空白でインデントすること**
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

- playbookの実行
```bash
ansible-playbook -i inventory xxxxx.yml
---

inventory：ホストやIPアドレスなど接続先の情報を記載
xxxx.yml：playbook.設定内容を記載する。yml形式以外にini形式がある

- debugの実行
事前にplaybookに以下設定を入れておく
```
debugger: on_failed
```
*debuggerキーワード*
|値|動作|
----|----
|always|結果にかかわらず常にデバッガを起動する|
|never|デバッガを起動しない|
|on_failed|タスクが失敗した時にデバッガを起動する|
|on_unreachable|ホストに到達しなかった場合にデバッガを起動する|
|on_skipped|タスクがスキップされた時にデバッガを起動する|

- デバッグモード時の操作
|コマンド(丸かっこは省略前のコマンド）|動作|
----|----
|p 値|モジュール実行に必要な値を表示する。表示できるのは以下5種類task/task.args/task_vars/host/result|
|task.args[key] = value|モジュールの変数を更新する|
|task_vars[key] = value|タスクの値を更新する  更新後は必ずupdate_taskをすること|
|u(update_task)|編集した内容でtaskを更新する|
|r(redo)|taskを再実行する|
|c(continue)|playの実行継続（次のtaskから実行)|
|q(quit)|playのデバック終了(playbookの実行も中断)|

