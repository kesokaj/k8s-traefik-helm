# k8s-traefik-helm

### Repo
````
helm repo add traefik https://helm.traefik.io/traefik
````


### values
````
deployment:
  kind: deployment
  enabled: true
  replicas: 2
  initContainers:
  - name: volume-permissions
    image: busybox:1.31.1
    command: ["sh", "-c", "chmod -Rv 600 /certs/*"]
    volumeMounts:
    - name: data
      mountPath: /certs
additionalArguments:
  - --ping=true
  - --api.insecure=true
  - --api.dashboard=true
  - --metrics.prometheus=true
  - --certificatesresolvers.default.acme.tlschallenge
  - --certificatesresolvers.default.acme.email=support@vmar.se
  - --certificatesresolvers.default.acme.storage=/certs/acme.json
ports:
  traefik:
    expose: true
    exposedPort: 9000
    port: 9000
    protocol: TCP
  web:
    expose: true
    exposedPort: 80
    port: 8000
    protocol: TCP
  websecure:
    expose: true
    exposedPort: 443
    port: 8443
    protocol: TCP
    tls:
      enabled: true
      certResolver: "default"
service:
  enabled: true
  type: LoadBalancer
rbac:
  enabled: true
  namespaced: false
logs:
  access:
    enabled: true
providers:
  kubernetesCRD:
    enabled: true
  kubernetesIngress:
    enabled: true
podSecurityPolicy:
  enabled: false
nodeSelector:
  traefik: "true"
persistence:
  enabled: true
  accessMode: ReadWriteMany
  path: /certs
  size: 128Mi
securityContext:
  capabilities:
    drop: [ALL]
  readOnlyRootFilesystem: true
  runAsGroup: 65532
  runAsNonRoot: true
  runAsUser: 65532
podSecurityContext:
  fsGroup: 65532
````
