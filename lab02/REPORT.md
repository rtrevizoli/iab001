# Lab 02 - Transaction Control
Rafael Trevizoli - 1460282423016  
Professor Carlos Augusto Lombardi Garcia

**Topic:** [ADM_BD_02_2_transacoes2.pptx](topic/ADM_BD_02_2_transacoes2.pptx)  
**Lab:** [ADM_LAB_02_TRANSACIONAL.docx](lab/ADM_LAB_02_TRANSACIONAL.docx)

## Execution instructions
Each item of the experiment must be carried out and its result documented in the report.

## Lab objective
Understand and study transaction control and Oracle commands for connecting to remote servers.

## Part I

### 1. (Explicar o que significa o comando), também pode ser usado o SQL Developer

```bash
$ sqlplus /nolog
```

According to the help text of `sqlplus`. the command above starts SQL*Plus without connecting to any database. It will be handy when you're want to avoid automatic connections or run maintenance scripts for multiple databases or connect to multiple databases in a loop.

### 2. Conectar como usuário do banco de dados (em geral é o HR)

<p align="center">
    <img src="lab/assets/01_Connect-as-HR.jpg" alt="Img 01 - Connect as the user HR" width="300" height="200"/><br>
    <em>Img 01 - Connect as the user HR</em>
</p>

### 3. Inserir uma linha numa tabela desse usuário sem executar commit

```SQL
INSERT INTO JOBS (
    JOB_ID, JOB_TITLE, MIN_SALARY, MAX_SALARY
) VALUES (
    'MG_DEV', 'Mega Dev', 50000.00, 100000.00
);
```

<p align="center">
    <img src="lab/assets/02_Insert-jobs.jpg" alt="Img 02 - Executes an INSERT query into jobs table" width="200" height="200"/><br>
    <em>Img 02 - Executes an INSERT query into jobs table</em>
</p>

### 4. Abrir outra janela com o SQLPlus e conectar com um usuário (pode ser o mesmo da etapa anterior)

<p align="center">
    <img src="lab/assets/03_Connect-HR2.jpg" alt="Img 03 - Connect as HR on new conn" width="300" height="200"/><br>
    <em>Img 03 - Connect as HR on new conn</em>
</p>

### 5. Executar uma consulta que tente recuperar a linha inserida acima

```SQL
SELECT * FROM JOBS WHERE JOB_ID = 'MG_DEV';
```

<p align="center">
    <img src="lab/assets/04_Select-HR2.jpg" alt="Img 04 - Executes a SELECT query into jobs table on a new connection" width="300" height="200"/><br>
    <em>Img 04 - Executes a SELECT query into jobs table on a new connection</em>
</p>

### 6. Documente o que aconteceu e explique

Performing a `SELECT` query in a new connection as the HR user, the record inserted in step 3 (without a commit) returns zero rows.

Indeed, the session did not actually register the `INSERT`.

### 7. Execute o `COMMIT` na primeira janela. Documente e explique (D & E - Documente & Explique)

The commit command is responsible for finalizing and registering the transaction started by the `INSERT` query, effectively recording the new data.

<p align="center">
    <img src="lab/assets/05_Commit-insert-jobs.jpg" alt="Img 05 - Executes the commit in the first section" width="300" height="200"/><br>
    <em>Img 05 - Executes the commit in the first section</em>
</p>

### 8. Repetir a consulta da linha inserida (D & E)

The query returns the row recorded after the transaction commit.

<p align="center">
    <img src="lab/assets/06_Select2-HR2.jpg" alt="Img 06 - Executes the SELECT query again (connected as HR)" width="300" height="200"/><br>
    <em>Img 06 - Executes the SELECT query again (connected as HR)</em>
</p>

### 9. Na primeira janela, execute um `UPDATE` na linha inserida sem commit

```SQL
UPDATE JOBS
SET JOB_ID = 'SP_DEV', JOB_TITLE = 'Super Dev'
WHERE JOB_ID = 'MG_DEV';
```

<p align="center">
    <img src="lab/assets/07_Update-Jobs.jpg" alt="Img 07 - Executes an UPDATE query in jobs table" width="300" height="200"/><br>
    <em>Img 07 - Executes an UPDATE query in jobs table</em>
</p>

### 10. Na segunda janela, execute outro `UPDATE` na mesma linha (D & E)

The window got stuck while trying to perform the `UPDATE` query. The ScriptRunner task keep waiting.

```SQL
UPDATE JOBS
SET MIN_SALARY = 70000
WHERE JOB_ID = 'MG_DEV';
```

<p align="center">
    <img src="lab/assets/08_Update-jobs-wout-commit.jpg" alt="Img 08 - Executes an UPDATE query without committing the last UPDATE in the same job (in a different session)" width="300" height="200"/><br>
    <em>Img 08 - Executes an UPDATE query without committing the last UPDATE in the same job (in a different session)</em>
</p>

### 11. Conectado como system, verifique a existência de lock (bloqueio) através do comando abaixo executado em um terceira janela.

```SQL
SELECT NVL(s.username, '(oracle)') AS username,
       s.sid,
       s.serial#,
       sw.event,
       sw.wait_class,
       sw.wait_time,
       sw.seconds_in_wait,
       sw.state
FROM   gv$session_wait sw,
       gv$session s
WHERE  s.sid = sw.sid
and lower(sw.event) like '%lock%'
ORDER BY sw.seconds_in_wait DESC;
```

<p align="center">
    <img src="lab/assets/09_Lock-select.jpg" alt="Img 09 - Executes a SELECT query looking for db locks" width="500" height="300"/><br>
    <em>Img 09 - Executes a SELECT query looking for db locks</em>
</p>

### 12. Para observar a hierarquia entre as sessões que estão gerando o lock nos registros, utilizar o comando abaixo.

```SQL
SELECT level,
       lpad(' ',(level - 1) * 2, ' ')
       || nvl(s.username, '(oracle)') AS username,
       s.osuser,
       s.sid,
       s.serial#,
       s.lockwait,
       s.status,
       s.module,
       s.machine,
       s.program,
       TO_CHAR(s.logon_time, 'DD-MON-YYYY HH24:MI:SS') AS logon_time,
       a.spid      processid,
       s.process   clientpid
FROM   gv$session   s,
       gv$process   a
WHERE  a.addr = s.paddr
       AND ( level > 1
             OR EXISTS (
              SELECT 1
              FROM   gv$session
              WHERE  blocking_session = s.sid 
             )
           )
CONNECT BY PRIOR s.sid = s.blocking_session
START WITH s.blocking_session IS NULL;
```

<p align="center">
    <img src="lab/assets/10_Select-section-hierarchy.jpg" alt="Img 10 - Executes a SELECT query looking for section hierarchy" width="700" height="450"/><br>
    <em>Img 10 - Executes a SELECT query looking for section hierarchy</em>
</p>

### 13. Faça o `COMMIT` na Janela 01 (D & E)

After the `COMMIT` in the transaction running on Window 01, the second transaction running on Window 02 was executed, the lock was released, and the **Section Hierarchy** showed no entries related to lock generation.

#### Finalize the `UPDATE` in the window 01

<p align="center">
    <img src="lab/assets/11_Commit-update-jobs.jpg" alt="Img 11 - Executes the commit in the window 1" width="300" height="200"/><br>
    <em>Img 11 - Executes the commit in the window 1</em>
</p>

#### Window 02 after `COMMIT` in the window 01

<p align="center">
    <img src="lab/assets/12_Update-jobs-wout-commit-lock-release.jpg" alt="Img 12 - Checks the UPDATE query in the window 2 after lock release" width="300" height="200"/><br>
    <em>Img 12 - Checks the UPDATE query in the window 2 after lock release</em>
</p>

#### Check the lock existance after `COMMIT` in the window 01

<p align="center">
    <img src="lab/assets/13_Lock-select-after-Update-jobs-commit.jpg" alt="Img 13 - Checks the lock existance after the commit in the window 1" width="500" height="300"/><br>
    <em>Img 13 - Checks the lock existance after the commit in the window 1</em>
</p>

#### Check the cection hierarchy after `COMMIT` in the window 01

<p align="center">
    <img src="lab/assets/14_Select-section-hierarchy-after-Update-jobs-commit.jpg" alt="Img 14 - Checks the section hierarchy after the lock release" width="700" height="450"/><br>
    <em>Img 14 - Checks the section hierarchy after the lock release</em>
</p>


### 14. Finalize finalizando a transação da janela 2
There is nothing to commit or change here because the `UPDATE` query in Window 1 already changed the `JOB_ID`.

The `UPDATE` in Window 2 completed successfully but did not affect any rows.

## Part II