![[Снимок экрана 2023-11-01 в 18.00.05.png]]


```yml
---
apiVersion: v1
kind: Secret
metadata:
  name: test
stringData:
  test: updated
...


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
        envFrom:
        - configMapRef:
            name: my-configmap-env
        env:
        - name: TEST
          value: foo
        - name: TEST_1
          valueFrom:
            secretKeyRef:
              name: test
              key: test1
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