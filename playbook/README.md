# Самоконтроль выполненения задания

1. Где расположен файл с `some_fact` из второго пункта задания?
```
https://github.com/pnadezhdin/netology/playbook/group_vars/all/examp.yml
https://github.com/pnadezhdin/netology/playbook/group_vars/deb/examp.yml
https://github.com/pnadezhdin/netology/playbook/group_vars/el/examp.yml
https://github.com/pnadezhdin/netology/playbook/group_vars/local/examp.yml
```
2. Какая команда нужна для запуска вашего `playbook` на окружении `test.yml`?
```
ansible-playbook -i inventory/test.yml site.yml 
```
3. Какой командой можно зашифровать файл?
```
ansible-vault encrypt group_vars/deb/examp.yml 
```
4. Какой командой можно расшифровать файл?
```
ansible-vault encrypt group_vars/el/examp.yml
```
5. Можно ли посмотреть содержимое зашифрованного файла без команды расшифровки файла? Если можно, то как?
```
ansible-vault view site.yml
```
6. Как выглядит команда запуска `playbook`, если переменные зашифрованы?
```
ansible-playbook all -m ansible.builtin.debug -a var="new_user_password" -e "@vars.yml" --vault-id dev@a_password_file
```
7. Как называется модуль подключения к host на windows?
```
Модуль - pywinrm
Нажно еще скачать и запустить от админа скрипт на виндовс машинах один раз. Это нужно, чтобы виндовс мог говорить с ансибл. https://github.com/ansible/ansible/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1

```
8. Приведите полный текст команды для поиска информации в документации ansible для модуля подключений ssh
```
ansible-doc -t connection ssh
```
9. Какой параметр из модуля подключения `ssh` необходим для того, чтобы определить пользователя, под которым необходимо совершать подключение?
```
remote_user
```
