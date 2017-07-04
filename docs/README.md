# Dockerfile for Glassfish5

**Glassfish5** の 配布パッケージから Docker Image を作成する Dockerfile です。

## 説明

Glassfish5 は Java EE 8 の参照実装となっていて、その Java EE 8 の公開（最終リリース）も 2017年7月という事で迫ってきました。
リリースに向けて Glassfish5 のビルドも盛んに行われています。
この Glassfish5 の配布パッケージを取得して、Oracle Java を使って動かす環境のDocker Image を作成する Dockerfile です。

## 動作イメージ

![](images/docker-glassfish01.gif)

## 特徴

- ２種類のビルドパッケージ毎の Dockerfile
  - [Nightly](http://download.oracle.com/glassfish/5.0/nightly/)
  - [Promoted](http://download.oracle.com/glassfish/5.0/promoted)
- Oracle Java ベース
  - [Oracle Java 8 SE (Server JRE)](https://store.docker.com/images/oracle-serverjre-8)
    - ※上記 Oracle Java は Docker Store で公開しているイメージのため、Docker Store のアカウントに使用権限を付与しておく必要があります
- リモート接続設定済み
  - Glassfish Console にローカル以外からアクセスする場合に必要となるリモート接続設定を予め構成
  - 管理者ユーザ ： admin / 管理者パスワード：glassfish　を設定

## 前提

- Docker Store のアカウントを保有していること
- Oracle Java の使用権を Docker Store に紐付けていること

## 使用方法

### Docer Image の取得 (`docker pull`)

`docker pull shinya/docker-glassfish5`

Docker Hub の以下に登録している Docker Image を取得します
https://hub.docker.com/r/shinya/docker-glassfish5/

### Docker Container の起動 (`docker run`)

#### 最新のバージョン
`docker run -d -it --rm -p 4848:4848 shinya/docker-glassfish5`

タグをつけない場合は、`latest` が起動します。このタグの実体は、Nightly Build の最新版になるようにビルドしています。

#### Promoted ビルド
`docker run shinya/docker-glassfish5:<ビルドバージョン>`

たとえば、`docker run shinya/docker-glassfish5:b10` とすると、一連のテストケースをクリアした安定しているビルドの **Promoted Build** のイメージを起動します。

#### Nightly ビルド
`docker run shinya/docker-glassfish5:<ビルドバージョン>-MM_DD_YYYY`

たとえば、`docker run shinya/docker-glassfish5:b10-07_04_2017` とすると、簡易的なテストケースのみをクリアしたビルドの **Nightly Build** のイメージが起動します。

#### ビルドバージョンの確認
Docker Hub の **Build Details** タブにDockefileからのビルド結果が一覧表示されています。以下のURLで確認できます:

- https://hub.docker.com/r/shinya/docker-glassfish5/builds/

### 管理コンソール (Glassfish Console) へのアクセス
上記のDocker の起動コマンドのオプションで、以下のパラメータを追加していました、

- **-p 4848:4848**

これは、Docker Container 内で起動している Glassfish の管理コンソールが Listen しているポート番号 **4848** に対して、外部（Glassfish コンテナ を起動しているホスト環境）のポート番号をマッピングする設定です。

これを行うことで、ローカル環境のIPアドレス、ホスト名や localhost の 4848 番ポートでか管理コンソールにアクセスできます。

- 管理コンソール: http://localhost:4848

なお管理者は、デフォルトでは設定済みの `admin/glassfish` を使用してください

## Licence

Released under the [MIT license](https://gist.githubusercontent.com/shinyay/56e54ee4c0e22db8211e05e70a63247e/raw/44f0f4de510b4f2b918fad3c91e0845104092bff/LICENSE)

## Author

[shinyay](https://github.com/shinyay)
