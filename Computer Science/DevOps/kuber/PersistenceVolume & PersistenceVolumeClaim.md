
**PV** - отрезанный кусок памяти, по сути ссылки на дисковые ресурсы. Нарезаются админами или настраивается provisioner который автоматически создает по запросу от pvc
**PVC** - это уже конкретный mount который прицепится к [[Pod]] 
PVC  - это по сути наше пожелание на то какое мы хотим хранилище себе подключить и ссылка на эту абстрацкию цепляется к нашем поду.




![[Pasted image 20231109220716.png]]

Как это выглядит:
![[Pasted image 20231109220842.png]]


Код.

```yml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: local-storage
provisioner: k8s.io/minikube-hostpath
volumeBindingMode: WaitForFirstConsumer
```

```yml
---
kind: PersistentVolume
apiVersion: v1
metadata:
  name: node1-pv1
spec:
  capacity:
    storage: 5Gi
  volumeMode: Filesystem
  accessModes:
  - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  storageClassName: local-storage
  local:
    path: /local/pv1
  nodeAffinity:
    required:
      nodeSelectorTerms:
        - matchExpressions:
            - key: kubernetes.io/hostname
              operator: In
              values:
                - 192.168.64.2

```


PersistentVolumeClaim 

```yml
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: fileshare
spec:
  storageClassName: local-storage
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 1Gi

```

```yaml
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: fileshare
data:
  default.conf: |
    server {
      listen       80 default_server;
      server_name  _;

      default_type text/plain;

      location / {
        return 200 '$hostname\n';
      }

      location /files {
        alias /data;
        autoindex on;
        client_body_temp_path /tmp;
        dav_methods PUT DELETE MKCOL COPY MOVE;
        create_full_put_path on;
        dav_access user:rw group:rw all:r;
      }
    }
```


```yml
---
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: fileshare
spec:
  replicas: 2
  selector:
    matchLabels:
      app: fileshare
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: fileshare
    spec:
      initContainers:
      - image: busybox
        name: mount-permissions-fix
        command: ["sh", "-c", "chmod 777 /data"]
        volumeMounts:
        - name: data
          mountPath: /data
      containers:
      - image: centosadmin/reloadable-nginx:1.12
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
          mountPath: /etc/nginx/conf.d
        - name: data
          mountPath: /data
      volumes:
      - name: config
        configMap:
          name: fileshare
      - name: data
        persistentVolumeClaim:
          claimName: fileshare

```



# PV/PVC  
Не работает на миникубе из-за каких то локальных заморочек
1) Применяем манифест pvc.yml  
  
```bash 
kubectl apply -f sc.yaml
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml  
kubectl get pvc
kubectl get pv
```  
  
2) Запустим приложение, использующее PV  
  
```bash  
kubectl apply -f .
```  
  
3) Посмотрим describe и смонтированные тома в контейнере  
  
```bash  
kubectl describe pod fileshare-<TAB>kubectl exec -it fileshare-<TAB> -- df -h
```  
  
4) Очищаем  
  
```bash  
kubectl delete -f .
```