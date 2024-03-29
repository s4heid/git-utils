# git-utils

Some utils for loading and creating ssh keys and tokens for authentication with git.

## Installation

This tool is intended to be used with an USB device on macOS. Other platforms are currently not supported.

1. Clone the repository

  ```console
  $ git clone git@github.com:s4heid/git-utils.git
  ```

2. Create a `config.yml` in the git-utils folder. Follow [configuration](#Configuration) for more information.

You also might want to create a wrapper script in the root of your USB device, e.g.

```console
$ cat > ./load <<EOF
#!/bin/bash
cd "$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
exec ./git-utils/load "$@"
EOF
$ chmod +x ./load
```

## Configuration

The load script expects a configuration file `config.yml`, which is located in
the git-utils repository.

| Parameter         | Required | Example                      | Description                                                |
| ----------------- | -------- | ---------------------------- | ---------------------------------------------------------- |
| `git.username`    | No       | `s4heid`                     | The username which will be set for the [git credential context](https://git-scm.com/docs/gitcredentials#Documentation/gitcredentials.txt-username). |
| `git.key`         | No       | `../id_rsa`                  | The path to the private ssh key relative to the load script. Default: ./id_rsa |
| `git.token`       | No       | `123456789abcdefghijkl`      | The git password which will be stored in the [git-credential-cache](https://git-scm.com/docs/git-credential-cache). |
| `git.host`        | No       | `mygithost`                  | The host which will be used for a specific git credential. Default: `github.com` |
| `ascii`           | No       | `./ascii.txt`                | The path to a text file which will be printed after loading the keys/tokens. Time to show off your [ascii art](https://en.wikipedia.org/wiki/ASCII_art) collection. |

### Example

```yaml
git:
  username: <my-user>
  token: <my-token>
  key: ./id_rsa
ascii: ./ascii.txt
```

## Usage

Insert your USB device and execute the load script and specify the time how long your keys should be loaded. See the help for more information about the usage:

```console
$ /Volumes/keys/load --help

Usage: load-keys

Remove all added identities and add a new identity with a lifetime

Options:
  -d | --dir=DIRECTORY     Filepath to keys. (default: same directory as load script)
  -t | --hours=DURATION    How many hours the key should be loaded (default: 1 hour)
       --eod               Load the private key until the end of the day (6 p.m.)
       --no-eject          Do not eject the usb device

  -h | --help              Show this help message
```

For security reasons, you might want to encrypt your USB device. Instructions can be found in this [excellent blog](http://tammersaleh.com/posts/building-an-encrypted-usb-drive-for-your-ssh-keys-in-os-x/).

## License

[Apache License, Version 2.0](./LICENSE)
