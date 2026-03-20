# kubara apt repository

This repository publishes the signed APT repository for `kubara`.

GitHub Pages serves the generated repository root with:

- `apt-public.key`
- `dists/stable/...`
- `pool/main/...`

## Triggering

The workflow can run in two ways:

1. `repository_dispatch` (`type: publish-apt`) from `kubara-io/kubara`
2. Manual `workflow_dispatch` with input `release_tag` (e.g. `v0.7.0`)

## Required secrets

Add these repository secrets in `kubara-io/apt`:

- `APT_GPG_PRIVATE_KEY`: ASCII-armored private key used to sign `Release` metadata
- `GPG_PASSPHRASE`: passphrase for the private key (optional if key has no passphrase)

## Expected source entry

Use this in `/etc/apt/sources.list.d/kubara.list`:

```text
deb [signed-by=/etc/apt/keyrings/kubara.gpg] https://kubara-io.github.io/apt stable main
```

And import the key:

```bash
sudo install -d -m 0755 /etc/apt/keyrings
curl -fsSL https://kubara-io.github.io/apt/apt-public.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubara.gpg
sudo apt update
```
