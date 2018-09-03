# 电商

| Python 系列 |  |
| :--- | :--- |
| [https://saleor.readthedocs.io/en/latest/architecture/payments.html](https://saleor.readthedocs.io/en/latest/architecture/payments.html) |  |

实施过程

**安装Python3 环境**

simply run

```text
virtualenv -p python3 envname
```

Update after OP's edit:

There was a bug in the OP's version of virtualenv, as described [here](https://github.com/pypa/virtualenv/issues/463). The problem was fixed by running:

```text
pip install --upgrade virtualenv
```

**安装postgresql**



FOR RHEL / CENTOS / SL / OL 5,6

```text
  service postgresql initdb
  chkconfig postgresql on
```

FOR RHEL / CENTOS / SL / OL 7 OR FEDORA 27 AND LATER DERIVED DISTRIBUTIONS:

```text
  postgresql-setup initdb
  systemctl enable postgresql.service
  systemctl start postgresql.service
```

\(env\) \[kevin@localhost saleor\]$ sudo postgresql-setup initdb

WARNING: using obsoleted argument syntax, try --help

WARNING: arguments transformed to: postgresql-setup --initdb --unit postgresql

 \* Initializing database in '/var/lib/pgsql/data'

 \* Initialized, logs are in /var/lib/pgsql/initdb\_postgresql.log

![](.gitbook/assets/image%20%281%29.png)

##  gtk+

fedora

```text
sudo yum install redhat-rpm-config python-devel python-pip python-cffi libffi-devel cairo pango gdk-pixbuf2
```



### Installation

1. Clone the repository \(or use your own fork\):

   ```text
   $ git clone https://github.com/mirumee/saleor.git
   ```

2. Enter the directory:

   ```text
   $ cd saleor/
   ```

3. Install all dependencies:

   We strongly recommend [creating a virtual environment](https://docs.python.org/3/tutorial/venv.html) before installing any Python packages.

   ```text
   $ pip install -r requirements.txt
   ```

4. Set `SECRET_KEY` environment variable.

   We try to provide usable default values for all of the settings. We’ve decided not to provide a default for `SECRET_KEY` as we fear someone would inevitably ship a project with the default value left in code.

   ```text
   $ export SECRET_KEY='<mysecretkey>'
   ```

   Warning

   Secret key should be a unique string only your team knows. Running code with a known `SECRET_KEY` defeats many of Django’s security protections, and can lead to privilege escalation and remote code execution vulnerabilities. Consult [Django’s documentation](https://docs.djangoproject.com/en/1.11/ref/settings/#secret-key) for details.

5. Create a PostgreSQL user:

   See [PostgreSQL’s createuser command](https://www.postgresql.org/docs/current/static/app-createuser.html) for details.

   Note

   You need to create the user to use within your project. Username and password are extracted from the `DATABASE_URL` environmental variable. If absent they both default to `saleor`.

   Warning

   While creating the database Django will need to create some PostgreSQL extensions if not already present in the database. This requires a superuser privilege.

   For local development you can grant your database user the `SUPERUSER` privilege. For publicly available systems we recommend using a separate privileged user to perform database migrations.

6. Create a PostgreSQL database

   See [PostgreSQL’s createdb command](https://www.postgresql.org/docs/current/static/app-createdb.html) for details.

   Note

   Database name is extracted from the `DATABASE_URL` environmental variable. If absent it defaults to `saleor`.

7. Prepare the database:

   ```text
   $ python manage.py migrate
   ```

   Warning

   This command will need to be able to create database extensions. If you get an error related to the `CREATE EXTENSION` command please review the notes from the user creation step.

8. Install front-end dependencies:

   ```text
   $ npm install
   ```

   Note

   If this step fails go back and make sure you’re using new enough version of Node.js.

9. Prepare front-end assets:

   ```text
   $ npm run build-assets
   ```

10. Compile e-mails:

    ```text
    $ npm run build-emails
    ```

11. Start the development server:

    ```text
    $ python manage.py runserver
    ```



