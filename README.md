# настраиваем split-dns

взять стенд https://github.com/erlong15/vagrant-bind
добавить еще один сервер client2
завести в зоне dns.lab
имена
web1 - смотрит на клиент1
web2 смотрит на клиент2

завести еще одну зону newdns.lab
завести в ней запись
www - смотрит на обоих клиентов

настроить split-dns
клиент1 - видит обе зоны, но в зоне dns.lab только web1

клиент2 видит только dns.lab

*) настроить все без выключения selinux
Критерии оценки: 4 - основное задание сделано, но есть вопросы
   
   5 - сделано основное задание
    6 - выполнено задания со звездочкой

# Решение

![alt tag](https://github.com/RaibeartRuadh/mydns/blob/main/pic3.png "")

Запускаем стенд.

    $ vagrant up

- Для полоноты картины добавлен еще один хост client2
- В настройки регистратора домена разместим записи типа CNAME. Это подтвердит право собственности на client1 и client2
- Для Мастер-сервера сделаем PTR-записи и новую зону named.newdns.lab где разместим информацию о клиентах.
- Создадим BIND для master - master-named.conf и для slave slave-named.conf 
- Чтобы не пришлось трогать Selinux, будем класть файлы с информацией по зонам в /var/named.
- Добавим view для клиентов для Master и Slave, а также отдельный default view на все остальные запросы на Master и Slave

Проверяем на Client1 (клиент1 - видит обе зоны, но в зоне dns.lab только web1):

![alt tag](https://github.com/RaibeartRuadh/mydns/blob/main/pic1.png "")

Проверяем на Client2 (видит только dns.lab):
![alt tag](https://github.com/RaibeartRuadh/mydns/blob/main/pic2.png "")

Проверяем состояние selinux на всех хостах

      $ getenforce

Selinux возвращает значение Enforcing.




