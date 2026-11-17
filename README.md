# Nosana Security Inspector launcher

Experimental password-protected delivery. This repository contains the encrypted
application and public decryption bootstrap, not the private development history.
Password holders can read/copy the application using `--show-source`.

This candidate has known coverage gaps and cannot certify a host as clean.
A private repository and a sharing password are separate access mechanisms.
No GitHub credentials are needed to download this launcher.

Use Bash, Python3.10+ on Linuxx86_64, curl or wget, and sudo. Download the complete
file, check its SHA256 from `releases/0.1.0-dev.1/build.json`, then execute it.
Do not execute an HTTP response directly as it streams. The launcher prompts for
the sharing password unless `NSI_PASSWORD` is set; that value is removed before
the installed inspector starts. No passwords or recovery identities are published.

Detailed reports are encrypted to the operator-provisioned recipient and remain
on the inspected host. Keep the recovery private key on a trusted review computer.

Default run: `inspect --deep --detach`. Source review: `--show-source`.
Lifecycle enrollment is separate and requires explicit `setup`.

Current source archive SHA256: `958889b0b1d834e4444ff00656a727280eec239dd1f6aa017504a2be2eb8cf57`.
Launcher SHA256: `5cf3edd826dd9007b6c54a55f9ab979f4065aae96fac695bd859b61e4d9d1a41`.
