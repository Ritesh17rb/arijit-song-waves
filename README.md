# Arijit Song Waves

An interactive single-page visualization of 150 Arijit Singh songs.

The page renders each song as a living wave:
- vertical order is based on BPM
- line weight is based on popularity
- hover reveals year, views, BPM, energy, mood, and danceability
- legend filters songs by mood clusters

## Files

- `index.html`: the visual, layout, and interaction logic
- `songs-data.js`: the generated song dataset used by the page

## Data

The current dataset was assembled from:
- iTunes catalog metadata
- local analysis of preview audio for tempo and mood-like features
- public YouTube search view counts for popularity scaling

## Run

Open `index.html` directly in a browser, or serve the folder with any static file server.

## Repo

Repository name: `arijit-song-waves`
