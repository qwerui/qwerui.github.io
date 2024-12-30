# DB Server

## Maria DB
1. apt -y install mariadb-server mariadb-client
2. systemctl restart mariadb
3. ufw allow 3306
4. /etc/mysql/mariadb.conf.d/50-server.cnf 수정
```
28 : 주석처리
```

### DB Root 암호화
1. mysqladmin -u root password '[비밀번호]'
2. systemctl restart mariadb
3. 이후 접속은 mysql -u root -p

## Oracle
1. Oracle Database 다운로드
2. 다운 경로에서 unzip oracle*
3. apt -y install alien libaio1 unixodbc
4. alien --scripts -d oracle* (3~4번은 .rpm -> .deb 과정)
5. dpkg --install oracle*.deb
6. /etc/init.d/oracle.xe configure으로 환경설정
7. systemctl start oracle-xe
8. ./u01/app/oracle/product/11.2.0/xe/bin/oracle_env.sh
9. /etc/bash/bashrc 수정 8번 문장 추가
10. sqlplus로 oracle 실행