## Poetry

Install beta version
```
curl -sSL https://install.python-poetry.org | python3 - --version 1.2.0b3
```

Uninstall
```
curl -sSL https://install.python-poetry.org | python3 - --uninstall
```

Update
```
poetry self update
```

### Issues
- [pytorch and poetry](https://github.com/python-poetry/poetry/issues/4231)

## Trivia

### Upgrade Python on Ubuntu 18.04 LTS
```shell
> add-apt-repository ppa:deadsnakes/ppa
> apt install pythonX.X
```
```shell
> update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 1
> update-alternatives --install /usr/bin/python3 python3 /usr/bin/pythonX.X 2
> update-alternatives --config python3
```

### System Python should not be linked to the one installed by brew
```
> brew unlink python3
```