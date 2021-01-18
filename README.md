# Projeto-Dag-Apache-Airflow
Este projeto tem a criação DAG do Apache Airflow com MySQL


## Criação de DAG No  Apache Airflow
## "Composição do Projeto"
##Construa um DAG que irá inserir os dados dos alunos
##como sua id e nome em alguma tabela MySQL. E, em seguida, copie essa tabela de origem em alguma tabela de backup.

#Importa Pacotes
from airflow import DAG
from datetime import datetime ,timedelta
from airflow.operators.mysql_operator import MySQLOperator
from airflow.operators.sensors import SqlSensor

#DAG's componente
  default_args ={
"Owner" :"airflow"
"depends_on_past":False,
"start_date":datetime.datetime(2020,11,10),
"retries":1,
"retry_delay":datetime,timedelta(minutes=5),

  }

dag =dag = DAG('assignment_2',

          schedule_interval='@daily',

          catchup=False


)

t1 = MySQLOperator(
task_id "cretae"create_table
mysql_conn_id="mysql_conn",
sql="Create TABLE IF NOT EXITS students(id int,name varchar(50));",
dag=dag

)

t2 = MySQLOperator(
task_id="insert_data",
mysql_conn_id="mysql_conn",
sql="INSERT INTO students VALUES(1,'Ana'),(2 'Paula'),(3, Hide),(4,Kakashi);",
dag=dag
)

t3 = SqlSensor(
  task_id='check_data_arrived',
  mysql_conn_id="mysql_conn",
  sql="SELECT COUNT (*) FROM students;",
  poke_intervalo=10,
  timeout=150,
  dag=dag
)

t4 =MySQLOperator(
task_id='Create_Backup_table',
mysql_conn_id="mysql_conn",
sql="CREATE TABLE students_backup " AS (SELECT * FROM students LIMIT 0);
INSERT INTO students_backup SELECT * FROM students;",
dag=dag

)

t1>> t2 >>  t3>> t4

