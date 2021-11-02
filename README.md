# k8s-traefik-helm

### Repo
````
helm repo add traefik https://helm.traefik.io/traefik
````
### Create ns
````
kubectl create ns traefik-system
````

### Redirect to https per ingressroute
````
---
apiVersion: traefik.containo.us/v1alpha1
kind: Middleware
metadata:
  name: http-to-https
  namespace: default
spec:
  redirectScheme:
    scheme: https
    permanent: true
````

### helm install
````
helm -n traefik-system install traefik traefik/traefik -f values.yaml
````

### values
````
additionalArguments:
  - --metrics.prometheus=true
  - --certificatesresolvers.default.acme.tlschallenge
  - --certificatesresolvers.default.acme.email=simon.ostling@arrow.com
  - --certificatesresolvers.default.acme.storage=/cert/acme.json
  - --entrypoints.web.http.redirections.entryPoint.to=websecure
  - --entrypoints.web.http.redirections.entryPoint.scheme=https  
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
    command: ["sh","-c", "touch /cert/acme.json && chmod 600 /cert/acme.json"]
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
# nodeSelector:
#  traefik: 'true'
persistence:
  accessMode: ReadWriteOnce
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
