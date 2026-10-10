
Например у нас есть log-файл - test.log
```
192.168.1.101 - - [15/May/2024:10:15:32 +0300] "GET /index.html HTTP/1.1" 200 3456 "https://example.com/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/91.0                                               
203.0.113.45 - - [15/May/2024:10:15:35 +0300] "POST /login.php HTTP/1.1" 401 1280 "https://example.com/login" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) Safari/605.1.15"                              
198.51.100.22 - - [15/May/2024:10:16:01 +0300] "GET /images/logo.png HTTP/1.1" 200 8923 "https://example.com/index.html" "Mozilla/5.0 (iPhone; CPU iPhone OS 14_0 like Mac OS X) AppleWebKit/605.1.15"         
192.168.1.105 - - [15/May/2024:10:17:23 +0300] "GET /api/users HTTP/1.1" 200 4521 "https://example.com/dashboard" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) Firefox/89.0"                                    
10.0.0.55 - - [15/May/2024:10:18:45 +0300] "GET /favicon.ico HTTP/1.1" 404 169 "https://example.com/" "Mozilla/5.0 (Linux; Android 11; SM-G991B) Chrome/91.0"   
203.0.113.45 - - [15/May/2024:10:19:12 +0300] "GET /admin/config.php HTTP/1.1" 403 892 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36" 172.16.8.3 - - [15/May/2024:10:20:05 +0300] "GET /css/style.css HTTP/1.1" 200 2048 "https://example.com/index.html" "Mozilla/5.0 (X11; Linux x86_64) Chrome/90.0" 
192.168.1.101 - - [15/May/2024:10:21:33 +0300] "GET /js/script.js HTTP/1.1" 200 5678 "https://example.com/index.html" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) Edge/91.0"
```

Чтобы вытащить только ip-aдреса из него можно использовать следующую запись.

```
awk '{print $1}' test.log | sort -rn | uniq -c | sort -r 
```

```
awk - разбивает его по разделителям, в данном случае разделитель пробел " " и указываем индекс разделителя. Так как строка начинается сразу с ip-адреса то он и выводит только его
```

```
uniq -c - считает повторения строк
```

```
sort - сортирует строки, в данном примере не видно наглядно, но полезная штука
```

```
head -5 обрезает вывод до 5 строк
```

Таким образом мы увидим:
1) Чистый вывод строки (чисто ip-адрес из лога)
2) Сколько раз эта строка повторяется в логе (сколько запросов по данному ip-адресу)
3) Сортируем по самому высокому числу
4) Топ - 5 ip-адресов по которым наибольшее количество запросов

Пример вывода:
```
2 203.0.113.45  
2 192.168.1.101  
1 198.51.100.22  
1 192.168.1.105  
1 172.16.8.3  
1 10.0.0.55
```
