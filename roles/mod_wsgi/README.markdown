# Notes on installing mod_wsgi on CentOS

paths:

- CentOS 7 apache modules folder: `/usr/lib64/httpd/modules` (alias `/etc/httpd/modules`)
- standard anaconda base home: `/opt/miniconda3`
- anaconda base site packages: `<anaconda_home>/lib/python3.9/site-packages`
- location of mod_wsgi so file in pip/conda install: `<anaconda_home>/lib/python3.9/site-packages/mod_wsgi/server/mod_wsgi-py39.cpython-39-x86_64-linux-gnu.so`
- default module load: `/etc/httpd/conf.modules.d/10-wsgi.conf`
- base module conf:

        LoadModule wsgi_module /usr/lib64/httpd/modules/mod_wsgi.so
        WSGIPythonHome /opt/miniconda3
        AddHandler wsgi-script .wsgi

# manual instructions

- `pip install mod_wsgi` in base, app uses virtualenv or conda env

    - in conda base, `pip install mod_wsgi`
    - copy `.so` file that results (`/opt/miniconda3/lib/python3.9/site-packages/mod_wsgi/server/mod_wsgi-py39.cpython-39-x86_64-linux-gnu.so`) into modules (`/etc/httpd/modules`)
    - reconfigure apache (`/etc/httpd/conf.modules.d/10-wsgi.conf`) to load new module rather than old. module conf:

            #LoadModule wsgi_module /usr/lib64/httpd/modules/mod_wsgi.so
            LoadModule wsgi_module /usr/lib64/httpd/modules/mod_wsgi-py39.cpython-39-x86_64-linux-gnu.so
            WSGIPythonHome /opt/miniconda3
            AddHandler wsgi-script .wsgi

    - restart apache.
    - touch wsgi.py to reload django application.
    - virtual end and conda env worked!

- `pip install mod_wsgi` in base, app uses conda env

    - in conda base, `pip install mod_wsgi`
    - copy `.so` file that results (`/opt/miniconda3/lib/python3.9/site-packages/mod_wsgi/server/mod_wsgi-py39.cpython-39-x86_64-linux-gnu.so`) into modules (`/etc/httpd/modules`)
    - reconfigure apache (`/etc/httpd/conf.modules.d/10-wsgi.conf`) to load new module rather than old. module conf:

            #LoadModule wsgi_module /usr/lib64/httpd/modules/mod_wsgi.so
            LoadModule wsgi_module /usr/lib64/httpd/modules/mod_wsgi-py39.cpython-39-x86_64-linux-gnu.so
            WSGIPythonHome /opt/miniconda3
            AddHandler wsgi-script .wsgi

    - restart apache.
    - touch wsgi.py to reload django application.
    - worked?

- `conda install -c conda-forge mod_wsgi` in base, app uses virtualenv
- `conda install -c conda-forge mod_wsgi` in base, app uses conda env

    - no dice.
