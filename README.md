Несколько рабочих playbook'ов для настройки centos 6.10 в рамках подготовки установки vipnet coordinator 4 for linux.

Пошагово:

* Не писал task для отключения selinux, поэтому отключить руками, перезагрузить. В `/etc/selinux/config` прописать `SELINUX=disable`
* Скопировать ssh ключ на удаленный хост `ssh-copy-id -i root@192.168.0.1`
* Исправить адрес в ./inventory и проверить доступность `ansible -m ping all`
* Отключить ipv6 `ansible-playbook disable-v6.yml`
* Отключить iptables `ansible-playbook firewall.yml`
* Включить forwarding `ansible-playbook forwarding.yml`
* reboot `ansible-playbook reboot.yml`
* Установить обновления и требуемые пакеты `ansible-playbook update.yml`
* reboot `ansible-playbook reboot.yml`
* ntp `ansible-playbook ntp.yml`
* Скопировать дистрибутив координатора в `/usr/src` и распаковать там `ansible-playbook copy-vipnet.yml`.`community.general.iso_extract` в `copy-vipnet.yml` требует привилегий, поэтому запускать плей с ключем -K или от root'а. Для копирования дистрибутива нужен образ с софтовым координатором `/vipnet_files/vipnet.iso`.
* Заменить конфиг для ротации логов `/etc/logrotate.d/vipnet.log.conf` на прилагаемый. В нем изменена ротация для файлов alg с учетом записи отладки во временный файл.
