# Nosana Security Inspector launcher

Experimental version **0.1.0-dev.2**. The private application is password-encrypted;
its decryption bootstrap is public. Password holders can read and copy the complete
application source with `--show-source`; no GitHub credentials are required.

This candidate reports explicit coverage gaps. It cannot certify a host as clean.
175 tests passed; real-engine YARA, an official memory fixture, harmless kernel
taints and isolated encrypted-filesystem checks are recorded in the application.
Known-positive rootkit detection and safe automatic job maintenance remain unvalidated.

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
nsi_url="https://raw.githubusercontent.com/MachoDrone/nosana-security-inspector-launcher/b9169e026cca33d16889b6fa60ef0fc8df433175/releases/0.1.0-dev.2/launch.sh"
if command -v curl >/dev/null 2>&1; then
  curl --fail --silent --show-error --location --proto '=https' --proto-redir '=https' --connect-timeout 10 --max-time 180 "$nsi_url" -o "$nsi_file"
else
  wget --https-only --timeout=30 --tries=1 -qO "$nsi_file" "$nsi_url"
fi
printf '%s  %s\n' "e3fa85d54057d4afaf999f7c9717f6eb9dceb1ecbc481fb0071f99970375d88b" "$nsi_file" | sha256sum --check --status
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

Source archive SHA256: `6414145aaabb0bcc3d444899655770303ceea38731466f16c70f6f392d156260`.
Launcher SHA256: `e3fa85d54057d4afaf999f7c9717f6eb9dceb1ecbc481fb0071f99970375d88b`.
Launcher size: **16,729,272 bytes**.
[Build metadata](releases/0.1.0-dev.2/build.json).

No sharing password, private recovery identity, plaintext application archive or
private host notes are published here. Previous immutable releases remain available
for traceability; use the version pinned above for new runs.
