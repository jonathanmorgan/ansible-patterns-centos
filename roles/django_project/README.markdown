# Troubleshooting

## Troubleshooting basics

- if the application gives 500 errors through apache and no error messages are output in either the logs or your python logging destinations, go back to the basics:

    - enable the virtualenv or conda env that you are using for your django application.
    - cd into the django project folder and try "python manage.py shell". This should show if there are errors in your application code, or the packages in your python environment.
    - if this works, try "python manage.py showmigrations".
    - if this works, try "python manage.py runserver". This should show problems with your wsgi.py file.

## Apache prefork vs. worker

- https://serverfault.com/questions/88000/how-do-i-tell-if-apache-is-running-as-prefork-or-worker
- Some issues here depend on knowing prefork or worker: 
    - https://modwsgi.readthedocs.io/en/master/user-guides/installation-issues.html

## No module "encoding"
If you get an error about no module encoding, this usually means that something caused the wsgi.py file to crash, and so your application is not at all initialized. Some things you can try:

- It could be a simple permissions problem. You can try to adjust permissions.
- A permissions problem with logging output (unwriteable log output file) can cause this also.
- If on CentOS 7 turn off SELinux (secure linux) and see if that fixes it. If so, then it is a selinux security thing. Options:
    - Look at specific error in /var/log/httpd/error_log and try to update settings until your error is resolved.
    - Turn off selinux:
        
        - Open /etc/selinux/config
        - Change value for SELINUX from "enforcing" to "permissive" (will log errors, but not enforce security) or "disabled" (turns off selinux entirely).
        - https://www.golinuxcloud.com/disable-selinux/
        - If disabling selinux fixes problem, then you can either keep it off, or try to figure out the exact setting change you need to fix your problem.
