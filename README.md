# Mysql
1.
create database project;
use project;
CREATE TABLE employee (
    empno INT,
    ename VARCHAR(255),
    job VARCHAR(255),
    mgr INT,
    hiredate DATE,
    sal FLOAT,
    comm FLOAT,
    deptno INT
);

INSERT INTO employee (empno, ename, job, mgr, hiredate, sal, comm, deptno)
VALUES
    (8369, 'SMITH', 'CLERK', 8902, '1990-12-18', 800.00, NULL, 20),
    (8499, 'ANYA', 'SALESMAN', 8698, '1991-02-20', 1600.00, 300, 30),
    (8521, 'SETH', 'SALESMAN', 8698, '1991-02-22', 1250.00, 500, 30),
    (8566, 'MAHADEVAN', 'MANAGER', 8839, '1991-04-02', 2985.00, NULL, 20),
    (8654, 'MOMIN', 'SALESMAN', 8698, '1991-09-28', 1250.00, 1400, 30),
    (8698, 'BINA', 'MANAGER', 8839, '1991-05-01', 2850.00, NULL, 30),
    (8882, 'SHIVANSH', 'MANAGER', 8839, '1991-06-09', 2450.00, NULL, 10),
    (8888, 'SCOTT', 'ANALYST', 8566, '1992-12-09', 3000.00, NULL, 20),
    (8839, 'AMIR', 'PRESIDENT', NULL, '1991-11-18', 5000.00, NULL, 10),
    (8844, 'KULDEEP', 'SALESMAN', 8698, '1991-09-08', 1500.00, 0, 30);
 Result : 10 row(s) affected Records: 10  Duplicates: 0  Warnings: 0	0.079 sec
select * from employee;
empno	ename	job	mgr	hiredate	sal	comm	deptno
8369	SMITH	CLERK	8902	12/18/1990	800	NULL	20
8499	ANYA	SALESMAN	8698	2/20/1991	1600	300	30
8521	SETH	SALESMAN	8698	2/22/1991	1250	500	30
8566	MAHADEVAN	MANAGER	8839	4/2/1991	2985	NULL	20
8654	MOMIN	SALESMAN	8698	9/28/1991	1250	1400	30
8698	BINA	MANAGER	8839	5/1/1991	2850	NULL	30
8882	SHIVANSH	MANAGER	8839	6/9/1991	2450	NULL	10
8888	SCOTT	ANALYST	8566	12/9/1992	3000	NULL	20
8839	AMIR	PRESIDENT	NULL	11/18/1991	5000	NULL	10
8844	KULDEEP	SALESMAN	8698	9/8/1991	1500	0	30


Write a query to display EName and Sal of employees whose salary are greater than or equal to 2200?

SELECT EName, Sal
FROM employee
WHERE Sal >= 2200;
Result :
EName	Sal
MAHADEVAN	2985
BINA	2850
SHIVANSH	2450
SCOTT	3000
AMIR	5000

Write a query to display details of employees who are not getting commission?

SELECT *
FROM employee
WHERE comm IS NULL;

Result :
empno	ename	job	mgr	hiredate	sal	comm	deptno
8369	SMITH	CLERK	8902	12/18/1990	800	NULL	20
8566	MAHADEVAN	MANAGER	8839	4/2/1991	2985	NULL	20
8698	BINA	MANAGER	8839	5/1/1991	2850	NULL	30
8882	SHIVANSH	MANAGER	8839	6/9/1991	2450	NULL	10
8888	SCOTT	ANALYST	8566	12/9/1992	3000	NULL	20
8839	AMIR	PRESIDENT	NULL	11/18/1991	5000	NULL	10


Write a query to display employee name and salary of those employees who don't have their salary in the range of 2500 to 4000?

SELECT EName, Sal
FROM employee
WHERE Sal < 2500 OR Sal > 4000;

Result :
EName	Sal
SMITH	800
ANYA	1600
SETH	1250
MOMIN	1250
SHIVANSH	2450
AMIR	5000
KULDEEP	1500

Write a query to display the name, job title and salary of employees who don't have a manager?

SELECT EName, job, Sal
FROM employee
WHERE mgr IS NULL;

Result:
EName	job	Sal
AMIR	PRESIDENT	5000


Write a query to display the name of an employee whose name contains "A" as the third alphabet?

SELECT EName
FROM employee
WHERE SUBSTRING(EName, 3, 1) = 'A';

Result: 
Ename 
O rows returned

Write a query to display the name of an employee whose name contains "T" as the last alphabet?

SELECT EName
FROM employee
WHERE RIGHT(EName, 1) = 'T';

Result : 
Ename 
Scott
--------------------------------
2.write a  program and using  jdbc connectivity, Create table and  insert below data .empcode 	empname	empage	empsalary
101	Jenny	25	10000
102	Jacky	30	20000
103	Joe	20	40000
104	John	40	80000
105	Shameer	25	90000



import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class Demo {

    public static void main(String[] args) {
        // Database connection details
        String dbUrl = "jdbc:mysql://localhost:3306/jdbc";
        String dbUser = "root";
        String dbPassword = "pass";

        // JDBC variables
        Connection connection = null;
        PreparedStatement preparedStatement = null;

        try {
            // Establishing the connection
            connection = DriverManager.getConnection(dbUrl, dbUser, dbPassword);

            // Creating the 'employee' table
            String createTableSQL = "CREATE TABLE IF NOT EXISTS employee ("
                    + "empcode INT PRIMARY KEY, "
                    + "empname VARCHAR(255), "
                    + "empage INT, "
                    + "empsalary DOUBLE)";
            preparedStatement = connection.prepareStatement(createTableSQL);
            preparedStatement.execute();

            // Inserting data into the 'employee' table
            String insertDataSQL = "INSERT INTO employee (empcode, empname, empage, empsalary) VALUES (?, ?, ?, ?)";
            preparedStatement = connection.prepareStatement(insertDataSQL);

            int[] empCodes = {101, 102, 103, 104, 105};
            String[] empNames = {"Jenny", "Jacky", "Joe", "John", "Shameer"};
            int[] empAges = {25, 30, 20, 40, 25};
            double[] empSalaries = {10000, 20000, 40000, 80000, 90000};

            for (int i = 0; i < empCodes.length; i++) {
                preparedStatement.setInt(1, empCodes[i]);
                preparedStatement.setString(2, empNames[i]);
                preparedStatement.setInt(3, empAges[i]);
                preparedStatement.setDouble(4, empSalaries[i]);

                // Execute the update
                preparedStatement.executeUpdate();
            }

            System.out.println("Table created and data inserted successfully!");

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // Closing resources
            try {
                if (preparedStatement != null) preparedStatement.close();
                if (connection != null) connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}

Result :
select * from employee;
empcode	empname	empage	empsalary
101	Jenny	25	10000
102	Jacky	30	20000
103	Joe	20	40000
104	John	40	80000
105	Shameer	25	90000

note : Jar file added to external library from project file structure after extracting connector jar file.My database name is jdbc

