# PostgreSQL Setup and Config
The following includes the instructions for downloading, setting up, and configuring PostgreSQL for our project. The following steps are for Mac OS X and is a general format. They may need some edits for other operating systems.

1. **Download Interactive Installer by EDB**
    - Use version 15 <br/>
    ```https://www.postgresql.org/download/```
    - Run the installer using the GUI
        - use default settings
        - remember to note the password set for the postgres user
          
2. **Setup PostgreSQL**
    - export PG_HOME/bin to PATH  <br/>
    ```export PATH="/Library/PostgreSQL/15/bin:$PATH"```

    - Start the server <br/>
    ```sudo -u postgres pg_ctl -D /Library/PostgreSQL/15/data start```
    - Stop the server  <br/>
    ```sudo -u postgres pg_ctl -D /Library/PostgreSQL/15/data stop```

    **Extra commands** <br/>
        - Check status of the server  <br/>
        ```sudo -u postgres pg_ctl -D /Library/PostgreSQL/15/data status``` <br/>
        - Restart the server <br/>
        ```sudo -u postgres pg_ctl -D /Library/PostgreSQL/15/data restart```
   

3. **Change authentication methods** <br/> <br/>
    ```sudo vim /Library/PostgreSQL/15/data/pg_hba.conf```
   
    - Change the file to match the following: <br/>
   ```
    #"local" is for Unix domain socket connections only
    #local user no authentication
    local   all             all                                     trust
    #IPv4 local connections:
    host    all             all             0.0.0.0/0               md5
    #IPv6 local connections:
    host    all             all             ::1/128                 md5
   ```
   - Restart the server <br/>
     ```sudo -u postgres pg_ctl -D /Library/PostgreSQL/15/data restart```

4. **Creating Sqlrewriter Database and connecting it** <br/>
    - Login with the default user <br/>
    ```psql -U postgres```
    - Create the sqlrewriter database <br/>
    ```CREATE DATABASE sqlrewriter;```
    - Check that the database has been created (sqlrewriter should show) <br/>
    ```SELECT datname FROM pg_database;```
    - Add the .env file to the root folder of our project <br/>
        - Paste this url into the file and set the user and password<br/>
        ```DATABASE_URL=postgresql://username:password@localhost:5432/sqlrewriter```
    - Open pgadmin4
        - Click on Servers dropdown
        - Try to log into the PostgreSQL 15 with your previous password to test connection
        - To create the sqlrewriter database here:
            - Click on top menu Object -> Register -> Server
            - Name: sqlrewriter
            - Click on Connection:
                - Host is localhost
                - Enter your password
        - Save
        - sqlrewriter should show up on the sidebar now
    - Run the server with [command]
        - the default is dev using SQLite DB
        - specifying prod such as ```[run command] prod``` uses PostgreSQL DB
    - The site is accessible and you can check data propagation on the pgadmin GUI
        - sqlrewriter -> Databases -> sqlrewriter -> Schemas -> Tables
        - View the data in the tables in pgadmin to make sure you have connected correctly
        - This can also be available through the terminal
            - Log into the user
            - Change the database <br/>
            ```\c [database name]```
            - Show the tables <br/>
            ```\dt```

    - Additionally, you can check that you can access the database in the terminal <br/>
    ```psql -h localhost -U postgres -d sqlrewriter```
