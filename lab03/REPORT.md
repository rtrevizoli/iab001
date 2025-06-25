# Lab 03 - Transaction Control
Rafael Trevizoli - 1460282423016  
Professor Carlos Augusto Lombardi Garcia

**Topic:** [ADM_BD_03.ppt](topic/ADM_BD_03.ppt)  
**Lab:** [Lab03_auditoria_trigger.txt](lab/Lab03_auditoria_trigger.txt)

## Execution instructions
Each item of the experiment must be carried out and its result documented in the report.

## Lab objective
The goal of this lab is to implement a database audit mechanism using a trigger in the Oracle HR schema. The main objective is to detect and document suspicious salary changes made by users who exploit the system by increasing their own salary just before payday (5th of each month), and then reverting it back afterward.

As the DBA, create the necessary tools to:
* Log salary changes between the 1st and 5th of each month.
* Identify abnormal behavior that might indicate policy violations or fraud.