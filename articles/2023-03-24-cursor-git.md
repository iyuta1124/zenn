---
title: "Gitで使用するエディターをcursorに変更する"
emoji: "🐥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["git", "cursor"]
published: true
---

ターミナルで下記のコマンドを入力し実行する

```
git config --global core.editor "cursor --wait"
```

git commit で cursor のエディタが表示されれば成功
