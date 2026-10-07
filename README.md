# 🎬 DSA 4060 – Personalized Movie Recommender

## Practical Quiz: Personalized Movie Recommender

---

## Student Information

| Field | Details |
|---|---|
| **Student Name** | Abdiqalaq Issack |
| **Student ID** | 671243 |
| **Assigned User ID** | 4 |
| **Course** | DSA 4060 – Recommender Systems |
| **Assessment** | Practical Quiz – Personalized Movie Recommender |
| **Recommendation Approach** | Content-Based Filtering |
| **Primary Content Feature** | Movie Genres |
| **Vectorization Method** | CountVectorizer |
| **Similarity Method** | Cosine Similarity |
| **Positive Rating Threshold** | Rating ≥ 4.0 |

---

# 1. Project Overview

Modern streaming platforms provide users with large catalogues of movies. Although having many choices is useful, the large number of available items can make it difficult for users to identify movies that match their individual interests.

Recommender systems address this problem by using information about users, items, and previous interactions to identify potentially relevant items.

This project develops a simple **personalized content-based movie recommender system** for an assigned user using historical movie ratings and movie genre information.

The assigned user for this project is **User 4**, calculated from Student ID **671243** according to the formula specified in the practical quiz.

The system first analyzes User 4's historical movie ratings to understand the user's observed preferences. It then identifies positively rated movies, transforms movie genres into numerical features using `CountVectorizer`, creates a user preference representation, and calculates similarity between that preference representation and movies in the catalogue using **cosine similarity**.

Movies that User 4 has already rated are excluded from the candidate set. The remaining unseen movies are ranked according to their similarity scores, and exactly **five movie recommendations** are returned.

The project demonstrates the complete recommendation process from raw data inspection to personalized recommendation generation and critical evaluation.

---

# 2. Project Objectives

The main objective of this practical quiz is to build a simple personalized movie recommender for the assigned user.

The specific objectives are to:

1. Load the supplied movie and ratings datasets using Python and pandas.
2. Inspect the structure and dimensions of both datasets.
3. Check the datasets for missing values.
4. Extract all ratings belonging to the assigned user.
5. Merge the user's ratings with the movie catalogue.
6. Identify the user's three highest-rated movies.
7. Analyze the user's apparent genre preferences using rating evidence.
8. Treat movies rated **4.0 or higher** as positively rated movies.
9. Convert movie genres into numerical features.
10. Construct a content-based representation of the user's preferences.
11. Calculate movie similarity using cosine similarity.
12. Remove movies already rated by the assigned user.
13. Rank unseen movies according to similarity.
14. Return exactly five personalized movie recommendations.
15. Provide a clear explanation for each recommendation.
16. Identify a limitation of the recommender.
17. Propose a realistic improvement that could improve personalization.

---

# 3. Assigned User Calculation

The practical quiz assigns each student a user based on the final two numeric digits of the Student ID.

The required formula is:

```text
Assigned User ID = (Last two digits of Student ID MOD 40) + 1
```

For Student ID:

```text
671243
```

the final two digits are:

```text
43
```

Therefore:

```text
43 MOD 40 = 3
```

and:

```text
3 + 1 = 4
```

Hence:

```text
Assigned User ID = 4
```

Therefore, all personalized analysis and recommendation results in this project are based specifically on:

> **User 4**

---

# 4. Repository Structure

The GitHub repository follows the required project structure:

```text
DSA4060-Recommender-671243/
│
├── README.md
├── DSA4060_Recommender_671243.ipynb
├── movies.csv
└── ratings.csv
```

## File Descriptions

| File | Description |
|---|---|
| `README.md` | Detailed documentation of the project, methodology, results, limitations, and execution instructions |
| `DSA4060_Recommender_671243.ipynb` | Fully executed Jupyter Notebook containing the complete implementation and outputs |
| `movies.csv` | Movie catalogue containing movie IDs, movie titles, and genres |
| `ratings.csv` | Historical user-movie ratings used to profile the assigned user |

---

# 5. Recommendation Workflow

The overall recommendation pipeline implemented in this project is:

```text
movies.csv + ratings.csv
          │
          ▼
Load datasets using pandas
          │
          ▼
Inspect first rows and dimensions
          │
          ▼
Check missing values
          │
          ▼
Filter ratings for User 4
          │
          ▼
Merge ratings with movie catalogue
          │
          ▼
Analyze User 4's rating history
          │
          ▼
Identify genre preferences
          │
          ▼
Select movies rated >= 4.0
          │
          ▼
Construct positive preference evidence
          │
          ▼
Prepare movie genre text
          │
          ▼
CountVectorizer
          │
          ▼
Numerical Genre Feature Matrix
          │
          ▼
Construct User Preference Vector
          │
          ▼
Cosine Similarity
          │
          ▼
Score catalogue movies
          │
          ▼
Remove movies already rated by User 4
          │
          ▼
Rank unseen candidates
          │
          ▼
Select Top 5
          │
          ▼
Explain Recommendations
```

---

# 6. Task 1 – Load and Inspect the Data

The first stage of the project involves loading and inspecting the two supplied datasets.

Both datasets are loaded into pandas DataFrames.

The purpose of this stage is to understand:

- the variables available;
- the size of each dataset;
- the relationship between the datasets;
- the structure of the movie catalogue;
- the structure of the user rating information; and
- whether missing values are present.

This step is important because the recommendation algorithm depends on correctly structured movie IDs, genre information, user IDs, and ratings.

---

## 6.1 Dataset Dimensions

The dataset inspection produced the following dimensions:

| Dataset | Rows | Columns |
|---|---:|---:|
| `movies.csv` | **36** | **3** |
| `ratings.csv` | **440** | **3** |

Therefore:

```text
Movies available = 36
Historical rating records = 440
```

The project therefore uses a catalogue containing **36 movies** and a historical ratings dataset containing **440 user-movie interactions**.

---

# 7. Movies Dataset

The `movies.csv` dataset contains:

```text
36 rows × 3 columns
```

The three variables are:

| Variable | Description |
|---|---|
| `movie_id` | Unique numerical identifier assigned to each movie |
| `title` | Movie title |
| `genres` | Genre or combination of genres associated with the movie |

The first five rows observed in the notebook are:

| movie_id | title | genres |
|---:|---|---|
| 1 | Inception | Sci-Fi / Thriller |
| 2 | Interstellar | Sci-Fi / Drama |
| 3 | The Matrix | Sci-Fi / Action |
| 4 | Arrival | Sci-Fi / Drama |
| 5 | Edge of Tomorrow | Sci-Fi / Action |

The preview demonstrates that a movie can belong to more than one genre.

For example:

```text
Inception
→ Sci-Fi | Thriller
```

and:

```text
The Matrix
→ Sci-Fi | Action
```

The `genres` field is particularly important because genre information is used as the primary content feature in the recommendation model.

---

# 8. Ratings Dataset

The `ratings.csv` dataset contains:

```text
440 rows × 3 columns
```

The three variables are:

| Variable | Description |
|---|---|
| `user_id` | Unique identifier for the user who submitted the rating |
| `movie_id` | Identifier of the movie being rated |
| `rating` | Numerical rating assigned by the user |

The first five observations are:

| user_id | movie_id | rating |
|---:|---:|---:|
| 1 | 2 | 3.5 |
| 1 | 3 | 4.5 |
| 1 | 5 | 3.5 |
| 1 | 6 | 3.5 |
| 1 | 11 | 3.0 |

Each row represents an interaction between a user and a movie.

For example:

```text
User 1
   ↓
Movie ID 2
   ↓
Rating = 3.5
```

The recommendation analysis later filters this dataset specifically for:

```text
user_id = 4
```

---

# 9. Relationship Between the Datasets

The movie catalogue and ratings dataset are connected using:

```text
movie_id
```

Conceptually:

```text
ratings.csv
│
├── user_id
├── movie_id ──────────────┐
└── rating                 │
                           │
                           ▼
                     movies.csv
                           │
                           ├── movie_id
                           ├── title
                           └── genres
```

This allows a rating record to be associated with the corresponding movie title and genres.

For example, instead of analyzing only:

```text
User 4 → Movie ID 26 → Rating 4.5
```

the merge allows the system to analyze:

```text
User 4
   ↓
A Quiet Place
   ↓
Horror | Thriller
   ↓
Rating 4.5
```

This relationship is essential for constructing a meaningful user preference profile.

---

# 10. Missing-Value Analysis

Missing values were checked before performing the recommendation analysis.

Missing information could create several problems. For example:

- a missing `user_id` would prevent a rating from being associated with a user;
- a missing `movie_id` would prevent a rating from being linked to the movie catalogue;
- a missing `rating` would make the interaction unusable for preference analysis;
- a missing movie title would make recommendation output incomplete; and
- missing genre information could prevent content-based similarity calculation.

## Ratings Dataset Missing Values

The executed notebook produced:

| Column | Missing Values |
|---|---:|
| `user_id` | **0** |
| `movie_id` | **0** |
| `rating` | **0** |

Therefore, the ratings dataset contains:

```text
0 missing user IDs
0 missing movie IDs
0 missing ratings
```

All **440 rating records** contain the three pieces of information required for the rating analysis.

No missing-value imputation or removal was required for `ratings.csv`.

The notebook also performs the required missing-value inspection on `movies.csv`; its executed output should be consulted directly for the corresponding movie-data counts.

---

# 11. Task 1 Interpretation

The initial data inspection establishes that the project contains:

```text
36 catalogue movies
440 historical ratings
```

The movie dataset supplies the content information required for recommendation, particularly the genre labels.

The ratings dataset provides behavioral information describing which movies users rated and the scores they assigned.

The shared `movie_id` variable makes it possible to combine these two forms of information.

The ratings dataset is complete across `user_id`, `movie_id`, and `rating`, meaning no missing-value treatment is necessary for those variables.

This provides a suitable foundation for filtering and analyzing the assigned user.

---

# 12. Task 2 – Profile Assigned User 4

The second stage focuses specifically on the assigned user.

The ratings dataset is filtered using:

```text
user_id = 4
```

The resulting records are then merged with the movie catalogue using `movie_id`.

This produces a personalized rating profile containing:

```text
movie_id
title
genres
rating
```

for each movie rated by User 4.

---

# 13. User 4's Historical Ratings

User 4 has rated **13 movies** in the supplied data.

The rating history is:

| Movie | Genres | Rating |
|---|---|---:|
| **A Quiet Place** | Horror / Thriller | **4.5** |
| **Get Out** | Horror / Thriller | **3.5** |
| Inception | Sci-Fi / Thriller | 3.0 |
| John Wick | Action / Thriller | 3.0 |
| The Notebook | Romance / Drama | 3.0 |
| The Pursuit of Happyness | Drama / Biography | 3.0 |
| Toy Story | Animation / Comedy | 3.0 |
| Creed | Sports / Drama | 3.0 |
| Hidden Figures | Drama / Biography | 2.5 |
| Knives Out | Mystery / Comedy | 2.5 |
| Sherlock Holmes | Mystery / Action | 2.5 |
| The Hangover | Comedy | 2.0 |
| Jumanji: Welcome to the Jungle | Comedy / Adventure | 2.0 |

The ratings range from:

```text
2.0 to 4.5
```

within User 4's observed history.

---

# 14. Three Highest-Rated Movies

The three highest-rated movies for User 4 are:

| Rank | Movie | Genres | Rating |
|---:|---|---|---:|
| **1** | **A Quiet Place** | Horror / Thriller | **4.5** |
| **2** | **Get Out** | Horror / Thriller | **3.5** |
| **3** | **Inception** | Sci-Fi / Thriller | **3.0** |

These results provide an early indication of User 4's preferences.

Both:

```text
A Quiet Place
```

and:

```text
Get Out
```

contain:

```text
Horror | Thriller
```

while:

```text
Inception
```

also contains:

```text
Thriller
```

Therefore, Thriller appears across all three highest-rated movies, while Horror appears in the two highest-rated movies.

---

# 15. Genre Preference Analysis

To analyze the user's preferences more systematically, the genres associated with User 4's rated movies were separated into individual categories.

For each genre, the analysis calculates:

1. the number of movies rated; and
2. the average rating.

The results are:

| Genre | Movies Rated | Average Rating |
|---|---:|---:|
| **Horror** | **2** | **4.00** |
| **Thriller** | **4** | **3.50** |
| Animation | 1 | 3.00 |
| Romance | 1 | 3.00 |
| Sci-Fi | 1 | 3.00 |
| Sports | 1 | 3.00 |
| Drama | 4 | 2.88 |
| Action | 2 | 2.75 |
| Biography | 2 | 2.75 |
| Mystery | 2 | 2.50 |
| Comedy | 4 | 2.38 |
| Adventure | 1 | 2.00 |

---

# 16. Interpretation of User 4's Genre Preferences

The genre statistics suggest that **Horror is User 4's strongest apparent genre preference**.

Horror has:

```text
Movies rated = 2
Average rating = 4.00
```

This is the highest average rating among all genres represented in User 4's rating history.

Thriller also provides strong evidence of preference:

```text
Movies rated = 4
Average rating = 3.50
```

Thriller is particularly important because it appears in all three of User 4's highest-rated movies:

```text
A Quiet Place → Horror | Thriller → 4.5
Get Out       → Horror | Thriller → 3.5
Inception     → Sci-Fi | Thriller → 3.0
```

The analysis also demonstrates why frequency alone should not be used to infer preference.

For example:

```text
Thriller movies rated = 4
Comedy movies rated   = 4
```

but their average ratings differ considerably:

```text
Thriller average = 3.50
Comedy average   = 2.38
```

Therefore, the rating evidence indicates that **Horror and Thriller are more strongly preferred than Comedy**, even though Thriller and Comedy appear equally often.

Overall, the strongest observed genre signals for User 4 are:

> **Horror and Thriller**

---

# 17. Task 3 – Build the Recommendation Logic

The recommendation model uses **content-based filtering**.

Content-based filtering recommends items according to their characteristics and their similarity to items that a user has previously preferred.

For this project:

| Component | Implementation |
|---|---|
| User | User 4 |
| Items | Movies |
| Content Features | Genres |
| Positive Preference | Rating ≥ 4.0 |
| Vectorization | CountVectorizer |
| Similarity | Cosine Similarity |
| Candidate Set | Movies not already rated |
| Output | Top 5 recommendations |

The central recommendation question is:

> Which unseen movies have genre characteristics most similar to the movies that User 4 positively rated?

---

# 18. Positive Rating Threshold

The practical quiz defines a liked movie as a movie rated:

```text
4.0 or higher
```

Therefore:

```text
Positive Movie = Rating >= 4.0
```

Applying this rule to User 4 produces:

| Movie | Rating | Qualifies as Positive? |
|---|---:|---|
| A Quiet Place | **4.5** | ✅ Yes |
| Get Out | 3.5 | ❌ No |
| Inception | 3.0 | ❌ No |

Therefore, only one movie qualifies:

> **A Quiet Place**

This distinction is important.

Although *Get Out* is User 4's second-highest-rated movie, its rating of **3.5** does not meet the required threshold and is therefore not used as positive preference evidence in the recommendation algorithm.

---

# 19. Positive Preference Profile

The only positively rated movie is:

```text
A Quiet Place
```

with:

```text
Rating = 4.5
Genres = Horror | Thriller
```

Therefore:

```text
A Quiet Place
      │
      ├── Horror
      └── Thriller
```

forms the positive content evidence used by the recommender.

As a result, candidate movies containing Horror or Thriller are expected to receive positive similarity scores.

---

# 20. Genre Feature Engineering

The movie genres are originally represented as text.

Examples include:

```text
Horror|Thriller
Action|Thriller
Sci-Fi|Drama
Action|Superhero
```

Machine-learning similarity calculations require numerical features.

Therefore, the genres are transformed using:

> **CountVectorizer**

from `scikit-learn`.

Before vectorization, the pipe separator is converted into a space.

For example:

```text
Horror|Thriller
```

becomes:

```text
Horror Thriller
```

This allows the vectorizer to interpret each genre as a separate feature.

---

# 21. Conceptual Genre Representation

After vectorization, movie genres can conceptually be represented as:

| Movie | Horror | Thriller | Action | Sci-Fi | Drama | Superhero |
|---|---:|---:|---:|---:|---:|---:|
| A Quiet Place | 1 | 1 | 0 | 0 | 0 | 0 |
| The Conjuring | 1 | 0 | 0 | 0 | 0 | 0 |
| Mission: Impossible - Fallout | 0 | 1 | 1 | 0 | 0 | 0 |
| Arrival | 0 | 0 | 0 | 1 | 1 | 0 |
| Avengers: Endgame | 0 | 0 | 1 | 0 | 0 | 1 |

This allows textual movie characteristics to be compared mathematically.

---

# 22. Constructing the User Preference Vector

The vectors corresponding to positively rated movies are used to construct a user preference representation.

If User 4 had several movies rated 4.0 or above, the genre information from those movies could collectively contribute to the profile.

However, User 4 has only one qualifying movie:

```text
A Quiet Place
```

Therefore, the user's positive preference vector is primarily based on:

```text
Horror
Thriller
```

This is an important characteristic of the current model because it directly affects the final similarity scores.

---

# 23. Cosine Similarity

The project uses **cosine similarity** to compare the user preference vector with the genre representation of each movie.

Conceptually:

```text
                         A · B
Cosine Similarity = ───────────────
                      ||A|| × ||B||
```

where:

- `A` is the User 4 preference vector;
- `B` is a candidate movie vector;
- `A · B` is their dot product;
- `||A||` is the magnitude of the preference vector; and
- `||B||` is the magnitude of the candidate movie vector.

The interpretation is:

```text
Higher cosine similarity
           ↓
Greater genre overlap
           ↓
Stronger content-based match
```

A score of zero indicates that the candidate movie does not share any genre features with the current positive preference representation.

---

# 24. Excluding Previously Rated Movies

A recommendation system should not recommend a movie that the user has already rated when the task specifically requires unseen recommendations.

Therefore, all `movie_id` values present in User 4's rating history are removed from the candidate set.

Conceptually:

```text
Movie Catalogue
       │
       ├── Already rated by User 4
       │              │
       │              └── EXCLUDE
       │
       └── Not rated by User 4
                      │
                      └── KEEP AS CANDIDATE
```

This ensures that all five final recommendations are unseen according to the supplied rating history.

---

# 25. Candidate Ranking

After similarity calculation and removal of previously rated movies, the candidate movies are ranked.

The ranking procedure is:

```text
Candidate movies
       ↓
Cosine similarity scores
       ↓
Sort from highest to lowest
       ↓
Select first five
       ↓
Final Recommendations
```

Exactly five movies are returned.

---

# 26. Task 4 – Final Recommendation Results

The content-based recommender generated the following results:

| Rank | Recommended Movie | Genres | Cosine Similarity |
|---:|---|---|---:|
| **1** | **The Conjuring** | Horror | **0.707107** |
| **2** | **Mission: Impossible - Fallout** | Action / Thriller | **0.500000** |
| **3** | **Arrival** | Sci-Fi / Drama | **0.000000** |
| **4** | **Avengers: Endgame** | Action / Superhero | **0.000000** |
| **5** | **Black Panther** | Action / Superhero | **0.000000** |

The system therefore returns exactly five recommendations as required.

---

# 27. Recommendation 1 – The Conjuring

### Genres

```text
Horror
```

### Cosine Similarity

```text
0.707107
```

*The Conjuring* is the strongest recommendation generated for User 4.

User 4's positive preference movie is:

```text
A Quiet Place → Horror | Thriller
```

while:

```text
The Conjuring → Horror
```

The two movies therefore share the **Horror** genre.

This explains the positive cosine similarity score.

The recommendation is also supported by the broader genre analysis because Horror has User 4's highest average genre rating:

```text
Average Horror rating = 4.00
```

Therefore, *The Conjuring* represents a strong and interpretable genre-based recommendation.

---

# 28. Recommendation 2 – Mission: Impossible - Fallout

### Genres

```text
Action | Thriller
```

### Cosine Similarity

```text
0.500000
```

*Mission: Impossible - Fallout* shares the **Thriller** genre with *A Quiet Place*.

The user's broader history also supports Thriller as an important preference:

```text
Thriller movies rated = 4
Average Thriller rating = 3.50
```

Therefore, the recommendation has both:

1. direct similarity with the positive preference vector; and
2. supporting evidence from User 4's broader rating history.

---

# 29. Recommendation 3 – Arrival

### Genres

```text
Sci-Fi | Drama
```

### Cosine Similarity

```text
0.000000
```

*Arrival* has no direct Horror or Thriller overlap with the positive preference profile.

Therefore, it receives:

```text
Cosine Similarity = 0
```

Its inclusion should not be interpreted as evidence of a strong content match.

Instead, after the positive-similarity candidates are ranked, it appears among the highest-ranked remaining eligible unseen movies under the implemented sorting procedure.

This result highlights the limitation of constructing a preference profile from only one positive movie.

---

# 30. Recommendation 4 – Avengers: Endgame

### Genres

```text
Action | Superhero
```

### Cosine Similarity

```text
0.000000
```

*Avengers: Endgame* does not contain either Horror or Thriller.

Therefore, there is no direct overlap with the positive user profile.

It remains an eligible candidate because User 4 has not previously rated it.

---

# 31. Recommendation 5 – Black Panther

### Genres

```text
Action | Superhero
```

### Cosine Similarity

```text
0.000000
```

*Black Panther* also has no direct genre overlap with the current positive preference representation.

Its similarity score is therefore zero.

It remains eligible because it is not among User 4's previously rated movies.

---

# 32. Overall Recommendation Interpretation

The recommendation results demonstrate how the content-based model behaves when positive preference information is limited.

The strongest recommendation is:

```text
The Conjuring
      │
      └── Horror overlap
              │
              ▼
       Similarity 0.707107
```

The second strongest is:

```text
Mission: Impossible - Fallout
      │
      └── Thriller overlap
              │
              ▼
       Similarity 0.500000
```

These two movies have direct genre overlap with the positive preference movie.

The remaining three recommendations have zero similarity because they do not contain either Horror or Thriller.

This does not necessarily mean User 4 would dislike those movies.

Instead, it means the current content-based model does not have sufficient positive genre evidence to distinguish strongly among those unseen candidates.

---

# 33. Requirements Verification

| Quiz Requirement | Implementation | Status |
|---|---|---|
| Load both datasets | pandas `read_csv()` | ✅ |
| Display first five rows | Both datasets inspected | ✅ |
| Show dataset dimensions | 36×3 and 440×3 | ✅ |
| Check missing values | Performed in notebook | ✅ |
| Analyze assigned user | User 4 | ✅ |
| List user's rated movies | 13 movies identified | ✅ |
| Three highest-rated movies | Identified | ✅ |
| Preferred genres | Horror and Thriller | ✅ |
| Positive threshold | Rating ≥ 4.0 | ✅ |
| Genre vectorization | CountVectorizer | ✅ |
| Similarity method | Cosine similarity | ✅ |
| Exclude rated movies | Applied | ✅ |
| Return exactly five movies | 5 returned | ✅ |
| Explain recommendations | Included | ✅ |
| Identify limitation | Included | ✅ |
| Suggest improvement | Included | ✅ |

---

# 34. Task 5 – Limitation

The main limitation of the recommender is that it relies exclusively on **movie genres** and only uses movies rated **4.0 or higher** as positive preference evidence.

User 4 has rated 13 movies, but only:

```text
A Quiet Place
Rating = 4.5
```

satisfies the required positive threshold.

Consequently, the positive preference profile is based primarily on:

```text
Horror
Thriller
```

This creates a narrow representation of the user's interests.

A candidate movie that contains neither Horror nor Thriller receives zero similarity even though the user could potentially enjoy it.

The recommender also ignores other potentially useful information such as:

- ratings from other users;
- actors;
- directors;
- plot descriptions;
- keywords;
- movie tags;
- release year;
- movie popularity; and
- richer patterns from the user's complete rating history.

---

# 35. Sparse Preference Information

The current results demonstrate a sparse-preference problem.

Although:

```text
User 4 has 13 ratings
```

only:

```text
1 movie
```

qualifies as positive according to the required threshold.

Therefore:

```text
13 historical ratings
        ↓
1 positive movie
        ↓
2 main positive genres
        ↓
Horror + Thriller
```

This limited preference information contributes to three of the final five recommendations receiving zero similarity.

---

# 36. Suggested Improvement

A realistic improvement would be to develop a **hybrid recommender system**.

The hybrid approach could combine:

```text
Content-Based Filtering
          +
Collaborative Filtering
```

The existing content-based component could continue using movie characteristics such as genres.

Collaborative filtering could use the ratings of other users to identify users whose rating patterns are similar to User 4.

Movies that similar users rated highly could then provide additional recommendation evidence.

---

# 37. How Collaborative Filtering Could Help

Conceptually:

```text
User 4
   │
   │ Similar rating behavior
   ▼
Similar Users
   │
   │ Movies they rated highly
   ▼
Unseen Movies
   │
   ▼
Additional Recommendation Evidence
```

This would allow the recommender to identify potentially relevant movies even when those movies do not directly share Horror or Thriller with *A Quiet Place*.

---

# 38. Benefits of a Hybrid Approach

A hybrid model could:

- reduce dependence on a single positively rated movie;
- use information from similar users;
- discover preferences beyond explicit genre overlap;
- improve ranking among zero-similarity candidates;
- increase recommendation diversity;
- combine item characteristics with user behavior; and
- potentially improve overall personalization.

---

# 39. Additional Possible Improvements

Several additional improvements could be explored.

## 39.1 Rating-Weighted User Profiles

Instead of treating movies simply as:

```text
positive / not positive
```

the model could use the actual rating values as weights.

For example:

```text
4.5 → very strong positive preference
4.0 → strong positive preference
3.0 → moderate preference
2.0 → weak preference
```

This would allow more of the user's rating history to contribute to the preference profile.

---

## 39.2 TF-IDF

The current system uses:

```text
CountVectorizer
```

An alternative could be:

```text
TF-IDF
```

which could change the relative importance of common and less-common genre features.

---

## 39.3 Richer Movie Metadata

The content-based representation could be expanded beyond genres.

Potential features include:

- actors;
- directors;
- plot summaries;
- keywords;
- tags;
- release year; and
- other descriptive metadata.

This could identify similarities between movies that are not captured by genre alone.

---

## 39.4 Collaborative Filtering

Future versions could use:

- user-user collaborative filtering;
- item-item collaborative filtering;
- matrix factorization; or
- latent-factor recommendation methods.

These approaches could learn relationships from the complete user-item rating matrix.

---

# 40. Technologies Used

| Technology | Purpose |
|---|---|
| **Python 3** | Main programming language |
| **Jupyter Notebook** | Interactive analysis environment |
| **VS Code** | Notebook and code development |
| **pandas** | Data loading, filtering, merging, grouping, and analysis |
| **NumPy** | Numerical array operations |
| **scikit-learn** | Feature vectorization and similarity calculation |
| **CountVectorizer** | Converts genres into numerical features |
| **cosine_similarity** | Measures similarity |
| **Git** | Version control |
| **GitHub** | Repository hosting and submission |

---

# 41. Main Python Libraries

The implementation uses:

```python
import pandas as pd
import numpy as np

from sklearn.feature_extraction.text import CountVectorizer
from sklearn.metrics.pairwise import cosine_similarity
```

## pandas

Used for:

- loading CSV files;
- inspecting datasets;
- filtering User 4's ratings;
- merging the movie and ratings datasets;
- sorting ratings;
- grouping genre information;
- calculating genre-level statistics;
- excluding already-rated movies; and
- formatting recommendation results.

## NumPy

Used for numerical array operations and for converting the user preference representation into a suitable format for similarity calculations.

## CountVectorizer

Used to transform genre text into numerical movie features.

## cosine_similarity

Used to compare candidate movie features against the user preference representation.

---

# 42. Software Requirements

The project requires:

```text
Python 3.x
pandas
numpy
scikit-learn
jupyter
```

The dependencies can be installed using:

```bash
pip install pandas numpy scikit-learn jupyter
```

---

# 43. How to Run the Project

## Step 1 – Clone the Repository

```bash
git clone https://github.com/abdiqalaq/DSA4060-Recommender-671243.git
```

Enter the repository:

```bash
cd DSA4060-Recommender-671243
```

---

## Step 2 – Verify the Required Files

The directory should contain:

```text
README.md
DSA4060_Recommender_671243.ipynb
movies.csv
ratings.csv
```

---

## Step 3 – Install Dependencies

If required:

```bash
pip install pandas numpy scikit-learn jupyter
```

---

## Step 4 – Launch the Notebook

Run:

```bash
jupyter notebook
```

Then open:

```text
DSA4060_Recommender_671243.ipynb
```

Alternatively, open the notebook directly in VS Code with Jupyter support.

---

## Step 5 – Execute All Cells

Run all notebook cells from top to bottom.

The notebook will:

1. import the required libraries;
2. load `movies.csv`;
3. load `ratings.csv`;
4. display the first five rows;
5. inspect dataset dimensions;
6. check missing values;
7. filter ratings for User 4;
8. merge ratings with movie information;
9. display all movies rated by User 4;
10. identify the user's three highest-rated movies;
11. calculate genre-level statistics;
12. interpret the user's apparent genre preferences;
13. identify movies rated 4.0 or higher;
14. prepare genre text;
15. apply CountVectorizer;
16. construct the user preference vector;
17. calculate cosine similarity;
18. exclude previously rated movies;
19. rank unseen movies;
20. select exactly five recommendations; and
21. display explanations for the recommendations.

---

# 44. Reproducibility

For consistent results:

- use the supplied `movies.csv`;
- use the supplied `ratings.csv`;
- keep both datasets accessible to the notebook;
- use User ID **4**;
- maintain the required positive threshold of **4.0**;
- execute notebook cells in order; and
- do not alter the supplied ratings or movie information.

The recommendation results documented in this README were generated from User 4's supplied rating history.

---

# 45. Expected Final Output

The final recommendation output is:

```text
1. The Conjuring
2. Mission: Impossible - Fallout
3. Arrival
4. Avengers: Endgame
5. Black Panther
```

with:

| Movie | Cosine Similarity |
|---|---:|
| The Conjuring | **0.707107** |
| Mission: Impossible - Fallout | **0.500000** |
| Arrival | 0.000000 |
| Avengers: Endgame | 0.000000 |
| Black Panther | 0.000000 |

---

# 46. Key Findings

## Finding 1 – Dataset Size

```text
Movies = 36
Ratings = 440
```

## Finding 2 – User 4 Has 13 Historical Ratings

The user's rating history contains 13 movies.

## Finding 3 – Horror Has the Highest Average Rating

```text
Horror average rating = 4.00
```

## Finding 4 – Thriller Has Strong Supporting Evidence

```text
Thriller movies rated = 4
Thriller average rating = 3.50
```

## Finding 5 – A Quiet Place Is the Strongest Positive Signal

```text
A Quiet Place
Rating = 4.5
Genres = Horror | Thriller
```

## Finding 6 – Only One Movie Meets the Positive Threshold

```text
Required threshold = Rating >= 4.0
```

Therefore:

```text
Positive movies = 1
```

## Finding 7 – The Conjuring Is the Strongest Recommendation

```text
Similarity = 0.707107
```

because it shares Horror with the positive preference profile.

## Finding 8 – Mission: Impossible - Fallout Is Second

```text
Similarity = 0.500000
```

because it shares Thriller with the positive preference profile.

## Finding 9 – Sparse Positive Information Affects the Ranking

Three recommendations receive zero similarity because the positive profile contains only Horror and Thriller evidence.

---

# 47. Conclusion

This project successfully develops a personalized **content-based movie recommendation system** for User 4.

The analysis begins with a movie catalogue containing **36 movies** and a ratings dataset containing **440 historical rating records**.

The movie dataset provides titles and genre information, while the ratings dataset provides historical user behavior. The two datasets are connected through `movie_id`.

The ratings dataset contains no missing values in `user_id`, `movie_id`, or `rating`, meaning all 440 rating records contain the essential information required for the recommendation analysis.

User 4 has **13 historical movie ratings**.

The three highest-rated movies are:

1. *A Quiet Place* – 4.5
2. *Get Out* – 3.5
3. *Inception* – 3.0

Genre-level analysis identifies **Horror** as the genre with the highest average rating at **4.00**.

**Thriller** also provides strong preference evidence because it appears in four rated movies with an average rating of **3.50**.

The user's strongest individual preference is *A Quiet Place*, rated **4.5**, which belongs to both Horror and Thriller.

Following the practical quiz requirement, only movies rated at least **4.0** are treated as positive preferences.

Therefore, *A Quiet Place* is the only movie used to construct the positive user preference profile.

The movie genres are transformed into numerical features using **CountVectorizer**.

A user preference vector is constructed from the positively rated movie, and **cosine similarity** is used to compare this profile against the movie catalogue.

Movies that User 4 has already rated are removed before ranking.

The resulting recommendations are:

1. **The Conjuring**
2. **Mission: Impossible - Fallout**
3. **Arrival**
4. **Avengers: Endgame**
5. **Black Panther**

*The Conjuring* is the strongest recommendation with a cosine similarity score of **0.707107** because it shares Horror with User 4's positive preference.

*Mission: Impossible - Fallout* is the second-strongest recommendation with a similarity score of **0.500000** because it shares Thriller.

The remaining three movies receive zero similarity, highlighting an important limitation of the current recommender: only one movie meets the positive-rating threshold, resulting in a narrow positive preference profile.

A future version could improve personalization by combining the current genre-based approach with **collaborative filtering**, creating a hybrid recommender that considers both movie characteristics and the behavior of similar users.

Overall, the project demonstrates the complete recommender-system workflow:

```text
Data Loading
     ↓
Data Inspection
     ↓
User Profiling
     ↓
Preference Analysis
     ↓
Positive Movie Selection
     ↓
Genre Vectorization
     ↓
User Profile Construction
     ↓
Cosine Similarity
     ↓
Candidate Filtering
     ↓
Recommendation Ranking
     ↓
Recommendation Explanation
     ↓
Critical Evaluation
```

---



---

# Author

**Abdiqalaq Issack**  
**Student ID:** 671243  
**Assigned User ID:** 4  
**Course:** DSA 4060 – Recommender Systems

---

# Submission Repository

```text
https://github.com/abdiqalaq/DSA4060-Recommender-671243
```
