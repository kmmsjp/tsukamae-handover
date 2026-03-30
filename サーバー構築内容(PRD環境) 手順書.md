# サーバー構築内容(PRD環境) 手順書

### 【STG環境 Windows Server 側での作業】

#### ■ PRD環境へ反映するフォルダーをPRDにコピーする
リモートデスクトップを利用してフォルダーをコピーする


### 【PRD環境 Windows Server 側での作業】

#### ■ 手順① コピーしたディレクト内にある仮想環境のフォルダーを削除する
`venv`フォルダーを削除
※ STG環境のパスで生成されている可能性が高いので、一度削除して作り直す

#### ■ 手順② 仮想環境（venv）の作成
`python -m venv venv`

#### ■ 手順③ 仮想環境の有効化
`venv\Scripts\Activate.ps1`

#### ■ 手順④ オフラインインストールの実行
`pip install --no-index --find-links=packages -r requirements.txt`

本手順により、外部ネットワーク通信を行わずに Python パッケージのインストールが可能である。

#### ■ 手順⑤ 環境変数をPRD用に修正する
https://docs.google.com/spreadsheets/d/1p7fA7Wkb-y-nrdWKYazgvisz4W3LhoD4/edit?gid=1632346771#gid=1632346771 を参照する
config.envをprd用の値で上書きをする


## タスクスケジューラの設定をする
https://github.com/kmmsjp/win-task-scheduler-ps1 のリポジトリを参照にしてタスクスケジューラ設定をする


## プログラムのデプロイ場所

* `C:\Users\THUSR098\Documents`の配下

