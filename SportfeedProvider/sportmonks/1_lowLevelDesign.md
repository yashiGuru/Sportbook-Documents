# Project Structure Documentation

## Directory Overview

### `src/`
The main source directory containing all application logic, including controllers, models, data layers, repositories, and providers.

#### `controllers/`
Contains all the controller logic that interacts with the routes and services.

#### `models/`
Holds the database models and schema definitions.

#### `dataLayers/`
Handles direct interactions with the database, abstracting raw database queries.

#### `repositories/`
Acts as a middle layer between data layers and services, facilitating data retrieval and business logic.

#### `providers/`
Contains third-party integrations and services, such as SportMonks data feeds.

---

## SportMonks Provider
Located in `providers/sportmonks`, responsible for fetching and processing SportMonks API data.

### `services/`
This directory houses the main service methods that interact with the SportMonks API.

- **`index.js`**
  - `getFootballMatchesByDateRange(startDate, endDate)`: Fetches football matches within a specified date range from the SportMonks API.
  - `getLatestFootballMatches()`: Retrieves the most recent football matches.
  - `getLatestFootballMarketOdds()`: Fetches the latest market odds for football matches.
  - `getFootballMarketOddsByMatchId(matchId)`: Retrieves market odds for a specific match using its ID.

### `crons/`
Contains scheduled jobs that run periodically to fetch and update football match and market odds data.

- **`fetchUpcomingMatches.cron.js`**
  - `fetchLatestFootballMatches()`: Periodically fetches the most recent football matches.
  - `fetchFootballMatchesByDateRange(startDate, endDate)`: Fetches matches within a date range at scheduled intervals.

- **`fetchUpcomingMarketOdds.cron.js`**
  - `fetchLatestFootballMarketOdds()`: Periodically fetches the latest market odds for matches.
  - `fetchFootballMarketOddsForMatch(matchId)`: Retrieves and updates market odds for a specific match.

- **`index.js`**
  - Entry point for cron jobs, ensuring they are initialized and scheduled properly.

### `adapters/`
Responsible for transforming raw API data into a structured format for use in the application.

- **`match.adapter.js`**
  - `transformFootballMatchData(rawData)`: Converts raw match data from SportMonks into a usable format.

- **`marketOdd.adapter.js`**
  - `transformFootballMarketOddsData(rawData)`: Transforms raw market odds data into a structured format.

### `config/`
Holds configuration settings related to the SportMonks provider.

- **`index.js`**
  - Contains API keys, endpoints, and other configurations for interacting with the SportMonks API.