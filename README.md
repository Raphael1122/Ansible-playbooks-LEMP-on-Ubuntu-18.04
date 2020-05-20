### Ansible playbooks: install LEMP (nginx + php7.4-fpm + Mysql) + Certbot on Ubuntu 18.04

-- Cloner le référentiel

-- À partir du fichier hosts.yml.example, créez un fichier hosts.yml fonctionnel avec des variables valides

-- Si nécessaire, ajoutez les extensions nécessaires pour php7.4-fpm dans le fichier install_php.yml

```
        -   name: Install php-fpm 7.4
            apt:
                name:
                    - php7.4-fpm
                    ...
                    ...
                    ...
                    - php7.4-your_ext
                state: present
                update_cache: yes
```  
-- Dans le dossier du projet, exécutez la commande `make init`

-- Si nécessaire, exécutez séparément la commande `make create_certbot` pour pouvoir travailler sur le protocole https

En conséquence, sur le site distant, le serveur LEMP sera déployé, un utilisateur sera également créé, dans lequel les sites seront stockés dans le répertoire personnel.

##### Pour créer un hôte virtuel

-- Nous clonons le dossier ** rôles / exemple_hôte ** avec le nom souhaité, par exemple ** rôles / api_host **

-- Dans le fichier ** roles / api_host / defaults / main.yml ** nous écrivons le nom d'hôte correct, par exemple ** api.example.com **

-- Renommez le fichier ** roles / api_host / templates / example.com.j2 ** en fonction de votre hôte, par exemple ** api.example.com.j2 ** 34 ** Important: le fichier doit être nommé exactement comme votre hôte! **

-- S'il n'est pas nécessaire de travailler avec le protocole https, dans le fichier ** roles / api_host / tasks / main.yml ** supprimez la ligne `- import_tasks: add_certificate.yml`


-- Créez un playbook modélisé d'après `add_host.yml.example`, par exemple,` add_api_host.yml`, définissez le rôle souhaité, par exemple:
```
    roles:
        - api_host
```

Exécutez la commande: `ansible-playbook -i hosts.yml add_api_host.yml`
