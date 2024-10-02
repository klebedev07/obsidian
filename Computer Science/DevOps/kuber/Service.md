Сервис (Service) в Kubernetes — это абстрактный объект, который определяет логический набор подов и политику доступа к ним. Сервисы создают слабую связь между подами, которые от них зависят. Сервис создаётся в формате YAML или JSON, как и все остальные объекты в Kubernetes. Как правило, набор [[Pod]] для сервиса определяется _селектором лейблов_ (label selector).

Service в первую очередь служит для внутрекластерной работы.

Сервис это по сути просто правило [[ipTables]] или [[IPVS]].

![[Pasted image 20231123205249.png]]

- _ClusterIP_ (по умолчанию) — открывает доступ к сервису по внутреннему IP-адресу в кластере. Этот тип делает сервис доступным только внутри кластера;

```yml 
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  ports:
  - port: 80
    targetPort: 80
  selector:
    app: my-app
  type: ClusterIP
```

- _NodePort_ — открывает сервис на том же порту каждого выбранного узла в кластере с помощью NAT. Делает сервис доступным вне кластера через `<NodeIP>:<NodePort>`. Является надмножеством ClusterIP. 
```yml
apiVersion: v1
kind: Service
metadata:
  name: my-service-np
spec:
  ports:
  - port: 80
    targetPort: 80
  selector:
    app: my-app
  type: NodePort
```

- _LoadBalancer_ — создает внешний балансировщик нагрузки в текущем облаке (если это поддерживается) и назначает фиксированный внешний IP-адрес для сервиса. Является надмножеством NodePort.  Работает только в облаках(amazon, yandex etc.)
```yml
apiVersion: v1
kind: Service
metadata:
  name: my-service-lb
spec:
  ports:
  - port: 80
    targetPort: 80
  selector:
    app: my-app
  type: LoadBalancer
```

- _ExternalName_ — открывает доступ к сервису по содержимому поля `externalName` (например, `example.com`), возвращая запись `CNAME` с его значением. При этом прокси не используется. Для этого типа требуется версия `kube-dns` 1.7+ или CoreDNS 0.0.8+. G По сути ведет на адрес указанный в externalName.
```yml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: LoadBalancer
  externalName: example.com
```


Примеры манифестов приложения для этих сервисов.  Как видим матчится по app: my-app

```yml
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
```

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 2
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
        readinessProbe:
          failureThreshold: 3
          httpGet:
            path: /
            port: 80
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 1
        livenessProbe:
          failureThreshold: 3
          httpGet:
            path: /
            port: 80
          periodSeconds: 10
          successThreshold: 1
          timeoutSeconds: 1
          initialDelaySeconds: 10
        resources:
          requests:
            cpu: 10m
            memory: 100Mi
          limits:
            cpu: 10m
            memory: 100Mi
        volumeMounts:
        - name: config
          mountPath: /etc/nginx/conf.d/
      volumes:
      - name: config
        configMap:
          name: my-configmap
```