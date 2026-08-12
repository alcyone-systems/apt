# Alcyone Systems APT repository

Signed Debian packages for [Tester](https://alcyone-systems.com/product/),
an on-prem test automation appliance. Published today for `arm64`
(Raspberry Pi and other 64-bit ARM hosts); `amd64` is built by CI and lands
here with the next release.

```sh
curl -fsSL https://alcyone-systems.com/apt/alcyone-archive-keyring.asc \
  | sudo tee /usr/share/keyrings/alcyone-archive-keyring.asc >/dev/null

echo 'deb [signed-by=/usr/share/keyrings/alcyone-archive-keyring.asc] https://alcyone-systems.com/apt stable main' \
  | sudo tee /etc/apt/sources.list.d/alcyone.list

sudo apt update && sudo apt install alcyone-tester
```

`signed-by` scopes this key to this repository alone, so it can never vouch
for any other source configured on the machine.

APT owns updates for an installation made this way: `apt upgrade` applies
them, and Tester's own updater stands down so the two never compete for the
same files. Run `tester update check` to see which channel an installation
is on.

## Where the bytes come from

`apt` reads the signed metadata in this repository, then downloads the
package itself from a release asset — the archive root redirects there,
because a ~195 MB package is past what a git host will store. That costs
nothing in trust: the SHA-256 in the signed `Release` file is what proves a
package genuine, and `apt` refuses any download that does not match it,
whichever host served the bytes.

## Verifying a package by hand

```sh
gpg --verify alcyone-tester_1.0.1-1_arm64.deb.asc alcyone-tester_1.0.1-1_arm64.deb
```

Signing key fingerprint:

```
EA26 D4FB A599 BB33 77F4  33FC AFF7 7FFE DB2E 35E2
```
