
# The Purpose of this database in the context of the startup, Sparkify, and their analytical goals.

With reference to the project introduction:

*"A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. Currently, they don't have an easy way to query their data, which resides in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app."*

IN a quest to help the analytics team it's recommended to create a star schema to simplify the queries through its denormalized structure that allows simple joins and fast aggregations.

# In addition to the data files, the project workspace includes six files:

* `test.ipynb` displays the first few rows of each table to check the database.
* `create_tables.py` drops and creates relations (tables). You run this file to reset your tables before each time you run your ETL scripts.
* `etl.ipynb` reads and processes a single file from `song_data` and `log_data` and loads the data into the respective tables. This notebook contains detailed instructions on the ETL process for each of the tables.
* `etl.py` reads and processes files from `song_data` and `log_data` and loads them into the tables.
* `sql_queries.py` contains all the sql queries, and is imported into the last three files above.
* README.md provides discussion on the project.

# Running the scripts:

The Script_running.ipynb used to run the create_tables.py and the etl.py files

# The database schema design and ETL pipeline overview

* Using the star schema with Fact and dimension tables working together to solve a business problems. 

* Fact table consists of the measurements, metrics of facts of a business process (made up of facts; events that have actually happened). the songplays table (transactional in nature) plays this role which in combination with dimension tables like users, artists, songs, time can tell us more about the songs most streamed, the artists most listened to or give a sense of the users' behaviour and preferences.

* The dimension tables that comprises users, songs, time, artists enable answering business questions and are joined with the songplays fact table via foreign keys through the star schema.

* Through simple joins the analytics team can answer many questions like:
    * How many times was a song played?
    * The total and avarege duration a user streamed listening. 
    * etc....

<img src="./Sparkify_star_schema.png" width="650">

* To maintain the referential integrity between tables, foreign keys have been added to the fact table referencing the primary keys in the dimensions table.

* The ETL pipline employed fuctions to extract and read json files from their respective directories.

* IN the etl.py code dropping duplicates rows from the user_df to keep unique userIds

`user_df.drop_duplicates(inplace=True)`


## Example query - Getting the five most users in terms of number of sessions

`%sql SELECT u.user_id, u.first_name, u.last_name, COUNT(sp.session_id) AS session_count FROM users u JOIN songplays sp ON u.user_id = sp.user_id GROUP BY 1,2,3 ORDER BY 4 DESC LIMIT 5;`

|user_id|first_name|last_name|session_count|
|-----|------|-----|-----|
|49|Chloe|Cuevas|689|
|80|Tegan|Levine|665|
|97|Kate|Harrell|557|
|15|Lily|Koch|463|
|44|Aleena|Kirby|397|
