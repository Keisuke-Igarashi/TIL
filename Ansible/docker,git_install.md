# **dockerとgitのインストール**

dockerとgitをcentOSにインストールするようのAnsibleソースを作成したのでメモに残す

1. kaliへのAnsibleインストール

    ```bash
    sudo apt update
    sudo apt install ansible
    ansible --version
    ```

    * PATH設定は特に必要なし

2. SSH接続 to CentOS

    ```bash
    $ ssh-keygen -t rsa -b 4096
    $ ssh-copy-id -i ~/.ssh/id_rsa.pub root@xxx.xxx.xxx.xxx
    $ ssh -i ~/.ssh/id_rsa root@192.168.11.103
    ```

    * ここでrootで接続しているけど、rootではないユーザでやったら以降もっと大変であろう

3. playbookの作成

    ```ansible
    - hosts: maj_server
    remote_user: root
    vars:
        - inventory_hostname: 192.168.11.100
        - ansible_ssh_port: 22
    tasks:
        - name: Remove installed docker
        yum:
            name:
            - docker
            - docker-client
            - docker-client-latest
            - docker-common
            - docker-latest 
            - docker-latest-logrotate 
            - docker-logrotate 
            - docker-engine
            state: absent
        - name: Install yum-utils for set up the repository
        yum:
            name:
            - yum-utils
            state: present
        - name: Set up yum config
        command: yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
        - name: Install Docker Engine
        yum:
            name:
            - docker-ce
            - docker-ce-cli
            - containerd.io
            - docker-compose-plugin
            state: present
        - name: Start Docker.
        systemd:
            name: docker.service
            state: restarted
            enabled: yes
        - name: verify than docker engine is installed
        command: docker run hello-world
        - name: docker-compose install
        shell: curl -L https://github.com/docker/compose/releases/download/1.29.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose 
        - name: add execute permission to binary
        command: chmod +x /usr/local/bin/docker-compose
        - name: Install git
        yum:
            name: 
            - git-all
            state: present
        ```

* 参考サイト１：docker

    * 'https://docs.docker.com/engine/install/centos/'
    
* 参考サイト２：docker-compose

    * 'https://github.com/docker/compose/tree/v2.6.0'

    ※docker-compose.ymlのバージョンと合わせないといけないので注意
    　バージョンあっていなくてエラーになった

* 参考サイト３：git
    * `https://git-scm.com/book/ja/v2/%E4%BD%BF%E3%81%84%E5%A7%8B%E3%82%81%E3%82%8B-Git%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB`

* curlを実行するときはなぜshellにしないといけないのか良くわかっていない。commandだとエラーになった。

* ネットの拾いサイトより公式のドキュメントのインストール方法を参考にすること

4. playbook実行

    ```bash
    ansible-playbook -i maj_sever_hosts maj_playbook.yml 
    ```

5. centOS上でアプリ起動

    ```bash
    mkdir git
    git clone 'リポジトリのURL'
    cd 'リポジトリのディレクトリ'
    docker-compose -f docker-compose.yml up --build
    ```

