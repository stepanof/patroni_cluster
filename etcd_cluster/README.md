Папка с файлами для кластера etcd.

#### Подготовка нод:

`git clone repo_path/patroni_cluster.git`

`sudo apt  install etcd-client`

В каждой папке (node01,node02,node03) лежит `docker-compose.yaml` для запуска инстанса etcd.

### Запуск кластера
Добавление нод в кластер происходит по очереди.

\> Переходим на node01

Запуск первой ноды кластера:

`cd ~/patroni_cluster/etcd_cluster/node01`

`docker-compose up -d`

Проверка, что etcd успешно запущен:

`etcdctl member list`

Добавление второй ноды:

`etcdctl member add etcd_node02 http://$NODE02_IP:2380`

**Output:**

*Added member named etcd_node02 with ID f57d457297e90ffd to cluster*

*ETCD_NAME="etcd_node02"*

*ETCD_INITIAL_CLUSTER="etcd_node01=http://$NODE01_IP:2380,etcd_node02=http://$NODE02_IP:2380"*

*ETCD_INITIAL_CLUSTER_STATE="existing"*

\> Переходим на node02

Запуск второй ноды кластера:

`cd ~/patroni_cluster/etcd_cluster/node02`

`docker-compose up -d`

\> Переходим на node01

Проверка успешного добавление второй ноды в кластер:

`etcdctl cluster-health`

**Output:**

*member 8005d49086947600 is healthy: got healthy result from http://$NODE02_IP:2379*

*member b7b0dd15f24055ed is healthy: got healthy result from http://$NODE01_IP:2379*

*cluster is healthy*

Добавление третьей ноды:

`etcdctl member add etcd_node03 http://$NODE03_IP:2380`

**Output:**
*Added member named etcd_node03 with ID c8ad7bcaa14237db to cluster*

*ETCD_NAME="etcd_node03"*

*ETCD_INITIAL_CLUSTER="etcd_node02=http://$NODE02_IP:2380,etcd_node01=http://$NODE01_IP:2380,etcd_node03=http://$NODE03_IP:2380"*

*ETCD_INITIAL_CLUSTER_STATE="existing"*

\> Переходим на node03

Запуск третьей ноды кластера:

`cd ~/patroni_cluster/etcd_cluster/node03`

`docker-compose up -d`

\> Переходим на node01

Проверка успешного добавление третьей ноды в кластер:

`etcdctl cluster-health`

**Output:**

*member 8005d49086947600 is healthy: got healthy result from http://$NODE02_IP:2379*

*member b7b0dd15f24055ed is healthy: got healthy result from http://$NODE01_IP:2379*

*member c8ad7bcaa14237db is healthy: got healthy result from http://$NODE03_IP:2379*

*cluster is healthy*

#### Docs
Документация по etcd:
https://manpages.org/etcd
