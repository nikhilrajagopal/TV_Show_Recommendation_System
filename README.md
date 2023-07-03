## Recommendation System

This code provides a recommendation system based on collaborative filtering using the user-item matrix. It predicts ratings for anime shows and generates personalized recommendations for users.

### Prerequisites

- Python 3.x
- Pandas
- NumPy
- scikit-learn

### Data

The code assumes the presence of the following CSV files in the current working directory:

- `anime.csv`: Contains information about anime shows, including anime_id, name, genre, type, episodes, rating, and members.
- `rating.csv`: Contains user ratings for anime shows, including user_id, anime_id, and rating.

### Code Overview for Collaborative Based Filtering

1. Data Loading and Preprocessing:
   - The code loads the `anime.csv` and `rating.csv` files into Pandas DataFrames.
   - Some data cleaning operations are performed, such as removing specific rows from `anime_data` and filtering out unwanted user IDs from `rating_data`.

2. Data Preparation:
   - The code calculates the average rating for each user and adds a column `adg_rating` that represents the adjusted rating by subtracting the user's average rating from their individual rating.
   - The relevant columns are selected and merged into `merged_datasets`, which combines the anime information with the user ratings.

3. User-Item Matrix:
   - The code generates a user-item matrix from `merged_datasets`, which represents the ratings given by users to different anime shows. Only the top 500 users with the most ratings are considered.
   - Missing values in the user-item matrix are filled with the mean value of the corresponding column.

4. Similarity Matrix Calculation:
   - The code defines a function `calculate_similarity_matrix` that computes the cosine similarity between users based on the user-item matrix.
   - The similarity matrix is created using the function and converted into a Pandas DataFrame.

5. Generating Recommendations:
   - The code defines a function `generate_recommendations` that takes a neighborhood size as input and generates personalized recommendations for a target user.
   - The function prompts the user to enter a user ID and then retrieves the most similar users to the target user based on cosine similarity scores.
   - The function calculates the predicted ratings for the target user by considering the ratings of similar users and their similarity values.
   - The top anime recommendations are determined by selecting the highest predicted ratings that the target user has not already rated.
   - The anime names and other features of the recommended shows are displayed as output.

6. Running the Recommendation System:
   - The code includes a `get_recommendations` function that calls `generate_recommendations` with a fixed neighborhood size of 25.
   - The user is prompted to enter a user ID, and the function generates and displays the top 10 anime recommendations for that user.

### Code Overview for Content Based Filtering

1. Data Loading and Preprocessing:
   - The code loads the `anime.csv` and `rating.csv` files into Pandas DataFrames.
   - Missing ratings are removed from the `anime_data` DataFrame.
   - The `rating_data` DataFrame is cleaned by removing rows with a rating of -1.

2. Data Integration and Cleaning:
   - The code loads the `animes.csv` file into another DataFrame, `other_anime_data`, and renames the `uid` column to `anime_id`.
   - The `anime_data` and `other_anime_data` DataFrames are merged on the `anime_id` column, keeping only the relevant columns.
   - Duplicate rows based on `anime_id` are dropped from the DataFrame.

3. Content Data Preprocessing:
   - The code creates a copy of the merged DataFrame as `content_data`.
   - Null values in the columns 'name', 'genre', 'type', and 'synopsis' are identified and removed from `content_data`.
   - Text fields, 'genre' and 'synopsis', are converted to lowercase and special characters are removed using regular expressions.
   - A new column, 'genre_synopsis', is created by combining 'genre' and 'synopsis'.

4. Generating Item Profiles:
   - The code defines a function, `generate_item_profiles(df)`, that uses the TF-IDF vectorizer to generate item profiles based on the 'genre_synopsis' column of the preprocessed DataFrame.

5. Calculating Item Similarity:
   - The code defines a function, `calculate_item_similarity(item_profiles)`, that calculates the cosine similarity between item profiles.

6. Finding Similar Items:
   - The code defines a function, `find_similar_items(genres, type_input, anime_names, item_similarity, df, top_n=100)`, that finds similar items based on user input, item similarity, and genre/type filters.
   - For each anime name provided by the user, the function retrieves the item indices and calculates similarity scores based on the item similarity matrix.
   - The most similar items, excluding the input anime, are selected and returned.

7. User Input and Recommendations:
   - The code defines a function, `get_recommendations()`, which prompts the user to enter genres, anime type, and anime names.
   - The data is preprocessed, item profiles and item similarity are generated, and similar items are retrieved based on user input.
   - The retrieved items are filtered based on genre, type, and rating.
   - If there are fewer than 10 recommendations, additional recommendations are filled based on similar anime names.
   - The top 10 recommendations, including their type, name, genre, and synopsis, are returned.

8. Execution:
   - The code calls the `get_recommendations()` function to generate anime recommendations based on user input.

### Instructions for Use

1. Ensure that the required CSV files (`anime.csv`, `animes.csv`, `rating.csv`) are present in the same directory as the Python script.

2. Run the code.

3. colloborative_based_filtering.ipynb:
    - When prompted, enter a user ID for which you want to generate recommendations. The user ID should be an integer corresponding to an existing user in the dataset.
    - The program will output the top 10 anime recommendations for the selected user, including the anime name, genre, type, episodes, and rating.

4. content_based_filtering.ipynb:
    - When prompted, enter a genre, type (movie, tv, ova, etc.), and anime name.
    - The program will output the top 10 anime recommendations for the selected user, including the type, anime name, genre, and synopsis.


### Limitations

- The code assumes that the required datasets (anime.csv, rating.csv, and animes.csv) are available in the same directory. If the files are not present, the code will not run.
- The code uses cosine similarity and TF-IDF vectorization, which might not capture all nuances of anime recommendations. Other recommendation algorithms and techniques could be explored for better results.
- The code is limited to the available data and its quality. If the datasets are incomplete or contain errors, it might affect the accuracy of the recommendations.


### Future Improvements

- Implement a more advanced collaborative filtering algorithm, such as Singular Value Decomposition (SVD) or Alternating Least Squares (ALS), to improve recommendation quality.
- Add user interfaces, such as a web application or command-line interface, to make the recommendation system more accessible and user-friendly.
- Optimize the code for performance and efficiency, especially for larger datasets.
- Provide additional options for selecting neighborhood size or tuning other parameters to enhance the recommendation system's flexibility.

