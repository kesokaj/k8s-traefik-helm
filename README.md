# k8s-traefik-helm

### Repo
````
helm repo add traefik https://helm.traefik.io/traefik
````


### values
````
additionalArguments:
  - --ping=true
  - --api.insecure=true
  - --api.dashboard=true
  - --metrics.prometheus=true
  - --certificatesresolvers.default.acme.tlschallenge
  - --certificatesresolvers.default.acme.email=support@vmar.se
  - --certificatesresolvers.default.acme.storage=/certs/acme.json
additionalVolumeMounts: []
affinity: {}
autoscaling:
  enabled: false
deployment:
  additionalContainers: []
  additionalVolumes: []
  annotations:
    metallb.universe.tf/address-pool: traefik
  enabled: true
  imagePullSecrets: []
  initContainers:
  - name: volume-permissions
    image: busybox:1.31.1
    command: ["sh", "-c", "chmod -Rv 600 /data/*"]
    volumeMounts:
    - name: data
      mountPath: /data
  kind: Deployment
  labels: {}
  podAnnotations: {}
  podLabels: {}
  replicas: 1
env: []
envFrom: []
experimental:
  kubernetesGateway:
    appLabelSelector: traefik
    certificates: []
    enabled: false
  plugins:
    enabled: false
globalArguments:
  - '--global.checknewversion'
  - '--global.sendanonymoususage'
hostNetwork: false
image:
  name: traefik
  pullPolicy: IfNotPresent
  tag: 'v2.4.8'
ingressClass:
  enabled: false
  isDefaultClass: false
ingressRoute:
  dashboard:
    annotations: {}
    enabled: true
    labels: {}
logs:
  access:
    enabled: true
    fields:
      general:
        defaultmode: keep
        names: {}
      headers:
        defaultmode: drop
        names: {}
    filters: {}
  general:
    level: ERROR
nodeSelector:
  traefik: 'true'
persistence:
  accessMode: ReadWriteMany
  annotations: {}
  enabled: true
  name: data
  path: /data
  size: 128Mi
pilot:
  enabled: false
  token: ''
podDisruptionBudget:
  enabled: false
podSecurityContext:
  fsGroup: 65532
podSecurityPolicy:
  enabled: false
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
      certResolver: 'default'
      domains: []
      enabled: true
      options: ''
priorityClassName: ''
providers:
  kubernetesCRD:
    enabled: true
    namespaces: []
  kubernetesIngress:
    enabled: true
    namespaces: []
    publishedService:
      enabled: false
rbac:
  enabled: true
  namespaced: false
resources: {}
rollingUpdate:
  maxSurge: 1
  maxUnavailable: 1
securityContext:
  capabilities:
    drop:
      - ALL
  readOnlyRootFilesystem: true
  runAsGroup: 65532
  runAsNonRoot: true
  runAsUser: 65532
service:
  annotations: {}
  enabled: true
  externalIPs: []
  labels: {}
  loadBalancerSourceRanges: []
  spec: {}
  type: LoadBalancer
serviceAccount:
  name: ''
serviceAccountAnnotations: {}
tlsOptions: {}
tolerations: []
volumes: []

````
