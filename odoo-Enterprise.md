
## Odoo Enterprise:

1. First Login to the server:

2. Create Database: 


Terminate All Connections to the Database, connect to your PostgreSQL server using a superuser (like postgres) and run:

```
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE datname = 'odoo'
  AND pid <> pg_backend_pid();
```


_Remove Old DB:_

```
drop database odoo;
drop user odoo;
```


```
CREATE USER odoo WITH ENCRYPTED PASSWORD 'demo123';
create database odoodb owner odoo;

GRANT ALL PRIVILEGES ON DATABASE odoodb TO odoo;
```


```
ALTER ROLE odoo WITH CREATEDB;
```


3. In that directory installed odoo community edition:

```
cd /home/uistl
```


_Clone Odoo Community Edition:_

```
git clone https://github.com/odoo/odoo.git --depth 1 --branch 18.0 --single-branch
```


Copied `odoo.conf` from debians folder to the main folder named odoo.

```
cp /home/uistl/odoo/debian/odoo.conf /home/uistl/odoo
```




4. Upload or Copy Enterprise Edition to odoo home directory: 

```
cp odoo_18.0+e.latest.tar.gz /home/uistl/odoo

tar -xvf odoo_18.0+e.latest.tar.gz
```



Edit the `odoo.conf` and adjust the Enterprise `addons_path`:

```
vim /home/uistl/odoo

[options]
; This is the password that allows database operations:
admin_passwd = admin123
db_host = 192.168.10.191
db_port = 5432
db_user = odoo
db_password = demo123
addons_path = /home/uistl/odoo/addons,/home/uistl/odoo/odoo-18.0+e.20250517/odoo/addons
default_productivity_apps = True
```



Then Installed requirements.txt file dependencies:

```
cd /home/uistl/odoo

pip3.11 install -r requirements.txt
```



5. Update:

```
python3.11 odoo-bin -c /home/uistl/odoo/odoo.conf -d ygodoodb -u all
```



6. Database Admin Password Change:

```
psql -U odoo -d odoodb -h 192.168.10.191
```


```
select id,active,login from res_users;
```


```
SELECT
    ru.id AS user_id,
    ru.login,
    rp.name,
    rp.email,
    ru.active,
    CASE
        WHEN ru.share = TRUE THEN 'External User'
        ELSE 'Internal User'
    END AS user_type
FROM
    res_users ru
JOIN
    res_partner rp ON ru.partner_id = rp.id
ORDER BY
    ru.id;
```


```
UPDATE res_users SET password = 'newpassword' WHERE login = 'admin';
```



7. For web_enterprise:

```
python3.11 odoo-bin -c /home/uistl/odoo/odoo.conf -d ygodoodb -i web_enterprise --stop-after-init
```


8. Subscription Active: 

While running this command we should install a app from odoo to register the database after that i give our subscription code and enable our lincese





### Links:
- [Odoo | github.com](https://github.com/odoo/odoo)
- [Odoo Download](https://www.odoo.com/page/download)
- [Odoo Source install](https://www.odoo.com/documentation/18.0/administration/on_premise/source.html)
- [Switch from Community to Enterprise](https://www.odoo.com/documentation/18.0/administration/on_premise/community_to_enterprise.html)


