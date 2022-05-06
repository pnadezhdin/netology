# Домашнее задание к занятию "08.01 Введение в Ansible"

## Подготовка к выполнению
1. Установите ansible версии 2.10 или выше.
2. Создайте свой собственный публичный репозиторий на github с произвольным именем.
3. Скачайте [playbook](./playbook/) из репозитория с домашним заданием и перенесите его в свой репозиторий.

## Основная часть
1.Попробуйте запустить playbook на окружении из test.yml, зафиксируйте какое значение имеет факт some_fact для указанного хоста при выполнении playbook'a.
root@ubuntu:/home/p/playbook# ansible-playbook -i inventory/test.yml site.yml

PLAY [Print os facts] **********************************************************

TASK [Gathering Facts] *********************************************************
ok: [localhost]

TASK [Print OS] ****************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **************************************************************
ok: [localhost] => {
    "msg": 12
}

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

root@ubuntu:/home/p/playbook# ansible-playbook -i inventory/test.yml site.yml

2. Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.

root@ubuntu:/home/p/playbook# cat group_vars/all/examp.yml
---
  some_fact: all default fact
root@ubuntu:/home/p/playbook# ansible-playbook -i inventory/test.yml site.yml 

PLAY [Print os facts] **********************************************************

TASK [Gathering Facts] *********************************************************
ok: [localhost]

TASK [Print OS] ****************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}

TASK [Print fact] **************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}

PLAY RECAP *********************************************************************
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

root@ubuntu:/home/p/playbook# 

3. Воспользуйтесь подготовленным (используется docker) или создайте собственное окружение для проведения дальнейших испытаний.

выполнено

4. Проведите запуск playbook на окружении из prod.yml. Зафиксируйте полученные значения some_fact для каждого из managed host.

root@ubuntu:/home/p/playbook# ansible-playbook -i inventory/prod.yml site.yml 

PLAY [Print os facts] **********************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos7]
ok: [ubuntu]

TASK [Print OS] ********************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ******************************************************************************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}

PLAY RECAP *******************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

5. Добавьте факты в group_vars каждой из групп хостов так, чтобы для some_fact получились следующие значения: для deb - 'deb default fact', для el - 'el default fact'.

root@ubuntu:/home/p/playbook# cat group_vars/deb/examp.yml 
---
  some_fact: "deb default fact"
root@ubuntu:/home/p/playbook# cat group_vars/el/examp.yml
---
  some_fact: "el default fact"
root@ubuntu:/home/p/playbook# 

6. Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные значения для всех хостов.

root@ubuntu:/home/p/playbook# ansible-playbook -i inventory/prod.yml site.yml 

PLAY [Print os facts] **********************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos7]
ok: [ubuntu]

TASK [Print OS] ********************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ******************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *******************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   


7. При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.

root@ubuntu:/home/p/playbook# ansible-vault encrypt group_vars/deb/examp.yml 
New Vault password: 
Confirm New Vault password: 
Encryption successful
root@ubuntu:/home/p/playbook# ansible-vault encrypt group_vars/el/examp.yml
New Vault password: 
Confirm New Vault password: 
Encryption successful
root@ubuntu:/home/p/playbook#

8. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь в работоспособности.

root@ubuntu:/home/p/playbook# ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password: 

PLAY [Print os facts] **********************************************************

TASK [Gathering Facts] *********************************************************
ok: [centos7]
ok: [ubuntu]

TASK [Print OS] ********************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ******************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP ******************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  

7. При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.

root@ubuntu:/home/p/playbook# cat group_vars/deb/examp.yml 
$ANSIBLE_VAULT;1.1;AES256
64646132363131613861393032666336653761666432363463343031626337656461363836646465
3164346635663739363666353561616438633036376534310a396334383036313234643337346532
39616233663533666632653436663865643262373966626630333038643665343262383131316634
3332323331386537360a346339373033383134383534306436323036616633373134666639656638
33636164613730373364353735383339393533653730393438613362343363623866346236393735
3236393639343835306233323165333565653166356131643662
root@ubuntu:/home/p/playbook# ansible-vault encrypt group_vars/deb/examp.yml 
New Vault password: 
Confirm New Vault password: 
ERROR! input is already encrypted
root@ubuntu:/home/p/playbook# ansible-vault decrypt group_vars/deb/examp.yml
Vault password: 
Decryption successful
root@ubuntu:/home/p/playbook# ansible-vault encrypt group_vars/deb/examp.yml 
New Vault password: 
Confirm New Vault password: 
Encryption successful
root@ubuntu:/home/p/playbook# ansible-vault decrypt group_vars/el/examp.yml
Vault password: 
Decryption successful
root@ubuntu:/home/p/playbook# ansible-vault encrypt group_vars/el/examp.yml 
New Vault password: 
Confirm New Vault password: 
Encryption successful
root@ubuntu:/home/p/playbook# 

8. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь в работоспособности.

root@ubuntu:/home/p/playbook# ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass
Vault password: 

PLAY [Print os facts] **********************************************************

TASK [Gathering Facts] *********************************************************
ok: [ubuntu]
ok: [centos7]

TASK [Print OS] ********************************************************************************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ******************************************************************************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *****************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

9. Посмотрите при помощи ansible-doc список плагинов для подключения. Выберите подходящий для работы на control node.

Нужен "local"

10. В prod.yml добавьте новую группу хостов с именем local, в ней разместите localhost с необходимым типом подключения.

root@ubuntu:/home/p/playbook# cat inventory/prod.yml
---
  el:
    hosts:
      centos7:
        ansible_connection: docker
  deb:
    hosts:
      ubuntu:
        ansible_connection: docker
  local:
    hosts:
      localhost:
        ansible_connection: local   

11. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь что факты some_fact для каждого из хостов определены из верных group_vars.

root@ubuntu:/home/p/playbook# ansible-playbook -i inventory/prod.yml site.yml --ask-vault-pass

Vault password: 

PLAY [Print os facts] **************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************
[DEPRECATION WARNING]: Distribution Ubuntu 20.04 on host localhost should use /usr/bin/python3, but is using /usr/bin/python for backward 
compatibility with prior Ansible releases. A future Ansible release will default to using the discovered platform python for this host. See 
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information. This feature will be removed in 
version 2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
ok: [localhost]
ok: [centos7]
ok: [ubuntu]

TASK [Print OS] ********************************************************************************************************************************
ok: [localhost] => {
    "msg": "Ubuntu"
}
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}

TASK [Print fact] ******************************************************************************************************************************
ok: [localhost] => {
    "msg": "all default fact"
}
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}

PLAY RECAP *************************************************************************************************************************************
centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

12. Заполните README.md ответами на вопросы. Сделайте git push в ветку master. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым playbook и заполненным README.md.

https://github.com/pnadezhdin/netology.

## Необязательная часть

1. При помощи `ansible-vault` расшифруйте все зашифрованные файлы с переменными.
2. Зашифруйте отдельное значение `PaSSw0rd` для переменной `some_fact` паролем `netology`. Добавьте полученное значение в `group_vars/all/exmp.yml`.
3. Запустите `playbook`, убедитесь, что для нужных хостов применился новый `fact`.
4. Добавьте новую группу хостов `fedora`, самостоятельно придумайте для неё переменную. В качестве образа можно использовать [этот](https://hub.docker.com/r/pycontribs/fedora).
5. Напишите скрипт на bash: автоматизируйте поднятие необходимых контейнеров, запуск ansible-playbook и остановку контейнеров.
6. Все изменения должны быть зафиксированы и отправлены в вашей личный репозиторий.

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
