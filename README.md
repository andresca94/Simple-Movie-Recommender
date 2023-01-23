# Simple-Movie-Recommender

## Simple-Movie-Recommender
<p align="justify">Simple recommenders are basic systems that recommend the top items based on a certain metric or score.I'm gonna create a simplified clone of IMDB Top 250 Movies using metadata collected from IMDB.</p>

The following are the steps involved:

* Decide on the metric or score to rate movies on.

* Calculate the score for every movie.

* Sort the movies based on the score and output the top results.

To build a clone of IMDB's Top 250, let's use a weighted rating formula as a metric/score.

$$\begin{equation} \text Weighted Rating (\bf WR) = \left({{\bf v} \over {\bf v} + {\bf m}} \cdot R\right) + \left({{\bf m} \over {\bf v} + {\bf m}} \cdot C\right) \end{equation}$$



Where:

* v is the number of votes for the movie;

* m is the minimum votes required to be listed in the chart;

* R is the average rating of the movie;

* C is the mean vote across the whole report.

<p align="justify">I already have the values to v (vote_count) and R (vote_average) for each movie in the dataset. It is also possible to directly calculate C from this data.Determining an appropriate value for m is a hyperparameter that you can choose accordingly since there is no right value for m. You can consider it as a preliminary negative filter that will simply remove the movies which have a number of votes less than a certain threshold m.I'm gonna use a cutoff m as the 90th percentile. In other words, for a movie to be featured in the charts, it must have more votes than at least 90% of the movies on the list. As percentile decreases, the number of movies considered will increase.</p>

Next and the most important step is to calculate the weighted rating for each qualified movie. To do this:

Define a function, weighted_rating();
I have calculated m and C so will be just pass them as an argument to the function;
Then select the vote_count(v) and vote_average(R) column from the q_movies data frame;
Finally, compute the weighted average and return the result.

## Content based recommender

### Plot Description Based Recommender
<p align="justify">I'm gonna build a system that recommends movies that are similar to a particular movie. To achieve this, I'm gonna compute pairwise cosine similarity scores for all movies based on their plot descriptions and recommend movies based on that similarity score threshold.</p>

The problem is a Natural Language Processing problem:

* First I need to extract some kind of features from the above text data before compute the similarity and/or dissimilarity between them.

* To put it simply, it is not possible to compute the similarity between any two overviews in their raw forms. To do this, you need to compute the word vectors of each overview or document, as it will be called from now on.

* As the name suggests, word vectors are vectorized representation of words in a document. The vectors carry a semantic meaning with it. For example, man & king will have vector representations close to each other while man & woman would have representation far from each other.

* To do this I'm gonna compute Term Frequency-Inverse Document Frequency (TF-IDF) vectors for each document. This will give me a matrix where each column represents a word in the overview vocabulary (all the words that appear in at least one document), and each column represents a movie, as before.

In its essence, the TF-IDF score is the frequency of a word occurring in a document, down-weighted by the number of documents in which it occurs. This is done to reduce the importance of words that frequently occur in plot overviews and, therefore, their significance in computing the final similarity score.
<p align="justify">
This system has done a decent job of finding movies with similar plot descriptions but the quality of recommendations is not that great. "The Dark Knight Rises" returns all Batman movies while it is more likely that the people who liked that movie are more inclined to enjoy other Christopher Nolan movies. This is something that cannot be captured by the present system.</p>

### Credits, Genres, and Keywords Based Recommender
<p align="justify">
The quality of your recommender would be increased with the usage of better metadata and by capturing more of the finer details. I'm gonna build a recommender system based on the following metadata: the 3 top actors, the director, related genres, and the movie plot keywords.The keywords, cast, and crew data are not available in the current dataset, so the first step would be to load and merge them into the main DataFrame metadata.From your new features, cast, crew, and keywords, you need to extract the three most important actors, the director and the keywords associated with that movie.</p>

## Suggestions

* Introduce a popularity filter: this recommender would take the 30 most similar movies, calculate the weighted ratings (using the IMDB formula from above), sort movies based on this rating, and return the top 10 movies.

* Other crew members: other crew member names, such as screenwriters and producers, could also be included.

* The increasing weight of the director: to give more weight to the director, he or she could be mentioned multiple times in the soup to increase the similarity scores of movies with the same director.
