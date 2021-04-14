# k8s-traefik-helm

### Repo
````
helm repo add traefik https://helm.traefik.io/traefik
````
### Create ns
````
kubectl create ns traefik-system
````

### values
````
additionalArguments:
  - --metrics.prometheus=true
  - --entrypoints.web.http.redirections.entryPoint.to=websecure
  - --entrypoints.web.http.redirections.entryPoint.scheme=https
  - --certificatesresolvers.default.acme.tlschallenge
  - --certificatesresolvers.default.acme.email=support@vmar.se
  - --certificatesresolvers.default.acme.storage=/cert/acme.json
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
    command: ["sh", "-c", "chmod -Rv 600 /cert"]
    volumeMounts:
    - name: cert
      mountPath: /cert
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
  name: cert
  path: /cert
  size: 128Mi 
pilot:
  enabled: false
  token: ''
podDisruptionBudget:
  enabled: false
podSecurityContext:
  fsGroup: null
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
    port: 80
    protocol: TCP
  websecure:
    expose: true
    exposedPort: 443
    port: 443
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
    drop: [ALL]
     add: [NET_BIND_SERVICE]
  readOnlyRootFilesystem: true
  runAsGroup: 0
  runAsNonRoot: false
  runAsUser: 0
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
