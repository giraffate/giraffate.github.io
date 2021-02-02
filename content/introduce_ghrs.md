+++
title = "giraffate/ghrs"
date = 2021-02-02
[taxonomies]
tags = ["rust"]
categories = ["programming"]
+++

何番煎じかわからないが [ghrs](https://github.com/giraffate/ghrs) という GitHub Client を Rust で書いた。最初は会社の評価資料を書くために GitHub API から Event を取得する CLI を書いていたのだがちょうどいい GitHub Client がなかったのでそこから自分で書いた。<!-- more -->

README に書いてある通り、 [Octocrab](https://github.com/XAMPPRocky/octocrab) のインターフェースをかなり参考にしている。ただ、 ちょっとした CLI を書くのに `async` 使うのはやりすぎと思っていたので、 Octocrab に似たようなインターフェースで blocking I/O でよいのでシンプルに書ける GitHub Client を探していたが見つからなかったので [ghrs](https://github.com/giraffate/ghrs) を書いた。

## Usage
[pull request の一覧を取得する](https://docs.github.com/en/rest/reference/pulls#list-pull-requests)場合は以下のように書ける。レスポンスの Link ヘッダーを見て次ページも取得できる。ただし、まだ issue, pull request, event の取得しか対応していない。

```rust
fn main() -> Result<(), Box<dyn std::error::Error + Send + Sync>> {
    let client = Client::new();
    let mut current_page = client
        .pulls("owner", "repo")
        .list()
        .per_page(100)
        .page(1)
        .send()?;

    // You get pull requests.
    let mut pull_requests = current_page.take_items();

    // If you want to get next pages, see here.
    while let Some(next_page) = current_page.get_next_page() {
        current_page = next_page;
        pull_requests.extend(current_page.take_items());
    }

    Ok(())
}
```

## Summary
機能は全然足りていないが、ちょうどいい具合に使いやすいものを書けたし会社の評価資料は書き始められたので、一旦満足してライブラリとしてまとめた。 [crates.io](https://crates.io/crates/ghrs) にも上げておいた。
