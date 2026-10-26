![](CTF/TryHackMe/Eazy/SimpleCTF/screenshots/image0.png)

*	Difficult: `Eazy`
## WriteUp

1. Перейдя по адресу не узнаем ничего интересного 

   ![](CTF/TryHackMe/Eazy/SimpleCTF/screenshots/image1.png)

2. Я просканировал сервисы, и нашел несколько точек входа 21 ftp порт позволяющий зайти под анонимным пользователем, но там ничего инетресного я не нашел, 2222 порт SSH и 80 порт http. Сканируя директории нашел директорию `/simple` и перейдя в нее увидел CMS, пролистав вниз страницы увидел версию `CMS Made Simple version 2.2.8` Она уязвима к time based blind SQLi в параметре `m1_idlist` модуля News

   ![[CTF/TryHackMe/Eazy/SimpleCTF/screenshots/image2.png]]

3. Я перехватил этот запрос в BurpSuite, и изменил метот запроса на POST. Добавил в тело запроса нужный параметр, который нашел в эксплоите на ExploitDB. Можно было сделать проще и просто запустить готовый скрипт, но я решил сделать всё руками.

   ![](CTF/TryHackMe/Eazy/SimpleCTF/screenshots/image4.png)

4. Чтобы проверить time based SQLi, нужно заставить страничку отгружаться допустим с задержкой в 5 секунд. Попробовал всё работало, но потом я понял что мне нужно будет брутить, а так как у меня нет BurpPro, решил просто не париться и запустить готовый эксплоит с ExploitDB.  Пришлось немного его адаптировать под python3, так как он написан был на python2.  Вот что он нашел.

   ![](CTF/TryHackMe/Eazy/SimpleCTF/screenshots/image5.png)

5. Сбрутил пароль через гидру

   ![](CTF/TryHackMe/Eazy/SimpleCTF/screenshots/image6.png)

6. Вошел по SSH и прочитал первый флаг

   ![](CTF/TryHackMe/Eazy/SimpleCTF/screenshots/image7.png)

7. Я узнал что могу выполнять `vim` от судо без пароля с текущего пользователя. Я выполнил команду `sudo vim -c ':!/bin/sh` и получил рут оболочку, после чего, прочитал финальный файл.

   ![](CTF/TryHackMe/Eazy/SimpleCTF/screenshots/image8.png)