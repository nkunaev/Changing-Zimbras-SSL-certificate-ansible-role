Changing Zimbras SSL certificate
=========

Роль для Ansible, которая заменяет SSL сертификат на почтовом сервере Зимбры

Role Variables
--------------

Необходимо указать путь к full chain сертификату, а также к key-файлу в формате абсолютного или относительного пути на локалхосте 
```yaml
path_to_fullchain_crt: "/absolute/path/to/crt.crt"  
path_to_key: "~/patch/to/key.key"  
```

Usage:
--------------

Для подключения роли необходимо в корневом каталоге, где лежит playbook, создать файл *requirements.yml*, следующего содержания:  
```yaml
---
- src: git@github.com:nkunaev/nkunaev-changing-zimbras-ssl-certificate.git
  scm: git
  version: main
  name: zimbra (имя роли можно указать любое)
```
Затем выполнить команду, которая создаст в рабочей директории каталог *roles*, в который скачает указанную роль:   
```ignorelang
ansible-galaxy install -r requirements.yml -p roles
```


Example Playbook
----------------
```yaml
---
- name: Update Zimbra crt  
  hosts: zimbra_server  
  roles:  
    - role: zimbra  (роль можно назвать как угодно при импорте из предыдущего шага)
      path_to_fullchain_crt: "./wildcard_domain_crt.crt"  
      path_to_key: "./wildcard_domain_key.key"  
```

Example Inventory
----------------

```yaml
---
zimbra_server:
  hosts:
    mail.nordavind.ru:
      ansible_connection: ssh
      ansible_host: 192.168.88.88
      ansible_user: "{{ login_user }}"
      ansible_ssh_private_key_file: "{{ path_to_priv_key }}"
      ansible_become_method: sudo
      ansible_become_password: "{{ su_password }}" 
```

Example group_vars/zimbra_server/vars.yml
----------------

```yaml
login_user: username
su_password: very_strong_password
```

Example group_vars/all/vars.yml
----------------

```yaml
path_to_priv_key: ~/.ssh/pkey_without_password
```

License
-------

MIT

Author Information
------------------

Nikita Kunaev (@vek010)