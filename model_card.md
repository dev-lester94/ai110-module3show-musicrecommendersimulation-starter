# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name  

Les Find Music Finder 1.0

---

## 2. Intended Use  

Describe what your recommender is designed to do and who it is for. 

It recommends songs based off exact genre and mood, and also energy level and acoustiness of user's preferences.
It assumes that the user has already set their preference upon rumnning the app.
This is not design for real users. It is more for classroom exploration.

---

## 3. How the Model Works  

Explain your scoring approach in simple language.  

The features we use for the songs are genre, mood, energy, and accoustiness. 
The features we use for user preferences are liked_genre, liked_mood, energy level, and liked_accoustiness. 
For scoring, we score a song out of 100 points with 4 criterias. 
  It compares the exact match of the users liked_genre to the songs genre.
  It compares the exact match of the users liked_mood to the songs mood
  It compares the energy of the song to the users energy level by taking the absolute difference of the two. The smallest difference of <= 0.1 gives the maximum points.
  It compares the accoustiness of a song to the users liked_accoustiness. Depending on the users liked_accoustiness boolean value we calculate the score differently.
After scoring, we get the top 5 songs with the highest score and present them in decending order

---

## 4. Data  

Describe the dataset the model uses.  

There are a little over 20 songs in the catalog.
Genres included are rock, pop, folk pop, and etc
I did not remove any of the the orginal songs given but added > 10 more songs later.
I would say their are genres like classical that are missing in the data

---

## 5. Strengths  

Where does your system seem to work well  

In my default scoring mechanism, matching genre makes a big impact of whether a song is recommended. 
So a user type where genre match and close energy and accuostiness but not mood can give a reasonable result.
Inversely, if song genre does not match it will score low and not get recommended
I knew genre would make a huge impact in getting songs recommended. But I was really surprise it beated songs by a landslide.


---

## 6. Limitations and Bias 

Where the system struggles or behaves unfairly. 

Assuming the test was done with my default scoring mechanism,
Genre again is very impactful.
Between genre and mood, mood is definitely underrepresented.
A perfect score in energy can still be beaten by a song who genre match but the energy does not match.
If the genre matches and there is one song for that genre then that song will get recommended always.


---

## 7. Evaluation  

How you checked whether the recommender behaved as expected. 

Prompts:  

- Which user profiles you tested  
- What you looked for in the recommendations  
- What surprised you  
- Any simple tests or comparisons you ran  

Tested profile with default scoring mechanism:
 Genre-vs-mood conflict
     Genre therotically beats mood. The ceiling for genre vs mood (75 vs 65)
  Near-miss compound genre user_prefs
    Result:
  Self-contradictory taste user_prefs 
    Genre  was so strong that songs with matching genre trumps 


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
