***********************
Java 저장 함수/프로시저
***********************

**Java Stored Function/Procedure**

**Overview**

Stored functions and procedures are used to implement complicated program logic that is not possible with SQL. They allow users to manipulate data more easily. Stored functions/procedures are blocks of code that have a flow of commands for data manipulation and are easy to manipulate and administer.

CUBRID supports to develop stored functions and procedures in Java. Java stored functions/procedures are executed on the JVM (Java Virtual Machine) hosted by CUBRID.

You can call Java stored functions/procedures from SQL statements or from Java applications using JDBC.

The advantages of using Java stored functions/procedures are as follows:

*   **Productivity and usability**
    : Java stored functions/procedures, once created, can be reused anytime. They can be called from SQL statements or from Java applications using JDBC.



*   **Excellent interoperability and portability**
    : Java stored functions/procedures use the Java Virtual Machine. Therefore, they can be used on any system where the Java Virtual Machine is available.



**Environment Configuration for Java Stored Function/Procedure**

To use Java-stored functions/procedures in CUBRID, you must have JRE (Java Runtime Environment) 1.6 or better installed in the environment where the CUBRID server is installed. You can download JRE from the Developer Resources for Java Technology (
`http://java.sun.com <http://java.sun.com>`_
).

If the java_stored_procedure parameter in the CUBRID configuration file (cubrid.conf) is set to yes, CUBRID 64-bit needs a 64-bit Java Runtime Environment, and CUBRID 32-bit needs a 32-bit Java Runtime Environment. For example, when you run CUBRID 64-bit in the system in which a 32-bit JAVA Runtime Environment is installed, the following error may occur.

% cubrid server start demodb

 

This may take a long time depending on the amount of recovery works to do.

WARNING: Java VM library is not found : /usr/java/jdk1.6.0_15/jre/lib/amd64/server/libjvm.so: cannot open shared object file: No such file or directory.

Consequently, calling java stored procedure is not allowed

Execute the following command to check the JRE version if you have it already installed in the system.

% java -version Java(TM) SE Runtime Environment (build 1.6.0_05-b13)

Java HotSpot(TM) 64-Bit Server VM (build 10.0-b19, mixed mode)

**Windows Environment**

For Windows, CUBRID loads the
**jvm.dll**
file to run the Java Virtual Machine. CUBRID first locates the
**jvm.dll**
file from the
**PATH**
environment variable and then loads it. If it cannot find the file, it uses the Java runtime information registered in the system registry.

You can configure the
**JAVA_HOME**
environment variable and add the directory in which the Java executable file is located to
**Path**
, by executing the command as follows: For information on configuring environment variables using GUI, see Installing and Configuring JDBC.

*   An example of installing 64 Bit JDK 1.6 and configuring the environment variables 



% set JAVA_HOME=C:\jdk1.6.0

% set PATH=%PATH%;%JAVA_HOME%\jre\bin\server

*   An example of installing 32 Bit JDK 1.6 and configuring the environment variables  



% set JAVA_HOME=C:\jdk1.6.0

% set PATH=%PATH%;%JAVA_HOME%\jre\bin\client

To use other vendor's implementation instead of Sun's Java Virtual Machine, add the path of the
**jvm.dll**
file to the
**PATH**
variable during the installation.

**Linux/UNIX Environment**

For Linux/UNIX environment, CUBRID loads the
**libjvm.so**
file to run the Java Virtual Machine. CUBRID first locates the
**libjvm.so**
file from the
**LD_LIBRARY_PATH**
environment variable and then loads it. If it cannot find the file, it uses the
**JAVA_HOME**
environment variable. For Linux, glibc 2.3.4 or later versions are supported. The following example shows how to configure the Linux environment variable (e.g.,
**.profile**
,
**.cshrc**
,
**.bashrc**
,
**.bash_profile**
, etc.).

*   An example of installing 64 Bit JDK 1.6 and configuring the environment variables in a bash shell  



% JAVA_HOME=/usr/java/jdk1.6.0_10

% LD_LIBRARY_PATH=$JAVA_HOME/jre/lib/amd64:$JAVA_HOME/jre/lib/amd64/server:$LD_LIBRARY_PATH

% export JAVA_HOME

% export LD_LIBRARY_PATH

*   An example of installing 32 Bit JDK 1.6 and configuring the environment variables in a bash shell



% JAVA_HOME=/usr/java/jdk1.6.0_10

% LD_LIBRARY_PATH=$JAVA_HOME/jre/lib/i386/:$JAVA_HOME/jre/lib/i386/client:$LD_LIBRARY_PATH

% export JAVA_HOME

% export LD_LIBRARY_PATH

*   An example of installing 64 Bit JDK 1.6 and configuring the environment variables in a csh



% setenv JAVA_HOME /usr/java/jdk1.6.0_10

% setenv LD_LIBRARY_PATH $JAVA_HOME/jre/lib/amd64:$JAVA_HOME/jre/lib/amd64/server:$LD_LIBRARY_PATH

% set path=($path $JAVA_HOME/bin .)

*   An example of installing 32 Bit JDK 1.6 and configuring the environment variables in a csh shell



% setenv JAVA_HOME /usr/java/jdk1.6.0_10

% setenv LD_LIBRARY_PATH $JAVA_HOME/jre/lib/i386:$JAVA_HOME/jre/lib/i386/client:$LD_LIBRARY_PATH

% set path=($path $JAVA_HOME/bin .)

To use other vendor's implementation instead of Sun's Java Virtual Machine, add the path of the JVM (
**libjvm.so**
) to the library path during the installation.

The path of the
**libjvm.so**
file can be different depending on the platform. For example, the path is the
**$JAVA_HOME/jre/lib/sparc**
directory in a SUN Sparc machine.

**How to Write Java Stored Function/Procedure**

Steps to write a Java stored function/procedure are as follows:

*   `Check the cubrid.conf file <#syntax_syntax_javasp_step_htm_co_244>`_



*   `Write and compile the Java source code <#syntax_syntax_javasp_step_htm_co_6558>`_



*   `Load the complied Java class into CUBRID <#syntax_syntax_javasp_step_htm_lo_8415>`_



*   `Publish the loaded Java class <#syntax_syntax_javasp_step_htm_re_3529>`_



*   `Call the Java stored function/procedure <#syntax_syntax_javasp_step_htm_ca_2641>`_



**Check the cubrid.conf file**

By default, the
**java_stored_procedure**
is set to
**no**
in the
**cubrid.conf**
file. To use a Java stored function/procedure, this value must be changed to
**yes**
. For details on this value, see
`Other Parameters <#pm_pm_db_classify_etc_htm>`_
in Database Server Configuration.

**Write and compile the Java source code**

Compile the SpCubrid.java file as follows:

public class SpCubrid{

    public static String HelloCubrid() {

        return "Hello, Cubrid !!";

    }

    public static int SpInt(int i) {

        return i + 1;

    }

    public static void outTest(String[] o) {

        o[0] = "Hello, CUBRID";

    }

}

 

%javac SpCubrid.java

Here, the Java class method must be public static.

**Load the compiled Java class into CUBRID**

Load the compiled Java class into CUBRID.

% loadjava demodb

**Publish the loaded Java class**

Create a CUBRID stored function and publish the Java class as shown below.

csql> create function hello() return string

as language java

name 'SpCubrid.HelloCubrid() return java.lang.String';

**Call the Java stored function/procedure**

Call the published Java stored function as follows:

csql> call hello() into :Hello;

  Result

=====================

'Hello, Cubrid !!'

**Using Server-side Internal JDBC Driver**

To access the database from a Java stored function/procedure, you must use the server-side JDBC driver. As Java stored functions/procedures are executed within the database, there is no need to make the connection to the server-side JDBC driver again. To acquire a connection to the database using the server-side JDBC driver, you can either use "
**jdbc:default:connection:**
" as the URL for JDBC connection, or call the
**getDefaultConnection**
() method of the
**cubrid.jdbc.driver.CUBRIDDriver**
class.

Class.forName("cubrid.jdbc.driver.CUBRIDDriver");

Connection conn =    DriverManager.getConnection("jdbc:default:connection:");

or

cubrid.jdbc.driver.CUBRIDDriver.getDefaultConnection();

If you connect to the database using the JDBC driver as shown above, the transaction in the Java stored function/procedure is ignored. That is, database operations executed in the Java stored function/procedure belong to the transaction that called the Java stored function/procedure. In the following example,
**conn.commit()**
method of the
**Athlete**
class is ignored.

import java.sql.*;

public class Athlete{

    public static void Athlete(String name, String gender, String nation_code, String event) throws SQLException{

        String sql="INSERT INTO ATHLETE(NAME, GENDER, NATION_CODE, EVENT)" + "VALUES (?, ?, ?, ?)";

        try{

            Class.forName("cubrid.jdbc.driver.CUBRIDDriver");

            Connection conn = DriverManager.getConnection("jdbc:default:connection:");

            PreparedStatement pstmt = conn.prepareStatement(sql);

      

            pstmt.setString(1, name);

            pstmt.setString(2, gender);

            pstmt.setString(3, nation_code);

            pstmt.setString(4, event);;

            pstmt.executeUpdate();

 

            pstmt.close();

            conn.commit();

            conn.close();

        } catch (Exception e) {

            System.err.println(e.getMessage());

        }

    }

}

**Connecting to Other Database**

You can connect to another outside database instead of the currently connected one even when the server-side JDBC driver is being used. Acquiring a connection to an outside database is not different from a generic JDBC connection. For details, see JDBC API.

If you connect to other databases, the connection to the CUBRID database does not terminate automatically even when the execution of the Java method ends. Therefore, the connection must be explicitly closed so that the result of transaction operations such as
**COMMIT**
or
**ROLLBACK**
will be reflected in the database. That is, a separate transaction will be performed because the database that called the Java stored function/procedure is different from the one where the actual connection is made.

import java.sql.*;

public class SelectData {

  public static void SearchSubway(String[] args) throws Exception {

Connection conn = null;

  Statement stmt = null;

  ResultSet rs = null;

  try {

    Class.forName("cubrid.jdbc.driver.CUBRIDDriver");

    conn =

DriverManager.getConnection("jdbc:CUBRID:localhost:33000:demodb:::","","");

    String sql = "select line_id, line from line";

    stmt = conn.createStatement();

    rs = stmt.executeQuery(sql);

    while(rs.next()) {

      int host_year = rs.getString("host_year");

      String host_nation = rs.getString("host_nation");

      System.out.println("Host Year ==> " + host_year);

      System.out.println(" Host Nation==> " + host_nation);

      System.out.println("\n=========\n");

    }

    rs.close();

    stmt.close();

    conn.close();

    } catch ( SQLException e ) {

         System.err.println(e.getMessage());

    } catch ( Exception e ) {

         System.err.println(e.getMessage());

    } finally {

          if ( conn != null ) conn.close();

    }

  }

}

When the Java stored function/procedure being executed should run only on JVM located in the database server, you can check where it is running by calling System.getProperty ("cubrid.server.version") from the Java program source. The result value is the database version if it is called from the database; otherwise, it is
**NULL**
.

**loadjava Utility**

**Description**

To load a compiled Java or JAR (Java Archive) file into CUBRID, use the
**loadjava**
utility. If you load a Java *.class or *.jar file using the
**loadjava**
utility, the file is moved to the specified database path.

**Syntax**

**loadjava**
<
*option*
>
*database-name*
*java-class-file*

*   *database-name*
    : The name of the database where the Java file is to be loaded.



*   *java-class-file*
    : The name of the Java class or jar file to be loaded.



*   <
    *option*
    > :



*   **-y**
    : Automatically overwrites a class file with the same name, if any. The default value is
    **no**
    . If you load the file without specifying the
    **-y**
    option, you will be prompted to ask if you want to overwrite the class file with the same name (if any).



**Loaded Java Class Publish**

**Overview**

In CUBRID, it is required to publish Java classes to call Java methods from SQL statements or Java applications. You must publish Java classes by using call specifications because it is not known how a function in a class will be called by SQL statements or Java applications when Java classes are loaded.

**Call Specifications**

To use a Java stored function/procedure in CUBRID, you must write call specifications. With call specifications, Java function names, parameter types, return values and their types can be accessed by SQL statements or Java applications. To write call specifications, use
**CREATE FUNCTION**
or
**CREATE PROCEDURE**
statement. Java stored function/procedure names are not case sensitive. The maximum number of characters a Java stored function/procedure can have is 254 bytes. The maximum number of parameters a Java stored function/procedure can have is 64.

**Syntax**

**CREATE**
{
**PROCEDURE**
*procedure_name*
[(
*param*
[,
*param*
]...] |
**FUNCTION**
*function_name*
[(
*param*
[,
*param*
]...]
**RETURN**
*sql_type*
}

{
**IS | AS**
}
**LANGUAGE JAVA**

**NAME**
'
*method_fullname*
(
*java_type_fullname*
[,
*java_type_fullname*
]... [
*return java_type_fullname*
]';

 

parameter_name [
**IN|OUT|IN OUT|INOUT**
]
*sql_type*

   (
*default*
**IN**
)

If the parameter of a Java stored function/procedure is set to
**OUT**
, it will be passed as a one-dimensional array whose length is 1. Therefore, a Java method must store its value to pass in the first space of the array.

**Example**

CREATE FUNCTION Hello() RETURN VARCHAR

AS LANGUAGE JAVA

NAME 'SpCubrid.HelloCubrid() return java.lang.String';

CREATE FUNCTION Sp_int(i int) RETURN int

AS LANGUAGE JAVA

NAME 'SpCubrid.SpInt(int) return int';

 

CREATE PROCEDURE Phone_Info(name varchar, phoneno varchar)

AS LANGUAGE JAVA

NAME 'PhoneNumber.Phone(java.lang.String, java.lang.String)';

When a Java stored function/procedure is published, it is not checked whether the return definition of the Java stored function/procedure coincides with the one in the declaration of the Java file. Therefore, the Java stored function/procedure follows the
*sql_type*
return definition provided at the time of registration. The return definition in the declaration is significant only as user-defined information.

**Data Type Mapping**

In call specifications, the data types SQL must correspond to the data types of Java parameter and return value. The following table shows SQL/Java data types allowed in CUBRID.

**Data Type Mapping**

+----------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| **SQL Type**                                 | **Java Type**                                                                                                                                                                                                                          |
|                                              |                                                                                                                                                                                                                                        |
+----------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| CHAR, VARCHAR                                | java.lang.String, java.sql.Date, java.sql.Time, java.sql.Timestamp, java.lang.Byte, java.lang.Short, java.lang.Integer, java.lang.Long, java.lang.Float, java.lang.Double, java.math.BigDecimal, byte, short, int, long, float, double |
|                                              |                                                                                                                                                                                                                                        |
+----------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| NUMERIC, SHORT, INT, FLOAT, DOUBEL, CURRENCY | java.lang.Byte, java.lang.Short, java.lang.Integer, java.lang.Long, java.lang.Float, java.lang.Double, java.math.BigDecimal, java.lang.String, byte, short, int, long, float, double                                                   |
|                                              |                                                                                                                                                                                                                                        |
+----------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| DATE, TIME, TIMESTAMP                        | java.sql.Date, java.sql.Time, java.sql.Timestamp, java.lang.String                                                                                                                                                                     |
|                                              |                                                                                                                                                                                                                                        |
+----------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| SET, MULTISET, SEQUENCE                      | java.lang.Object[], java primitive type array, java.lang.Integer[] ...                                                                                                                                                                 |
|                                              |                                                                                                                                                                                                                                        |
+----------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| OBJECT                                       | cubrid.sql.CUBRIDOID                                                                                                                                                                                                                   |
|                                              |                                                                                                                                                                                                                                        |
+----------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| CURSOR                                       | cubrid.jdbc.driver.CUBRIDResultSet                                                                                                                                                                                                     |
|                                              |                                                                                                                                                                                                                                        |
+----------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**Checking the Published Java Stored Function/Procedure Information**

You can check the information on the published Java stored function/procedure The
**db_stored_procedure**
system virtual table provides virtual table and the
**db_stored_procedure_args**
system virtual table. The
**db_stored_procedure**
system virtual table provides the information on stored names and types, return types, number of parameters, Java class specifications, and the owner. The
**db_stored_procedure_args**
system virtual table provides the information on parameters used in the stored function/procedure.

SELECT * from db_stored_procedure;
sp_name     sp_type   return_type    arg_count
sp_name               sp_type               return_type             arg_count  lang target                owner
================================================================================
'hello'               'FUNCTION'            'STRING'                        0  'JAVA''SpCubrid.HelloCubrid() return java.lang.String'  'DBA'
 
'sp_int'              'FUNCTION'            'INTEGER'                       1  'JAVA''SpCubrid.SpInt(int) return int'  'DBA'
 
'athlete_add'         'PROCEDURE'           'void'                          4  'JAVA''Athlete.Athlete(java.lang.String, java.lang.String, java.lang.String, java.lang.String)'  'DBA'

SELECT * from db_stored_procedure_args;
sp_name   index_of  arg_name  data_type      mode
=================================================
 'sp_int'                        0  'i'                   'INTEGER'             'IN'
 'athlete_add'                   0  'name'                'STRING'              'IN'
 'athlete_add'                   1  'gender'              'STRING'              'IN'
 'athlete_add'                   2  'nation_code'         'STRING'              'IN'
 'athlete_add'                   3  'event'               'STRING'              'IN'

**Deleting Java Stored Functions/Procedures**

You can delete published Java stored functions/procedures in CUBRID. To delete a Java function/procedure, use the
**DROP FUNCTION**
*function_name*
or
**DROP PROCEDURE**
*procedure_name*
statement. Also, you can delete multiple Java stored functions/procedures at a time with several
*function_name*
s or
*procedure_name*
s separated by a comma (,).

A Java stored function/procedure can be deleted only by the user who published it or by DBA members. For example, if a
**PUBLIC**
user published the 'sp_int' Java stored function, only the
**PUBLIC**
or
**DBA**
members can delete it.

drop function hello[, sp_int]

drop procedure Athlete_Add

**Java Stored Function/Procedure Call**

**Using CALL Statement**

You can call the Java stored functions/procedures by using a
**CALL**
statement, from SQL statements or Java applications.

The following shows how to call them by using the
**CALL**
statement. The name of the Java stored function/procedure called from a
**CALL**
statement is not case sensitive.

**Syntax**

**CALL**
{
*procedure_name*
([
*param*
[,
*param*
]...) |
*function_name*
([
*param*
[,
*param*
]...)
**INTO**
:
*host_variable*

*param*
{
*literal*
| :
*host_variable*
}

**Example**

call Hello() into :HELLO;

call Sp_int(3) into :i;

call phone_info('Tom','016-111-1111');

In CUBRID, the Java functions/procedures are called by using the same
**CALL**
statement. Therefore, the
**CALL**
statement is processed as follows:

*   It is processed as a method if there is a target class in the
    **CALL**
    statement.



*   If there is no target class in the
    **CALL**
    statement, it is checked whether a Java stored function/procedure is executed or not; a Java stored function/procedure will be executed if one exists.



*   If no Java stored function/procedure exists in step 2 above, it is checked whether a method is executed or not; a method will be executed if one with the same name exists.



The following error occurs if you call a Java stored function/procedure that does not exist.

CALL deposit()

ERROR: Stored procedure/function 'deposit' does not exist.

 

CALL deposit('Tom', 3000000)

ERROR: Methods require an object as their target.

If there is no argument in the
**CALL**
statement, a message "ERROR: Stored procedure/function 'deposit' does not exist." appears because it can be distinguished from a method. However, if there is an argument in the
**CALL**
statement, a message "ERROR: Methods require an object as their target." appears because it cannot be distinguished from a method.

If the
**CALL**
statement is nested within another
**CALL**
statement calling a Java stored function/procedure, or if a subquery is used in calling the Java function/procedure, the
**CALL**
statement is not executed.

call phone_info('Tom', call sp_int(999));

call phone_info((select * from Phone where id='Tom'));

If an exception occurs during the execution of a Java stored function/procedure, the exception is logged and stored in the
*dbname*
**_java.log**
file. To display the exception on the screen, change a handler value of the
**$CUBRID/java/logging.properties**
file to " java.lang.logging.ConsoleHandler." Then, the exception details are displayed on the screen.

**Calling from SQL Statement**

You can call a Java stored function from a SQL statement as shown below.

select Hello() from db_root;

select sp_int(99) from db_root;

You can use a host variable for the IN/OUT data type when you call a Java stored function/procedure as follows:

SELECT 'Hi' INTO :out_data FROM db_root;

CALL test_out(:out_data);

SELECT :out_data FROM db_root;

The first clause calls a Java stored procedure in out mode by using a parameter variable; the second is a query clause retrieving the assigned host variable out_data.

**Calling from Java Application**

To call a Java stored function/procedure from a Java application, use a
**CallableStatement**
object.

Create a phone class in the CUBRID database.

CREATE TABLE phone(

     name varchar(20),

     phoneno varchar(20)

)

Compile the following
**PhoneNumber.java**
file, load the Java class file into CUBRID, and publish it.

import java.sql.*;

import java.io.*;

public class PhoneNumber{    public static void Phone(String name, String phoneno) throws Exception{

        String sql="INSERT INTO PHONE(NAME, PHONENO)"+ "VALUES (?, ?)";

        try{

            Class.forName("cubrid.jdbc.driver.CUBRIDDriver");

            Connection conn = DriverManager.getConnection("jdbc:default:connection:");

            PreparedStatement pstmt = conn.prepareStatement(sql);

       

            pstmt.setString(1, name);

            pstmt.setString(2, phoneno);

            pstmt.executeUpdate();

 

            pstmt.close();

            conn.commit();

            conn.close();

        } catch (SQLException e) {

            System.err.println(e.getMessage());

        }

    }

}

create PROCEDURE phone_info(name varchar, phoneno varchar)

as language java

name 'PhoneNumber.Phone(java.lang.String, java.lang.String)';

Create and run the following Java application.

import java.sql.*;

public class StoredJDBC{

    public static void main(){

        Connection conn = null;

        Statement stmt= null;

        int result;

        int i;

 

        try{

 Class.forName("cubrid.jdbc.driver.CUBRIDDriver");

            conn = DriverManager.getConnection("jdbc:CUBRID:localhost:33000:demodb:::","","");

 

            CallableStatement cs;

            cs = conn.prepareCall("call PHONE_INFO(?, ?)");

 

            cs.setString(1, "Jane");

            cs.setString(2, "010-1111-1111");

            cs.executeUpdate();

 

            conn.commit();

            cs.close();

            conn.close();

        } catch (Exception e) {

            e.printStackTrace();

        }

    }

}

Retrieve the phone class after executing the program above; the following result would be displayed.

SELECT * from phone;

name                  phoneno

============================================

    'Jane'                '010-111-1111'

**Caution**

**Returning Value of Java Stored Function/Procedure and Precision Type on IN/OUT**

To limit the return value of Java stored function/procedure and precision type on IN/OUT, CUBRID processes as follows:

Checks the sql_type of the Java stored function/procedure.

Passes the value returned by Java to the database with only the type converted if necessary, ignoring the number of digits defined during creating the Java stored function/procedure. In principle, the user manipulates the passed data directly in the database.

Take a look at the following
**typestring**
() Java stored function.

public class JavaSP1{

    public static String typestring(){

        String temp = " ";

        for(int i=0 i< 1 i++)

            temp = temp + "1234567890";

        return temp;

}

}

 

CREATE FUNCTION typestring() return char(5)

as language java

name 'JavaSP1.typestring() return java.lang.String';

Call typestring()

  Result

======================

  ' 1234567890'

**Returning java.sql.ResultSet in Java Stored Procedure**

In CUBRID, you must use
**CURSOR**
as the data type when you declare a Java stored function/procedure that returns a
**java.sql.ResultSet**
.

CREATE FUNCTION rset() return cursor

as language java

name 'JavaSP2.TResultSet() return java.sql.ResultSet'

Before the Java file returns
**java.sql.ResultSet**
, it is required to cast to the
**CUBRIDResultSet**
class and then to call the
**setReturnable**
() method.

public static class JavaSP2 {

public static ResultSet TResultSet(){

    try{

            Class.forName("cubrid.jdbc.driver.CUBRIDDriver");

    Connection conn = DriverManager.getConnection("jdbc:default:connection:");

    ((CUBRIDConnection)con).setCharset("euc_kr");

    String sql = "select * from station";

    Statement stmt=con.createStatement();

    ResultSet rs = stmt.executeQuery(sql);

    ((CUBRIDResultSet)rs).setReturnable();

    return rs;

    } catch (Exception e) {

              e.printStackTrace();

    }

    return null;

  }

}

In the calling block, you must set the OUT argument with
**Types.JAVA_OBJECT**
, get the argument to the
**getObject**
() function, and then cast it to the
**java.sql.ResultSet**
type before you use it. In addition, the
**java.sql.ResultSet**
is only available to use in
**CallableStatement**
of JDBC.

import java.sql.*;

 

public class TestResultSet{

  public static void main(String[] args) {

    Connnection conn = null;

    Statement stmt= null;

    int result;

    int i;

 

    try{

           Class.forName("cubrid.jdbc.driver.CUBRIDDriver");

    conn = DriverManager.getConnection("jdbc:CUBRID:localhost:33000:demodb:::","","");

  

    CallableStatement cstmt = con.prepareCall("?=CALL rset()");

    cstmt.registerOutParameter(1, Types.JAVA_OBJECT);

    cstmt.execute();

    ResultSet rs = (ResultSet) cstmt.getObject(1);

    while(rs.next()) {

      System.out.println(rs.getString(1));

    }

      rs.close();

     } catch (Exception e) {

             e.printStackTrace();

    }

}

You cannot use the
**ResultSet**
as an input argument. If you pass it to an IN argument, an error occurs. An error also occurs when calling a function that returns
**ResultSet**
in a non-Java environment.

**IN/OUT of Set Type in Java Stored Function/Procedure**

If the set type of the Java stored function/procedure in CUBRID is IN OUT, the value of the argument changed in Java must be applied to IN OUT. When the set type is passed to the OUT argument, it must be passed as a two-dimensional array.

Create procedure setoid(x in out set, z object)

as language java name

'SetOIDTest.SetOID(cubrid.sql.CUBRIDOID[][], cubrid.sql.CUBRIDOID';

 

public static void SetOID(cubrid.sql.CUBRID[][] set, cubrid.sql.CUBRIDOID aoid){

  Connection conn=null;

  Statement stmt=null;

  String ret="";

  Vector v = new Vector();

  cubrid.sql.CUBRIDOID[] set1 = set[0];

  try {

    if(set1!=null) {

      int len = set1.length;

      int i = 0;

      for (i=0 i< len i++)

        v.add(set1[i]);

    }

  v.add(aoid);

  set[0]=(cubrid.sql.CUBRIDOID[]) v.toArray(new cubrid.sql.CUBRIDOID[]{});

  } catch(Exception e) {

    e.printStackTrace();

    System.err.pirntln("SQLException:"+e.getMessage());

  }

}

**Using OID in Java Stored Function/Procedure**

In case of using the OID type value for IN/OUT in CUBRID, use the value passed from the server.

create procedure tOID(i inout object, q string)

as language java

name 'OIDtest.tOID(cubrid.sql.CUBRIDOID[], java.lang.String)';

 

public static void tOID(CUBRIDOID[] oid, String query)

{

  Connection conn=null;

  Statement stmt=null;

  String ret="";

 

  try {

    Class.forName("cubrid.jdbc.driver.CUBRIDDriver");

    conn=DriverManager.getConnection("jdbc:default:connection:");

 

    conn.setAutoCommit(false);

    stmt = conn.createStatement();

    ResultSet rs = stmt.executeQuery(query);

    System.out.println("query:"+ query);

 

    while(rs.next()) {

      oid[0]=(CUBRIDOID)rs.getObject(1);

      System.out.println("oid:"+oid[0].getTableName());

    }

    stmt.close();

    conn.close();

  } catch (SQLException e) {

    e.printStackTrace();

    System.err.println("SQLException:"+e.getMessage());

  } catch (Exception e) {

    e.printStackTrace();

    system.err.println("Exception:"+ e.getMessage());

  }

}