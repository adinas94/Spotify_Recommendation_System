# Spotify_Recommendation_System
#### By: Adina Steinman 
### Objective: Using the Spotify API to build a Music Recommendation System 
### Business Problem
The aim of this project is to build a recommendation system for a new music streaming platform. First, a classification model was built, in order to address the cold start problem and have a mechanism to generate ratings for new users. Next, an SVD-based model was created to provide recommended songs to users based on their existing preferences and ratinsg towards songs. This will allow the platform to build playlists that are tailored to an individual's tastes and hopefully win new customers will this personalized song-recommendation approach.

### Data Collection
A playlist of approximately 10,000 songs was extracted from the Spotify API using the Spotipy wrapper. Dfiferent endpoints were used to collect different information that was available, including:
- Artist data
- Track data 
- Genre data
- Audio features data (danceability, liveness, valence, etc.) 

### Feature Engineering 
Firstly, I created a set of dummy variables that specified which decade the song was played in; it could be that newer songs have higher ratings than older songs due to their increased popularity. I then built a ‘ratings’ variable that compressed the original “popularity” measure into a 1-5 scale in order to more easily differentiate ratings from one another. Lastly, since the dataset contained around 500 genres, I put this data into “buckets” that contained mainstream genre categories (pop, dance, etc.) and then each unique genre was transformed into a dummy variable. 

### Exploratory Data Analysis
The merged dataframe contained around 10,000 and exactly 26 columns containing information on audio features, artist information, track information, dummy decade variables, and dummy genre variables. 

The popularity and ratings distributions were looked at, and they seem to follow a similar overall distribution.

<img src="https://github.com/adinas94/Spotify_Recommendation_System/blob/main/Images/popularity distribution.png" alt="alt text" width="400" height="300">

<img src="https://github.com/adinas94/Spotify_Recommendation_System/blob/main/Images/ratings distribution.png" alt="alt text" width="400" height="300">

Also, the frequency of genres in the playlist was looked at - the most popular genres were dance, pop, and rock.

<img src="https://github.com/adinas94/Spotify_Recommendation_System/blob/main/Images/genre EDA .png" alt="alt text" width="600" height="400">

Certain features were measured on a 0-1 scale; when I plotted their frequencies, I founnd that overall, the playlist had songs with high danceability (songs suitable for dancing), high energy (high intensity and noise) and high valence (happy and cheery). It appears that we have a playlist full of fun, happy, dance songs! 

### Classification Models
Many vanilla models were run, with the results as follows:

<img src="https://github.com/adinas94/Spotify_Recommendation_System/blob/main/Images/vanilla models.png" alt="alt text" width="1000" height="400">

For this analysis, the goal is to optimize test precision. This is because we do not want any false positives in our model and want to ensure that ratings are classified correctly. Additionally, the model is less sensitive to false negatives, as an incorrect rating of 2 if the actual rating is 3, will not have a significant impact on the interpretability of our model. Therefore, I further tuned the XGBoost and SVM models as these models had the highest test precision scores in the vanilla models.

#### XGBoost
XGBoost was tuned using GridSearch to find the optimal hyperparameters. When the hyperparameters were included, the results only improved slightly compared to the vanilla model.

<img src="https://github.com/adinas94/Spotify_Recommendation_System/blob/main/Images/XGBoost result.png" alt="alt text" width="1000" height="100">

Next, I used the XGBoost predict_proba method to get predicted probabilities for ratings. By utilizing the probability of obtaining certain ratings, it allows the actual ratings to take on non-integer values. This will likely reduce the errors that are found between the actual ratings and the future predicted ratings, as the numbers are not restricted to strict integer values. These probabilistic ratings were then used in the recommendation system. 

#### SVM Model 

The SVM model produced stronger results, with an improvement in RMSE, once the best hyperparameters from GridSearch were included.

<img src="https://github.com/adinas94/Spotify_Recommendation_System/blob/main/Images/SVM model result.png" alt="alt text" width="900" height="80">

### Recommendation System 

For the integer ratings, models for KNNBasic, KNNBaseline, KNNWithMeans and SVD were all run, with SVD performing the strongest. The SVD scores, after hyperparameter tuning, were the following:

<img src="https://github.com/adinas94/Spotify_Recommendation_System/blob/main/Images/SVD tuned RMSE.png" alt="alt text" width="180" height="50">

Then, SVD model was run with the probabilistic ratings from the XGBoost model. The scores improved significantly when these ratings were used.

<img src="https://github.com/adinas94/Spotify_Recommendation_System/blob/main/Images/SVD prob tuned RMSE.png" alt="alt text" width="180" height="50">

The estimated ratings, based on these probabilistic ratings, were then generated through the SVD model. Below is a glimpse of 5 examples of these estimated ratings.

<img src="https://github.com/adinas94/Spotify_Recommendation_System/blob/main/Images/estimated head.png" alt="alt text" width="600" height="200">

Here is a sample of song recommendations that were created through the model, based off of a user's existing ratings:

<img src="https://github.com/adinas94/Spotify_Recommendation_System/blob/main/Images/recommended songs.png" alt="alt text" width="600" height="150">

### Post-Model EDA & Conclusions 

Post-model EDA was done on the estimation of the probabilistic ratings from the XGBoost model. Most notably, the model's errors were calculated, and the result was normally distributed, indicating that the model is fit well to the dataset. 

Conclusion: Our post-modeling EDA shows rather different results than our pre-modeling EDA; this is due to the probabilistic nature of the ratings, allowing our model to produce more precise ratings, and ultimately provide more accurate recommendations. Overall, the model performs very well with a small RMSE of 0.25; we see from the distribution of our errors that the errors are normally distributed. With the combined classification model and the SVD model, the music app can not only provide recommendations for existing users, but we can avoid the cold-start problem and generate recommendations for new users as well. As the app evolves, the data science team should look to continuously update the recommendation system model as additional user data is collected.
