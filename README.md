

# HR- EMPLOYEE Data Analytics & Visualization 

### Dashboard Link : https://app.powerbi.com/groups/me/reports/384d017e-e935-44dc-9e7d-1626c1a36de1/ReportSection

## Problem Statement

To assist HR professionals, managers, and executives in making informed decisions by presenting relevant data in a clear and concise manner. A comprehensive yet a precise way to display all key data and metrics. 
Covers the following key areas: 

- Employee turnover rate 
- Key data which assists HR for overall organizational development  evaluating and improving employee engagment. 
- Tracking talent development which is valuable in identifying high-potential employees and planning for future leadership needs.
- Visualization of gender ratio in the organization to foster diversity, inclusion, and a promote a more innovative and equitable workplace culture.


### Steps followed 
#### Postgres Database and Tables creation in Python 
   Libraries & Modules used: 
   *Pandas, Io, psycopg2, sqlalchemy*

Step 1: Database Creation and Table Creation

Defining function create_database() that establishes a connection to a PostgreSQL database and creates a new database named "EMPLOYEE."
Inside the database, it creates four tables: employee_data, employee_survey, employee_training, and recruitment.
         def create_database():
    conn= psycopg2.connect("host=127.0.0.1 dbname=postgres user=postgres password=admin123")
    conn.set_session(autocommit=True)
    cur = conn.cursor()
    
    cur.execute("DROP DATABASE if exists EMPLOYEE")
    cur.execute("CREATE DATABASE EMPLOYEE")
    
    # closing connection to default db 
    conn.close()
    
    conn= psycopg2.connect("host=127.0.0.1 dbname=employee user=postgres password=admin123")
    cur = conn.cursor 
    
    return cur, conn


    # CREATE TABLE FUNCTION
    def create_tables(cur, conn): 
    for query in create_table_queries: 
        cur.execute(query)
        conn.commit()
           
Step 2: Data Loading and Data Type Exploration

Loading four CSV files (employee_data.csv, employee_engagement_survey_data.csv, training_and_development_data.csv, and recruitment_data.csv) into Pandas DataFrames: employee_data, emp_survey, training_dev, and recruitment_data.

         
       employee_data = pd.read_csv("C:/Users/User/ Documents/PORTFOLIO/DATA/customer/employee_data.csv")
       emp_survey = pd.read_csv("C:/Users/User/Documents/PORTFOLIO/DATA/customer/employee_engagement_survey_data.csv")
       training_dev = pd.read_csv("C:/Users/User/Documents/PORTFOLIO/DATA/customer/training_and_development_data.csv")
       recruitment_data = pd.read_csv("C:/Users/User/Documents/PORTFOLIO/DATA/customer/recruitment_data.csv")

Step 3: Database Table Creation

Defining SQL statements for creating tables (employee_data, employee_survey, employee_training, and recruitment) with specified columns and data types.

        employee_table_create = ("""create table if not exists employee_data(EmpID  int,                      
                                 FirstName varchar, LastName varchar,StartDate date,   
                                 ExitDate  date, Title  varchar, Supervisor  varchar,                       
                                 ADEmail varchar, BusinessUnit varchar,
                                 EmployeeStatus varchar,EmployeeType varchar,  
                                 PayZone varchar,  EmployeeClassificationType varchar, 
                                 TerminationType varchar,TerminationDescription varchar,
                                 DepartmentType varchar,Division  varchar, DOB varchar, 
                                 State varchar,JobFunctionDescription varchar,
                                 GenderCode varchar,
                                 LocationCode int, RaceDesc varchar,
                                 MaritalDesc varchar,
                                 Performance_Score varchar,
                                 Current_Employee_Rating int)""")
                                
        cur = conn.cursor()
        cur.execute(employee_table_create)
        conn.commit()

        employee_survey_create= ("""create table if not exists employee_survey(
                                Employee_id int, Survey_date varchar,
                                Engagement_Score int,Satisfaction_Score int,
                                Work_life_Balance_Score int)""")

        cur = conn.cursor()
        cur.execute(employee_survey_create)
        conn.commit()

        training_table_create=("""create table if not exists employee_training(
                                 Employee_ID int, Training_date date, 
                                 Training_program varchar,Training_Type varchar,
                                 Training_status varchar,Location varchar,
                                 Trainer  varchar,Training_Duration int, Training_Cost decimal)""")
        

        cur = conn.cursor()
        cur.execute(training_table_create)
        conn.commit()

        recruitment_table_create = ("""create table if not exists recruitment(
                                 Applicant_id int,Application_Date varchar,
                                 First_Name varchar,Last_Name varchar,
                                 Gender  varchar,DOB varchar,
                                 Phone_Number varchar,Email varchar, Address varchar, 
                                 City varchar, State varchar, Zip_code int, 
                                 Country varchar, Education_Level varchar, 
                                 experience_years int, expected_Salary decimal, 
                                 Job_Title varchar, Status varchar)""")

        cur = conn.cursor()
        cur.execute(recruitment_table_create)
        conn.commit()

Step 4: Creating a SQLAlchemy Engine 

Creating an SQLAlchemy engine to connect to the PostgreSQL database.
         engine = create_engine(
        'postgresql+psycopg2://postgres:admin123@127.0.0.1:5432/employee')

Step 5: Copying Data from Pandas DataFrames to Database Tables:

Using the to_sql method of Pandas to insert the entire DataFrames into the corresponding database tables.
Additionally the copy_from method is also used to copy data from DataFrames to database tables. This method is used for efficient bulk data insertion.

        emp_survey.head(0).to_sql('employee_survey', engine, if_exists='replace',index=False)
        conn = engine.raw_connection()
        cur = conn.cursor()
        output = io.StringIO()
        emp_survey.to_csv(output, sep='\t', header=False, index=False)
        output.seek(0)
        contents = output.getvalue()
        cur.copy_from(output, 'employee_survey', null="") # null values become ''
        conn.commit()
        cur.close()
        conn.close()

        recruitment_data.head(0).to_sql('recruitment', engine, if_exists='replace',index=False)
        conn = engine.raw_connection()
        cur = conn.cursor()
        output = io.StringIO()
        recruitment_data.to_csv(output, sep='\t', header=False, index=False)
        output.seek(0)
        contents = output.getvalue()
        cur.copy_from(output, 'recruitment', null="") # null values become ''
        conn.commit()
        cur.close()
        conn.close()

        training_dev.head(0).to_sql('employee_training', engine, if_exists='replace',index=False)
        conn = engine.raw_connection()
        cur = conn.cursor()
        output = io.StringIO()
        training_dev.to_csv(output, sep='\t', header=False, index=False)
        output.seek(0)
        contents = output.getvalue()
        cur.copy_from(output, 'employee_training', null="") # null values become ''
        conn.commit()
        cur.close()
        conn.close()

        employee_data.head(0).to_sql('employee_data', engine, if_exists='replace',index=False)
        conn = engine.raw_connection()
        cur = conn.cursor()
        output = io.StringIO()
        employee_data.to_csv(output, sep='\t', header=False, index=False)
        output.seek(0)
        contents = output.getvalue()
        cur.copy_from(output, 'employee_data', null="") # null values become ''
        conn.commit()
        cur.close()
        conn.close()

Step 6: Closing Database Connections. 

After data insertion, connection to the database is closed. 

#### Data Cleaning in Postgres

- Checking for null values in employee  survey questions 
         
         select * from employee_survey
         where "EngagementScore" is null
         or "SatisfactionScore" is null or
         "WorklifeScore" is null

- Renaming column names for employee_training table 

 
      ALTER Table employee_training RENAME "Employee ID"  TO "Employee_id";
      ALTER Table employee_training RENAME "Training Date"  TO "Training_date";
      ALTER Table employee_training RENAME "Training Program Name"  TO "Training_program";
      ALTER Table employee_training RENAME "Training Type"  TO "Training_type";
      ALTER Table employee_training RENAME "Training Outcome"  TO "Training_status";
     ALTER TABLE employee_training RENAME "Training Duration(Days)"  TO "Training_duration";
     ALTER TABLE employee_training RENAME "Training Cost"  TO "Training_cost";

-  Checking to see correlation between Cost of training and number of days 

      
       SELECT corr("Training_cost", "Training_duration") from employee_training


-  Renaming recruitment data columns 
  
        ALTER Table recruitment RENAME "Applicant ID"  TO "application_id";
        ALTER Table recruitment RENAME "Application Date"  TO "application_date";
        ALTER Table recruitment RENAME "First Name"  TO "first_name";
        ALTER Table recruitment RENAME "Last Name"  TO "last_name";
        ALTER Table recruitment RENAME "Date of Birth"  TO "dob";
        ALTER Table recruitment RENAME "Phone Number"  TO "phone";
        ALTER Table recruitment RENAME "Zip Code"  TO "zip_code";
        ALTER Table recruitment RENAME "Education Level"  TO "education_level";
        ALTER Table recruitment RENAME "Years of Experience"  TO "experience";
        ALTER Table recruitment RENAME "Job Title"  TO "job_title";

-  Changing dates type from varchar to date 

       ALTER TABLE employee_data 
       ALTER COLUMN “DOB” TYPE DATE USING to_date( “DOB”, 'YYYY-MM-DD');

       ALTER TABLE employee_data 
       ALTER COLUMN “StartDate” TYPE DATE USING   to_date(“StartDate”, 'YYYY-Mon-DD');

       ALTER TABLE employee_data 
       ALTER COLUMN “ExitDate” TYPE DATE USING   to_date(“ExitDate”, 'YYYY-Mon-DD');

       ALTER TABLE recruitment 
       ALTER COLUMN "application_date" TYPE DATE  USING to_date("application_date", 'DD-Mon-YY');

       ALTER TABLE recruitment 
       ALTER COLUMN "dob" TYPE DATE USING to_date("dob", 'DD-MM-YYYY');

- Updating values for termination type of active employees from Unk to “Active Employee” 


      UPDATE employee_data 
      SET "TerminationType"='Active Employee' 
      WHERE "TerminationType"='Unk' and    "EmployeeStatus"='Active';

-  Change employee status to “ Voluntarily Terminated” where status was active where termination type was “ voluntary and exit date was also there. 

       UPDATE employee_data 
       SET "EmployeeStatus"='Voluntarily   Terminated' 
       WHERE "TerminationType"='Voluntary' and  "EmployeeStatus"='Active' and "ExitDate" is not null


- change type  terminated for cause when termination type was volunatary, resignation and retirement but the employee status was active and exit date wasn’t null

       UPDATE employee_data 
       SET "EmployeeStatus"='Terminated for Cause' 
       WHERE "TerminationType" in ('Involuntary', 'Resignation','Retirement') and "EmployeeStatus"='Active' and "ExitDate" is not null

- For missing phone numbers adding n/a

      select "application_id" from recruitment   where "phone"= '###############################################################################################################################################################################################################################################################'

      UPDATE recruitment
      SET "phone"='N/A' 
      WHERE "application_id"='[range of values  found from previous query]


### Dax Measures 
  - Active Head count of Employees 
     
        headcount = CALCULATE(count('public employee_data'[EmpID])
                      , 'public employee_data'[StartDate]<= MAX('Date'[Dates])
                      && ('public employee_data'[ExitDate] > MAX('Date'[Dates])|| ISBLANK('public employee_data'[ExitDate])

   - Employee Age brackets 

         Age_Bucket = 
         VAR _Age = Year(today())- Year('public employee_data'[DOB])
         VAR _Result = 
         SWITCH(
         TRUE(),
             _Age < 20 , "under 20",
             _Age >= 20 && _Age < 30, "20-30",
             _Age >= 30 && _Age < 40, "30-40",
             _Age >= 40 && _Age < 50, "40-50 ",
             _Age >= 50 && _Age <60,"50-60" ,
             _Age >=60, "60+"
         )
          Return
           _Result         

   - Rate of Candidates offered Jobs 

          offer_acceptance_rate = 
          var _offersaccepted = CALCULATE(
          COUNT('public recruitment'[application_id]), 'public recruitment'[Status]=   "Offered") 
          var _totalOffers=  CALCULATE(
          COUNT('public recruitment'[application_id])) 
             return DIVIDE(_offersaccepted, _totalOffers)*100   

 - Employee leaving

       Leavers = COUNTROWS(FILTER('public employee_data',
        NOT(ISBLANK('public  employee_data'[ExitDate])) && 
        'public employee_data'[ExitDate] = SELECTEDVALUE('Date'[Dates])))  

 - Turnover rate 

       Headcount = 

       VAR MaxDate = Max ( 'Date'[Dates] )
       VAR MinDate = Min ( 'Date'[Dates] )
       RETURN
       0 +
       CALCULATE (
       COUNTROWS('public employee_data'),
       'public employee_data'[StartDate] <= MaxDate,
       'public employee_data'[ExitDate]>= MinDate || ISBLANK('public employee_data'    [ExitDate]),
       All('Date'[Dates]))


       Leavers = 
       VAR MaxDate = Max ( 'Date'[Dates] )
       VAR MinDate = Min ( 'Date'[Dates] )
       RETURN
       0 +
       CALCULATE (
       COUNTROWS('public employee_data'),
       'public employee_data'[ExitDate] <= MaxDate,
       'public employee_data'[ExitDate] >= MinDate,
       All('Date'))
       
          
       Turnover = 
       VAR MaxDate = Max('Date'[Dates])
       VAR MinDate = Min('Date'[Dates])
       VAR DayCount = 1 + (MaxDate - MinDate)

       RETURN

       DIVIDE (
       [Leavers] * DayCount,
       SUMX('Date', [Headcount]),0) 

 - Women Employee Ratio 
       
       Women_ratio measure: = 
       DIVIDE (
       CALCULATE ( COUNTROWS ( 'public employee_data' ), 
       'public employee_data'[GenderCode] = "Female" ),
       COUNTROWS ( 'public employee_data' )
        ) 


![Snap_1](https://github.com/RizwanaTairr/HR-Employee-Analysis/assets/126111004/34ab868d-e62b-4518-b53c-003d3c3797f2)

![Snap_2](https://github.com/RizwanaTairr/HR-Employee-Analysis/assets/126111004/9bcaf44a-ed7d-4e4d-a5ee-9f88cb533f7f)

![Snap_3](https://github.com/RizwanaTairr/HR-Employee-Analysis/assets/126111004/df792535-f2ae-46c4-b08a-06d1abceeb28)

![Snap_4](https://github.com/RizwanaTairr/HR-Employee-Analysis/assets/126111004/4b01e7cd-7ed4-4089-ab54-08382198b63b)







  



           
        
