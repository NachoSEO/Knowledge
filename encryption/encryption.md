# Encryption
Technologies that you can use to encrypt/decrypt data.

You can encrypt a file using:
* Helm secrets: Plugin of helm that encrypts & decrypts files. Uses SOPS as its main tool
* SOPS: Tool to manage secrets using PGP (Pretty Good Privacy) algorithm
* GPG: Tool to create keys, encrypt & decrypt files using PGP algorithm
* [Git-crypt](https://github.com/AGWA/git-crypt)

## Scheme
* `Helm secrets` (Helm plugin that runs SOPS under the hood)
  * `SOPS` <-- Tool to manage secrets (Uses PGP as encryption algorithm)
    * `PGP Pretty Good Privacy` (encryption algorithm) <-- `GnuPG` tool to create public/private keys.


## Installation

SOPS
```sh
brew install sops
```

GPG
```sh
brew install gnupg
```

## Usage

### GPG (GnuPG)
Add these commands to your `.zshrc` file:

```sh
GPG_TTY=$(tty)
export GPG_TTY
```

1. Generate a key pair: `gpg --gen-key`
    1. To list all of the keys: `gpg --list-keys`
2. Encrypt a file attaching to a recipient (i.e 'Nacho Mascort)': `gpg --encrypt --recipient <person_name> <filename>` 
    1. or `gpg -e -r <person_name> <filename>`
3. A file encrypted with a `.gpg` extension will be created
4. Decrypt: `gpg --decrypt <filename.gpg>`

[More info](https://blog.ghostinthemachines.com/2015/03/01/how-to-use-gpg-command-line/)


#### Sign commits
You can sign commits using your GPG keys in order to verified them in github. When you commit new changes just add the flag `-S`:

`git commit -S -m your commit message`

Be sure that you [tell Git about your key](https://docs.github.com/en/github/authenticating-to-github/telling-git-about-your-signing-key)


### SOPS (Secrets Operations)
SOPS will only encrypt the value not the key.

1. Add a config file in the root `.sops.yaml` with the PGP fingerprint of the public key (Use the  `gpg --list-keys` to get the fingerprint
```yaml
creation_rules:
  - pgp: '05DCDCA00197777174F8D9AF2C70C53546A78B7F'
```
2. Encrypt the file: `sops -e <filename>` (to print the output in the console)
    * To save the data encrypted: `sops -e <filename> > <filename_encrypted>`
    ```yaml
    key1:
        key2: ENC[AES256_GCM,data:tOOHUw==,iv:6A24EmCX3nDFUW/69hsxHwC6qqgVyr5BdLbr3eoYzCg=,tag:S65s1rjHzukoGgVq19fIxg==,type:str]
    sops:
        kms: []
        gcp_kms: []
        azure_kv: []
        hc_vault: []
        age: []
        lastmodified: "2021-04-27T16:40:24Z"
        mac: ENC[AES256_GCM,data:eAuZyHjBq5drfJCOZFwJjAJADN48hi1DpqvFhZV2OLUQy/wsCRqkDKGOjMvphcwgr/Pk1Wm+yiyA9yaRUj5EdKIMW1jYoQFlIf1l0eWWVqUVXev81BhzVzhGjQ7mdipv7kPqxUErvr8ZAC/z9EfRpyIGdCB2LE7fhmOyLVwQBtU=,iv:zF0FzzJXEO/+MEWnHsigkXihSAgM6/Bxted+mUWhdgI=,tag:rZYQe/QaSptCbt+vmc05lA==,type:str]
        pgp:
            - created_at: "2021-04-27T16:40:24Z"
              enc: |
                -----BEGIN PGP MESSAGE-----

                hF4DfYWZxTIQvLASAQdA0a3YBelke1ueJJZufmKLg+PXX84jMw/oqvuef5FsjQUw
                cpmjYOYcrliypcBBMlN79pT3qQaaQ74/Wg25VxPLKcMIjowMkjh/pOPlONYcrNZ2
                1GgBCQIVfts1k5rXWxX3Kt1TNrcxj6+aD23Pr6BMrP8wUxG7YLTmTLjMZ7gkjv4K
                x7er/8OrVy3/nYkTF8izbNIvMQztxc+wxflwwYDlTFs8Eh9FgJFQ+JtJ77nKjjU9
                oahcozwo4DIm1w==
                =JAfZ
                -----END PGP MESSAGE-----
              fp: 05DCDCA00197777174F8D9AF2C70C53546A78B7F
        unencrypted_suffix: _unencrypted
        version: 3.7.1
    ```

3. Decrypt the file: `sops -d <filename>`
  * Output:
  ```yaml
  key1:
    key2: hello
  ```

### Helm secrets
* View: `helm secrets view <filename>`
* Encrypt: `helm secrets enc <filename>`
  * It will encrypt the same input file
* Decrypt: `helm secrets dec <filename>`
  * It will created a new `.dec` file
* Decrypt-encrypt: `helm secrets edit <filename>`