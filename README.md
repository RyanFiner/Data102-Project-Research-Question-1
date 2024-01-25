# Data102-Project-Research-Question-1

Question: Trying to predict success of a movie by the actors in it.

Data acquired from IMDB non-commercial dataset, oscar data from Kaggle, and a subset of the TMDB dataset from Kaggle.

3.1 Predicting the Success of a Movie
Within prediction, it is important to understand your goals and data within
your data set. Success can be classified in many ways, how a movie exists within
the cultural sphere long after it has been released, the ratings and awards it
received, through financial success, or a combination of these features. Within
our data, we decided trying multiple of these metrics using the features given
within the data we obtained. We focused our effort on the popularity which was
how popular the movie’s page was on the website TMDb, the TMDb rating, and
the profit which was the adjusted budget subtracted from the adjusted revenue.
Prediction of adjusted revenue was done keeping in mind the temporally
of the data meaning that to minimize data-leakage certain features needed to
be excluded otherwise information from the future would be used to predict
whether a movie was financially successful in the past. Furthermore, this dataleakage needed to be minimized to make the predictions meaningful as after the
fact information makes our model biased towards old movies as their actors can
have decades of movie experience leading towards the most recent dates of the
data.

For other metrics such as popularity and the TMDb vote score, such dataleakage measures could not be preserved, so the analysis of this information
gives a perspective of hindsight on how these metrics are correlated to other
features within the data.
The main methods of prediction will be with parametric frequentest GLM
models and with the non-parametric Random Forest and MLP regression tree
model. These models were chosen due to the complexity of the data and the
multi-variable nature of the data which would of made creating priors difficult.
These different models are all capable of performing regression as predicting
success is with continuous values so we cannot use models that are classifiers.
Assumptions

For prediction assumptions were made about the data. We assume that there
is some sort of linear relationship between the features and that the predictors
are relevant to the target hence the models. A primary assumption being made
is that actors and genres’ impact on films are the same regardless of the film.
This means that an actor in the past has the same impact on the film as they
do in the present. This assumption is made because within the model actors are
processed with multi-label binarization which splits a set of actors into either 1’s
if they are in the film or not. If this assumption is not made then it breaks the
other assumption that there is no future data within the model as previously
mentioned.

Implementation
Different outlier methods were tested with them being interquartile range,
standard deviation, and winsorization. All the methods still had many outliers
present. The data was right-skewed with some blockbuster films having massive
profit margins that were the outliers. The method of winsorization was chosen to 
maximize the amount of data we could use and sacrifice the accuracy. Ultimately
this decision was also made because of the knowledge gained from blockbusters
was minimal since these outliers would still have a large profit and be useful
towards influencing the models.

Finding the cumulative sum of each actor within each movie was a task
needed to keep only past information in the prediction. This lead to an issue of
computing power as working with multiple large data sources lead to difficult
computing time. After this Oscar data was added as features to the model.
The reason this information is used from outside data sources is that the data
is complete from these sources and is independent of our data. Since the data
is complete with the IMDB data, our data is essential because a subsection of
that data had the financial information that our question focuses on.
Standard scaling was attempted to be used but instead scaling was all that
we could do. The primary issue was with centering the data was that it broke
the Poisson model with it encountering infinite values. This was likely from
issues with the link function and applying log to 0 values. This could of had an
effect on results as the profit and budget values were centered primarily around
the mean.

Within the model actors were multi-label binorized which served as a method
to one-hot encode the one-to-many relationship that existed between actor and
movie. Additionally, the actors were fitted to only the top fifty percent of actors
with actors needing to be present in at least three films which was the mean
of appearances. This resulted in thousands of features within the data that
had little correlation with the target variable, so lasso regression was used to
select features. This was selected with a threshold of an arbitrary number of
.001 meaning that values that zeroed out or were less then this value were not
selected for the models.

Colinearity was an issue faced within the model. Many actors pairs were
colinear with an example of Daniel Radcliff and Rupert Grint who were in the
Harry Potty series had high correlation. This actor pair problem was solved
by dropping one of the actors. This solution was chosen because primary component analysis failed with not enough features capturing enough variance to
reduce the feature size.

Lastly, the regression models were mainly unmodified with fitting. The exception was the Negative Binomial model which was unable to converge with
other methods and needed the log-likelihood function to use the geometric function that excludes alpha. This could of possible arisen from the sparse nature
of many of the features as they were binary multi label features.

Interpretation

Linear reggression RMSE: 121.36312325097859
Linear reggression AIC: 7250.589213135677
Poisson reggression RMSE: 126.80943889985578
Poisson reggression AIC: 5719.711275570056
Negattive-Binomial reggression RMSE: 131.92244459068834
Negattive-Binomial reggression AIC: 5918.7754699785655
Random Forest reggression RMSE: 116.76598157890236
Random Forest reggression AIC: N/A
MLP reggression REGRESSION RMSE: 122.07755914615204
MLP reggression REGRESSION AIC: N/A

From the graphs and the metrics we can see that the best model according
to AIC is the Poisson model. All of the models have a Root mean squared error
above 100 which since it was translated back with the scaler means 100 million
dollars. From the graphs we can see some explanation of what is happening.
The models fail to predict many of the huge blockbuster and follow the general
path of the data. Additionally, the models fail to predict movies that don’t
make a profit. Ultimately, most of the models fail to predict the trends. This
however is not all bad considering the strict limitations on the data size, and
not using information from the future. With a larger sample size the model
could possibly pick up on some of the actor trends that could predict things
better. However, this could also be a sign that these features alone are not
enough information to make predictions and more features are needed.
