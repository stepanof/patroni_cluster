Папка с файлами для запуска patroni на нескольких нодах (1 мастер + 1 и более реплик).

Для запуска patroni с помощью docker-compose необходимо перейти в соотвествующую ноде папку и выполнить:

`cd patroni_cluster/patroni_cluster/node0X/`

`docker-compose up -d`

Для управления кластером необходимо подключиться к контейнеру и использовать команду patronictl.

Например, для просмотра текущего списка нод в кластере, в терминале на node01 выполните:

`docker exec -it patroni_node01 /bin/bash`

`patronictl -c /etc/patroni/postgres.yaml list batman`

для смены мастера выполните:

`patronictl -c /etc/patroni/postgres.yaml switchover`

Полный список команд смотрите в офф.документации или выполните:

`patronictl --help`
