Использование конфигмапов (`ConfigMaps`) позволяет разделять конфигурационные файлы и контейнеры с приложениями, избавляя от необходимости упаковывать конфиги в docker-образ. 

```bash
kubectl create configmap <map-name> <data-source>
```

```yml
kubectl get configmaps <map-name> -o yaml 

---
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap-env
data:
  dbhost: postgresql
  DEBUG: "false"
...

```


**Пример [[Deployment]] с конфиг мапой**

```yaml

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - image: nginx:1.20
        name: nginx
        env:
        - name: TEST
          value: foo
        envFrom:
        - configMapRef:
            name: my-configmap-env
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 50m
            memory: 100Mi
          limits:
            cpu: 100m
            memory: 100Mi
...


```


Пример конфиг мапы в нестандартном виде
```yaml
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  default.conf: |
    server {
        listen       80 default_server;
        server_name  _;

        default_type text/plain;

        location / {
            return 200 '$hostname\n';
        }
    }
...

```


пример использования 

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - image: nginx:1.20
        name: nginx
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 50m
            memory: 100Mi
          limits:
            cpu: 100m
            memory: 100Mi
        volumeMounts:
        - name: config
          mountPath: /etc/nginx/conf.d/
      volumes:
      - name: config
        configMap:
          name: my-configmap
...

```




Посмотреть все переменные окружения 
```bash
kubectl exec -it my-deployment-< RANDOM > -- env
```