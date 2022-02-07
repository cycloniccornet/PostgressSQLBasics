
#PostgreSQL basics assignment


##Assignment description

I denne assignment skal du vise basic forståelse af nogle kernekoncepter i at arbejde med Postgres.

Deriblandt arbejde med Docker og designe systemer ud fra ideen om at bruge virtual machines.

Du *kan* bruge Docker til at køre PostGres i et virtuelt miljø - beskriv fordelene ved at bruge virtuelle maskiner og CLI fremfor din egen.


###Using PostgreSQL with docker

####Setup


    1. docker pull postgres

    2. $ docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres


####Crud in PostgreSQL
    
Creating new database

    $createdb someDatabaseName

Selecting newly created db

    $\c someDatabaseName

You are now connected to database "someDatabaseName" as user "postgres".

Creating a table 

    CREATE TABLE countries (
        country_code char (2) PRIMARY KEY, 
        country_name text UNIQUE
    );

    $\dt
        List of relations
        Schema |   Name    | Type  |  Owner
        --------+-----------+-------+----------
        public | countries | table | postgres
        (1 row)

**Creating**

    INSERT INTO countries (country_code, country_name) 
    VALUES ('us','United States'), ('mx','Mexico'), ('au','Australia');

**Reading**

    SELECT * FROM countries;
    
    country_code | country_name
    --------------+---------------
    us | United States
    mx | Mexico
    au | Australia
    (3 rows)

**Delete**

    DELETE FROM countries WHERE country_code = 'us';

Hvis bare det var så nemt at slippe af med dem i virkeligheden...\
![img_1.png](img_1.png)

**Update**

    UPDATE countries SET country_code = '404' WHERE country_name = 'United States'


    test=# SELECT * FROM countries;
    country_code | country_name
    --------------+---------------
    us           | United States
    mx           | Mexico
    au           | Australia
    (3 rows)

###Connector in java

    import java.sql.Connection;
    import java.sql.DriverManager;
    
    public class PostgreSQLJDBC {
        public static void main(String args[]) {
            Connection c = null;
            try {
                Class.forName("org.postgresql.Driver");
                c = DriverManager.getConnection("jdbc:postgresql://localhost:5432/testdb",
                "postgres", "123");
            } catch (Exception e) {
                e.printStackTrace();
                System.err.println(e.getClass().getName()+": "+e.getMessage());
                System.exit(0);
            }
            System.out.println("Opened database successfully");
        }
    }

##Why use docker

1. Docker gør det muligt at replikere et miljø, som et stykke software har brug, på hvilken som helst maskine. Dette forgår gennem en virtual machine.

2. Jeg kan slippe for at downloade PostgreSQL lokalt og skulle kæmpe med at installere det. Og så kan jeg sende mit image til en ven i nød.