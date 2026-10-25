# ansible-cloudflared

Installs the cloudflared binary at `/opt/cloudflared/bin/cloudflared` on
RedHat-family hosts with amd64 or arm64 processors. The binary is root-owned
with mode `0755`. The role does not configure tunnels or manage services.

## Variables

- `cloudflared_version` defaults to `latest`. Each run resolves GitHub's latest
  stable release. Set an exact release tag such as `"2026.10.3"` to pin a version;
  changing the pin can also downgrade the installation. Empty and null values
  are invalid.
- `cloudflared_checksum` defaults to `""`. Set a 64-character SHA-256 digest to
  override the checksum for the resolved version and architecture. Prefer an
  exact version pin when supplying a checksum.
- `cloudflared_checksums` defaults to embedded checksums for `2026.9.1` on
  `amd64` and `arm64`. It maps exact version tags to architecture-to-SHA-256
  mappings. When no explicit or embedded checksum applies, the role uses
  GitHub release metadata. Downloads always require checksum verification.

Run with privilege escalation and gathered facts. GitHub access is required on
the managed host for installation and on every run when tracking `latest`.
Transfers use the shared `escapace.utilities` helpers with bounded retries;
metadata retries honor GitHub's rate-limit instructions when the wait fits their
budget. Unrecoverable failures stop the run rather than retaining a potentially
outdated version. An unchanged fixed version needs no release lookup or download.

The caller controls runtime configuration and service management. The role
leaves existing configuration, helper scripts, services, and accounts untouched;
it does not clean up artifacts from earlier installations.
