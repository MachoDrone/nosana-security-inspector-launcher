# Nosana Security Inspector launcher

Experimental version **0.1.0-dev.3**. The private application is password-encrypted;
its decryption bootstrap is public. Password holders can read and copy the complete
application source with `--show-source`; no GitHub credentials are required.

This candidate reports explicit coverage gaps. It cannot certify a host as clean.
193 tests passed; real-engine YARA, an official memory fixture, harmless kernel
taints and isolated encrypted-filesystem checks are recorded in the application.
Known-positive rootkit detection and safe automatic job maintenance remain unvalidated.

Compatible integrity checks now use two bounded lanes: module inspection alongside
persistence/process checks, after priority observations. CPU/memory/read budgets do
not increase. Add `--workers 1` to select serial execution. Memory capture, heavy
analysis and cleanup retain their ordering.

## One-paste launch

Requires Linux x86_64, Bash, Python 3.10+, systemd, sudo, sha256sum, and curl or wget.
First provision your **public** age recipient at
`/etc/nosana-security-inspector/recipient.txt` (root-owned, mode0644); keep its
private recovery identity on your trusted review computer.

This command downloads, verifies the complete immutable launcher, prompts for the
sharing password, starts a detached scan and removes the temporary download.
There is no manual pre-download step. Copy the entire block into the host terminal:

```bash
bash -s -- inspect --deep --detach <<'NSI_LAUNCH'
set -euo pipefail
nsi_file=$(mktemp /tmp/nsi-launch.XXXXXXXX)
trap 'rm -f -- "$nsi_file"' EXIT
nsi_url="https://raw.githubusercontent.com/MachoDrone/nosana-security-inspector-launcher/7fcf52b8807af726f64de06f4d3f5f6954d275d6/releases/0.1.0-dev.3/launch.sh"
if command -v curl >/dev/null 2>&1; then
  curl --fail --silent --show-error --location --proto '=https' --proto-redir '=https' --connect-timeout 10 --max-time 180 "$nsi_url" -o "$nsi_file"
else
  wget --https-only --timeout=30 --tries=1 -qO "$nsi_file" "$nsi_url"
fi
printf '%s  %s\n' "95cc5749bae28c1befc8108192b794177de392df2d99c617596ad28373c75904" "$nsi_file" | sha256sum --check --status
bash "$nsi_file" "$@"
NSI_LAUNCH
```

For source review, replace `inspect --deep --detach` on the first line with
`--show-source`. For progress, use `status`; for run history, `list-runs`.
The launcher prompts through the terminal unless `NSI_PASSWORD` is already set;
it removes that variable before launching the installed application.

Detailed evidence is age-encrypted and stays on the inspected host. No automatic
uploads, job interruptions, reboots, host package installations or hook enrollment
occur. Lifecycle enrollment requires separate explicit `setup`. Deep acquisition
skips unsupported/missing privacy or exact-symbol prerequisites and reports them.

Source archive SHA256: `566fc9f88d040c71da6ac552b8357a33c0406702a5d2037485141d1f13d3b4c4`.
Launcher SHA256: `95cc5749bae28c1befc8108192b794177de392df2d99c617596ad28373c75904`.
Launcher size: **16,730,752 bytes**.
[Build metadata](releases/0.1.0-dev.3/build.json).

No sharing password, private recovery identity, plaintext application archive or
private host notes are published here. Previous immutable releases remain available
for traceability; use the version pinned above for new runs.
