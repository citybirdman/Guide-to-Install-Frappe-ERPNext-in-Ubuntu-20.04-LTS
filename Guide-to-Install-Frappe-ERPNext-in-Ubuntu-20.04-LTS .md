# Guide-to-Install-Frappe-ERPNext-in-Ubuntu-20.04-LTS
A complete Guide to Install Frappe Bench in Ubuntu 22.04 LTS and install Frappe/ERPNext Application

### Pre-requisites 

      Python 3.6+
      Node.js 14+
      Redis 5                                       (caching and real time updates)
      MariaDB 10.3.x / Postgres 9.5.x               (to run database driven apps)
      yarn 1.12+                                    (js dependency manager)
      pip 20+                                       (py dependency manager)
      wkhtmltopdf (version 0.12.5 with patched qt)  (for pdf generation)
      cron                                          (bench's scheduled jobs: automated certificate renewal, scheduled backups)
      NGINX                                         (proxying multitenant sites in production)



### STEP 1 Install git
    sudo apt-get install git

### STEP 2 install python-dev

    sudo apt-get install python3-dev

### STEP 3 Install setuptools and pip (Python's Package Manager).

    sudo apt-get install python3-setuptools python3-pip

### STEP 4 Install virtualenv
    
    sudo apt-get install virtualenv
    
  CHECK PYTHON VERSION 
  
    python3 -V
  
  IF VERSION IS 3.8.X RUN
  
    sudo apt install python3.8-venv

### STEP 5 Install MariaDB

    sudo apt-get install software-properties-common
    sudo apt install mariadb-server
    sudo mysql_secure_installation
    
    
### STEP 6  MySQL database development files

    sudo apt-get install libmysqlclient-dev
###      STEP 7 Mariadb configuration
Create a file `/etc/my.cnf.d/frappe.cnf` or `/etc/mysql/mariadb.conf.d/frappe.cnf` and add

### For MariaDB <= 10.2
      
      [mysqld]
      innodb-file-format=barracuda
      innodb-file-per-table=1
      innodb-large-prefix=1
      character-set-client-handshake = FALSE
      character-set-server = utf8mb4
      collation-server = utf8mb4_unicode_ci
      
      [mysql]
      default-character-set = utf8mb4

### For MariaDB >= 10.3

      [mysqld]
      character-set-client-handshake = FALSE
      character-set-server = utf8mb4
      collation-server = utf8mb4_unicode_ci
      
      [mysql]
      default-character-set = utf8mb4
      
###      For MariaDB >= 10.6

https://discuss.erpnext.com/t/table-tabdefaultvalue-missing-error-when-creating-new-site/78019/10

      Restart MariaDB

###       Now press (Ctrl-X) to exit

    sudo service mysql restart

### STEP 8 install Redis
    
    sudo apt-get install redis-server

### STEP 9 install Node.js 14.X package

    sudo apt install curl 
    curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
    source ~/.profile
    nvm install 14.15.0  

### STEP 10  install Yarn

    sudo apt-get install npm

    sudo npm install -g yarn

### STEP 11 install wkhtmltopdf

    sudo apt-get install xvfb libfontconfig wkhtmltopdf
    

### STEP 12 install frappe-bench

    sudo -H pip3 install frappe-bench
    
    bench --version
    
### STEP 13 initilise the frappe bench & install frappe latest version 

    bench init --frappe-branch=version-13 --python=python3.8 erpnext
    
    cd erpnext/
    bench start
    
### STEP 14 create a site in frappe bench 
    
    bench new-site dcode.com
    
    bench use dcode.com

### STEP 15 install ERPNext latest version in bench & site

    bench get-app erpnext --branch version-13
    ###OR
    bench get-app https://github.com/frappe/erpnext --branch version-13

    bench --site dcode.com install-app erpnext
    
    bench start

### Advices for you while restoring Data bases
 For restoring data from file called `data.sql` use this command
 
      bench --site {your_site} restore /path/to/file/data.sql

 For force install database neglicting there is any issues use `--force`   

      bench --site {your_site} restore /path/to/file/data.sql --force

 Remove any app that is not exist any more with that command

      bench --site {your_site} remove-from-installed-apps {app_name}

