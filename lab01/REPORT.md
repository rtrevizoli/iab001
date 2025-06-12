# Lab 01 - Users and Privileges
Rafael Trevizoli - 1460282423016  
Professora Carlos Augusto Lombardi Garcia

**Topic:** [ADM_BD_01_privilegios.pptx](topic/ADM_BD_01_privilegios.pptx)  
**Lab:** [ADM_BD_01_privilegios.pptx](lab/ADM_LAB_01_PRIVILEGIOS.txt)

## Execution instructions
Each item of the experiment must be carried out and its result documented in the report.

## Lab objective
Understand and study the data dictionary views and the Oracle commands that manage user privileges.

> Some dictionary views will be used for this purpose.  

## Connect with the user SYSTEM
<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/01_Connect-w-user-SYSTEM.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                Img 01 - Testing connection into orcl with SYSTEM (Success)
            </td>
        </tr>
    </table>
</center>

## Explain the purpose of the v$version view

`V$VERSION` displays the version number of Oracle Database. The database components have the same version number as the database, so the version number is returned only once.

```SQL
SELECT * FROM v$version; -- explique a finalidade da visão v$version.
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/02_Select-v$version.jpg" alt="Diagram" width="500" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 02 - Select * from v$version view
                </center>
            </td>
        </tr>
    </table>
</center>

## Explain the purpose of the dba_users view

`DBA_USERS` describes all users of the database.

```SQL
SELECT username FROM dba_users;  -- explique a finalidade da visão dba_users.
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/03_Select-dba_users.jpg" alt="Diagram" width="500" height="350"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 03 - Select username from dba_users view
                </center>
            </td>
        </tr>
    </table>
</center>

## Create the user USR_LAB01

```SQL
CREATE USER USR_LAB01 IDENTIFIED BY SENHA default tablespace users  quota unlimited on users;  
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/04_Create-user-USR_LAB01.jpg" alt="Diagram" width="500" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 04 - Creation of the user USR_LAB01
                </center>
            </td>
        </tr>
    </table>
</center>

## Explain the purpose of the roles (CONNECT and RESOURCE)
### CONNECT
The `CONNECT` role has only the `CREATE SESSION` privilege, all other privileges are removed.

Although the `CONNECT` role has frequently been used when provisioning new accounts in the Oracle database, simply connecting to the database does not require all those privileges. Making this change enables new and existing database customers to enforce good security practices more easily.

Each user should have only those privileges appropriate to the tasks she needs to do, an idea termed the principle of least privilege. Least privilege mitigates risk by limiting privileges, so that it remains easy to do what is needed while concurrently reducing the ability to do inappropriate things, either inadvertently or maliciously.

### RESOURCE
The `RESOURCE` role grants a user the privileges necessary to create procedures, triggers and, in Oracle8, types within the user’s own schema area. Granting a user `RESOURCE` without `CONNECT`, while possible, does not allow the user to log in to the database. Therefore, if you really must grant a user `RESOURCE`, you have to grant `CONNECT` also — or, at least, `CREATE SESSION` — so the user can log in.

```SQL
GRANT CONNECT, RESOURCE to USR_LAB01;  -- explique pela documentação da oracle a finalidade das roles connect e resource
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/05_Grant-roles-to-USR_LAB01.jpg" alt="Diagram" width="500" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 05 - Grant CONNECT and RESOURCE to USR_LAB01
                </center>
            </td>
        </tr>
    </table>
</center>

## In another window connect with the user created above

```SQL
-- abra outra janela e conecte com o usuário criado acima. Foi possível conectar?
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/06_Connect-w-user-USR_LAB01.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 06 - Testing connection into orcl with USR_LAB01 (Success)
                </center>
            </td>
        </tr>
    </table>
</center>

## Change the passwaord of USR_LAB01 connected with SYSTEM

```SQL
-- execute o comando abaixo na janela conectado como SYSTEM
ALTER USER USR_LAB01 IDENTIFIED BY new_password;
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/07_Alter-USR_LAB01-pwd-connected-w-SYSTEM.jpg" alt="Diagram" width="500" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 07 - Alter USR_LAB01's password connected as SYSTEM
                </center>
            </td>
        </tr>
    </table>
</center>

## Check the connection of USR_LAB01 in its window

```SQL
-- Volte na janela do usuário criado e verifique se ele continua conectado através do comando abaixo:
select table_name from all_tables;
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/08_Select-all-tables-as-USR_LAB01.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 08 - Select all tables as the user USR_LAB01
                </center>
            </td>
        </tr>
    </table>
</center>

## Reconect as USR_LAB01

Reconnecting as USR_LAB01 with the same password throws me the error `Status: Failure -Test failed: ORA-01017: invalid username/password; logon denied`. Its the reflect of the password that was changed.

A new connection was successfully established using the new password.

```SQL
-- encerre a conexão dessa janela e tente conectar novamente usando a mesma senha. Você conseguiu conectar? Tente usar a nova senha alterada no comando ALTER USER. O que aconteceu?
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/09_Connecting-as-USR_LAB01-w-old-pwd.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 09 - Connecting as USR_LAB01 with the old password
                </center>
            </td>
        </tr>
    </table>
</center>

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/10_Connecting-as-USR_LAB01-w-new-pwd.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 10 - Connecting as USR_LAB01 with the new password
                </center>
            </td>
        </tr>
    </table>
</center>

## In the SYSTEM user window, run the command below

```SQL
-- a partir da janela do usuário system execute os comandos abaixo.
```

### Show user

```SQL
SHOW USER;
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/11_Show-user.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 11 - Show user
                </center>
            </td>
        </tr>
    </table>
</center>

### Create table in SYSTEM user

The command below creates the xtz table in the `SYSTEM` user schema.

```SQL
CREATE TABLE xyz (name VARCHAR2(30));  -- esse comando cria a tabela xyz em qual usuário? 
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/12_Create-xyz-in-SYSTEM.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 12 - Create table xyz
                </center>
            </td>
        </tr>
    </table>
</center>

### Create table in USR_LAB01 using SYSTEM user

The command below creates the xtz table in the `USR_LAB01` user schema using the `SYSTEM` user.

The `CREATE ANY <object_type>` privilege is required to create objects in other database schemas. 

```SQL
CREATE TABLE USR_LAB01.xyz (name VARCHAR2(30));  -- esse comando cria a tabela xyz em qual usuário? Que nível de privilégio foi necessário para que isso seja possível?
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/13_Create-xyz-in-USR_LAB01.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 13 - Create table xyz in the USR_LAB01 user schema
                </center>
            </td>
        </tr>
    </table>
</center>

## Desc `<table>`

```SQL
-- volte na janela do usuário USR_LAB01 e rode o comando abaixo. Se ele funcionar é que a tabela pertence a esse usuário.
```

### Own schema

```SQL
DESC xyz;
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/14_Desc-xyz-as-USR_LAB01.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 14 - Desc table xyz as USR_LAB01
                </center>
            </td>
        </tr>
    </table>
</center>

### Other user schema

This command returns the error `ERROR: ORA-04043: object system.xyz does not exist` because the `USR_LAB01` user does not have visibility of the `SYSTEM` user schema.

```SQL
DESC system.xyz;   -- esse comando funcionou? O que falta ao usuário USR_LAB01 para que esse comando funcione?
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/15_Desc-system-xyz-as-USR_LAB01.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 15 - Desc xyz table of the SYSTEM user schema
                </center>
            </td>
        </tr>
    </table>
</center>

## Grant `<table>` privileges to `<user>`

```SQL
-- volte na janela do usuário SYSTEM
```

### Creat user `USR_LAB02`

```SQL
CREATE USER USR_LAB02 IDENTIFIED BY SENHA default tablespace users;
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/16_Create-user-USR_LAB02.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 16 - Creation of the user USR_LAB02
                </center>
            </td>
        </tr>
    </table>
</center>

### Grant `<table>` privileges

Below is a privilege grant operation, that allows `USR_LAB02` to (**INSERT** | **DELETE** | **SELECT**) on the xyz table in the `USR_LAB01` user schema.

```SQL
GRANT INSERT, DELETE, SELECT ON USR_LAB01.XYZ TO USR_LAB02;  -- que operação está acontecendo aqui?
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/17_Grant-table-privileges-to-user-USR_LAB02.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 17 - Grant table privileges to user USR_LAB02
                </center>
            </td>
        </tr>
    </table>
</center>

### Grant `<connect>` role

```SQL
grant connect to USR_lab02;
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/18_Grant-connect-to-user-USR_LAB02.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 18 - Grant CONNECT role to user USR_LAB02
                </center>
            </td>
        </tr>
    </table>
</center>

### Check `<table>` privileges

This query returns the privileges granted to the user `USR_LAB02`.

```SQL
select * from dba_tab_privs where grantee = 'USR_LAB02';   -- qual o significado do resultado dessa consulta? 
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/19_Check-user-USR_LAB02-privileges.jpg" alt="Diagram" width="550" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 19 - Check the user USR_LAB02 privileges
                </center>
            </td>
        </tr>
    </table>
</center>

## Test the privileges granted to `<user>`  

```SQL
-- abra uma nova janela e conecte com o usuário usr_lab02. Execute o comando abaixo.
```

### Insert

```SQL
insert into usr_lab01.xyz values ('teste de nome');

commit;
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/20_Test-insert-USB_LAB02.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 20 - Test the user USR_LAB02 insert privilege in the USR_LAB01.xyz table
                </center>
            </td>
        </tr>
    </table>
</center>

### Select

#### Select with proper privilege

The command below was executed successfully because `USR_LAB02` was granted the `SELECT` privilege on this table.

```SQL
select * from usr_lab01.xyz;  -- mostre o resultado desse comando e explique por que ele funcionou.
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/21_USR_LAB02-select-USR_LAB01-xyz.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 21 - USR_LAB02 executes a SELECT query in the USR_LAB01.xyz table
                </center>
            </td>
        </tr>
    </table>
</center>

#### Select with proper privilege

The command returns the error `ORA-00942: table or view does not exist` because the user `USR_LAB02` does not have the necessary privileges to access objects in the `SYSTEM` schema.

```SQL
select * from system.xyz; -- mostre o resultado desse comando e explique por que ele NÃO funcionou.
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/22_USR_LAB02-select-SYSTEM-xyz.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 22 - USR_LAB02 executes a SELECT query in the SYSTEM.xyz table
                </center>
            </td>
        </tr>
    </table>
</center>

#### Select in own schema

The command returns the error: `ORA-00942: table or view does not exist` because in the `USR_LAB02` schema do not exists the xyz object.

```SQL
select * from xyz; -- mostre o resultado desse comando e explique por que ele NÃO funcionou.
```

<center>
    <table>
        <tr>
            <td>
                <center>
                    <img src="lab/assets/23_USR_LAB02-select-xyz.jpg" alt="Diagram" width="300" height="200"/>
                </center>
            </td>
        </tr>
        <tr>
            <td>
                <center>
                    Img 23 - USR_LAB02 executes a SELECT query in its own schema
                </center>
            </td>
        </tr>
    </table>
</center>

```SQL
-- na janela do usuário usr_lab01.
-- a visão dba_sys_privs requer privilégio específico para ser acessada. O usuário usr_lab01 ainda não tem esse privilégio. rode o comando abaixo e veja se funciona?

select * from dba_sys_privs;

-- na janela do usuário system.

CREATE ROLE new_dba;

GRANT CONNECT TO new_dba;

GRANT SELECT ANY TABLE TO new_dba;

GRANT select_catalog_role TO new_dba;

grant new_dba to USR_LAB01;

-- na janela do usuário usr_lab01. 
-- refaça a sua conexão para garantir que os privilégios tenham sido atualizados.
-- execute o comando e veja que ele funciona.
-- explique como foi o processo de atribuição do privilégio ao usuário usr_lab01 que permitiu a ele acessa a tabela.

Através das views a seguir, exibir os privilégios dos usuários e roles criados nesse lab.
select * from dba_sys_privs;

SELECT * FROM DBA_ROLE_PRIVS;
SELECT * FROM ROLE_ROLE_PRIVS;
SELECT * FROM ROLE_SYS_PRIVS;
SELECT * FROM ROLE_TAB_PRIVS;
```
