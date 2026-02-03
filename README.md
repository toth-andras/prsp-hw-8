## ПРСП, ДЗ 8
### Тот Андраш, БПИ22

#### Описание решения
Логи пишутся в файл, хранящийся в volume. К каждому сервису muffin-wallet в виде sidecar-а подключается promtail, который вычитывает логи из volum-а.
Zipkin, Grafana и Loki разварачиваются локально в docker-compose.

#### Как развернуть дз
1) Развернуть сервисы из docker-compose.yaml
```
docker-compose up -d
```
2) Добавить ingress в minikube:
```
minikube addons enable ingress
```
3) Сделать port-forward для ingress:
```
kubectl port-forward --namespace=ingress-nginx service/ingress-nginx-controller 8080:80
```
4) Добавить следующую строку в /etc/hosts:
```
127.0.0.1 muffin-wallet.ru
```
5) Развернуть сервисы в k8s:
```
kubectl apply -f k8s.yaml 
```
Сервис muffin-wallet будет доступен по адресу: http://muffin-wallet.ru:8080/swagger-ui/index.html
Grafana будет доступна по адресу: http://localhost:3000/ . Логин и пароль: admin

#### Дашборд с логами
<img width="1467" height="558" alt="image" src="https://github.com/user-attachments/assets/0f5d5546-d083-406f-b178-e2645be5ce52" />

#### Трейсы в Zipkin:
<img width="1709" height="945" alt="image" src="https://github.com/user-attachments/assets/e797fd34-f623-49f2-92f3-8c1912f3d984" />

Трейсы можно занести на дашборд:
<img width="1465" height="367" alt="Pasted Graphic 1" src="https://github.com/user-attachments/assets/2e9aecac-c86d-4472-88a9-ac59fbf3dab1" />
Но из-за ограничений grafana для поиска по traceId придется Visualization, поэтому я в итоге решил отказаться от трейсов на дашборде

