# Administracion de Seguridad en SQLSERVER


## Fundamentos

 | Concepto | Nivel | Proposito | Ejemplo | 
 |---|---|---|--|
 |**Login**| Instancia de SQL Server|Autenticar una identidad ante el servidor|'admin_sql' |
|**User**| Base de Datos | Representa la identidad del servidor|'dbo'|
|**Rol de servidor**| Instancia | Agrupa privilegios administrados a nivel servidor|'sysadmin'|
|**Base de Datos Actual**| Sesión | Determinar el contexto de ejecucion de una consulta|'master'|

# 1. Diagnostico de la conexion Administrativa

**Comprobar el login actual**

--SQL

SELECT SUSER_NAME () AS LoginActual;

--Verificar privilegios sysadmin

SELECT IS_SRVROLEMEMBER('sysadmin') AS [EsSysadmin]



| Resultado| Interpretacion|
 |---|--|
 |1|pertenece al rol consultado
 |0|no pertenece a ese ROLE
 |Null|No se puede determinar el resultado

SELECT
	SUSER_NAME () AS LoginActual;
	IS_SRVROLEMEMBER('sysadmin') AS [EsSysadmin]
GO


**consultar estado de SA**
SELECT
	name,
	type_desc,
	is_disabled
FROM sys.server_principals
WHERE NAME = 'sa'

| Resultado| Interpretacion|
 |---|--|
 |name|nombre del principal
 |type_desc|tipo de principal, para este caso el principal en SQL_LOGIN
 |is_disable|0 significa habilitado y 1 deshabilitado 



**Verificar base de datos actual**
SELECT
	DB_NAME() AS BaseActual;
GO

--Consulta completa del contexto
SELECT
SUSER_NAME() AS LoginActual,
USER_NAME() AS UsuarioBaseActual,
DB_NAME() AS BaseActual,
IS_SRVROLEMEMBER('sysadmin') AS EsSysadmin;



## CREACION Y PRUEBA DE UN ADMIBNISTRADOR ALTERNATIVO


--Creacion  y prueba de un administrador alternativo

--Prea hacer un login siempre debemos estan en master

USE master;
GO

--Crear el login admin_sql
CREATE LOGIN admin_sql
WITH PASSWORD = 'Admin@cloud#12',
CHECK_POLICY = ON;


'CREATE LOGN admin_sql' : Crea una identidad que puede autenticarse ante la instancia

'WITH PASSWORD' : Define una contraseña de autenticacion SQL Server 

'CHECK_POLICY = ON' : Solicita aplicar la politica de contraseñas correspondientes cuando 