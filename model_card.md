# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name  

Give your model a short, descriptive name.  
Example: **VibeFinder 1.0**  

VibeFinder 1.0


---

## 2. Intended Use  

Describe what your recommender is designed to do and who it is for. 

Prompts:  

- What kind of recommendations does it generate  
- What assumptions does it make about the user  
- Is this for real users or classroom exploration  

**Intended Use**
- Recommend music based on user preferences.
- Help users discover songs similar to their tastes.

**Non-Intended Use**
- Predict a person's personality or emotions.
- Make important decisions about people.
- Recommend songs that are not in the dataset.

This recommender suggests songs that best match a user's music preferences. It ranks songs based on how similar they are to the user's favorite genre, mood, and audio features.


---

## 3. How the Model Works  

Explain your scoring approach in simple language.  

Prompts:  

- What features of each song are used (genre, energy, mood, etc.)  
- What user preferences are considered  
- How does the model turn those into a score  
- What changes did you make from the starter logic  

Avoid code here. Pretend you are explaining the idea to a friend who does not program.

The dataset contains songs with metadata and audio features. Metadata includes the title, artist, genre, and mood. Audio features include energy, valence, danceability, acousticness, and tempo. The recommender can only suggest songs that are already in the dataset.

---

## 4. Data  

Describe the dataset the model uses.  

Prompts:  

- How many songs are in the catalog  
- What genres or moods are represented  
- Did you add or remove data  
- Are there parts of musical taste missing in the dataset  

The recommender compares each song to the user's profile. Songs receive points for matching the user's preferred genre and mood. It also compares numerical audio features like energy and acousticness. Songs with the highest total scores are recommended first.

---

## 5. Strengths  

Where does your system seem to work well  

Prompts:  

- User types for which it gives reasonable results  
- Any patterns you think your scoring captures correctly  
- Cases where the recommendations matched your intuition  

The recommender works best when the user's favorite genre exists in the dataset. Because genre and mood require exact matches, it may miss songs from similar genres. It also cannot recommend new or missing songs that are not included in the dataset.


---

## 6. Limitations and Bias 

Where the system struggles or behaves unfairly. 

Prompts:  

- Features it does not consider  
- Genres or moods that are underrepresented  
- Cases where the system overfits to one preference  
- Ways the scoring might unintentionally favor some users  

The recommender works best when the user's favorite genre exists in the dataset. Because genre and mood require exact matches, it may miss songs from similar genres. It also cannot recommend new or missing songs that are not included in the dataset.


---

## 7. Evaluation  

How you checked whether the recommender behaved as expected. 

Prompts:  

- Which user profiles you tested  
- What you looked for in the recommendations  
- What surprised you  
- Any simple tests or comparisons you ran  

No need for numeric metrics unless you created some.

I tested the recommender with different user profiles and compared the recommended songs to the users' preferences. I also changed individual preferences, such as genre and energy, to make sure the recommendations changed as expected.


---

## 8. Future Work  

Ideas for how you would improve the model next.  

Prompts:  

- Additional features or preferences  
- Better ways to explain recommendations  
- Improving diversity among the top results  
- Handling more complex user tastes  

## Ideas for Improvement
- Support similar genres instead of only exact genre matches.
- Learn from user feedback to improve future recommendations.
- Use a larger and more diverse music dataset.
---

## 9. Personal Reflection  

A few sentences about your experience.  

Prompts:  

- What you learned about recommender systems  
- Something unexpected or interesting you discovered  
- How this changed the way you think about music recommendation apps  
