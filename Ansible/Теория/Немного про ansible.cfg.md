Давай разберём на примере просто конфига.

ansible.cfg
```
[defaults]  
host_key_checking = false  
fact_caching = jsonfile  
fact_caching_connection = /tmp/facts_cache  
library = library  
gather_facts = true  
interpreter_python = auto  
  
[connection]  
pipelining = True  
  
[ssh_connection]  
pipelining = True
```

#### [defaults]
`host_key_checking` - Отключаем проверку SSH ключа, чтобы каждый раз не ругалось, можно оставить в true если хочешь упороться по безопаности

`fact_caching` - Включаем кеширование фактов. Каждый раз когда Ansible подключается к серверу, он собирает информацию об этом сервере

`library` - В какой папке будет происходить поиск дополнительные модули. Если пишешь свои модули то может пригодиться. Но на первое время не особо нужно, можно пока убрать из конфига

`gather_facts` - Сообщаем Ansible, чтобы перед выполнением задач, он собирал факты с серверов (ip адреса, интерфейсы и т.п.)

`interpreter_python` - Автоматически определять какой интерпретатор python запустить. Например если у тебя их установлено несколько, то он снимает с тебя такую ответственность и выберет самостоятельно.

#### [connection]
`pipelining` - Повышает производительность за счёт уменьшения количества операций передачи данных между хостами. Полезно при медленных сетевых соединениях.
Аналогично для блока [ssh_connection]

>В инфраструктуре продакшена можно опустить сбор всех фактов, это ощутимо ускорит работу. А оставить только сбор нужных фактов
>Сделать это можно так ->

```
[defaults]
gather_subset = network, hardware
```


Вроде как разобрались с параметрами, кстати, ты можешь проверить какие параметры активны, используй для этого команду
```
ansible-config dump
```

На выходе получишь подобное
```
❯ ansible-config dump | head -5
ACTION_WARNINGS(default) = True
AGNOSTIC_BECOME_PROMPT(default) = True
ALLOW_BROKEN_CONDITIONALS(default) = False
ALLOW_EMBEDDED_TEMPLATES(default) = True
ANSIBLE_CONNECTION_PATH(default) = None
```