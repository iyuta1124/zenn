---
title: "Linuxの基本コマンド"
emoji: "🐧"
type: "tech"
topics: ["linux", "basic"]
published: true
---

# Linuxとは

業務用コンピュータに向いた基本ソフトウェア(OS)のことです。

## 主要なディストリビューション

2つの主要なディストリビューションがあります：

- Debian、Ubuntuタイプ
- RedHat Linux、CentOS、Amazon Linux2（最新版は2023）

それぞれ操作性やコマンド、思想が異なってカスタマイズされています。

## Linuxシステムの3つのレベル

### 1. ユーザープロセス
- コマンドを受け付ける

### 2. カーネル
- OSの中核
- メモリ上に存在するソフトウェア
- CPUに何をすべきか指示を出す
- ハードウェアと実行中のプログラムの伝達役

### 3. ハードウェア
- システムの基盤
- メモリ、CPU、ディスク、ネットワークインターフェースなど

# Linuxにおけるユーザー

プロセスを実行したりファイルを所有したりできる独立した存在のことです。

- **ルートユーザー（スーパーユーザー）**：システム上どのファイルでも読み書きできる最強の権限を持っています。

# シェルとは

ユーザーが入力するコマンドを実行する役割を担う基本プログラムのことです。
- bash、zshなど
- ボーンシェルの拡張バージョンが多い

# 基本的なLinuxコマンド

## ls
ファイルの一覧表示をします。

```bash
ls -l        # 詳細情報を表示
ls -l hello.cs  # hello.csのみ表示
```

## cat
ファイルの中身を表示します（concatenate：連結する）。

```bash
cat hello.c     # 中身を表示
cat -n hello.c  # 行数も表示
```

## clear
画面をクリアします。

## cp
ファイルをコピーします。

```bash
cp hello.c hello.c.2
```

## mv
ファイル名の変更や移動を行います。

```bash
mv hello.c hello.c.3     # ファイル名の変更
mv hello.c.3 ../         # 1つ上のディレクトリに移動
```

## cd
ディレクトリの移動を行います。

```bash
cd ../    # 1つ上のディレクトリに移動
cd -      # ひとつ前のディレクトリに戻る
```

## touch
ファイルの作成や更新時刻の変更を行います。

```bash
touch newfile           # 新規ファイル作成
touch newfile1 newfile2 # 複数ファイルの作成
```

## rm
ファイルを削除します。

```bash
rm newfile
```

## echo
指定した引数を標準出力に表示します。

```bash
echo $HOME  # 環境変数HOMEの値を表示
```

## mkdir
ディレクトリを作成します。

```bash
mkdir ディレクトリ名
```

## rmdir
ディレクトリを削除します。

```bash
rmdir ディレクトリ名
```

## grep
特定の文字列を検索します。

```bash
grep root /etc/passwd  # passwdファイルからrootを含む行を表示
grep root /etc/*       # etcディレクトリ内のファイルからrootを含む行を表示
grep -i               # 大文字小文字を区別しない
grep -v               # 結果を反転（マッチしない行を抽出）
```

## less
ログファイルの閲覧などに便利なページャーです。

```bash
less /var/log/dnf.log
grep root /etc/* | less  # パイプと組み合わせて使用可能
```

## sudo
スーパーユーザーとしてコマンドを実行します。

```bash
sudo cat test.go
sudo su -  # スーパーユーザー状態に切り替え（プロンプトの頭に#がつく）
```

# 特殊文字とリダイレクト

## > （大なり）
ファイルへの出力リダイレクトを行います。

```bash
echo hello > text   # 上書き
echo hello >> text  # 追記
```

## | （パイプ）
コマンドの出力を別のコマンドの入力として渡します。

```bash
ps aux | grep ssh  # プロセス一覧からssh関連のものを抽出
```

## ~ （チルダ）
ホームディレクトリを表します。

## ` （バッククォーテーション）
コマンドの実行結果を文字列として返します。

```bash
echo "本日の日付 `date`"
# 出力例：本日の日付 Sat Mar 8 23:37:59 JST 2025
```

## " （ダブルクォーテーション）
文字列を扱いながら変数の展開やエスケープシーケンスの解釈を行います。

```bash
name="taro"
echo "名前は $name です"
# 出力：名前は taro です
```

# viエディタ

テキストファイルを編集するためのエディタです。

```bash
vi test  # testファイルの編集（存在しない場合は新規作成）
```

## 基本的な操作
- `i`：INSERTモードに切り替え
- `ESC`：INSERTモードを終了
- `:w`：ファイルを保存
- `:q`：viを終了
- `:q!`：変更を破棄して終了

# パスについて

## 絶対パス
ルートディレクトリを起点としたパスです。
例：`/home/dir`

## 相対パス
カレントディレクトリを起点としたパスです。
例：`./password`

# パーミッション

ファイルのアクセス権限を表します（`ls -l`で表示）。

```
-rw-r--r--@ 1 yutainoue staff 6725 2 18 15:10 Makefile
```

## 権限の種類
- `r`：Read（読み取り権限）- 4
- `w`：Write（書き込み権限）- 2
- `x`：Execute（実行権限）- 1
- `-`：権限なし

`chmod`コマンドで権限を変更できます（2進数で4.2.1、最大7）。

# シェル変数と環境変数

## シェル変数
プロンプト内で保持したい変数に使用します。

```bash
shell_var=test123
echo $shell_var  # test123
```

## 環境変数
シェルを切り替えても保持したい変数に使用します。

```bash
export shell_var
# シェルを切り替えても
echo $shell_var  # test123
```

# パス環境変数

## コマンドの場所
```bash
which ls  # lsコマンドの場所を表示
# 出力例：/usr/bin/ls
```

## パスの設定
実行ファイルをコマンドとして認識させるために、PATH環境変数にディレクトリを追加します。

```bash
echo $PATH
# 出力例：/home/ec2-user/.local/bin:/home/ec2-user/bin:/home...

PATH=$PATH:/usr/bin  # 既存のパスに/usr/binを追加
```

# パッケージ管理

yumとdnfについての詳細は[こちら](https://zenn.dev/awonosuke/scraps/764c4bbb3f82ce)を参照してください。
