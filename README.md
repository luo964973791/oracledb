## rpm包安装oracle

### 1、先下载安装依赖包

```
mkdir -p /tmp/yum
yum install oracle-database-preinstall-19c-1.0-1.el7.x86_64.rpm --downloadonly --downloaddir=/tmp/yum
cd /tmp/yum && rpm -Uvh *
```

### 2、安装oracle

```
rpm -ivh oracle-database-ee-19c-1.0-1.x86_64.rpm
```

### 3、oracle19c的修改配置文件。

```
vi /etc/init.d/oracledb_ORCLCDB-19c
export ORACLE_VERSION=19c
export ORACLE_SID=ORA19C
export TEMPLATE_NAME=General_Purpose.dbc
export CHARSET=ZHS16GBK
export PDB_NAME=ORA19CPDB
export CREATE_AS_CDB=true
```

### 4、复制文件

```
cp oracledb_ORCLCDB-19c.conf  oracledb_ORA19C-19c.conf
```

### 5、初始化.

```
/etc/init.d/oracledb_ORCLCDB-19c configure
```

### 6、添加环境变量

```
vi /etc/profile.d/oracle19c.sh
export  ORACLE_HOME=/opt/oracle/product/19c/dbhome_1
export  PATH=$PATH:/opt/oracle/product/19c/dbhome_1/bin
export  ORACLE_SID=ORA19C
```

### 7、修改Oracle用户的密码.

```
passwd oracle
```

### 8、使用Oracle登录进行相关的处理并查看pdb

```
sqlplus / as sysdba
show pdbs
```

###   9、创建pdb触发器.

```
CREATE TRIGGER open_all_pdbs
   AFTER STARTUP ON DATABASE
BEGIN
   EXECUTE IMMEDIATE 'alter pluggable database all open';
END open_all_pdbs;
```

