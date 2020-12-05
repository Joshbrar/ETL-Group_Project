# ETL-Group_Project

When it comes to streaming services, there are a lot of options available. However, is there a better option for streaming service depending on the genre of movie or television that you prefer? Our purpose of this ETL work is to extract, transform, and finally load our data to determine if there is a preferred streaming service for the consumer based on their preferences. 

### Extract: 
    Using datasets found on Kaggle, we are looking at a dataset that contains information on streaming platforms that contain the title of the series, year the series was released, ratings and scores, the genre of the series, a brief description, the length of the series (or the number of seasons) and the streaming platforms that the series can be found on). Additionally, we are using another dataset for movies on the streaming platforms. This dataset contains the title of the movies, the year the movie was released, rating, the IMDB and Rotten Tomatoes scores, which streaming platform the movies is on, who directed the movie, the genre of the movie, and other information about the movie such as the country of origin, the languages the movie was released in and the run time. Both data sets were CSVs that would allow us to open, read, and clean/manipulate our data if necessary.

### Transform:
    To begin the data cleaning process, we imported our dataset for the movies to our juypter notebook and began the cleaning process. After reading the dataset using the read_csv() function, we then re-named the columns that we were going to use and removed the genre column for the time being as we will add them back on as the last column for our data frame. We then dropped any duplicates from the id column and set an index. Adding the genre column back on to the end of our data frame, we used a .dropna() function in the genre column.

        To continue with the transformation process of our movies data frame, we need to prepare the data frame to load our information to our data base. In order to do this we are using our movies_transformed data frame with the .to_sql() function and appending any information to that database. We prepare and test our query by performing a simple query statement (‘select * from movies_tbl’) using our connection. In order to continue the preparation for loading to our data frame, we needed to construct an empty data frame object and add our genres to the data frame using a forloop. In the forloop, we assign our movies_transformed_genras data frame and add our distict genras_id to the movies, and adding a column that confirms if the title is that of a movie (‘Movie_YN), and the genre to the movies, eventually creating a new movies_genras_df

    To clean the tv shows dataset, was again used our read_csv() function to create our tv shows data frame. Once in the data frame we began the cleaning process for our genre column. In order to avoid an errors when we upload both data frames to our database, we added 35000 to the ids in order to avoid having duplicate ides. Since the ids will be our primary keys for this data frame, the primary keys will start with 35000. The in order to begin cleaning, we re-named the columns and dropping some of the columns that we did not need (“Description”, “No of Seasons”) and temporarily removed the genre column as we want to add this column to the end of our data frame to match our movies data frame. When examining this genre column, we discovered multiple errors in the column that included the released date in column, the network in the column, or the streaming platform in the column. In order to address the issue and preserve as much of the data as possible, we used the .unique() function for the column in order to see the array of all the unique values in the column. Taking the values that do not belong in the column, we removed all of the values that did not belong and left the genres that we needed to use. There were some rows of data that were lost during this process due to either unclean or incomplete data and the data was unsalvageable and would not be available to use in our database. After cleaning the genre column, we needed to create a genre_id column and dropped any duplicate ids.

    In order to get the data frame to match the data frame that we need to create a forloop using the genre index and the streaming platform. Using the forloop we are creating new columns for the streaming platforms and coded our loop to add a “1” under the column for the streaming platform if that tv show is on the platform based on the information from our initial dataset. At the end of the loop we append our id to the column and create a new tv_shows_transformed_df. Once the new data frame was in place, we then added the Ids to the data frame. After adding the ids to the data frame, we needed to prepare our data frame to be uploaded to our final database. 

### Load: 
    
    To prepare our data, we created a connection_string and an engine with our connection string. We then tested to confirm our tables were set up correctly, we used the engine.table_names() function and confirmed the following tables were created: ‘genras_tbl’, ‘movies_tbl’, ‘movies_shows_genras_tbl’. After confirming that the tables are correct, our transformed data is ready to be loaded to our database. Using our tv_shows_transformed_df and .to_sql() function, we loaded the data frame to our ‘tv_show_tbl’ and appended the information creating our tv_shows_tbl. We repeated the process with our new_movie_genras_df and .to_sql() function to local to our ‘movies_shows_genras_tbl’ and appended the information to the table. Repeating the process with our movies_transformed data frame and the .to_sql() function, we appended the information to our ‘movies_tbl’ table in the database.
    After loading all (4) tables, we then would need to perform a join between the movies_tbl and the genras_names_tbl on the id columns. Performing this join allows us to determine the number of genres for the movies in our database. We would need to perform an additional join where we use our previous join statement and replace our movies_tbl with the tv_shows_tbl to find the number of genres for our movies and TV shows. Using this information we would be able to make a decision about which streaming platform would be best depending on our favorite genre of movie or TV show.

    During the closing of this project, we felt that it was best to use a relational database as it would help us determine if our conclusion was valid. Using the relational database allowed us to set keys and link our tables when performing our joins. This allowed us to reach our conclusion and determine if we were able to ultimately answer our questions that we posed in the beginning of the project.






