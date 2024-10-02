Поднять можно через команду
```bash
docker run -p 8080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:21.1 start-dev
```

Url с информацией по конфигу и ссылкам
```
https://auth.protonmath.ru/realms/protonmath/.well-known/openid-configuration


```

![[Pasted image 20230820152359.png]]



1. Устанавливаем  роли в клиент роли, в скоуп маппер
2. "resource_access" провалидировать
3. Получаем роли resource_access и матчим их на роли клиента . Путь до роли на скрине снизу
4. Если совпали то успех , если нет то редиректим на страницу и говорим что 
5. ![[Снимок экрана 2024-03-08 в 00.04.15.png]]