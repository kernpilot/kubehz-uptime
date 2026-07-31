# kubehz-uptime

External dead-man watchdog for the [kubehz](https://kubehz.cloud) public plane.

Every 15 minutes a GitHub-hosted runner — infrastructure entirely outside the
platform it watches — probes the public endpoints:

- `https://api.kubehz.cloud/`
- `https://app.kubehz.cloud/`
- `https://auth.kubehz.cloud/`

**Alive** means: public DNS resolves, the load balancer answers, the TLS
certificate verifies against public CAs, and the edge proxy returns any
non-5xx HTTP response. A connect failure, TLS failure, timeout, or 5xx fails
the run — the red workflow pages the maintainers and opens a deduplicated
`deadman` issue.

Why a separate public repo: the platform's own alerting stack lives inside
the cluster it monitors, so a full-cluster outage is silent by construction.
This watchdog shares no fate with it. There are no secrets here — the probe
is unauthenticated against public hostnames.

A monthly keepalive commit stops GitHub from disabling the schedule after 60
days of repo inactivity.
