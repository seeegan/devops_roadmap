### Написать bash-скрипт для мониторинга Nginx

Скрипт называется `nginx_monitor.sh` и должен лежать в `/home/devops/scripts/`.

### Функционал скрипта:

1. Проверить, **работает ли Nginx** (порт 80 должен слушаться).  
    _Важно:_ пользоваться `ss`, а не `systemctl status`.
    
2. Если **НЕ работает** — записать в лог-файл `/var/log/nginx_monitor.log` сообщение:
    
    text
    
    ```
    [ДАТА ВРЕМЯ] Nginx is DOWN. Attempting to restart...
    ```
    
3. Попытаться **перезапустить** Nginx через `systemctl`.
    
4. Проверить, **запустился ли** Nginx после попытки.
    
5. Если запустился — записать в лог:
    
    
    ```
    [ДАТА ВРЕМЯ] Nginx successfully RESTARTED.
    ```
    
6. Если **НЕ запустился** — записать в лог:

    
    ```
    [ДАТА ВРЕМЯ] CRITICAL: Nginx failed to start! Manual intervention required.
    ```
    

---

### Требования

- В начале файла — `#!/bin/bash`
    
- Скрипт должен быть исполняемым (`chmod +x`)
    
- Запрещено использовать `systemctl status` для проверки статуса (только `ss`)
    

---

### Проверка

1. Запусти скрипт, когда Nginx **работает** — он не должен писать в лог (или писать, что всё хорошо — на твое усмотрение).
    
2. Останови Nginx (`systemctl stop nginx`) и запусти скрипт снова — проверь, что в лог записались сообщения.
    
3. Запусти скрипт, когда Nginx уже запущен — он не должен писать ложных срабатываний.
    

---

### Дополнительный вопрос (⭐)

Как сделать так, чтобы этот скрипт запускался **автоматически каждые 5 минут**?  
_(Ответ напиши в документации, но на сервере пока не настраивай).

Ответы:
```shell
#!/bin/bash
LOG_FILE_PATH="/var/log/nginx/nginx.monitor.log"
ss -tulpn | grep "nginx" &> /dev/null
if [ $? -eq 1 ]; then
        echo "$(date) Nginx is DOWN. Attempting to restart..." >> $LOG_FILE_PATH
        systemctl restart nginx.service
        ss -tulpn | grep "nginx" &> /dev/null

        if [ $? -eq 0 ]; then
                echo "$(date) Nginx successfully RESTARTED." >> $LOG_FILE_PATH
        else
                echo "$(date) CRITICAL: Nginx failed to start! Manual intervention required." >> $LOG_FILE_PATH
fi
fi

```
Вот сам скрипт.

Касательно дополнительного вопроса 
Я думаю можно добавить мой скрипт как сервис, в unit-файле описать периодичность запуска скрипта, таким образом он скажем так будет опрашивать nginx по его состоянию, своего рода healthcheck

Ошибка, делается это через cron. Но это уже потом