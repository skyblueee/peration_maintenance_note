## Preparation
1. `java -version`(should be >=1.8)
1. install and configure mysqld, `vim /etc/mysql/mysql.conf.d/mysqld.cnf`
    ```
    collation_server=utf8_bin
    character_set_server=utf8
    skip-character-set-client-handshake
    default-storage-engine=INNODB
    max_allowed_packet=256M
    innodb_log_file_size=2GB
    binlog_format=row
    transaction-isolation=READ-COMMITTED
    ```
1. run `service mysql start & mysql` as root:
    ```mysql
    create database jiradb default character set utf8 collate utf8_bin;
    create user 'jira'@'localhost' identified by 'jirapasswd';
    grant all privileges on jiradb.* to 'jira'@'%' identified by 'jirapasswd';
    flush privileges;
    ```
## Installation
1. `wget https://downloads.atlassian.com/software/jira/downloads/atlassian-jira-software-7.3.8-x64.bin`
1. `chmod +x atlassian-jira-software-7.3.8-x64.bin`
1. `./atlassian-jira-software-7.3.8-x64.bin`
1. `firefox localhost:8080`

## Firewall
1. `firewall-cmd --zone=public --add-port=8080/tcp --permanent`
1. `firewall-cmd --reload`

## Crack[Test only]
1. `service jira stop`
1. `cp Crack/atlassian-extras-3.1.2.jar Crack/mysql-connector-java-5.1.42-bin.jar /opt/atlassian/jira/atlassian-jira/WEB-INF/lib/`
1. `service jira start`
1. `firefox localhost:8080` -> I'll set it up myself -> TestConnection(jira/jirapasswd) -> Setup App -> license(generate a trial license) -> Setup Administrator
