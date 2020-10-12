+++
title = "Zolaでoutput_dirを設定で有効にできるようにした"
date = 2020-10-12
[taxonomies]
tags = ["rust"]
categories = ["programming"]
+++

# tl;dr
[Zola](https://github.com/getzola/zola)に`config.toml`で`output-dir`を指定できるようにした。

# Motivation
[discourse](https://zola.discourse.group/t/output-dir-in-config-toml/563)に書いたことだが、Zolaではビルドしするとファイルはデフォルトで`public`フォルダに生成されるのだけれど、これを`zola build --output-dir docs`のようにフラグで生成されるフォルダを変更できる。自分はGitHub Pagesを使っていてこれは`docs`フォルダを使えるのだけど、毎回コマンド実行時にフォルダ指定するのはだるいので`config.toml`に生成先のフォルダを設定しておきたかった。[前に使ってたHugoだとできたし](https://gohugo.io/getting-started/usage/#the-hugo-command)。

# Implementation
Zolaではfeature requestするときはまずdiscourseにスレッドを立てるということだったので上述のスレッドを立てた。メンテナーからいいんじゃないのといったコメントをもらったので[PR](https://github.com/getzola/zola/pull/1200)を開いた。それほど難しくもなく実装はすぐに終えることができそのままマージしてもらった。

`config.toml`に以下のように記載すればフラグを指定しなくても生成先のフォルダを変更できるようになる。
```toml
output_dir = "docs"
```

# Conclusion
普段はissueベースでパッチを送るのだけど、自分でこの機能がほしいと言って実装したのは初めてだった。前記事でも書いたが多少完成度は低くてもこうやって使いながら不足している機能があればパッチを送って育てていくのは楽しい。