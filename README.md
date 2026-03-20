# Arijit Song Waves

`Arijit Song Waves` is an interactive single-page music visualization built around a large Arijit Singh song map.

Instead of showing songs as a list, the project turns each track into a moving waveform on a dark canvas. The result feels closer to an atmosphere than a dashboard: dense, musical, and exploratory.

## What The Page Does

- Renders 150 Arijit Singh songs as animated horizontal waves
- Sorts the full field by BPM, so the page reads like a tempo spectrum
- Uses popularity to control line weight and brightness
- Shows song year, views, BPM, energy, mood, and danceability on hover
- Lets the viewer filter songs into `euphoric`, `intense`, and `melancholic` clusters

## Why It Feels Cool

- The page is not a static chart. Every song breathes.
- Dense songs create a musical topography instead of a spreadsheet.
- Hovering one line isolates it and pushes everything else into the background.
- The page works both as a data piece and as a visual moodboard for Arijit Singh's catalog.

## Project Structure

- `index.html`
  The full interface, layout, styles, Paper.js animation logic, hover card, and filter interactions.
- `songs-data.js`
  The generated dataset loaded by the page at runtime.
- `.gitignore`
  Keeps intermediate generation artifacts out of the repo.

## Data Pipeline

The current dataset is assembled from three sources:

- iTunes catalog metadata
  Used to discover tracks, get release dates, and access preview URLs.
- Local audio analysis on preview clips
  Used to estimate tempo, energy, valence-like mood, and danceability-like rhythm features.
- Public YouTube search result pages
  Used to estimate popularity via view counts without depending on the YouTube Data API quota for every track.

## How The Dataset Is Built

### 1. Discover Tracks

The song list starts from the Arijit Singh iTunes artist catalog.

Then the dataset generator filters the catalog down by:

- removing duplicate titles
- removing obvious remix / lofi / edit variants
- keeping tracks that include a valid preview clip

That gives a larger song pool, from which the current 150-song set is derived.

### 2. Clean Titles

Raw catalog names are cleaned before search and display.

Examples of cleanup:

- remove `From "Movie"` suffixes where needed
- normalize soundtrack collection names into cleaner movie labels
- keep meaningful labels like `Title Track` when they matter

### 3. Estimate Popularity

For each song, the generator searches public YouTube results and scores likely matches.

The scoring prefers:

- titles containing the song name
- titles containing the movie name
- official-looking uploads
- recognized music channels such as `T-Series`, `Sony Music India`, `YRF`, `Zee Music Company`, `Tips Official`, and similar channels

The scoring penalizes:

- playlists
- shorts
- teaser / promo uploads
- status-video style uploads
- heavily misleading lyric / audio-only noise when a better match exists

The selected result's view count becomes the page's popularity signal.

### 4. Measure Audio Features

Each iTunes preview clip is analyzed locally.

The current feature pipeline:

- loads the preview audio to mono
- separates harmonic and percussive components
- estimates beat tempo from the percussive signal
- normalizes RMS loudness into an energy score
- uses onset strength and beat regularity to build a danceability-style score
- uses chroma + spectral brightness heuristics to derive a valence-like mood score

These are not official Spotify-style API features. They are locally estimated approximations derived from the audio preview itself.

## Visual Mapping

The page maps data to visuals like this:

- vertical order: BPM
- line thickness: popularity / views
- line opacity: popularity / views
- wave amplitude: energy
- wave motion frequency: tempo
- hover card label: song metadata
- color grouping: valence-based mood bucket

## Mood Logic

Mood groups are derived from the valence score:

- `euphoric`: `valence >= 0.75`
- `melancholic`: `valence <= 0.50`
- `intense`: everything in between

This is intentionally simple so the interaction stays legible.

## Interaction Notes

- Hover highlights one song and mutes the rest of the field.
- On mobile, touch interaction drives the same detail card behavior.
- The legend acts as a mood filter, dimming tracks outside the active cluster.
- The page resizes responsively and recomputes layout metrics on resize.

## Tradeoffs

There are a few deliberate compromises in the current build:

- YouTube matching is heuristic, so some ambiguous songs may still need hand-curation.
- Audio features are estimated from previews, not full tracks.
- Popularity uses public search-result views rather than a fully verified official video map for every song.

Even with those tradeoffs, the output is grounded in real catalog and public platform data rather than placeholder numbers.

## Running The Project

Open `index.html` directly in a browser, or serve the folder locally with any static file server.

## If You Want To Improve It Further

- add a dedicated dataset build script to regenerate `songs-data.js`
- hand-curate a small list of ambiguous song-title matches
- add a timeline scrubber by release year
- add channel / movie / soundtrack filters
- add a detail panel with poster art or soundtrack metadata

## Repo

GitHub repository: `Ritesh17rb/arijit-song-waves`
