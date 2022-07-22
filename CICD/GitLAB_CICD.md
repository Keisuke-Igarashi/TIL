# GitLab CI/CD

* GitLabの機能として提供されているCI/CDのこと
* 少なくとも1つのGitLabサーバと1つのGitLab Runnerが必要
* .gitlab-ci.ymlにてGitLab CI/CDの手順を管理する
* GitLab CI/CDのクイックスタートガイド：https://docs.gitlab.com/ee/ci/quick_start/

## GitLab Runner

GitLab CI/CDと連携してビルド等の各種ジョブの実行を行うアプリケーション

![image-20220721095810605](img/image-20220721095810605.png)

オープンソースであり、Go(Googleが開発したプログラミング言語)で記述されている。
また、Linux、macOS、Windows、FreeBSD、Dockerなど様々なプラットフォーム上で動
作する。
ジョブの結果はGitLab CI/CDから確認することができる。
さらに詳細を知りたい場合は以下の公式ドキュメントを参照する。
参考：https://docs.gitlab.com/runner/

### GitLab Runner:①パイプライン

* GitLab Runnerはパイプライン方式でジョブを実行する
* ステージ内ジョブがすべて成功すると次のステージへ
* 公式ドキュメント
  https://gitlab-docs.creationline.com/ee/ci/pipelines/index.html

### GitLab Runner:②runner

GitLabからのCI/CDのジョブを実行するエージェント

* GitLab Runnerでは使用するrunnerを登録する必要がある
* アクセス許可の範囲によって3つのタイプが存在
  * shared runner
    プロジェクトをまたいで共通で利用可能
  * group runner
    グループ内すべてのプロジェクトとサブグループで共通で
  * specific runner
    特定のプロジェクトに紐付けて専用利用

* グループの例

  ATT7			　	：グループ
  |- WinAPI			：サブグループ
  |- Git-Practic		：サブグループ

### GitLab Runner:③ジョブの実行方式

ジョブの内容に応じて適切なexecutorと呼ばれる実行方式を選択

![image-20220721100448774](img/image-20220721100448774.png)

### GitLab CI/CDの使い方

GitLab CI/CDを使うための準備

1. リポジトリの準備

   1. リポジトリを作成する

   2. Settings > General > Merge requests > Merge checks から、
      "Piplines must succeed" にチェックを入れる

   3. 作業用ブランチの作成

      ```git
      git checkout -b ci_settings
      ```

   4. 

2. runnerの設定
   （shared runnerの場合は必要なし）

   * 検証サーバーにGitLab Runnerをインストール

     runnerを使用する方法は以下の2通り

     1. GitLabで用意されているshared runnerを使用

     2. GitLab Runnerをインストールしたサーバで登録した独自のrunnerを使用
        * 参考：https://docs.gitlab.com/runner/#runner-on-gitlabcom
        * GitLabで用意されているshared runnerで利用するリソースのスペックは以下の	ドキュメントに記載されている。
          	https://docs.gitlab.com/ee/user/gitlab_com/#linux-shared-runner
        * 各ランナーの特徴の詳細は以下のドキュメントに記載されている。
          	https://docs.gitlab.com/ee/ci/runners/runners_scope.html

     * GitLab Runnerのインストール手順(ubuntuで実施する際のコマンド)
       https://docs.gitlab.com/runner/install/linux-repository.html

       1. 公式のGitLabリポジトリを参照し、GitLab Runnerの使用可能
          バージョンを確認する

          ```bash
          curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
          ```

          ```bash
          apt-cache madison gitlab-runner
          ```

          ```bash
          export GITLAB_RUNNER_DISABLE_SKEL=true; sudo -E apt-get install gitlab-runner
          ```

          ```bash
          sudo gitlab-runner status
          Runtime platform                                    arch=amd64 os=linux pid=5530 revision=7f093137 version=15.2.0
          gitlab-runner: Service is running
          ```

   * runnerの登録(GitLab上で操作)

     * Settings > CI/CD > runner

     * specific runnerのtokenをコピー

     * runnerをspecific runnerとして登録

       ```bash
       sudo gitlab-runner register
       ```

       ![image-20220721104137122](img/image-20220721104137122.png)

   * GitLabとGitLab Runnerの連携

     Settings > CI/CD > specific runner

     ![image-20220721105521071](img/image-20220721105521071.png)

     !になっている場合は以下コマンドを実行して再確認すること

     ```bash
     sudo gitlab-runner run
     ```

### Shared runnersの無効化

Settings > CI/CD > runner > Enable shared runners forthis projectをオフにする

![image-20220721105919294](img/image-20220721105919294.png)

1. 各ステージのジョブを設定（.gitlab-ci.yml）

![image-20220721110014077](img/image-20220721110014077.png)

.gitlab-ci.ymlの公式ドキュメント
https://docs.gitlab.co.jp/ee/ci/yaml/
YAMLの文法の参考リンク
https://docs.ansible.com/ansible/2.9_ja/reference_appendices/YAMLSyntax.html

### .gitlab-ci.yml

gitlab-ci.ymlの編集方法は以下の2種類

* ローカルのエディタで編集して、レポジトリにpush

* gitlab.com > CI/CD > Editorから編集
  以下の機能がある

  - リアルタイムの文法チェック
  - ステージやジョブの内容を可視化

* CI/CD > Editor を開き、対象ブランチを作成した作業用ブランチに設定し、「Configurepipeline」を押して.gitlab-ci.ymlを作成する。

  ```yaml
  # This file is a template, and might need editing before it works on your project.
  # To contribute improvements to CI/CD templates, please follow the Development guide at:
  # https://docs.gitlab.com/ee/development/cicd/templates.html
  # This specific template is located at:
  # https://gitlab.com/gitlab-org/gitlab/-/blob/master/lib/gitlab/ci/templates/Getting-Started.gitlab-ci.yml
  
  # This is a sample GitLab CI/CD configuration file that should run without any modifications.
  # It demonstrates a basic 3 stage CI/CD pipeline. Instead of real tests or scripts,
  # it uses echo commands to simulate the pipeline execution.
  #
  # A pipeline is composed of independent jobs that run scripts, grouped into stages.
  # Stages run in sequential order, but jobs within stages run in parallel.
  #
  # For more information, see: https://docs.gitlab.com/ee/ci/yaml/index.html#stages
  
  stages:          # List of stages for jobs, and their order of execution
    - build
    - test
  
  build-job:       # This job runs in the build stage, which runs first.
    tags:
      - cicd-shell
    stage: build
    script:
      - pip install flake8
      - export PATH=$PATH:/home/gitlab-runner/.local/bin
      - flake8 src/
  
  test-job:
    tags:
      - cicd-shell
    stage: test
    script:
      - pip install pytest numpy
      - export PATH=$PATH:/home/gitlab-runner/.local/bin
      - pytest tests/
  ```

  **※exportコマンドで、flake8などpipインストールした場所のPATHを認識させている。**

* .gitlab-ci.ymlの修正をコミットするとjobが実行される

  ![image-20220721111924600](img/image-20220721111924600.png)

* CI/CD→ジョブ→ジョブのリンクを押下

  ![image-20220721112001487](img/image-20220721112001487.png)

![image-20220721112518127](img/image-20220721112518127.png)

pytestが失敗するとジョブとしても失敗する。

![image-20220721112609787](img/image-20220721112609787.png)

* この状態ではci_settingsのmainへのマージリクエストができないため、再度修正をしてpushすると自動でパイプラインが動く

  ![image-20220721113435602](img/image-20220721113435602.png)

### GitLab CI/CDの使い方:デリバリー

![image-20220721113623492](img/image-20220721113623492.png)

AWS上の環境に配置することなども可能である。
参考：https://dev.classmethod.jp/articles/gitlab-runner-ci-cd-1/

### Access Tokenの設定

Settings > Access Tokens

deliveryのジョブでgit cloneを行う処理があったが、GitLabの設定によってはパスワード
やAccess TokenをURLに埋め込まないとgit cloneが実行できないことがある。
しかし、.gitlab-ci.ymlもリポジトリ上にpushされるため、内容を確認できてしまう。
そのため、ここではAccess TokenをGitLab CI/CDの環境変数に設定し、実行時に設定し
た環境変数から取得する

Access Tokenが漏洩した際の影響を考慮し、Access Tokenには最低限の権限や有効期限
を設定することが望ましい

git cloneのコマンド（ACCESS KEY付）

```bash
 git clone http://oath2:$ACCESS_TOKEN_READ@172.16.2.11/att7/cicd/cicd_igarashi.git ~/cicd_practice
```

※CI時に設定した環境変数を呼び出す際、Linux環境では$変数名で呼び出し可能だが、
Windows環境においてはPowerShellであれば$env:変数名、cmdであれば%変数名%
と呼び出す。
用いるrunnerやexecuterの種類によって環境変数の呼び出し方法が異なることに注意する。

```yaml
# This file is a template, and might need editing before it works on your project.
# To contribute improvements to CI/CD templates, please follow the Development guide at:
# https://docs.gitlab.com/ee/development/cicd/templates.html
# This specific template is located at:
# https://gitlab.com/gitlab-org/gitlab/-/blob/master/lib/gitlab/ci/templates/Getting-Started.gitlab-ci.yml

# This is a sample GitLab CI/CD configuration file that should run without any modifications.
# It demonstrates a basic 3 stage CI/CD pipeline. Instead of real tests or scripts,
# it uses echo commands to simulate the pipeline execution.
#
# A pipeline is composed of independent jobs that run scripts, grouped into stages.
# Stages run in sequential order, but jobs within stages run in parallel.
#
# For more information, see: https://docs.gitlab.com/ee/ci/yaml/index.html#stages

stages:          # List of stages for jobs, and their order of execution
  - build
  - test
  - delivery

build-job:       # This job runs in the build stage, which runs first.
  tags:
    - cicd-shell
  stage: build
  script:
    - pip install flake8
    - export PATH=$PATH:/home/gitlab-runner/.local/bin
    - flake8 src/

test-job:
  tags:
    - cicd-shell
  stage: test
  script:
    - pip install pytest numpy
    - export PATH=$PATH:/home/gitlab-runner/.local/bin
    - pytest tests/

delivery-job:
  tags:
    - cicd-shell
  stage: delivery
  script:
    - rm -rf ~/cicd_practice
    - git clone http://oath2:$ACCESS_TOKEN_READ@172.16.2.11/att7/cicd/cicd_igarashi.git ~/cicd_practice
    - cd ~/cicd_practice
    - /home/testuser/.pyenv/shims/poetry install
```

ubuntuでの確認コマンド

```
$ cd /home/gitlab-runner/cicd_practice
$ poetry config cache-dir “/home/gitlab-runner/.cache/pypoetry”
$ poetry run python src/sample.py
```

## その他のCI/CDツール

* GitHub Actions
  * GitHub上で利用可能なCI/CDツール
  * yamlファイルを用いてジョブを定義
  * Linux、macOS、Windowsに対応
* Jenkins
  * フリーかつオープンソースの自動化サーバーソフトウェア

