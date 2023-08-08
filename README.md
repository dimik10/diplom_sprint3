# diplom_sprint3

Спринт 3

ЗАДАЧА

```
1. Настройка сборки логов.

Представьте, что вы разработчик, и вам нужно оперативно получать информацию с ошибками работы приложения.
Выберите инструмент, с помощью которого такой функционал можно предоставить. 
Нужно собирать логи работы пода приложения. Хранить это всё можно либо в самом кластере Kubernetes, либо на srv-сервере.

2. Выбор метрик для мониторинга.

Так, теперь творческий этап. Допустим, наше приложение имеет для нас некоторую важность. 
Мы бы хотели знать, когда пользователь не может на него попасть — время отклика, сертификат, статус код и так далее. 
Выберите метрики и инструмент, с помощью которого будем отслеживать его состояние.
Также мы хотели бы знать, когда место на srv-сервере подходит к концу.
Важно! Весь мониторинг должен находиться на srv-сервере, чтобы в случае падения кластера мы все равно могли узнать об этом.

3. Настройка дашборда.

Ко всему прочему хотелось бы и наблюдать за метриками в разрезе времени.
Для этого мы можем использовать Grafana и Zabbix — что больше понравилось.

4. Алертинг.

теперь добавим уведомления в наш любимый мессенджер, точнее в ваш любимый мессенджер. 
Лучше в почту)
```

РЕШЕНИЕ

- Стек использования:
Prometheus
Grafana
Blackbox
Alertmanager
node-exporter
loki

- Уставнавливаем через docker-compose это всё на srv-0 ноду для мониторинга.
клонируем репозиторий  (git clone https://github.com/dimik10/diplom_sprint3) и запускаем.

docker compose up -d 

- Устанавливаем Loki в кластер k8s из helm chart:
  ```
  helm repo add bitnami https://charts.bitnami.com/bitnami -n monitoring 
  helm repo update
  helm install --namespace monitoring loki bitnami/grafana-loki --set global.dnsService=coredns --set spec.type=NodePort --set loki.auth_enabled=false
  ```
- Подключаем всё к Grafana.
из общего репозитория на сайте выбираем нужные дашборды.
https://grafana.com/grafana/dashboards/1860-node-exporter-full/
https://grafana.com/grafana/dashboards/7587-prometheus-blackbox-exporter/
https://grafana.com/grafana/dashboards/13639-logs-app/
Адрес сервера графана:
  ```
  http://158.160.64.45:3000/
  ```
- В docker-compouse.yaml файле на сервере srv вносим свои данные телеграмм, перезапускаем контейнер и моделируем срабатывание аллерта.
 либо отправляем письмо.
