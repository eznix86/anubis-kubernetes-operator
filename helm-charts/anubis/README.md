# Anubis Chart Values

Reference for the values consumed by the Anubis operator chart.

For operator installation and a full `AnubisProxy` example, see the top-level [README](../../README.md).

For upstream Anubis runtime behavior and configuration details, see:

- https://github.com/TecharoHQ/anubis/tags
- https://anubis.techaro.lol/docs/admin/installation#configuration

## Values

For upstream runtime configuration details behind values such as `anubis.envExtra` and `anubis.policyFile`, see:

- https://anubis.techaro.lol/docs/admin/installation#configuration

| Key | Default | Description |
| --- | --- | --- |
| `nameOverride` | `""` | Override the chart name portion used in generated resource names. |
| `fullnameOverride` | `""` | Override the full generated resource name. |
| `target.service.name` | `""` | Name of the backend Kubernetes Service Anubis should protect. |
| `target.service.port` | `80` | Port of the backend Kubernetes Service Anubis should protect. |
| `anubis.image.repository` | `ghcr.io/techarohq/anubis` | Anubis runtime image repository. |
| `anubis.image.tag` | `latest` | Anubis runtime image tag. |
| `anubis.image.pullPolicy` | `IfNotPresent` | Image pull policy for the Anubis container. |
| `anubis.replicas` | `1` | Number of Anubis replicas to run. |
| `anubis.port` | `8923` | Container port exposed by the Anubis HTTP server. |
| `anubis.metrics.enabled` | `true` | Enable the Anubis metrics endpoint in the pod. |
| `anubis.metrics.port` | `9090` | Metrics port exposed by the Anubis container. |
| `anubis.metrics.service.enabled` | `false` | Create a dedicated Service for metrics scraping. |
| `anubis.metrics.service.type` | `ClusterIP` | Service type for the dedicated metrics Service. |
| `anubis.metrics.service.port` | `9090` | Service port for the dedicated metrics Service. |
| `anubis.metrics.service.annotations` | `{}` | Extra annotations for the dedicated metrics Service. |
| `anubis.metrics.service.labels` | `{}` | Extra labels for the dedicated metrics Service. |
| `anubis.targetOverride` | `""` | Override the computed `TARGET` URL when Anubis should proxy somewhere custom. |
| `anubis.policyFile` | `/etc/anubis/botPolicies.yaml` | Mount path used for inline or external policy configuration. |
| `anubis.envExtra` | `[DIFFICULTY, SERVE_ROBOTS_TXT, OG_PASSTHROUGH, OG_EXPIRY_TIME]` | Extra environment variables passed directly to the Anubis container. |
| `anubis.keys.existingSecret` | `""` | Existing Secret name containing `ED25519_PRIVATE_KEY_HEX`. |
| `anubis.config` | `""` | Inline `botPolicies.yaml` content. Generates a ConfigMap when set. |
| `anubis.existingConfigMap` | `""` | Existing ConfigMap name providing `botPolicies.yaml`. |
| `anubis.resources` | requests/limits block | Container resource requests and limits for Anubis. |
| `anubis.persistence.enabled` | `true` | Provision a PVC and mount it for persistent challenge state. |
| `anubis.persistence.size` | `1Gi` | PVC size. |
| `anubis.persistence.storageClassName` | `""` | Storage class for the PVC. Empty = default StorageClass. |
| `anubis.persistence.existingClaim` | `""` | Use an existing PVC instead of creating one. |
| `anubis.persistence.mountPath` | `/data` | Mount path inside the container. |
| `anubis.securityContext.runAsUser` | `1000` | Pod-level user ID for the Anubis pod. |
| `anubis.securityContext.runAsGroup` | `1000` | Pod-level group ID for the Anubis pod. |
| `anubis.securityContext.runAsNonRoot` | `true` | Require the Anubis pod to run as a non-root user. |
| `anubis.securityContext.seccompProfile.type` | `RuntimeDefault` | Pod-level seccomp profile for the Anubis pod. |
| `anubis.containerSecurityContext.allowPrivilegeEscalation` | `false` | Disable privilege escalation for the Anubis container. |
| `anubis.containerSecurityContext.capabilities.drop` | `[ALL]` | Linux capabilities dropped from the Anubis container. |
| `service.type` | `ClusterIP` | Service type for the main Anubis HTTP Service. |
| `service.port` | `80` | Service port for the main Anubis HTTP Service. |
| `networkPolicy.enabled` | `false` | Create a NetworkPolicy for Anubis pods. HTTP stays open by default. |
| `networkPolicy.metrics.from` | `[]` | NetworkPolicy peers allowed to reach the metrics port. |
| `ingress.enabled` | `false` | Create an Ingress pointing to the main Anubis Service. |
| `ingress.className` | `""` | Ingress class name for the generated Ingress. |
| `ingress.annotations` | `{}` | Extra annotations for the generated Ingress. |
| `ingress.hosts` | `[{host: "", paths: [{path: "/", pathType: Prefix}]}]` | Host and path rules for the generated Ingress. |
| `ingress.tls` | `[]` | TLS configuration for the generated Ingress. |
| `gateway.enabled` | `false` | Create an `HTTPRoute` pointing to the main Anubis Service. |
| `gateway.parentRefs` | `nil` | Gateway API parent references for the generated `HTTPRoute`. |
| `gateway.hostnames` | `nil` | Gateway API hostnames for the generated `HTTPRoute`. |
| `mode` | `"proxy"` | Operating mode: `"proxy"` (default, Anubis as a reverse proxy) or `"forwardAuth"` (Traefik `forwardauth` middleware). |
| `forwardAuth.redirectDomains` | `[]` | Domains Anubis can redirect to (supports globs). Defaults to `forwardAuth.traefik.host` when empty. |
| `forwardAuth.cookieDomain` | `""` | Cookie domain for the challenge cookie. Defaults to `forwardAuth.traefik.host` when empty. |
| `forwardAuth.traefik.host` | `""` | Public hostname routed by the generated Traefik `IngressRoute`. **Required when `mode: forwardAuth`.** |
| `forwardAuth.traefik.entryPoints` | `[websecure]` | Traefik entry points the generated `IngressRoute` binds to. |
| `forwardAuth.traefik.tlsSecret` | `""` | Existing Secret name with the TLS cert for the generated `IngressRoute`. |
| `forwardAuth.traefik.wwwRedirect` | `false` | Generate a `redirectRegex` Middleware that redirects `www.<host>` → `<host>`, and wire it into the generated `IngressRoute`. `host` must be a bare hostname (no `www.` prefix) when enabled. |
| `forwardAuth.traefik.bypassRoute.enabled` | `true` | Generate a high-priority bypass route for Anubis's challenge assets under `/.within.website/`. |
| `forwardAuth.traefik.ingressRoute.annotations` | `{}` | Annotations applied to the generated `IngressRoute`. |
| `forwardAuth.traefik.ingressRoute.extraRoutes` | `[]` | Extra `Route` entries appended to the generated `IngressRoute` (e.g. additional hosts or paths). |
| `forwardAuth.traefik.middleware.annotations` | `{}` | Annotations applied to the generated Traefik `Middleware`. |
| `forwardAuth.traefik.middleware.spec` | `{}` | Middleware-level fields (e.g. `maxResponseBodySize`) merged at the spec root. |
| `forwardAuth.traefik.middleware.forwardAuth` | `{}` | `forwardAuth`-level fields merged into `spec.forwardAuth`. Sensible defaults for `trustForwardHeader`, `preserveLocationHeader`, `preserveRequestMethod`, and forwarded headers are applied first. |

## Modes

The chart supports two operating modes, switched by `mode`:

### `proxy` (default)

Anubis sits in the request path as a reverse proxy.

```
User → Ingress / HTTPRoute → Anubis → Backend
```

Anubis serves the challenge page when needed, then proxies the verified request to the backend. The chart creates the `Deployment`, `Service`, optional `Ingress`, and optional `HTTPRoute` for this mode.

### `forwardAuth` (Traefik only)

Traefik consults Anubis as an **external authorizer** via the `forwardauth` middleware, and Anubis is never in the data path of the response.

```
User → Traefik → Backend
              ↘ Anubis (sub-request to /api/check)
```

For each request, Traefik sends a parallel sub-request to `http://<release>-anubis.<ns>.svc:80/.within.website/x/cmd/anubis/api/check`. Anubis returns `200` (cookie valid) or `302/401` (challenge needed). Traefik enforces the verdict before forwarding to the backend.

The chart generates a `traefik.io/v1alpha1` `Middleware` (`<release>-anubis-auth`) and an `IngressRoute` that wires it up, plus an optional high-priority bypass route for `/.within.website/` so the challenge assets are served without recursion.

> **Note:** This mode is **Traefik-only** for now. The middleware spec is a `traefik.io/v1alpha1` CRD, and the `forwardauth` semantics don't map cleanly to other ingress controllers (NGINX, Gateway API, etc.). For non-Traefik clusters, use `mode: proxy`.

**Trade-offs vs. proxy mode:**

- The backend sees the original client IP and headers unchanged.
- Anubis does not buffer backend responses, so large/static workloads benefit.
- Two requests per user request until the challenge cookie is set.

## Examples

### Secret

Create the signing key Secret referenced by `anubis.keys.existingSecret`:

```bash
kubectl create secret generic anubis-key \
  --namespace default \
  --from-literal=ED25519_PRIVATE_KEY_HEX="$(openssl rand -hex 32)"
```

### Inline policy file

Use `anubis.config` to generate the policy ConfigMap from your `AnubisProxy` values:

```yaml
anubis:
  keys:
    existingSecret: anubis-key
  config: |
    rules:
      - path: /login
        rateLimit: 5
      - path: /api/v1/internal
        action: block
```

### Extra environment variables

Use `anubis.envExtra` for upstream Anubis settings that are not exposed as first-class values:

```yaml
anubis:
  envExtra:
    - name: REDIRECT_DOMAINS
      value: app.example.com
    - name: SLOG_LEVEL
      value: INFO
```

### Runtime version override

Use `anubis.image.tag` to pin a specific upstream Anubis version for one `AnubisProxy`.
Available tags are published at:

- https://github.com/TecharoHQ/anubis/tags

```yaml
anubis:
  image:
    tag: latest
```

### Traefik forwardAuth

Set `mode: forwardAuth` and provide the public hostname routed by Traefik. The chart renders the `Middleware` and `IngressRoute` automatically — no extra resources to manage.

```yaml
mode: forwardAuth

target:
  service:
    name: forgejo
    port: 80

forwardAuth:
  traefik:
    host: git.example.com
    tlsSecret: git-example-tls
```

`forwardAuth.traefik.host` is required; the chart fails to render otherwise. To attach a TLS cert to the generated `IngressRoute`, point `tlsSecret` at an existing `kubernetes.io/tls` Secret.
