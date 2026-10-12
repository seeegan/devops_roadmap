<div align="center">

# 🚀 DevOps Roadmap

**Мой путь от OPS-инженера до DevOps / Cloud Platform Engineer**

[![Made with Obsidian](https://img.shields.io/badge/Made%20with-Obsidian-7C3AED?style=for-the-badge&logo=obsidian&logoColor=white)](https://obsidian.md)
[![Markdown](https://img.shields.io/badge/Markdown-000000?style=for-the-badge&logo=markdown&logoColor=white)](https://www.markdownguide.org)
[![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)](https://git-scm.com)
[![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)](https://www.linux.org)

[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com)
[![Ansible](https://img.shields.io/badge/Ansible-EE0000?style=for-the-badge&logo=ansible&logoColor=white)](https://www.ansible.com)
[![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)](https://www.terraform.io)

📍 Yerevan, Armenia &nbsp;|&nbsp; 📧 seeganfocus@gmail.com &nbsp;|&nbsp; 💼 [LinkedIn](https://www.linkedin.com/in/sergey-ganin-4433a343a)

</div>

---

## 📖 О репозитории

Это не просто «конспекты». Это **система подготовки к DevOps-позиции**, которую я веду каждый день:

- 📝 **Дневник** — ежедневные отчёты о том, что разобрал и сделал
- 📚 **База знаний** — структурированные конспекты по темам
- 🎯 **Трекер** — таблица откликов на вакансии и статус интервью
- 🗺 **Roadmap** — план подготовки на 8 недель

**Цель:** получить оффер DevOps / Cloud Platform Engineer в Армении или удалённо.

---

## 🎯 Прогресс

| Неделя | Тема | Статус |
|---|---|---|
| Week 1 — Day 1 | Файловые системы, inode, монтирование | ✅ |
| Week 1 — Day 2 | df, du, lsof +L1 — диагностика диска | ✅ |
| Week 1 — Day 3 | Процессы и сигналы | ⏳ |
| Week 1 — Day 4 | — | ⬜ |
| Week 1 — Day 5 | — | ⬜ |

**Всего дней:** 3 / 40  
**Откликов на вакансии:** 6  
**Интервью:** 0

---

## 📂 Структура репозитория

```
devops_roadmap/
├── 📁 pages/                # База знаний по темам
│   ├── Linux/               # Linux: inode, монтирование, df/du/lsof, процессы
│   ├── Docker/              # Docker: namespace, cgroup, runc
│   ├── Ansible/             # Ansible: playbooks, roles, шаблоны
│   └── Python/              # Python: основы, автоматизация
│
├── 📁 weeks/                # Дневник по дням
│   └── week-1/
│       ├── day-1/           # Файловые системы, inode, монтирование
│       ├── day-2/           # df, du, lsof +L1
│       └── day-3/           # Процессы и сигналы
│
├── 📁 tools/                # Инструменты
│   └── quest_tracker.html   # Геймифицированный трекер привычек
│
├── 📄 roadmap.md            # План подготовки на 8 недель
├── 📄 jobs.md               # Таблица откликов на вакансии
└── 📄 README.md             # Этот файл
```

---

## 📚 Что уже разобрано

### 🐧 Linux — база

<details>
<summary><b>Day 1 — Файловые системы, inode, монтирование</b></summary>

- Что такое inode, что хранит, что нет
- Отличие inode от file descriptor
- Слои абстракции: диск → раздел → ФС → точка монтирования
- Loopback-устройства (`losetup`, `mount -o loop`)
- Создание ФС через `mkfs.ext4`
- Сценарий «кончились inode» на практике

📄 [Отчёт](weeks/week-1/day-1/day_1.md) · 📝 [Конспект](pages/Linux/Inode%20в%20Linux.md)

</details>

<details>
<summary><b>Day 2 — df, du, lsof +L1: диагностика диска</b></summary>

- Разница `df` vs `du`
- `df -h`, `df -i`, `df -hT` — что показывают
- `du -sh`, `--max-depth`, `-ah`, `sort -h`
- `lsof -p`, `-i`, `+L1` — работа с открытыми файлами
- Классический сценарий: «диск заполнен, du пустой»
- Почему `rm` не всегда освобождает место

📄 [Отчёт](weeks/week-1/day-2/day2.md) · 📝 [Конспект](pages/Linux/df_du_lsof.md)

</details>

<details>
<summary><b>Day 3 — Процессы и сигналы</b></summary>

- `ps aux`, `ps -ef`, `top`, `htop`
- Состояния процессов: R, S, D, Z, T
- D-state — почему не убивается даже `kill -9`
- Zombie-процессы
- Сигналы: SIGTERM, SIGKILL, SIGHUP, SIGSTOP, SIGCONT
- Приоритеты: `nice`, `renice`

📄 [Отчёт](weeks/week-1/day-3/day_3.md) · 📝 [Конспект](pages/Linux/processes.md)

</details>

---

## 🛠 Стек, который изучаю

<div align="center">

| Категория | Технологии |
|---|---|
| **ОС** | Linux (Ubuntu, Debian), macOS |
| **Контейнеры** | Docker, Kubernetes, Helm, Helmfile |
| **CI/CD** | Jenkins, GitLab CI, ArgoCD |
| **IaC** | Ansible, Terraform |
| **Мониторинг** | Prometheus, Grafana, VictoriaMetrics, Zabbix, ELK |
| **Сети** | TCP/IP, DNS, HTTP/HTTPS, TLS, Istio |
| **Скрипты** | Bash, Python |
| **БД** | PostgreSQL, MongoDB |

</div>

---

## 🗓 План на 8 недель

| Неделя | Тема | Практика |
|---|---|---|
| **1** | Linux, сети, Git | Linux runbook, Docker, CI/CD |
| **2** | Docker, CI/CD | Dockerfile, GitLab CI, Jenkins |
| **3** | Kubernetes core | Deployment, Service, Ingress |
| **4** | Helm, GitOps | Helm chart, ArgoCD |
| **5** | Мониторинг, логи | Prometheus, Grafana, ELK |
| **6** | Terraform, Ansible, Cloud | VPC, VM, S3, IAM |
| **7** | SRE, Security | SLI/SLO, RBAC, TLS |
| **8** | Mock-интервью, capstone | Capstone-проект |

📄 [Полный план →](roadmap.md)

---

## 💼 Отклики на вакансии

Веду таблицу всех откликов с конкретным next action:

| Компания | Позиция | Статус |
|---|---|---|
| SMARTSOFT LLC | DevOps Engineer | Applied |
| ChillBase | — | Applied |
| Social Discovery Group | Junior System Administrator | Applied |
| Broadridge | — | Applied |
| BettingJobs | — | Applied |
| Canonical | DevOps Engineer | Applied |

📄 [Полная таблица →](jobs.md)

---

## 🎮 Геймификация

Для поддержания дисциплины использую:

- **Бумажный трекер** — ежедневно 5 чекбоксов: DevOps, English, Armenian, Applications, Sport
- **HTML-трекер** — визуальный quest tracker

📄 [Quest Tracker →](tools/quest_tracker.html)

---

## 📈 Философия подготовки

> **70% качества, опубликовано, можешь объяснить — достаточно.**

Мой подход:

1. **Одна тема в день.** Не «учить Linux», а «разобрать inode».
2. **Практика обязательна.** Не просто читаю — воспроизвожу в VM.
3. **Конспект после практики.** Не до, не вместо — после.
4. **Самопроверка вслух.** Проговариваю ответы, как на интервью.
5. **Ежедневный коммит.** Регулярность > интенсивность.

---

## 📬 Контакты

<div align="center">

**Sergey Ganin**

📧 [seeganfocus@gmail.com](mailto:seeganfocus@gmail.com)  
💬 [Telegram @seegan1](https://t.me/seegan1)  
💼 [LinkedIn](https://www.linkedin.com/in/sergey-ganin-4433a343a)  
🐙 [GitHub](https://github.com/seeegan)

**Открыт к предложениям: DevOps / Cloud Platform Engineer**  
Локация: Yerevan, Armenia · Удалённо · Релокация

</div>

---

<div align="center">

⭐ Если тебе интересно следить за прогрессом — подпишись на репозиторий

![Visitors](https://visitor-badge.laobi.icu/badge?page_id=seeegan.devops_roadmap)

</div>