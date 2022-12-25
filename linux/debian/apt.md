**CLI warning:** APT does not have a stable CLI interface. Use with caution in scripts.

Checking whether PACKAGES are installed:
```
apt -qq list PACKAGES
```
`-q` is a synonym for `--quiet` option. The first `-q` prevents the word "Done" being printed. The second also prevents "Listing... " from being printed.
