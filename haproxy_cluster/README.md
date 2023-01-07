Папка с файлами для запуска HAProxy на нескольких нодах.

В файле `haproxy.cfg` содержится конфиг для HAProxy.
Конфигурация для запуска HAProxy одинакова на всех нодах кластера.

Однако различаются файлы `docker-compose.yaml` - необходимо присвоить контейнерам на разных машинах разные имена.

Для запуска HAProxy перейдите в папку, соответствующую ноде, на который будет подниматься приложение и выполните:

`cd ~/patroni_cluster/haproxy_cluster/node0X`

`docker-compose up -d`

HAProxy посылает REST API запросы по адресам $NODE01_IP:8008, $NODE02_IP:8008, и получает в ответ http код '200', если нода является мастером patroni. Затем HAProxy перенаправляет траффик с порта 5432 на ноду с мастером patroni на порт 5433.

$VIP_IP (VIP) - витуальный IP, который обеспечивается работой keepalived.
Если мастер patroni изменится, то от бывшего мастера придет http код 503 (или иной, но не равный 200). Таким образом HAProxy поймет, что мастер сломан, и станет перенаправлять траффик на другую ноду (с которой будет приходить ответный http код, равный 200).

В качестве образа используется офф.образ HAProxy + установленный curl.

Также HAProxy слушает порт 8888, который использует docker для проверки состояния контейнера (healthcheck), а также этот порт используется keepalived для проверки, что HAProxy на выбранной ноде работает в штатном режиме.