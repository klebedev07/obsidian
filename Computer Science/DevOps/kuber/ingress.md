Ingress — свод правил, в которых описано, как внешний трафик получает доступ к сервисам в кластере.

Ingress-контроллер — pod (базовый блок Kubernetes), который реализует правила, описанные в Ingress. По сути, это приложение-контроллер.

Контроллеры реализованы на различном софте. Один из самых простых и популярных среди веб-разработчиков —  Nginx Ingress.

это следующая абстракция идущая за [[Service]]


![[Pasted image 20231130181409.png]]



```yml 
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress-nginx
  annotations:
    kubernetes.io/ingress.class: "nginx"
spec:
  rules:
  - host: <мой домен>.com
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
	          ## здесь указываем сервис на который дальше пускаем трафик 
            name: my-service 
            port:
              number: 80
```

С помощью анотаций можем дополнительно конфигурировать наш ингрес-контроллер. Например в примере выше мы говорил что тип контроллера будет nginx. Или например мржем задать тип трафка **HTTPS** 
``` nginx.ingress.kubernetes.io/backend-protocol: "HTTPS" ```

Так можем задать сертификаты  
![[Pasted image 20231130182410.png]]


![[Pasted image 20231130182444.png]]
