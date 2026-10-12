<div align="center">

```
seegan@devops:~$ whoami
Sergey Ganin — DevOps / Cloud Platform Engineer

seegan@devops:~$ pwd
Yerevan, Armenia

seegan@devops:~$ cat README.md
Материалы подготовки к DevOps-позиции.
```

📍 Yerevan, Armenia · 📧 [seeganfocus@gmail.com](mailto:seeganfocus@gmail.com) · 💼 [LinkedIn](https://www.linkedin.com/in/sergey-ganin-4433a343a)

</div>

---

## О репозитории

Здесь я собираю материалы для подготовки к DevOps / Cloud Platform Engineer:

- **Дневник** — ежедневные отчёты: что разобрал, что делал на практике
- **База знаний** — конспекты по темам
- **Трекер откликов** — таблица вакансий и статусов

Репозиторий ведётся открыто. Каждый день — коммит.

---

## Структура

```
devops_roadmap/
├── pages/                  # База знаний
│   ├── Linux/              # inode, монтирование, df/du/lsof, процессы
│   ├── Docker/             # namespace, cgroup, runc
│   ├── Ansible/            # playbooks, roles, шаблоны
│   └── Python/             # основы, автоматизация
│
├── weeks/                  # Дневник по дням
│   └── week-1/
│       ├── day-1/          # Файловые системы, inode, монтирование
│       ├── day-2/          # df, du, lsof +L1
│       └── day-3/          # Процессы и сигналы
│
├── jobs.md                 # Таблица откликов
└── README.md               # Этот файл
```

---

## Содержание

### Linux

| Тема | Отчёт | Конспект |
|---|---|---|
| Файловые системы, inode, монтирование | [Day 1](weeks/week-1/day-1/day_1.md) | [inode.md](pages/Linux/Inode%20в%20Linux.md) |
| df, du, lsof +L1 | [Day 2](weeks/week-1/day-2/day2.md) | [df_du_lsof.md](pages/Linux/df_du_lsof.md) |
| Процессы и сигналы | [Day 3](weeks/week-1/day-3/day_3.md) | [processes.md](pages/Linux/processes.md) |

### Docker

| Тема | Конспект |
|---|---|
| Namespace и cgroup | [namespace_cgroup.md](pages/Docker/namespace%20и%20cgroup.md) |

### Ansible

| Тема | Конспект |
|---|---|
| Основы | [Ansible.md](pages/Ansible/Ansible.md) |

### Python

| Тема | Конспект |
|---|---|
| Основы языка | [Основы языка](pages/Python/) |

---

## План подготовки

Темы, которые нужно разобрать:

- **Linux** — файловые системы, процессы, сети, systemd
- **Docker** — образы, контейнеры, сети, volumes
- **CI/CD** — GitLab CI, Jenkins, ArgoCD
- **Kubernetes** — Deployment, Service, Ingress, Helm
- **IaC** — Ansible, Terraform
- **Мониторинг** — Prometheus, Grafana, ELK
- **SRE** — SLI/SLO, инциденты, postmortem

Порядок изучения — от базы (Linux) к платформе (Kubernetes).

---

## Отклики

Актуальная таблица со статусами — в [jobs.md](jobs.md).

| Компания | Позиция | Дата |
|---|---|---|
| SMARTSOFT | DevOps Engineer | 10.10.2026 |
| ChillBase | — | 10.10.2026 |
| Social Discovery Group | Junior System Administrator | 10.10.2026 |
| Broadridge | — | 12.10.2026 |
| BettingJobs | — | 12.10.2026 |
| Canonical | DevOps Engineer | 12.10.2026 |

---

## Принципы

- Одна тема в день.
- Практика обязательна — воспроизвожу сценарии в VM.
- Конспект после практики.
- Проговариваю ответы вслух.
- Ежедневный коммит.

---

## Контакты

**Sergey Ganin**

📧 [seeganfocus@gmail.com](mailto:seeganfocus@gmail.com)  
💬 [Telegram @seegan1](https://t.me/seegan1)  
💼 [LinkedIn](https://www.linkedin.com/in/sergey-ganin-4433a343a)