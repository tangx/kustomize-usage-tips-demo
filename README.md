# kusutomize-usage-tips-demo

## usage

```bash
$ kubectl kustomize overlays/online
```

**result** 

```yaml
# make dryrun.online

---

kubectl kustomize overlays/online
apiVersion: v1
data:
  app.cfg: |
    # Uncomment the next line to enable TCP/IP SYN cookies
    # See http://lwn.net/Articles/277146/
    # Note: This may impact IPv6 TCP sessions too
    #net.ipv4.tcp_syncookies=1

    # Uncomment the next line to enable packet forwarding for IPv4
    net.ipv4.ip_forward=1
  app.conf: |
    # Uncomment the next line to enable TCP/IP SYN cookies
    # See http://lwn.net/Articles/277146/
    # Note: This may impact IPv6 TCP sessions too
    #net.ipv4.tcp_syncookies=1

    # Uncomment the next line to enable packet forwarding for IPv4
    net.ipv4.ip_forward=1
kind: ConfigMap
metadata:
  name: app-config
---
apiVersion: v1
data:
  app.cfg: IyBVbmNvbW1lbnQgdGhlIG5leHQgbGluZSB0byBlbmFibGUgVENQL0lQIFNZTiBjb29raWVzCiMgU2VlIGh0dHA6Ly9sd24ubmV0L0FydGljbGVzLzI3NzE0Ni8KIyBOb3RlOiBUaGlzIG1heSBpbXBhY3QgSVB2NiBUQ1Agc2Vzc2lvbnMgdG9vCiNuZXQuaXB2NC50Y3Bfc3luY29va2llcz0xCgojIFVuY29tbWVudCB0aGUgbmV4dCBsaW5lIHRvIGVuYWJsZSBwYWNrZXQgZm9yd2FyZGluZyBmb3IgSVB2NApuZXQuaXB2NC5pcF9mb3J3YXJkPTEK
  app.conf: IyBVbmNvbW1lbnQgdGhlIG5leHQgbGluZSB0byBlbmFibGUgVENQL0lQIFNZTiBjb29raWVzCiMgU2VlIGh0dHA6Ly9sd24ubmV0L0FydGljbGVzLzI3NzE0Ni8KIyBOb3RlOiBUaGlzIG1heSBpbXBhY3QgSVB2NiBUQ1Agc2Vzc2lvbnMgdG9vCiNuZXQuaXB2NC50Y3Bfc3luY29va2llcz0xCgojIFVuY29tbWVudCB0aGUgbmV4dCBsaW5lIHRvIGVuYWJsZSBwYWNrZXQgZm9yd2FyZGluZyBmb3IgSVB2NApuZXQuaXB2NC5pcF9mb3J3YXJkPTEK
kind: Secret
metadata:
  name: app-secret
type: Opaque
---
apiVersion: v1
data:
  MYSQL_PASSWD: cm9vdDEyMw==
kind: Secret
metadata:
  name: mysql-secret
type: Opaque
---
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: nginx-demo
  name: nginx-demo
spec:
  ports:
  - name: 80-80
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx-demo
  type: ClusterIP
status: null
---
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx-demo
  name: nginx-demo
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx-demo
  strategy: {}
  template:
    metadata:
      labels:
        app: nginx-demo
    spec:
      containers:
      - image: cr.aliyun.com/nginx:latest
        name: nginx
        resources:
          limits:
            cpu: 1
            memory: 1Gi
          requests:
            cpu: 1
            memory: 1Gi
      nodeSelector:
        node-env: online

```
