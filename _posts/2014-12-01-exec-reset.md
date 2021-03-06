---
category: Database Connection Control Functions
apipath: 'exec(''reset'',...)'
title: 'Reset the communication channel to the server'
type: 'RESET'

layout: null
---

 CONNECT opens a new database connection
%
 Usage: dbConn=pgmex('connect',connStr)
%
 INPUT:
   optional:
     connStr: char [1,N] - connection string (see
         http://www.postgresql.org/docs/9.4/static/libpq-connect.html#LIBPQ-CONNSTRING)
         Default values are used for missing parameters.
 OUTPUT:
   regular:
     dbConn: uint32 or uint64 [1,1] - C pointer to a PGconn structure

 Failure to connect triggers an exception.

 Example: Try connecting to a database at localhost, port 5432 with no
 password:

 dbConn=pgmex('connect','dbname=test user=postgres');
