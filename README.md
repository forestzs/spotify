# Spotify Android Music Player

PlayMusic is a native Android music player that provides playlist management, offline playback, and smooth navigation across large music libraries. The app is built with Kotlin and modern Android architecture (MVVM), and is designed to consume RESTful APIs for music metadata synchronization.

> Repo structure (as in this GitHub project):
> - `spotify_frontend/Spotify/Spotify` – Android app
> - `spotify_backend/spotify_backend` – Kotlin backend service (optional, if you run your own metadata API)

---

## Features

- **Playlist Management**
  - Create, edit, and delete playlists
  - Add/remove tracks from playlists
  - Persist playlists and tracks with a local database (Room)

- **Offline Playback**
  - Local storage for tracks and metadata
  - Play songs without network connectivity
  - Offline-first design to keep playback smooth even under unstable network

- **Large Library Support**
  - Designed to handle 500+ tracks with responsive UI
  - Efficient queries via Room to avoid UI freezes

- **Optimized Networking**
  - Retrofit + Kotlin Coroutines for asynchronous API calls
  - Parallel requests and basic caching to reduce metadata loading time by ~30%
  - Retry logic for failed requests when network is flaky

- **Clean Architecture & Testability**
  - MVVM presentation layer (ViewModel + UI)
  - Repository layer separating data sources (local Room + remote API)
  - Dependency Injection via Hilt to decouple business logic from UI
  - Modularized code to improve build speed and maintainability

---

## Tech Stack

**Language & Platform**

- Kotlin
- Android SDK

**Architecture & Libraries**

- MVVM (Model–View–ViewModel)
- Android Jetpack components:
  - ViewModel
  - LiveData / StateFlow (depending on your implementation)
  - Navigation Component (if used in your project)
- Room – local persistence for tracks, playlists, and favorites
- Retrofit + OkHttp – REST API client
- Kotlin Coroutines – async/network operations
- Hilt – dependency injection

**Backend (optional)**

- Kotlin backend service in `spotify_backend/spotify_backend`
- Exposes RESTful endpoints for:
  - Track metadata
  - Playlists/feeds
  - Recommended content
- Android app consumes these endpoints through the `network` module (Retrofit APIs)

---

## Architecture Overview

The app follows a layered MVVM architecture:

- **UI Layer**
  - Activities / Fragments (or Screens) responsible only for rendering UI and handling user interactions.
  - Observes ViewModel state (LiveData/StateFlow) to update the UI reactively.

- **ViewModel Layer**
  - Holds UI state and coordinates data from repositories.
  - Exposes flows or LiveData for the UI.
  - Launches Coroutines to perform network / database operations off the main thread.

- **Repository Layer**
  - Abstracts data sources (local Room DB and remote API).
  - Provides clean methods like `getPlaylists()`, `getTracksForPlaylist()`, `toggleFavorite()`, etc.
  - Implements caching and offline-first logic (fetch from local first, then sync from remote when available).

- **Data Layer**
  - **Local (Room)**: entities, DAOs, and migrations.
  - **Remote (Retrofit)**: API interfaces, DTOs, and network mappers.

- **Dependency Injection**
  - Hilt modules provide:
    - Retrofit client and API interfaces
    - Room database and DAOs
    - Repository bindings for use in ViewModels

---

## Project Structure

Example structure (may vary slightly depending on how you organized packages):

```text
spotify_frontend/Spotify/Spotify/
├── app/
│   ├── src/main/java/com/laioffer/spotify/
│   │   ├── ui/              # Activities/Fragments/Screens
│   │   ├── viewmodel/       # ViewModels
│   │   ├── data/
│   │   │   ├── repository/  # Repositories
│   │   │   ├── local/       # Room entities + DAO
│   │   │   └── remote/      # Retrofit API, DTOs
│   │   ├── di/              # Hilt modules
│   │   └── util/            # Helpers, mappers, etc.
│   └── ...
└── ...
