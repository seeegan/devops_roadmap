# План подготовки к собеседованию DevOps Engineer (Middle / Middle+)

**Дата собеседования:** 26.11.2026  
**Период подготовки:** 17.11–25.11.2026

## Цель

Подготовиться к техническому собеседованию на позицию DevOps Engineer (Middle / Middle+) в Employcity.

Основной акцент:

- Kubernetes и диагностика проблем.
    
- Helm и Helmfile.
    
- GitLab CI/CD.
    
- GitOps и Argo CD.
    
- Docker, Kaniko, BuildKit.
    
- Linux и системная диагностика.
    

Приоритет — не просто знать определения, а уметь объяснить механизм работы, найти проблему и показать конкретные команды.

---

## P0 — Критически важные темы

### 1. Kubernetes: эксплуатация и диагностика

- Архитектура Kubernetes: API Server, etcd, Scheduler, Controller Manager, kubelet, kube-proxy.
    
- Жизненный цикл Pod.
    
- Deployment, ReplicaSet, StatefulSet, DaemonSet, Job, CronJob.
    
- Rolling Update: `maxSurge`, `maxUnavailable`.
    
- Readiness, Liveness, Startup Probes.
    
- Requests и Limits: CPU, Memory, QoS.
    
- OOMKilled, Evicted, Pending, CrashLoopBackOff, ImagePullBackOff.
    
- ConfigMap и Secret.
    
- ServiceAccount и RBAC.
    
- imagePullSecrets.
    
- Service: ClusterIP, NodePort, LoadBalancer.
    
- DNS внутри Kubernetes.
    
- Ingress и базовое понимание Ingress Controller.
    
- PersistentVolume, PersistentVolumeClaim, StorageClass.
    
- Диагностика проблем Pod, Deployment и Service.
    
- Rollout, rollback и история ревизий.
    

**Практика команд:**

```
kubectl get
kubectl describe
kubectl logs
kubectl logs --previous
kubectl exec
kubectl get events
kubectl rollout status
kubectl rollout history
kubectl rollout undo
kubectl top
kubectl explain
```

### 2. Helm и Helmfile

- Что такое Helm Chart.
    
- Структура Chart: `Chart.yaml`, `values.yaml`, `templates/`, `charts/`, `_helpers.tpl`.
    
- Назначение каждого файла и директории.
    
- Как работают Helm Templates.
    
- Переменные `.Values`, `.Release`, `.Chart`.
    
- Условия `if`, циклы `range`, функции `default`, `required`, `include`, `tpl`.
    
- Named Templates и `_helpers.tpl`.
    
- Передача собственных values-файлов.
    
- Переопределение значений через `--set`.
    
- Dependencies и subcharts.
    
- Helm hooks.
    
- `helm install`, `helm upgrade`, `helm rollback`, `helm uninstall`.
    
- `helm history`, `helm list`, `helm get values`.
    
- `helm template`, `helm lint`, `helm diff`.
    
- Как Helm хранит историю релизов.
    
- Как Helm взаимодействует с Kubernetes API.
    
- Helmfile: назначение, структура, environments, releases, зависимости.
    
- Как Helm используется вместе с Argo CD.
    

**Практика:**  
Создать небольшой Helm Chart для приложения с Deployment, Service и ConfigMap. Развернуть, изменить values, обновить релиз и выполнить rollback.

### 3. GitOps и Argo CD

- Принципы GitOps.
    
- Desired State и Actual State.
    
- Pull-модель GitOps.
    
- Архитектура и компоненты Argo CD.
    
- Application и AppProject.
    
- Automated и Manual Sync.
    
- Self Heal и Prune.
    
- Sync Status и Health Status.
    
- Как Argo CD обнаруживает изменения в Git.
    
- Как CI обновляет версию образа в конфигурационном репозитории.
    
- Разделение приложений и окружений.
    
- Promotion между Test, Stage и Production.
    
- Rollback в GitOps: почему ручной rollback в Kubernetes может быть отменён Argo CD.
    
- Конфликты и расхождения между Git и кластером.
    

### 4. GitLab CI/CD

- Архитектура GitLab CI/CD.
    
- GitLab Runner: executor, регистрация, теги, параллельное выполнение.
    
- Структура `.gitlab-ci.yml`.
    
- `stages`, `jobs`, `script`, `before_script`, `after_script`.
    
- `rules`, `only`, `except`.
    
- `needs` и `dependencies`.
    
- `artifacts` и `cache`.
    
- Variables и защищённые переменные.
    
- `include`, `extends`, YAML anchors.
    
- `workflow: rules`.
    
- Manual jobs и approvals.
    
- Environments и deployment jobs.
    
- Multi-stage pipelines.
    
- Child и parent pipelines.
    
- Передача артефактов между этапами.
    
- Безопасная работа с токенами и секретами.
    
- Git branching strategies.
    
- Merge Request, protected branches, tags.
    
- Как организовать CI и CD для нескольких окружений.
    

**Практика:**  
Спроектировать Pipeline: тестирование → сборка образа → публикация в Registry → обновление GitOps-репозитория → деплой через Argo CD.

### 5. Linux: процессы, память и системная диагностика

- Процесс и поток: различия.
    
- PID, PPID, fork(), exec(), wait().
    
- Системные вызовы и переход user space/kernel space.
    
- Zombie и Orphan процессы.
    
- Сигналы: SIGTERM, SIGKILL, SIGSTOP, SIGHUP.
    
- Состояния процессов: R, S, D, T, Z.
    
- OOM Killer: механизм работы и диагностика.
    
- `/proc`: назначение и важные файлы.
    
- Файловая система: inode, directory entry, data blocks.
    
- Hard link и symbolic link.
    
- Удаление файла при открытом file descriptor.
    
- `ulimit`, soft limit и hard limit.
    
- File descriptors и лимиты открытых файлов.
    
- cgroups v2: CPU, memory, pids controllers.
    
- `memory.max`, `memory.current`, `pids.max`.
    
- Как systemd управляет процессами через cgroups.
    

**Практика команд:**

```
ps aux
top
htop
pstree
strace
lsof
ulimit -a
free -h
vmstat
dmesg
journalctl
cat /proc/<PID>/status
cat /proc/<PID>/limits
```

### 6. Linux: systemd

- Архитектура systemd.
    
- Unit-файлы и их структура.
    
- Service, Target, Timer, Socket.
    
- `Wants`, `Requires`, `After`, `Before`.
    
- `systemctl start`, `stop`, `restart`, `enable`, `disable`.
    
- `systemctl status`, `list-units`, `list-dependencies`.
    
- Restart policies.
    
- `LimitNOFILE`.
    
- Resource limits.
    
- Systemd sandboxing.
    
- Socket activation.
    
- Timer units.
    
- Диагностика сервисов через journalctl.
    

### 7. Linux: сети и TCP/IP

- TCP/IP и OSI: уровни и их назначение.
    
- TCP handshake.
    
- FIN/ACK и закрытие TCP-соединения.
    
- Состояния ESTABLISHED, LISTEN, TIME_WAIT, CLOSE_WAIT.
    
- Причины большого количества CLOSE_WAIT и TIME_WAIT.
    
- Сокеты и порты.
    
- Routing table и default gateway.
    
- DNS resolution.
    
- ARP.
    
- iptables: таблицы и цепочки.
    
- NAT, SNAT, DNAT, conntrack.
    
- Как работает packet filtering.
    
- Диагностика соединений через ss, tcpdump, lsof.
    

**Практика команд:**

```
ip addr
ip route
ip neigh
ss -tulpn
ss -tan
tcpdump
iptables -L -n -v
iptables -t nat -L -n -v
dig
curl -v
traceroute
```

---

## P1 — Важные темы из вакансии

### 8. Docker и сборка образов

- Архитектура Docker: daemon, client, container runtime.
    
- Image и Container.
    
- Dockerfile: FROM, RUN, COPY, ADD, CMD, ENTRYPOINT.
    
- Docker layers и кеширование.
    
- Multi-stage builds.
    
- Docker volumes и networks.
    
- Container namespaces и cgroups.
    
- Различия Docker, Kaniko и BuildKit.
    
- Сборка образов без Docker daemon.
    
- BuildKit: cache mounts, secrets, multi-platform builds.
    
- Kaniko: механизм сборки образа в Kubernetes.
    
- Оптимизация размера Docker Image.
    
- Тегирование образов и immutable tags.
    
- Container Registry.
    

### 9. Базы данных и инфраструктурные сервисы

В вакансии перечислены MySQL, PostgreSQL, Redis, Kafka, ClickHouse, MongoDB и Vault.

Времени на глубокое изучение всех этих систем нет. Нужно подготовить основы эксплуатации.

- PostgreSQL: подключения, роли, базы, backup/restore.
    
- MySQL: базовая архитектура и администрирование.
    
- Redis: persistence, eviction policies, memory limits.
    
- Kafka: brokers, topics, partitions, replicas, consumer groups.
    
- ClickHouse: назначение, таблицы, репликация на базовом уровне.
    
- MongoDB: Replica Set и базовая диагностика.
    
- Vault: secrets, policies, authentication methods.
    
- Работа приложений с внешними зависимостями.
    
- Диагностика проблем подключения к БД.
    

### 10. CUE и стандартизация конфигураций

- Что такое CUE.
    
- Для чего используется CUE в инфраструктуре.
    
- Отличия CUE от YAML.
    
- Валидация конфигураций.
    
- Использование CUE вместе с Helm и Kubernetes.
    
- Зачем стандартизировать Kubernetes-компоненты.
    

---

## P2 — Повторение и закрепление

### 11. Git

- merge и rebase.
    
- cherry-pick.
    
- revert и reset.
    
- Работа с конфликтами.
    
- Git tags.
    
- Protected branches.
    
- Git flow и trunk-based development.
    
- Работа с несколькими репозиториями.
    

### 12. Мониторинг и Observability

- Prometheus и VictoriaMetrics.
    
- PromQL.
    
- Метрики, логи, трейсы.
    
- RED и USE методологии.
    
- Grafana dashboards.
    
- Alertmanager.
    
- SLI, SLO, SLA.
    
- MTTD, MTTR, RTO, RPO.
    

---

## План по дням

|Дата|Основной фокус|
|---|---|
|17.11|Диагностика Kubernetes, rollout, rollback|
|18.11|Helm и Helmfile|
|19.11|Argo CD и GitOps|
|20.11|GitLab CI/CD и Git|
|21.11|Docker, Kaniko, BuildKit|
|22.11|Linux: процессы, память, cgroups, файловая система|
|23.11|Linux: systemd, TCP/IP, сети, iptables|
|24.11|Базы данных, Kafka, Vault, CUE|
|25.11|Полное пробное техническое собеседование|

## Критерии готовности

К собеседованию необходимо уметь:

- Объяснить архитектуру CI/CD от коммита до Production.
    
- Спроектировать GitOps-процесс с Argo CD.
    
- Самостоятельно написать простой Helm Chart.
    
- Объяснить механизм работы Helm upgrade и rollback.
    
- Диагностировать `ImagePullBackOff`, `CrashLoopBackOff`, `OOMKilled`.
    
- Разобрать зависший Rolling Update.
    
- Объяснить работу Kubernetes requests и limits.
    
- Объяснить механику процессов Linux, fork/exec, сигналов и OOM Killer.
    
- Диагностировать проблемы systemd и TCP-соединений.
    
- Объяснить назначение cgroups v2 и их основные контроллеры.
    
- Написать и объяснить GitLab CI Pipeline.
    
- Рассказать о Kaniko и BuildKit, не путая их с Docker daemon.
    

## Правило подготовки

Не тратить время на заучивание определений без понимания механики.

Для каждой темы:

1. Разобраться, как работает механизм.
    
2. Уметь объяснить его своими словами.
    
3. Разобрать практический сценарий отказа.
    
4. Знать основные команды диагностики.
    
5. Ответить на несколько вопросов в формате технического собеседования.
    

**Главный приоритет:** Kubernetes, Helm, GitOps, GitLab CI/CD и Linux. Остальные темы изучать настолько глубоко, насколько позволит оставшееся время.