# 🎵 Music Recommender Simulation

## Project Summary

In this project you will build and explain a small music recommender system.

Your goal is to:

- Represent songs and a user "taste profile" as data
- Design a scoring rule that turns that data into recommendations
- Evaluate what your system gets right and wrong
- Reflect on how this mirrors real world AI recommenders

Replace this paragraph with your own summary of what your version does.

---

## How The System Works

Major streaming platforms like Spotify uses a combination of collaborative filtering and content based-filtering to recommend songs to users. Collaborative filtering recommends songs based across many user's behavior such as if a song is being listened by millions of viewers then it would recommended to other users like you. Content-base filtering compares the attributes of a song (i.e. genre) and compares that to the user profiles of what they like. In this project, we will be focusing on purely content-based filtering. We do not have a notion of other users. And are given classes and methods that describes a song and user preferences and scoring them.

Song class can be broken down into three categories. Energy, tempo_bpm, valence, danceability, acousticness are continuous audio features where they are numerical values that represent measureable characteristics of a song. Genre, mood, artist are categorical tags where they label a song for classification and are not numerical. Id and title are metadata to help identify a song.

The UserProfile class stores the user's music preferences, such as favorite_genre, favorite_mood, target_energy, and likes_acoustic. These preferences may be collected explicitly during onboarding or inferred from the user's listening history by analyzing and averaging the features of songs they have liked. This profile is used to recommend songs that best match the user's tastes.

The Recommender computes a score of a song if it matches the user profile preference. In my first iteration of the recommender, I'll first look at genre preference and giving a score of +35 if match and +0 if it does not. For mood, it is very similar but I'll give +25 if match and +0 if not. I choose to do this because I feel genre helps narrow down the musical style than mood does as it is broader. For target energy I would have to take the absolute value and difference of it and the song and the more smaller difference the more points we give out. The max score is +20 and goes down to 0.

For target energy score:
diff <= 0.1 -> 20
diff <= 0.2 -> 15
diff <= 0.3 -> 10
diff <= 0.4 -> 5
diff <= 0.5 -> 0

The last preference like_acoustic gets match with the song accoustiness value. We will always compute a score of +20 and goes down to 0 but the score will be computed differently if the like_acoustic is true or false.

For like_acoustic score if true:
song.acousticness * 20
Acoustiness value | Score
1.0	| 20
0.8	| 16
0.5	| 10
0.2	| 4
0.0	| 0

For like_acoustic score if false:
(1 - song.acousticness) * 20
Acoustiness value | Score
0.0 |	20
0.2	| 16
0.5	| 10
0.8	| 4
1.0	| 0

After everything is calculated, the song will have a final score out of 100. The recommender will then compare each of the song's score and will recommend the top k songs with the highest scores and sorted in decending order. 

Again the potential bias for our recommendation system is that it priortizes genre over mood. Sometimes a user would wants to listen to an indie pop happy song but it can be outranked by a rock intense  song since rock is the genre they liked. Another bias is genre and mood are exact matching. If a user likes pop and a song is folk pop the folk pop will get outranked by the pop song since folk pop does not exactly match with pop. And akso some inconsistency on scoring such as if genre and mood does not exactly match it goes to 0 while target_energy and liked_acoustiness drops gradually.

### Data Flow

See [diagram.md](diagram.md) for a Mermaid diagram of the data flow (Input → Process → Output).

---

## Getting Started

### Setup

1. Create a virtual environment (optional but recommended):

   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Mac or Linux
   .venv\Scripts\activate         # Windows

2. Install dependencies

```bash
pip install -r requirements.txt
```

3. Run the app:

```bash
python -m src.main
```

### Running Tests

Run the starter tests with:

```bash
pytest
```

You can add more tests in `tests/test_recommender.py`.

---

## Sample Recommendation Output

Paste a sample of your recommender's output here as a text block so a reader can see what it produces:

```
# e.g.:
    # Starter example profile.
    # Keys match the UserProfile schema: favorite_genre, favorite_mood,
    # target_energy, likes_acoustic.
    user_prefs = {
        "favorite_genre": "pop",
        "favorite_mood": "happy",
        "target_energy": 0.8,
        "likes_acoustic": False,
    }

Sunrise City - Score: 96.40
Because: Genre matches your favorite (pop); Mood matches your favorite (happy); Energy (0.82) is close to your target (0.80); Matches your preference for non-acoustic tracks

Gym Hero - Score: 69.00
Because: Genre matches your favorite (pop); Energy (0.93) is close to your target (0.80); Matches your preference for non-acoustic tracks

Rooftop Lights - Score: 58.00
Because: Mood matches your favorite (happy); Energy (0.76) is close to your target (0.80); Matches your preference for non-acoustic tracks

Honeyed Strings - Score: 48.40
Because: Mood matches your favorite (happy); Energy (0.63) is close to your target (0.80); Matches your preference for non-acoustic tracks

Tambourine Heart - Score: 45.60
Because: Mood matches your favorite (happy); Energy (0.62) is close to your target (0.80); Matches your preference for non-acoustic tracks
```

**Screenshot or video** *(optional)*: <!-- Insert a screenshot or demo video link here -->

---

## Experiments You Tried

Use this section to document the experiments you ran. For example:

- What happened when you changed the weight on genre from 2.0 to 0.5
- What happened when you added tempo or valence to the score
- How did your system behave for different types of users

---

## Limitations and Risks

Summarize some limitations of your recommender.

Examples:

- It only works on a tiny catalog
- It does not understand lyrics or language
- It might over favor one genre or mood

You will go deeper on this in your model card.

---

## Reflection

Read and complete `model_card.md`:

[**Model Card**](model_card.md)

Write 1 to 2 paragraphs here about what you learned:

- about how recommenders turn data into predictions
- about where bias or unfairness could show up in systems like this



