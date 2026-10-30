
![](CTF/TryHackMe/Eazy/Dav/screenshots/image0.png)

*	Difficult: `Eazy`
## WriteUp

1. Зайдя по адресу увидел начальную страничку
   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image1.png)

2. Сканирование не показало других открытых портов, по этому решил протестить директории

3. В директориях видно `webdav` и `server-status`. Пошел в `webdav`
   ***`webdav` - расширение для протокола http позволяющее работать с файлами на удаленных серверах, так же как на смонтированном диске. Проще говоря, удаленный жесткий диск или облако***
   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image2.png)

4. У меня запросили пароль, и я затестил первое, что пришло в голову
   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image4.png)

5. Введенные мной`webdav:webdav` не подошли и я пошел гуглить дефолтные учетные данные под этот протокол на apache. Так же выяснил что авторизация идёт через basiс. username:password кодируются в base64 при отправке запроса.
   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image5.png)

6. Нашел и попробовал ввести данные `wampp:xampp` и они подошли

   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image7.png)
   
7. В директории лежит файл `passwd.dav`. Зашел туда
   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image6.png)

8. Увидел юзнейм и хэш, пока что мне хэш не пригодится и я пошел гуглить как проверить, можем ли мы загружать файлы через webdav

   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image8.png)
   
9. Прочитал статью про этот протокол на ***hacktricks*** и решил использовать ту же методологию - заюзать инструмент `devtest`. Он проверяет, можем ли мы загружать, файлы черезе `webdav` и оказалось, что можем, причем можем загружать php.
   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image9.png)

10. С помощью инструмента `cadaver`, который является консольным клиентом webdav и работает по аналогии с ftp, я зашел в клиент с теми же данными, что вводил в браузере и загрузил подготовленный PHPwebshell на сервер

![](CTF/TryHackMe/Eazy/Dav/screenshots/image10.png)

![](CTF/TryHackMe/Eazy/Dav/screenshots/image11.png)

10. Затем в браузере кликнул на загруженный файл

   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image12.png)
   
11. Webshell работает. Далее, для удобства я решил получить ReverseShell
   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image13.png)
   
12. Для получения обратной оболочки, использовал уже знакомую команду.
   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image14.png)
   
13. После получения оболочки, удалил файлы которые были созданы через `davtest`,  стабилизировал шелл, посмотрел какие есть домашние директории пользователей и какие файлы там лежат и прочитал первый флаг.
   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image15.png)
   
14. Следующий шаг - повышение привилегий. Я проверил какие файлы содержат SUID бит, но ничего интересного тут не оказалось. Проверил, какие команды можно выполнять текущему пользователю от sudo и вывод команды говорит о том, что пользователь www-data может запускать команду `cat` от имени sudo без пароля, тоесть я могу читать любые файлы в системе. 

![](CTF/TryHackMe/Eazy/Dav/screenshots/image16.png)

11. Так как обычно, флаг лежит всегда по пути /root/root.txt я сразу же его прочитал.
   ![](CTF/TryHackMe/Eazy/Dav/screenshots/image17.png)