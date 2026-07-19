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


Stress Test with Diverse Profiles
# 1. Genre-vs-mood conflict
{"favorite_genre": "rock", "favorite_mood": "happy", "target_energy": 0.9, "likes_acoustic": False}

Top recommendations:

Storm Runner - Score: 73.00
Because: Genre matches your favorite (rock); Energy (0.91) is close to your target (0.90); Matches your preference for non-acoustic tracks

Sunrise City - Score: 61.40
Because: Mood matches your favorite (happy); Energy (0.82) is close to your target (0.90); Matches your preference for non-acoustic tracks

Rooftop Lights - Score: 53.00
Because: Mood matches your favorite (happy); Energy (0.76) is close to your target (0.90); Matches your preference for non-acoustic tracks

Honeyed Strings - Score: 43.40
Because: Mood matches your favorite (happy); Energy (0.63) is close to your target (0.90); Matches your preference for non-acoustic tracks

Tambourine Heart - Score: 40.60
Because: Mood matches your favorite (happy); Energy (0.62) is close to your target (0.90); Matches your preference for non-acoustic track


# 2. Near-miss compound genre
{"favorite_genre": "pop", "favorite_mood": "happy", "target_energy": 0.6, "likes_acoustic": True}

Top recommendations:

Sunrise City - Score: 73.60
Because: Genre matches your favorite (pop); Mood matches your favorite (happy); Energy (0.82) is close to your target (0.60); Matches your preference for acoustic tracks

Tambourine Heart - Score: 59.40
Because: Mood matches your favorite (happy); Energy (0.62) is close to your target (0.60); Matches your preference for acoustic tracks

Sunlit Porch - Score: 58.60
Because: Mood matches your favorite (happy); Energy (0.58) is close to your target (0.60); Matches your preference for acoustic tracks

Copper Skies - Score: 58.00
Because: Mood matches your favorite (happy); Energy (0.60) is close to your target (0.60); Matches your preference for acoustic tracks

Honeyed Strings - Score: 56.60
Because: Mood matches your favorite (happy); Energy (0.63) is close to your target (0.60); Matches your preference for acoustic tracks


# 4. Self-contradictory taste
{"favorite_genre": "rock", "favorite_mood": "intense", "target_energy": 0.9, "likes_acoustic": True}

Storm Runner - Score: 82.00
Because: Genre matches your favorite (rock); Mood matches your favorite (intense); Energy (0.91) is close to your target (0.90); Matches your preference for acoustic tracks

Gym Hero - Score: 46.00
Because: Mood matches your favorite (intense); Energy (0.93) is close to your target (0.90); Matches your preference for acoustic tracks

Tambourine Heart - Score: 24.40
Because: Energy (0.62) is close to your target (0.90); Matches your preference for acoustic tracks

Sunrise City - Score: 23.60
Because: Energy (0.82) is close to your target (0.90); Matches your preference for acoustic tracks

Rooftop Lights - Score: 22.00
Because: Energy (0.76) is close to your target (0.90); Matches your preference for acoustic tracks

Three takeaways from doing the stress test.

Genre is structually undefeatable. If we compare the celings for genre vs mood, the maximum score is 75 (genre match / mood mismatch) vs 65 (mood match / genre not match). Since 75 > 65, a genre-only match can never lose to a mood-only match, no matter how perfectly the mood song nails energy and acoustic. Test 1 confirms this isn't theoretical — Storm Runner is intense while the user wants happy (the literal opposite mood), and it still wins by 11.6 points over the genuinely happy Sunrise City. 

Genre can beat a perfect score on another dimension entirely. Looking at Test 2 more closely, User preference has a target_energy=0.6 and Copper Skies has its own energy=0.60. That's a perfect, zero-distance energy match (+20, the max). It still loses to Sunrise City by 15.6 points (58.00 vs 73.60), because Sunrise City's exact "pop" genre match (+35) outweighs Copper Skies' perfect energy fit combined with its near-miss "acoustic pop" genre (+0). 

Storm Runner is the only rock song in the entire catalog. Any profile with favorite_genre = "rock" will recommend Storm Runner on top due to the genre category being weighted too strong. I should definitely add more rock songs next time.

```

**Screenshot or video** *(optional)*: <!-- Insert a screenshot or demo video link here -->

---

## Experiments You Tried

In my experiment, I made the genre give a maximum of +15 points and energy to a max of +40 points and gradually go down by 10 by a difference of 0.1 still.

user_prefs = {
    "favorite_genre": "pop",
    "favorite_mood": "happy",
    "target_energy": 0.8,
    "likes_acoustic": False,
}

Top recommendations:

Sunrise City - Score: 96.40
Because: Genre matches your favorite (pop); Mood matches your favorite (happy); Energy (0.82) is close to your target (0.80); Matches your preference for non-acoustic tracks

Rooftop Lights - Score: 78.00
Because: Mood matches your favorite (happy); Energy (0.76) is close to your target (0.80); Matches your preference for non-acoustic tracks

Gym Hero - Score: 64.00
Because: Genre matches your favorite (pop); Energy (0.93) is close to your target (0.80); Matches your preference for non-acoustic tracks

Honeyed Strings - Score: 63.40
Because: Mood matches your favorite (happy); Energy (0.63) is close to your target (0.80); Matches your preference for non-acoustic tracks

Tambourine Heart - Score: 60.60
Because: Mood matches your favorite (happy); Energy (0.62) is close to your target (0.80); Matches your preference for non-acoustic tracks

#Stress Test with Diverse Profiles
#1. Genre-vs-mood conflict
user_prefs = {
    "favorite_genre": "rock",
    "favorite_mood": "happy",
    "target_energy": 0.9,
    "likes_acoustic": False
}

Top recommendations:

Sunrise City - Score: 81.40
Because: Mood matches your favorite (happy); Energy (0.82) is close to your target (0.90); Matches your preference for non-acoustic tracks

Storm Runner - Score: 73.00
Because: Genre matches your favorite (rock); Energy (0.91) is close to your target (0.90); Matches your preference for non-acoustic tracks

Rooftop Lights - Score: 68.00
Because: Mood matches your favorite (happy); Energy (0.76) is close to your target (0.90); Matches your preference for non-acoustic tracks

Gym Hero - Score: 59.00
Because: Energy (0.93) is close to your target (0.90); Matches your preference for non-acoustic tracks

Honeyed Strings - Score: 53.40
Because: Mood matches your favorite (happy); Energy (0.63) is close to your target (0.90); Matches your preference for non-acoustic tracks

#2. Near-miss compound genre
user_prefs = {
    "favorite_genre": "pop",
    "favorite_mood": "happy",
    "target_energy": 0.6,
    "likes_acoustic": True
}

Top recommendations:

Tambourine Heart - Score: 79.40
Because: Mood matches your favorite (happy); Energy (0.62) is close to your target (0.60); Matches your preference for acoustic tracks

Sunlit Porch - Score: 78.60
Because: Mood matches your favorite (happy); Energy (0.58) is close to your target (0.60); Matches your preference for acoustic tracks

Copper Skies - Score: 78.00
Because: Mood matches your favorite (happy); Energy (0.60) is close to your target (0.60); Matches your preference for acoustic tracks

Honeyed Strings - Score: 76.60
Because: Mood matches your favorite (happy); Energy (0.63) is close to your target (0.60); Matches your preference for acoustic tracks

Sunrise City - Score: 63.60
Because: Genre matches your favorite (pop); Mood matches your favorite (happy); Energy (0.82) is close to your target (0.60); Matches your preference for acoustic tracks


#4. Self-contradictory taste
user_prefs = {
    "favorite_genre": "rock",
    "favorite_mood": "intense",
    "target_energy": 0.9,
    "likes_acoustic": True
}

Top recommendations:

Storm Runner - Score: 82.00
Because: Genre matches your favorite (rock); Mood matches your favorite (intense); Energy (0.91) is close to your target (0.90); Matches your preference for acoustic tracks

Gym Hero - Score: 66.00
Because: Mood matches your favorite (intense); Energy (0.93) is close to your target (0.90); Matches your preferencefor acoustic tracks

Sunrise City - Score: 43.60
Because: Energy (0.82) is close to your target (0.90); Matches your preference for acoustic tracks

Rooftop Lights - Score: 37.00
Because: Energy (0.76) is close to your target (0.90); Matches your preference for acoustic tracks

Night Drive Loop - Score: 34.40
Because: Energy (0.75) is close to your target (0.90); Matches your preference for acoustic track

From this experiment, I saw the reverse where the mood in theory will beat genre now. The ceiling for mood match with 85 points beats ceiling for genre with 75 points. We can see this in stress test 1 where Sunrise City beats Storm Runner (81.40 to 73.00) vs the original result where Storm Runner beats Sunrise City (73.00 to 61.40).

I also saw that a perfect match in a criteria made a big impact in getting a song recommended. In Stress test 2 Copper Skies has a perfect, zero-distance energy match to the target. Under the old weights it scored only 58.00 and lost to Sunrise City's exact-but-poorly-fit genre match (73.60). Under the new weights, Copper Skies jumps to 78.00 and now beats Sunrise City (63.60) outright — genuinely nailing energy now outweighs an exact genre label with a bad energy fit. 

I saw that energy alone can carry a song while I made genre nearly irrelavent. In stress test 1, I saw that Gym Hero it matches neither genre nor mood, yet still scores 59.00 (40 energy + 19 acoustic), nearly rivaling genre-matched Storm Runner. Genre's max contribution is now much smaller (15) and arugbly the weakness criteria in scoring the song. It could be seens an over correction in our system.

And I saw despite these changes, in stress test 4 Storm Runner scores exactly 82.00 in both test. Old genre max + energy max (35 + 20 = 55) vs New genre max + new energy max (15 + 40 = 55). This change in criterion had zero effect where song did match exactly with genre and lands in the same energy band. We just reshuffled how the points were calculated.

---

## Limitations and Risks

Summarize some limitations of your recommender.

Examples:

- It only works on a tiny catalog
- It does not understand lyrics or language
- It might over favor one genre or mood

You will go deeper on this in your model card.

The system has a bias against users who perfer calm music. If the user has an energy preference of 0.17 or below, then a song can never reach the max potential energy score because the lowest energy song is 0.28 (SpaceWalks Thoughts).

A majority of genres (rock, jazz, indie pop, etc.) have one song. This means if the user preference matches one of those genres, then they have a huge lead in being recommended. They feel like uncontested "winners".

Since genre is being match exactly, then songs with related genres would not be recommended. For an example, if a user likes pop and a song is folk pop then the song would not get points for the genre criteria as it is not exactly "pop". It gets hung up recommending the same handful of songs and has no way for users to widen their exposure to songs in related genres.

---

## Reflection

Read and complete `model_card.md`:

[**Model Card**](model_card.md)

Write 1 to 2 paragraphs here about what you learned:

- about how recommenders turn data into predictions
- about where bias or unfairness could show up in systems like this



