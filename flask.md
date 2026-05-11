# Suggestify – Hybrid Recommendation System using ML

## 1. Flask Backend (app.py)

```python
from flask import Flask, request, jsonify
from flask_cors import CORS
import pandas as pd
from sklearn.metrics.pairwise import cosine_similarity

app = Flask(__name__)
CORS(app)

# Sample movie dataset
movies = pd.DataFrame({
    'movie': ['Avengers', 'Interstellar', 'Batman', 'Inception'],
    'genre': ['Action', 'Sci-Fi', 'Action', 'Sci-Fi']
})

# Sample user ratings
ratings = pd.DataFrame({
    'user': ['User1', 'User1', 'User2', 'User2'],
    'movie': ['Avengers', 'Batman', 'Avengers', 'Interstellar'],
    'rating': [5, 4, 5, 5]
})

@app.route('/recommend/<movie_name>', methods=['GET'])
def recommend(movie_name):
    recommended = movies[movies['genre'] == movies[movies['movie'] == movie_name]['genre'].values[0]]
    result = recommended['movie'].tolist()
    return jsonify(result)

if __name__ == '__main__':
    app.run(debug=True)
```

---

# 2. React Frontend (App.js)

```javascript
import React, { useState } from 'react';

function App() {
  const [movie, setMovie] = useState('');
  const [recommendations, setRecommendations] = useState([]);

  const getRecommendations = async () => {
    const response = await fetch(`http://127.0.0.1:5000/recommend/${movie}`);
    const data = await response.json();
    setRecommendations(data);
  };

  return (
    <div style={{ padding: '20px' }}>
      <h1>Suggestify</h1>

      <input
        type="text"
        placeholder="Enter movie name"
        value={movie}
        onChange={(e) => setMovie(e.target.value)}
      />

      <button onClick={getRecommendations}>Recommend</button>

      <h2>Recommended Movies:</h2>
      <ul>
        {recommendations.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

---

# 3. Collaborative Filtering Example

```python
import pandas as pd
from sklearn.metrics.pairwise import cosine_similarity

ratings = pd.DataFrame({
    'Avengers': [5, 5, 0],
    'Batman': [4, 0, 5],
    'Interstellar': [0, 5, 4]
}, index=['User1', 'User2', 'User3'])

similarity = cosine_similarity(ratings)

print(similarity)
```

Explanation:

* If User1 and User2 like similar movies,
* The system recommends User2’s liked movies to User1.

---

# 4. Content-Based Filtering Example

```python
movies = {
    'Avengers': 'Action',
    'Batman': 'Action',
    'Interstellar': 'Sci-Fi',
    'Inception': 'Sci-Fi'
}

liked_movie = 'Interstellar'
liked_genre = movies[liked_movie]

recommendations = [movie for movie, genre in movies.items() if genre == liked_genre]

print(recommendations)
```

Explanation:

* If user likes Sci-Fi movies,
* System recommends similar Sci-Fi movies.

---

# 5. Hybrid Recommendation Logic

```python
content_based = ['Interstellar', 'Inception']
collaborative = ['Avengers', 'Batman']

hybrid_recommendation = content_based + collaborative

print(hybrid_recommendation)
```

Explanation:

* Combines both recommendation techniques.
* Improves accuracy and personalization.

---

# 6. Install Required Packages

```bash
pip install flask flask-cors pandas scikit-learn
```

---

# 7. Run Backend

```bash
python app.py
```

---

# 8. Run React Frontend

```bash
npm install
npm start
```

---

# 9. Simple Seminar Explanation

“Our website collects user preferences and movie ratings.
The Machine Learning model analyzes similar users and similar movie genres.
Then the system generates personalized movie recommendations using hybrid recommendation techniques.”
