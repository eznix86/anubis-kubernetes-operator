# Anubis Firewall Kubernetes Operator

![TecharoHQ Logo](https://raw.githubusercontent.com/TecharoHQ/anubis/main/web/static/img/happy.webp)

Kubernetes operator for managing [Anubis](https://github.com/TecharoHQ/anubis) Firewall proxies with the `AnubisProxy` custom resource.

## Install

```bash
kubectl apply -f \
  "https://github.com/eznix86/anubis-kubernetes-operator/releases/download/v0.2.0/install.yaml"
```

This installs:
- the `AnubisProxy` CRD
- `anubis-operator-system` namespace
- operator RBAC
- the controller

After that, you create one or more `AnubisProxy` resources for the applications you want to protect.

## Upgrade

Apply the newer release manifest:

```bash
kubectl apply -f \
  "https://github.com/eznix86/anubis-kubernetes-operator/releases/download/v<version>/install.yaml"
```

This updates the installed operator to the selected release version.

## Uninstall

```bash
kubectl delete -f \
  "https://github.com/eznix86/anubis-kubernetes-operator/releases/download/v0.2.0/install.yaml"
```

Uninstalling the operator removes the controller and its resources.

## Usage

After the operator is installed, apply an `AnubisProxy` resource.

The operator supports two operating modes: **proxy** (default, Anubis as a reverse proxy) and **forwardAuth** (Traefik `forwardauth` middleware — Anubis is consulted per request but never sees the response body). Traefik is the only supported ingress for forwardAuth today.

For the full values reference used by the operator chart, see [helm-charts/anubis/README.md](helm-charts/anubis/README.md).

Before creating an `AnubisProxy`, create the Anubis signing key Secret:

For example:

```bash
kubectl create secret generic anubis-key \
  --namespace app \
  --from-literal=ED25519_PRIVATE_KEY_HEX="$(openssl rand -hex 32)"
```

> [!NOTE]
> The controller will not use this key. So the key should be created where the `AnubisProxy` is located (same namespace).

To pin a specific Anubis runtime version (`spec.anubis.image.tag` default is `latest`) per `AnubisProxy`, choose a tag from:

- https://github.com/TecharoHQ/anubis/tags

```yaml
apiVersion: anubis.techaro.dev/v1alpha1
kind: AnubisProxy
metadata:
  name: app
  namespace: app

spec:
  # Select an SVC you want to proxy
  target:
    service:
      name: app-service
      port: 80

  anubis:
    # Use a configMap for your custom Policies
    existingConfigMap: anubis-policy
    keys:
      existingSecret: anubis-key

    envExtra:
      - name: DIFFICULTY
        value: "4"

      - name: SERVE_ROBOTS_TXT
        value: "true"

      - name: OG_PASSTHROUGH
        value: "true"

      - name: OG_EXPIRY_TIME
        value: 24h

      - name: REDIRECT_DOMAINS
        value: "example.com"

  # You may also use gateway api
  ingress:
    enabled: true

    hosts:
      - host: example.com
        paths:
          - path: /
            pathType: Prefix

    tls:
      - hosts:
          - example.com
        secretName: example-tls
```

Apply the `AnubisProxy` manifest.

The operator will do the rest.

Examples:

- [config/samples/anubis_v1alpha1_anubisproxy.yaml](config/samples/anubis_v1alpha1_anubisproxy.yaml)
- [config/samples/anubis_v1alpha1_anubisproxy_k3s_dev_echo.yaml](config/samples/anubis_v1alpha1_anubisproxy_k3s_dev_echo.yaml)

## License

[MIT](LICENSE)

## Credits

The logo is owned by TecharoHQ.
