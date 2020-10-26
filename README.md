# Django + Apache Deployment Cheatsheet
This is a cheatsheet for steps and commands for deploying your django project using Apache



## Django Project Hello world
 1. ```$ curl https://bootstrap.pypa.io/getpip.py o getpip.py; python3 getpip.py```
 2. ```$ pip install django```
 3. ```$ pip install virtualenv```
 4. ```$ mkdir djangoprojects```
 5. ```$ cd djangoprojects```
 6. ```$ virtualenv venv```
 7. ```$ source venv/bin/activate```
 8. ```$ djangoadmin startproject myproject```
 9. ```$ python manage.py runserver```

## Deployment
- add your media, database, venv, __pycache__ to the .gitignore; there is a compelete list that you can find here:
    - https://github.com/jpadilla/django-project-template/blob/master/.gitignore
- keep migration files in the git 
    - you will need to migrate them in target server
- don't run "makemigrations" in the target server 
    - you will need to ONLY run "migrate"
- Make a requirements list:
    - ```$ pip freeze > requirements.txt ```
- make appropriate changes in your project settings.py file
    - For examples:
        - change DEBUG to False
        - use a stong website key and put in environment variable 
        - look at django documentation for deployment checklist and etc 
- push your code to your git-server
- pull your code in your target server
- give right permissions to the web-server (e.g. $ chown www-data:www-data -R /var/www/myproject)
- make a new venv in the target server and activate it
- Install requirements.txt
    - ```$ sudo pip install -r requirements.txt ```
- Migrate database:
    - ```$ sudo ./venv/bin/python3 manage.py migrate ```
- Give permissions to www-data user:
    - ```$ chown www-data:www-data -R /var/www/Project ```
- Config the apache2
    - ```$ cd /etc/apache2/sites-available ```
    - Edit conf-file
    - 
        ``` Apache Config file:
        ===========================                                                                                                         ayass_lab_management_system.conf                                                                                                                           
        <VirtualHost *:80>
            <Directory /var/www/ayass_lab_management_system>
                <Files wsgi.py>
                    Require all granted
                </Files>
            </Directory>
        
            WSGIDaemonProcess ayass_lab_management_system python-path=/var/www/ayass_lab_management_system python-home=/var/www/ayass_lab_management_system/venv
            WSGIProcessGroup ayass_lab_management_system
            WSGIScriptAlias / /var/www/ayass_lab_management_system/ayass_lab_management_system/wsgi.py
        
            Alias /static /var/www/ayass_lab_management_system/static
            <Directory /var/www/ayass_lab_management_system/static>
                Require all granted
            </Directory>
        </VirtualHost>
        ==============================
        ```

    - for ssl configurations:
        - ```$ sudo a2enmod ssl ```
- Enable conf-file
    - ```$ sudo a2ensite conf-file```
    - Some useful commands for Apache debugging:
        - ```$ tail /var/log/apache2/error.log ```

        - ```$ systemctl status apache2.service ```
- Reload Apache
    - ```$ sudo service apache2 reload```
- Restart your web-server
    - ```$ sudo service apache2 restart```


# Some useful commands
- Copy some files from local to server
    - ```$ scp -P [port number] -r /home/dir_to_upload [username]@[host-domain(e.g. web@edu.net)]:/var/www```
- Copy some files from server to local
    - ```$ scp -P [port number] [username]@[host domain]:/var/www/file.txt /home/```











