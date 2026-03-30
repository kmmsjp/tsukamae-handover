# サーバー構築内容(STG環境) 手順書

## pip install の実施手順（オフラインインストール）

### 【背景・課題】
Windows Server（VPS）環境では、セキュリティ制限により pip install 実行時の外部通信が禁止されているため、通常の方法では Python パッケージのインストールができない。
そのため、pip download を用いたオフラインインストール方式を採用する。

---

### 【Mac 環境での事前作業（pip download）】

※ 本手順は、貸与 Mac 環境にて実施可能であることを確認済み。
※ 本手順で取得したパッケージを用いて、Windows Server 上で正常に環境構築できることを確認済み。

#### ■ 前提条件
* Mac に Python がインストールされていること
* pip が利用可能であること
* Windows Server 側の Python バージョンを事前に把握していること

#### ■ 手順① 作業ディレクトリ作成
`mkdir offline_build`
`cd offline_build`

#### ■ 手順② 仮想環境の作成（Mac）
`python3 -m venv venv`
`source venv/bin/activate`

#### ■ 手順③ pip のアップデート
`pip install --upgrade pip`

#### ■ 手順④ requirements.txt の配置
* 必要な Python パッケージを記載した requirements.txt を本ディレクトリに配置する

#### ■ 手順⑤ Windows 向けパッケージのダウンロード
`pip download -r requirements.txt --platform win_amd64 --python-version 314 --only-binary=:all: --dest packages`

※ python-version は **Windows Server 側の Python バージョンに合わせて**指定する

#### ■ 手順⑥ 仮想環境の終了
`deactivate`

#### ■ VPS へコピーするファイル
* packages フォルダ
* requirements.txt

---

### 【Windows Server 側での作業】

#### ■ 手順① 仮想環境（venv）の作成
`python -m venv venv`

#### ■ 手順② 仮想環境の有効化
`venv\Scripts\activate`

#### ■ 手順③ オフラインインストールの実行
`pip install --no-index --find-links=packages -r requirements.txt`

本手順により、外部ネットワーク通信を行わずに Python パッケージのインストールが可能である。

---

### 【検証結果】
* 貸与 Mac 環境にて pip download を実施可能
* 取得した Windows 向けパッケージを Windows Server にコピー
* Windows Server 上で venv を作成し、オフラインで pip install が正常に完了
* Python プログラムの実行が正常に行えることを確認済み

---

## APIキーの格納場所と検証

APIキーは STG環境・PRD環境の各スクリプトのフォルダー内に、config.envの名前で保管されいます。

---

## プログラムのデプロイ場所

* `C:\Users\THUSR100\Documents`の配下
---

## プログラムの起動方法

1. PowerShell を起動
2. プログラム配置ディレクトリへ移動
   `cd C:\Users\THUSR100\Documents\treasure-kintone-upsert`
3. 仮想環境を有効化
   `venv\Scripts\activate`
4. Python プログラムを実行
   `python kintone_upsert.py`