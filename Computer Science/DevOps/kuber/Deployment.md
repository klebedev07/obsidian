
Это абстракция которая существует поверх [[ReplicaSet]] . Нужен для того что бы обновлять версии нашего приложения. Деплоймент создает [[ReplicaSet]]  , а он в свою очередь создает [[Pod]] . 
Добавлять переменные окружения и всякие конфиги можно через [[configMap]]

Дока https://kubernetes.io/docs/concepts/workloads/controllers/deployment/


# Deployment  
  
## 1. Создаем deployment  
  
Для этого выполним команду:  

```yml
---   
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
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
```
  
```bash  

apply -f deployment.yaml

```  
  
Проверяем список pods, для этого выполним команду:  
  
```bash  
kubectl get pod
```  
  
Результат должен быть примерно таким:  
  
```bash  
NAME READY STATUS RESTARTS AGE
my-deployment-7c768c95c4-47jxz 0/1 ContainerCreating 0 2s
my-deployment-7c768c95c4-lx9bm 0/1 ContainerCreating 0 2s
```  
  
Проверяем список replicaset, для этого выполним команду:  
  
```bash  
kubectl get replicaset
```  
  
Результат должен быть примерно таким:  
  
```bash  
NAME DESIRED CURRENT READY AGE
my-deployment-7c768c95c4 2 2 2 1m
```  
  
## 2. Обновляем версию image  
  
Обновляем версию image для container в deployment my-deployment.  
Для этого выполним команду:  
  
```bash  
kubectl set image deployment my-deployment nginx=nginx:1.21  
```  
  
Проверяем результат, для этого выполним команду:  
  
```bash  
kubectl get pod
```  
  
Результат должен быть примерно таким:  
  
```bash  
NAME                          READY STATUS RESTARTS AGE
my-deployment-685879478f-7t6ws 0/1 ContainerCreating 0 1s
my-deployment-685879478f-gw7sq 0/1 ContainerCreating 0 1s
my-deployment-7c768c95c4-47jxz 0/1 Terminating 0 5m
my-deployment-7c768c95c4-lx9bm 1/1 Running 0 5m
```  
  
И через какое-то время вывод этой команды станет:  
  
```bash  
NAME READY STATUS RESTARTS AGE
my-deployment-685879478f-7t6ws 1/1 Running 0 33s
my-deployment-685879478f-gw7sq 1/1 Running 0 33s
```  
  
Проверяем что в новых pod новый image. Для этого выполним команду,  
заменив имя pod на имя вашего pod:  
  
```bash  
kubectl describe pod my-deployment-685879478f-7t6ws
```  
  
Результат должен быть примерно таким:  
  
```bash  
Image: nginx:1.21  
```  
  
Проверяем что стало с replicaset, для этого выполним команду:  
  
```bash  
kubectl get replicaset
```  
  
Результат должен быть примерно таким:  
  
```bash  
NAME DESIRED CURRENT READY AGE
my-deployment-685879478f 2 2 2 2m
my-deployment-7c768c95c4 0 0 0 7m
```  
  
## 3. Чистим за собой кластер  
  
```bash  
kubectl delete deployment --all
```
