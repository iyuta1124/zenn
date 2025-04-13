---
title: 'Dockerの基本'
emoji: '��'
type: 'tech'
topics: ['docker', 'container', 'devops']
published: false
---

# Docker の用語について

## Docker Engine

コンテナ型ソフトウェアの部分。Linux で動くソフトウェア。

CLI も提供されている。

## Docker CLI

Docker Engine に提供され、`docker run` や `docker build`のような`docker`で始まるコマンドで命令できる。

ちなみに  `docker`  コマンドは  `dockerd`  というデーモンに命令を伝え  `containerd`  というランタイムを操作しますが、一般に  `dockerd`  や  `containerd`  は隠蔽されており意識することはない。

## **Docker Desktop**

Windows や Mac で Docker を使うための GUI アプリケーションのこと。

GUI でコンテナなどを確認したり停止したりすることができる。

一般に「Windows や Mac にホストマシンに Docker をインストールする」とは「Docker Desktop アプリケーションをインストールする」ということになる。

Docker Desktop には Docker Compose や Kubernetes なども含まれている。

## **Docker Compose**

Docker CLI をまとめて実行してくれる便利なツールのこと

`docker compose up`  のような  `docker compose`  ではじまるコマンドを提供してくれる。

「２つのコンテナを起動し」「それぞれのネットワークを構築し」「コンテナのデータをホストマシンと共有させる」という複雑なコマンドを、Yaml ファイルを書くことで実現できるツール。

## **Docker Hub**

Docker のイメージレジストリである Saas サービスのこと

公開されているイメージを  `git pull`  したり、構築したイメージを  `git push`  する先のように理解すればよい。

## **ECS / GKE**

[Amazon Elastic Container Service ( ECS )](https://docs.aws.amazon.com/ja_jp/AmazonECS/latest/developerguide/Welcome.html)  と  [Google Kubernetes Engine ( GKE )](https://cloud.google.com/kubernetes-engine)  は、コンテナ管理サービスのこと。**Docker Engine の入った Linux のこと**  で、**ローカル開発に使ったコンテナをそのままデプロイできる場所**  のこと。

## **ECR / GCR とは**

[Amazon Elastic Container Registry ( ECR )](https://docs.aws.amazon.com/ja_jp/AmazonECR/latest/userguide/what-is-ecr.html)  と  [Google Container Registry ( GCR )](https://cloud.google.com/container-registry)  は、非公開のイメージのレジストリ。**プライベートな Docker Hub のこと**  で、「商用サービスのイメージを Docker Hub で公開したくないけど、レジストリに登録しないとデプロイできない」というときなどに使うことになる。

## **Kubernetes**

Kubernetes ( 略して k8s とも ) は多数のコンテナを管理するオーケストレーションソフトウェアで、`kubectl apply`  のような  `kubectl`  ではじまるコマンドを提供してくれる。

オーケストレーションにより、ロードバランサーの作成やアクセス集中時のスケーリングなどが簡単に実現できるようになります。要約すると  **コンテナを運用するためのツール**。

# **Docker の基本の要素**

## コンテナ

特定のコマンドを実行するために作られる  **ホストマシン上の隔離された領域のこと。**

Docker に不慣れな間はコンテナをホストマシン上で動いている仮想 OS のように感じるかもしれないが、コンテナの実体は Linux の Namespace という機能によりほかと分離されたただの１プロセスでしかない。
Namespace を簡単に作ったり消したりできるようなコマンドを提供してくれたりするものが Docker と言える。

コンテナ型仮想化には OS は含まれておらず、ホストマシンのカーネルを使っている。

## コンテナの特徴

1. コンテナはイメージをもとに作られる
2. Docker の CLI や API を使って、生成や起動や停止を行うことができる
3. 複数のコンテナは互いに独立していて影響できず、独自に動作する
4. Docker Engine の上ならローカルマシンでも仮想マシンでもクラウド環境でも動かせる

## イメージ

コンテナの実行に必要なパッケージで、**ファイルやメタ情報を集めたもの。**

イメージは  [Docker Hub](https://hub.docker.com/)  で公開されています。( Docker Hub に公開されているのは Dockerfile ではない)

そのおかげで、たとえばチームで「`php8.1`  のイメージを使う」と決めておけば、同じ開発環境を構築することが容易にできる。

## **Dockerfile**

**既存のイメージにレイヤーをさらに積み重ねるためのテキストファイル**  のこと

インターネット上に公開されているイメージではインストールしてあるコマンドが足りないなどの問題がある場合に、自分に都合の良いイメージを作るために Dockerfile を作成する。

公開されているイメージに Dockerfile でレイヤーを乗せるだけなので、OS から構築したりする手間は不要でとても簡単である。

Dockerfile は  [GitHub](https://github.co.jp/)  などで共有するのが望ましいです。( Git 管理するのはイメージではない）

それにより、たとえばチームで「`php8.1`  のイメージに  `git`  を入れたイメージを使う」という構成を共有することができます。

# 基本のコマンド

### コンテナを起動する

`container run`  **イメージ**  から  **イメージからコンテナを１つ作るだけのコマンド。**

次のコマンドを一気に行う便利コマンドなのですが、この本ではコマンド個別の解説は行わずに  `container run`  として解説します。

- `image pull`
- `container create`
- `container start`

### **イメージを作成する**

`image build` **Dockerfile**  から  **イメージを作成する**  コマンド。

Dockerfile はベースとなるイメージを指定し、「コマンドのインストール」や「設定ファイルの配置」などのレイヤーを重ねて、新たなイメージを作るためのもの。

そうして自分で作成したイメージは、ほかのイメージと全く同様に  `container run`  で使うことができる。

### **コンテナを操作する**

たとえば  `container exec`  は  **コンテナ**  に  **命令を送る**  コマンド。

対象がコンテナなので、必然的に  `container run`  のあとに使うコマンドになる。

**イメージ**  や  **Dockerfile**  に対しては命令できない。

`container exec`  で「ログを見せろ」「コンパイルしろ」「テストを実行しろ」のような命令を送ることで、起動しているコンテナに様々な処理をさせることができます。

ほかにも  `container stop`  という  **コンテナ**  を  **停止する**  コマンドなど、コンテナを対象になんらかを行うコマンドはたくさんあります。

# **コマンドの形を意識する**

これらの基本の要素と、コマンドを理解できたら、その基礎を最大限に活かすために「コマンドが  **なに**  を  **どうする**  のか」を常に考える習慣を作る。

初めは大変で辛く感じるかもしれませんが、やみくもにコマンドを覚えるのとこれを続けるのでは、**理解の早さと深さが圧倒的に違います**。

## **「なに」を「どうする」か**

`docker container run`  や  `docker image build`  は  `docker なに ( を ) どうする`  と読み替えられるということです。普段から Docker を利用されている方はお気付きでしょうが、この本では  `docker run`  ではなく  `docker container run`  を使っています。

Docker のコマンドは 2017 年 1 月にリリースされた v1.13 で大幅な変更が行われており、`docker run`  が旧コマンド、`docker container run`  が新コマンドとなっています。

これは  `docker run`  や  `docker build`  のような  `docker`  直下のコマンドが増えすぎて  **なに**  がわかりづらくなってしまったためで、v1.13 からはそれぞれ対象を明示できるサブコマンド形式の方を使うことが推奨されています。

| **旧コマンド**  | **新コマンド**            |
| --------------- | ------------------------- |
| `docker build`  | `docker image build`      |
| `docker run`    | `docker container run`    |
| `docker pull`   | `docker image pull`       |
| `docker create` | `docker container create` |
| `docker start`  | `docker container start`  |
| `docker images` | `docker image ls`         |
| `docker ps`     | `docker container ls`     |

# **Docker と周辺ツールを分ける**

## **Docker Compose**

Docker Compose は Yaml ファイルを書くことにより、複数のコンテナをまとめて起動したりしてくれるツール。

Docker Compose は Docker Desktop に含まれており、`docker compose xxx`  のようなコマンド体系になっています。

Docker CLI のみで環境を構築しようとすると、多量の複雑なコマンドを誰でも何度でも同じく実行するためには  **Docker コマンドの手順書が必要**  になります。

この問題を「Docker Compose を使い詳細な命令は  **全部 Yaml に書いて GitHub で共有**  する」という方法で解決することができるようになります。

「Docker を正しく理解する」→「Docker を使って環境構築をする」→「Docker Compose に置き換えて楽をする」という流れで導入し、最終的にはとても複雑な構成を  `compose up`  のみで即時起動できる状態を目指します。

## **Kubernetes**

Kubernetes は Json ファイルや Yaml ファイルを書くことにより、同じコンテナを複数台起動してクラスタを構築したり、コンテナの監視やコンテナの自動再起動を簡単に行えるオーケストレーションソフトウェアです。

Kubernetes も Docker Desktop に含まれており、`kubectl xxx`  のようなコマンド体系になっています。

開発環境を構築する場合、ロードバランサーを作って Web サーバを冗長構成にしたりアクセス増加時にコンテナを増やしたりする必要はないので、Kubernetes は使いません。

# コンテナの基礎操作

## **コンテナを起動する - `container run`**

```docker
新コマンド
$ docker container run [option] <image> [command]

旧コマンド
$ docker run [option] <image> [command]
```

| **オプション**  | **方針**                   | **実運用時の考え方**                                                     |
| --------------- | -------------------------- | ------------------------------------------------------------------------ |
| `--interactive` | 対話操作をする場合は指定   | 常に指定しても害はない                                                   |
| `--tty`         | 対話操作をする場合は指定   | 常に指定しても害はない                                                   |
| `--detach`      | 対話操作をしない場合は指定 | デバッグなどでは外した方が良いこともある                                 |
| `--rm`          | 常に指定                   | 停止済コンテナを活用するかで判断する                                     |
| `--name`        | 常に指定                   | 衝突さえ理解していれば指定して損はない                                   |
| `--platform`    | 必要な場合のみ指定         | Intel or AMD CPU なら検討不要、見ても無視可 ARM CPU ならケースバイケース |

## **コンテナ一覧を確認する -`container ls`**

```docker
新コマンド
$ docker container ls [option]

旧コマンド
$ docker ps [option]
```

| **オプション** | **意味**                 | **用途**                       |
| -------------- | ------------------------ | ------------------------------ |
| `-a--all`      | 全てのコンテナを表示する | 起動中以外のコンテナを確認する |

## **コンテナを停止する - `container stop`**

```docker
新コマンド
$ docker container stop [option] <container>

旧コマンド
$ docker stop [option] <container>
```

## **コンテナを削除する - `container run`**

```docker
新コマンド
$ docker container rm [option] <container>

旧コマンド
$ docker rm [option] <container>
```

| **オプション** | **意味**                       | **用途**                 |
| -------------- | ------------------------------ | ------------------------ |
| `-f--force`    | 実行中のコンテナを強制削除する | 停止と削除をまとめて行う |

# コンテナの状態遷移

自分のコンテナが意図した通りに動いているかを正しく把握するために、コンテナがどのような状態遷移をするか理解する。

コンテナが停止する理由を理解できていないと、「起動したはずのコンテナが見当たらない」とか「ターミナルが固まらないので起動に失敗したんだ」という勘違いに繋がるため。

## **コンテナとプロセスについて**

コンテナは Namespace によって隔離されたプロセスである。

またコンテナにはイメージによって定められたデフォルトの命令があることと、`container run`  実行時に任意の命令を指定できる。

- コンテナは  **ある１つのコマンドを実行するため**  に起動している
  - それはデフォルト命令か指定命令の  **どちらか**  で、**`PID`  が  `1`  になる**
- 複数のコンテナの  `PID = 1`  は  **Linux の Namespace 機能により衝突しない**

便宜上  `PID = 1`  のプロセスを  **メインプロセス** 、それを起動するコマンドを  **メインコマンド**  と呼びます。

たとえばコンテナが仮想サーバではなくただの１プロセスに見えてくると、「Ubuntu に PHP と MySQL と Apache を入れたコンテナを作ろう」ではなく「PHP と MySQL と Apache が必要だから３コンテナ作ろう」という発想に自然と変わる。

また、**メインプロセスが終了したコンテナは自動で停止する**  という重要な点を理解できるようになります。

## **コンテナが停止するパターン**

1. コンテナを停止する
2. メインプロセスが終了する

**コンテナ**  か  **メインプロセス**  のどちらかが先に終われば、もう片方も連動する。

# コンテナの状態保持
