# 🎬 Movie Recommendation System

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.28+-red.svg)](https://streamlit.io/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen.svg)](https://github.com/NirlepSanap/movie-recommendation-system)

A sleek and responsive movie recommendation system built with **Streamlit** that suggests similar movies based on content-based filtering. Features an intuitive web interface with movie posters fetched from TMDB API.

## 🌟 Live Demo

![Movie Recommender Demo](https://via.placeholder.com/800x400/007bff/white?text=Movie+Recommender+System+Demo)

## ✨ Key Features

- **Content-Based Filtering**: Recommends movies based on movie similarity using pre-computed similarity matrix
- **Interactive Web Interface**: Clean, responsive Streamlit UI with custom CSS styling
- **Movie Posters**: Real-time poster fetching from The Movie Database (TMDB) API
- **Fast Performance**: Pre-computed similarity matrix for instant recommendations
- **Responsive Design**: Mobile-friendly interface with adaptive columns
- **Custom Styling**: Modern light theme with smooth animations and hover effects

## 🛠️ Tech Stack

- **Frontend**: Streamlit with custom CSS
- **Backend**: Python
- **Data Processing**: Pandas, Pickle
- **API Integration**: TMDB API for movie posters
- **ML/Similarity**: Pre-computed cosine similarity matrix
- **Styling**: Custom CSS with Google Fonts (Poppins)

## 📁 Project Structure

```
movie-recommendation-system/
│
├── app.py                        # Main Streamlit application
├── movie_list.pkl                # Preprocessed movie data
├── similarity.pkl                # Pre-computed similarity matrix (184MB)
├── requirements.txt              # Python dependencies
├── README.md                     # Project documentation
└── .gitattributes               # Git LFS configuration for large files
```

## 🚀 Quick Start

### Prerequisites
- Python 3.8+
- Git with Git LFS (for large files)
- Internet connection (for TMDB API)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/NirlepSanap/movie-recommendation-system.git
   cd movie-recommendation-system
   ```

2. **Install dependencies**
   ```bash
   pip install streamlit pandas requests pickle5
   ```

3. **Run the application**
   ```bash
   streamlit run app.py
   ```

4. **Access the app**
   - Open your browser and go to `http://localhost:8501`
   - Select a movie from the dropdown
   - Click "Show Recommendation" to get 5 similar movies

## 💻 Usage

### Web Interface
1. **Select a Movie**: Choose from the dropdown menu containing thousands of movies
2. **Get Recommendations**: Click the "Show Recommendation" button
3. **View Results**: See 5 similar movies with their posters in a responsive grid

### Code Structure Overview

```python
# Core recommendation function
def recommend(movie):
    # Find movie index
    index = movies[movies['title'] == movie].index[0]
    
    # Get similarity scores
    distances = sorted(list(enumerate(similarity[index])), 
                      reverse=True, key=lambda x: x[1])
    
    # Extract top 5 recommendations
    recommended_movies = []
    for i in distances[1:6]:
        movie_id = movies.iloc[i[0]].movie_id
        recommended_movies.append({
            'name': movies.iloc[i[0]].title,
            'poster': fetch_poster(movie_id)
        })
    
    return recommended_movies
```

## 🎨 Features in Detail

### 1. Content-Based Filtering
- Uses pre-computed similarity matrix for fast recommendations
- Based on movie features like genre, cast, crew, and plot
- Cosine similarity algorithm for finding similar movies

### 2. TMDB Integration
```python
def fetch_poster(movie_id):
    url = f"https://api.themoviedb.org/3/movie/{movie_id}?api_key=YOUR_API_KEY"
    data = requests.get(url).json()
    poster_path = data.get('poster_path', '')
    return "https://image.tmdb.org/t/p/w500/" + poster_path
```

### 3. Custom Styling
- **Modern Design**: Clean light theme with Poppins font
- **Responsive Layout**: Adapts to different screen sizes
- **Smooth Animations**: Fade-in effects for movie cards
- **Interactive Elements**: Hover effects on buttons

### 4. Performance Optimization
- **Pre-computed Similarity**: 184MB similarity matrix for instant results
- **Efficient Data Storage**: Pickled DataFrames for fast loading
- **API Caching**: Reduces redundant API calls

## 📊 Dataset Information

- **Total Movies**: ~5,000 movies
- **Similarity Matrix**: 184MB pre-computed cosine similarity
- **Data Source**: Processed movie metadata with features like:
  - Movie titles and IDs
  - Genres, cast, crew information
  - Plot summaries and keywords
  - TMDB movie IDs for poster fetching

## 🔧 Configuration

### TMDB API Setup
1. Get your API key from [TMDB](https://www.themoviedb.org/settings/api)
2. Replace the API key in `app.py`:
   ```python
   url = f"https://api.themoviedb.org/3/movie/{movie_id}?api_key=YOUR_API_KEY"
   ```

### Customizing Recommendations
```python
# Change number of recommendations (currently 5)
for i in distances[1:6]:  # Change 6 to desired number + 1

# Modify similarity threshold
distances = [d for d in distances if d[1] > 0.1]  # Only similar movies
```

## 🚀 Deployment

### Streamlit Cloud
1. Push your repository to GitHub
2. Connect to [Streamlit Cloud](https://streamlit.io/cloud)
3. Deploy directly from your repository

### Local Deployment
```bash
# Install requirements
pip install -r requirements.txt

# Run application
streamlit run app.py --server.port 8501
```

### Docker Deployment
```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY . .

RUN pip install streamlit pandas requests

EXPOSE 8501

CMD ["streamlit", "run", "app.py", "--server.address", "0.0.0.0"]
```

## 🔍 Algorithm Details

### Content-Based Filtering Process
1. **Feature Extraction**: Movie metadata processed into feature vectors
2. **Similarity Computation**: Cosine similarity calculated between all movies
3. **Matrix Storage**: Pre-computed similarity matrix saved as pickle file
4. **Recommendation**: Top-K similar movies retrieved instantly

### Similarity Calculation
```python
# Conceptual similarity calculation
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity

# Feature vectorization
tfidf = TfidfVectorizer(stop_words='english')
tfidf_matrix = tfidf.fit_transform(movie_features)

# Similarity matrix
similarity_matrix = cosine_similarity(tfidf_matrix, tfidf_matrix)
```

## 📱 Responsive Design

The application features a mobile-friendly design with:
- **Adaptive Columns**: Stack vertically on mobile devices
- **Touch-Friendly**: Large buttons and touch targets
- **Optimized Images**: Responsive movie posters
- **Fast Loading**: Optimized for mobile connections

## 🐛 Known Issues & Limitations

1. **Large File Size**: 184MB similarity matrix requires Git LFS
2. **API Dependencies**: Requires internet connection for movie posters
3. **Limited Dataset**: ~5,000 movies (can be expanded)
4. **Cold Start**: No user-based recommendations (content-based only)

## 🔮 Future Enhancements

- [ ] **User Ratings**: Add user rating system for hybrid recommendations
- [ ] **Search Functionality**: Add movie search with autocomplete
- [ ] **More Metadata**: Include release year, runtime, ratings
- [ ] **Collaborative Filtering**: Add user-based recommendations
- [ ] **Movie Details**: Show plot, cast, and crew information
- [ ] **Favorites**: Allow users to save favorite movies
- [ ] **Performance**: Optimize similarity matrix loading

## 📋 Requirements

```txt
streamlit>=1.28.0
pandas>=1.5.0
requests>=2.28.0
pickle5>=0.0.11
```

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

**Nirlep Sanap**
- GitHub: [@NirlepSanap](https://github.com/NirlepSanap)
- LinkedIn: [Connect on LinkedIn](https://linkedin.com/in/nirlepsanap)

## 🙏 Acknowledgments

- [The Movie Database (TMDB)](https://www.themoviedb.org/) for movie data and posters
- [Streamlit](https://streamlit.io/) for the amazing web framework
- [scikit-learn](https://scikit-learn.org/) for similarity computation algorithms
- Google Fonts for the Poppins font family

## 📞 Support

If you encounter any issues or have questions:
1. Check the [Issues](https://github.com/NirlepSanap/movie-recommendation-system/issues) page
2. Create a new issue with detailed description
3. For urgent matters, reach out via LinkedIn

---

<div align="center">
  <strong>⭐ If you like this project, please give it a star on GitHub! ⭐</strong>
  <br><br>
  <em>Made with ❤️ using Streamlit</em>
</div>
