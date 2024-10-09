Как добавить кейтаб в секреты

 Получаем список доступных кластеров 
 ```bash
 kubectl config get-contexts
```

Выбираем нужный из списка 
```bash
 kubectl config use-context CONTEXT_NAME
```

Далее выполняем следующую команду
```bash
kubectl create secret generic --from-file ./keytabname.keytab keytabname-keytab -n namespace
```


kubectl create secret generic --from-file ~/keytabs/perm/srv-t-rbpperm-kafka.keytab srv-t-rbpperm-kafka-keytab -n authorization