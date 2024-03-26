Title: Bulk delete databases in MSSQL using Python
Date: 2024-03-26

**THIS IS SUPER SIMPLE BUT I HAVE FOUND A LOT OF VALUE OUT OF THIS SCRIPT**

In my current position in software support, I need to restore multiple databases a week for different versions of our software. With that comes a lot of bloating of my local SQL instance.

For a while now I have been talking about culling my restored databases but I didn't want to have to go through the GUI and my SQL knowledge is very much limited to basic queries so I feel a lot more comfortable dealing with Python and scripting that way rather than doing this natively through SSMS.

I was at least tipped off about the following query:


```SQL
SELECT name FROM sys.sysdatabases
```

Which returns a list of all databases. So the next step was excluding databases as MSSQL has 4 system databases and there were a couple databases I didn't want deleted.

I opted to exclude the system databases from within the script and then have the user update a text file that would contain databases to exclude from the delete list.

From here the flowchart was essentially boiled down to 

**Grab databases to exclude > Grab all databases minus the ones to exclude > Loop through dropping databases from the delete list.**

The resulting script looks as follows

```Python
import pymssql
import sys

excluded_dbs = []
db_list = []
system_dbs = ['master','model','msdb','tempdb']

def get_exclusions():
    with open("excluded_dbs.txt", 'r') as file:
        for line in file:
            excluded_dbs.append(line.strip())

def get_databases(cursor):
    try:
        cursor.execute("SELECT name FROM sys.sysdatabases")
        for row in cursor:
            if row['name'] not in excluded_dbs and row['name'] not in system_dbs:
                db_list.append('[' + row['name'] + ']')
    except Exception as e:
        print("An exception occurred: ", e)
        sys.exit(1)

def delete_databases(cursor):
    try:
        for item in db_list:
            cursor.execute("DROP DATABASE " + item + ';')
            print("DROPPED DATABASE " + item)
    except Exception as e:
        print("An exception occurred: ", e)
        sys.exit(1)
    
def main():
    server = input("Server Name: ")
    user = input("Username: ")
    password = input("Password: ")
    try:
        conn = pymssql.connect(server, user, password, 'master')
        cursor = conn.cursor(as_dict=True)
    except Exception as e:
        print("An exception occurred: ", e)
        sys.exit(1)
    conn.autocommit(True)
    get_exclusions()
    get_databases(cursor)
    delete_databases(cursor)
    conn.close()

if __name__ == "__main__":
    main()
```

The source code and such can be found at my [GitHub](https://github.com/jymmyboi/MSSQL-Bulk-Database-Delete)
