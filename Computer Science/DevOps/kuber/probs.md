Есть три вида проб в кубере. 

- Liveness Probe
- Readiness probe
- StartUp probe


![[Pasted image 20231120224540.png]]


Liveness и readiness пробы отправляют разные сигналы. Каждый имеет свое определенное значение и они не взаимозаменяемы.
- Не пройденная liveness проверка говорит, что контейнер должен быть перезапущен.
- Не пройденная readiness проба говорит  придержать трафик от отправки в этот контейнер.

Пример [[Deployment]]  с пробами

```yml
---
# file: practice/5.network-abstractions/1.probes/deployment-with-stuff.yaml
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
        startupProbe:
          httpGet:
            path: /
            port: 80
          failureThreshold: 30
          periodSeconds: 10
        resources:
          requests:
            cpu: 50m
            memory: 100Mi
          limits:
            cpu: 100m
            memory: 100Mi
...

```