Install rustup.

`rustup install [stable, nightly, ...]`

`rustup default [stable, nightly, ...]`

`rustup toolchain list`

## LSP (rust-analyzer)

*rust-analyzer* is available in *rustup*, but [only in the nightly toolchain](https://rust-analyzer.github.io/manual.html#rustup): `rustup +nightly component add rust-analyzer-preview
`.

Install through OS package manager (from [doc of Doom Emacs](https://docs.doomemacs.org/latest/modules/lang/rust/)).

### References
- https://rust-analyzer.github.io/manual.html