## Install rustup (assuming using fish)

```
1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
>2

Modify PATH variable? (Y/n)
n
```

Add path to fish: `fish_add_path $HOME/.cargo/env`

## Common cmds of rustup

`rustup install [stable, nightly, ...]`

`rustup default [stable, nightly, ...]`

`rustup toolchain list`

## LSP (rust-analyzer)

*rust-analyzer* is available in *rustup*, but [only in the nightly toolchain](https://rust-analyzer.github.io/manual.html#rustup): `rustup +nightly component add rust-analyzer-preview
`.

Install through OS package manager (from [doc of Doom Emacs](https://docs.doomemacs.org/latest/modules/lang/rust/)).

## References
- https://rust-analyzer.github.io/manual.html