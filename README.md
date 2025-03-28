# Anime Recommendation System

A Python-based Anime Recommendation System that fetches data from MyAnimeList, cleans and processes anime titles using a language model API, computes similarity scores based on combined textual features, and provides interactive recommendations via a Gradio interface.

![Image 1](/Image/image1.png "image1")
Made with DALL-E

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Content](#Content)
- [Installation](#installation)
- [Usage](#usage)
- [Data and APIs](#data-and-apis)
- [Implementation Details](#implementation-details)
- [Future Work](#future-work)
- [Delegations](#Delegations)
- [Contributing](#contributing)
---

## Overview

This project is an end-to-end Anime Recommendation System that leverages data from the MyAnimeList API to fetch anime rankings. It then preprocesses and cleans the data, uses a Gemini language model API to simplify anime titles, and computes text-based similarities using TF-IDF and cosine similarity. Finally, an interactive Gradio interface allows users to select their favorite anime and receive recommendations based on their selections.

---

## Features

- **Data Acquisition:**  
  Fetches anime ranking data from the MyAnimeList API using a provided client ID.

- **Data Preprocessing:**  
  Processes the raw JSON data into a structured pandas DataFrame including fields such as title, synopsis, genres, mean score, media type, and status.

- **Title Cleaning with Gemini:**  
  Utilizes the Gemini API (via the `langchain_google_genai` package) to simplify anime titles by removing season numbers and special identifiers.

- **Textual Feature Combination:**  
  Merges multiple text fields (titles, alternative titles, synopsis, genres, etc.) into a single string for each anime to serve as input for vectorization.

- **Similarity Computation:**  
  Uses TF-IDF vectorization and cosine similarity to generate a similarity matrix that underpins the recommendation engine.

- **Recommendation Engine:**  
  Suggests similar anime based on user-selected favorites from the list.

- **Interactive Interface:**  
  Built with Gradio, enabling users to add anime to a personal list, clear the list, and fetch recommendations with the click of a button.
  
- **Cosine Similarity**
  In an inner product space containing two non-zero vectors, cosine similarity serves as a way of determining the similarity of the two vectors
  Cosine similarity is found by taking the dot product of the two vectors and dividing by the product of their magnitudes

- **Embedding**
 Machine Learning and AI models can be integrated into an application or a device, which enables the software to complete jobs such as image recognition, natural language processing, and decision-making while bypassing the need for an external server for processing

---
## Content

- **Summary**
  -The summary, or synopsis, includes the information from Wikipedia and other official material about the show and movie

- **Genre**
  -Genre includes any tropes or categories that the anime show falls into: fantasy, action thriller, historical, etc.
  -Anime also has its own unique genres, such as Isekai, when the protagonist is transported to another world

- **Title**
  -Normally, the title would be the Japanese title of the show put in a phonetic English spelling with shorthand
  -Anything that would not be the normal English keyboard would return the closest value (i.e. infinity would return 8, a letter with an accent mark on it would return the plain letter)
  -The alternative title would return the translated English version (as opposed to the phonetic English version, which is just Japanese words spelt out in English) with some shorthand titles


- **Mean**
    - The project takes the mean score of all anime ratings on the API (18 million users' worth of ratings from over 200 countries for 21,500 anime appearances)
    - The project will outpiut anime with mean ratings similar to the mean ratings of those that the user input
    - The higher the mean rating, the more likely the project is to recommend it to you

- **Popularity**
    - The project returns anime whose user's scores were closest to the rating of the shows that they have rated
    - For example, if a user seems to enjoy controversial anime which only have received ratings of 0 or 10, the project will return similarly controversial anime

- **Ranking**
    - Ranking takes the selected anime's mean score and compares it to that of every other anime (of the 21,500 anime) and produces a number based on the comparison

- **Media Type**
    - What type of release is it?  It could be a movie, a planetarium special, a theatrical release, a show, or even a side story put into a play
    - If the user seems to prefer movies, the project will output movies, etc.

- **Status**
    - A 3-way variable of whether it's currently airing, finished, or on hiatus
    - The project can now determine whether the user prefers to binge watch shows all at once enjoys following the most recent ones and waiting for new episodes, and can recommend shows accordingly
  


---

## Installation

### Prerequisites

- **Python 3.7+**
- **Required Python Packages:**
  - `requests`
  - `pandas`
  - `scikit-learn`
  - `gradio`
  - `langchain_google_genai`

### Setup Steps

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/WPitera/Show-Recommendation-System.git
   cd Show-Recommendation-System
   ```

2. **Create and Activate a Virtual Environment (optional but recommended):**

   ```bash
   python -m venv venv
   source venv/bin/activate   # On Windows use: venv\Scripts\activate
   ```

3. **Install Dependencies:**

   If a `requirements.txt` file exists, run:

   ```bash
   pip install -r requirements.txt
   ```

   Otherwise, install manually:

   ```bash
   pip install requests pandas scikit-learn gradio langchain_google_genai
   ```

4. **API Credentials:**

   - **MyAnimeList API:** Replace the `CLIENT_ID` in the code with your valid MyAnimeList client ID.
   - **Gemini API:** Replace the `GEMINI_API_KEY` with your actual Gemini API key.

---

## Usage

1. **Run the Notebook or Python Script:**

   You can run the provided code either in a Jupyter Notebook environment or as a standalone Python script. Ensure the following snippet is executed when running as a script:

   ```python
   if __name__ == "__main__":
       demo.launch()
   ```

2. **Using the Gradio Interface:**

   - The interface displays a dropdown populated with anime titles fetched and processed from MyAnimeList.
   - **Add to List:** Select an anime from the dropdown and click the "Add to List" button to add it to your personal list.
   - **Clear List:** Use the "Clear List" button to reset your selections.
   - **Get Recommendations:** Once you have your favorite anime in the list, click "Get Recommendations" to see similar anime suggestions.

3. **Recommendation Engine Details:**

   The recommendation function computes cosine similarity based on combined text fields (title, synopsis, genres, etc.) to find and suggest anime that are most similar to the user’s favorites.

---

## Data and APIs

### Data Source

- **MyAnimeList API:**  
  The system fetches anime ranking data from the endpoint:  
  `https://api.myanimelist.net/v2/anime/ranking`  
  The data includes fields such as `id`, `title`, `synopsis`, `genres`, `mean`, `media_type`, and `status`.

### External APIs

- **Gemini (via langchain_google_genai):**  
  The Gemini API is used to simplify and clean anime titles by removing extraneous identifiers such as season numbers, "Movie", "Special", and "OVA". This helps in creating a uniform title for better matching.

---

## Implementation Details

### Code Structure

1. **Data Fetching and Preprocessing:**
   - `fetch_anime_list(offset)`: Retrieves anime ranking data from MyAnimeList in batches.
   - `preprocess_data(anime_list, existing_df)`: Converts raw API responses into a pandas DataFrame, merging with existing data if needed.

2. **Title Cleaning:**
   - `extract_base_title_with_gemini(title)`: Uses the Gemini API to simplify titles. The code iterates over the DataFrame to update the titles.

3. **Combining Data Copies:**
   - `combine_copies(new_data)`: Groups and aggregates data by title, combining multiple entries to remove duplicates and aggregate information such as alternative titles, synopsis, and genres.

4. **Similarity Computation:**
   - `compute_similarity(df)`: Combines various text fields into a single column, applies TF-IDF vectorization, and computes a cosine similarity matrix for the anime dataset.

5. **Recommendation Engine:**
   - `recommend_anime(user_favorites, df, similarity_matrix)`: Finds similar anime by checking the similarity matrix for each user favorite.

6. **Interactive Gradio UI:**
   - Built with Gradio Blocks, the UI provides dropdowns, buttons, and text boxes to add anime to a list, display the list, and output recommendations interactively.

### Dependencies and Libraries

- **requests:** For API calls to MyAnimeList.
- **pandas:** For data manipulation and DataFrame operations.
- **scikit-learn:** For TF-IDF vectorization and computing cosine similarity.
- **gradio:** For building the interactive web UI.
- **langchain_google_genai:** For interfacing with the Gemini API to process anime titles.

---

## Future Work

- **Performance Improvements:**  
  Optimize data fetching and processing for larger datasets.

- **Enhanced Recommendation Algorithms:**  
  Incorporate collaborative filtering or hybrid recommendation methods to refine suggestions.

- **UI Enhancements:**  
  Expand the Gradio interface to include additional filters and richer visualizations.

- **Error Handling and Logging:**  
  Improve error management for API calls and provide detailed logging for debugging.
  
- **Gemini:**
  Find an alternative method for using Gemini that does not require premium key or manual intervention

- **Speed:**
  Find a way for the program to automatically update the list of current anime or speed up the process or retrieval or preprocessing of the data from the three hours that it currently takes, since there are new anime coming out of the data every day 
  
---
## Delegations

- William handled the coding, with some help from John
- John organized the presentation, with some help from William
- We both worked on the readme

## Contributing

Contributions are welcome! Please follow these guidelines:

- Fork the repository.
- Create a new branch for your feature or bug fix.
- Submit a pull request with a detailed description of your changes.
- Follow the existing code style and include appropriate tests.



---
