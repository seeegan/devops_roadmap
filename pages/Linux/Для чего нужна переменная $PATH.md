Переменная окружения PATH содержит абсолютные пути директорий, в которых производится поиск исполняемых файлов при вводе команд

Давай посмотрим что это вообще за такая переменная и что она в себе содержит
```
echo $PATH                                                                       
```

Вывод:
```
/Users/seegan/python_project/python_project/bin:/Users/seegan/.local/bin:/Users/seegan/.pyenv/shims:/Users/seegan/.pyenv/bin:/opt/homebrew/bin:/opt/homebrew/sbin:/usr/local/bin:/System/Cryptexes/App/usr/bin:/usr/bin:/bin:/usr/sbin:/sbin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/local/bin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/bin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/appleinternal/bin:/opt/pmk/env/global/bin:/usr/local/go/bin
```

При вводе команды в терминале система сначала проверяет есть ли путь до бинарника в переменной PATH. То есть нам достаточно просто ввести команду.
Пример:
```
 ls                                                                              
```                                      

Вывод
```
 dir1   dir3  󰡯 file1  󰡯 file3  󰡯 list  test.log         
 dir2   dir4  󰡯 file2  󰡯 file4  󰡯 list_duplicate
```

А давайте посмотрим где вообще находится эта команда "ls".

```
whereis ls                              
ls: /bin/ls /usr/share/man/man1/ls.1
```

Как мы видим он находится в корневой директории /bin. Выполнилась она без проблем так как этот путь прописан в $PATH. Аналогично с другими командами или утилитами которые мы ставим на свою машину. Необходимо чтобы был не только бинарник самой утилиты но и прописанный путь в переменной $PATH (ну либо можете каждый раз вводить абсолютный путь😀)

Думаю что тему я раскрыл.