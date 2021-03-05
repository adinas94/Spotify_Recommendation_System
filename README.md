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

### 
