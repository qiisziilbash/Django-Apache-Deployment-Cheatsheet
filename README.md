# Django-Apache-Deployment-Cheatsheet
This is a cheatsheet for steps and commands for deploying your django project using Apache

- Don’t forget to change the DEBUG mode in setting file after final push
- Migrate database in production  when you change the models; NO need for “makemigration” in production
  - sudo ./venv/bin/python3 manage.py migrate
- sudo pip install -r requirements.txt
