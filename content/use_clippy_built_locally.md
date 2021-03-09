+++
title = "Use Clippy built on my local machine"
date = 2021-03-09
+++

This is a note about the way to use Clippy built on my local machine. It's helpful when checking the implementation of new lints, and finding some bugs and false positives.

1. Build Clippy
```
$ cargo build
```

2. Set the `LD_LIBRARY_PATH` and run Clippy on the repository which I want to do for
```
$ LD_LIBRARY_PATH=$(rustc --print sysroot)/lib  path/to/rust-clippy/target/debug/cargo-clippy --all-targets --all-features -- -Wclippy::all
```

3. Oh, at the time of writing this note, I may need to set the toolchain same as the one Clippy use (see `rust-toolchain` file in Clippy repo). Anyway, I overrode it.
```
$  rustup override set nightly-2021-02-25
```

4. Clippy works on my local machine!

