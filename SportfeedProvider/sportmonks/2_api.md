### Explanation of the API Request:

#### **Request URL:**

##### **GET Latest Updated Fixtures**
```
https://api.sportmonks.com/v3/football/fixtures/latest?include=sport:id,name;league:id,name;league.country:id,name;participants:id,name;scores&select=id,sport_id,state_id,starting_at&api_token=cvHx9GnLIFjxZ9dqaNOBfQbDnYX67zQZSSMwgx6DmaoiSzLGMv4nVWelZzFv
```

##### **GET Fixtures by Date Range**
```
https://api.sportmonks.com/v3/football/fixtures/between/2025-02-12/2025-03-28?include=sport:id,name;league:id,name;league.country:id,name;participants:id,name;scores&select=id,sport_id,state_id,starting_at&page=1&per_page=50&api_token=cvHx9GnLIFjxZ9dqaNOBfQbDnYX67zQZSSMwgx6DmaoiSzLGMv4nVWelZzFv
```


#### **Breakdown of URL Parameters:**
- **`v3/football/fixtures/between/2025-02-12/2025-03-28`** → Fetches football fixtures scheduled between February 12, 2025, and March 28, 2025.
- **`include=sport:id,name;league:id,name;league.country:id,name;participants:id,name;scores`** → Specifies related data to be included in the response:
  - `sport`: ID and name of the sport.
  - `league`: ID and name of the league.
  - `league.country`: ID and name of the country of the league.
  - `participants`: ID and name of the teams/participants.
  - `scores`: Match scores.
- **`select=id,sport_id,state_id,starting_at`** → Specifies fields to retrieve:
  - `id`: Match ID.
  - `sport_id`: ID of the sport.
  - `state_id`: Match state/status.
  - `starting_at`: Match start time.
- **`page=1&per_page=50`** → Fetches the first page of results with 50 matches per page.
- **`api_token=...`** → API authentication token.

---

### Explanation of the Response:

#### **Match Information:**
```json
{
  "id": 19130351,
  "sport_id": 1,
  "state_id": 5,
  "starting_at": "2025-02-14 18:00:00",
  "round_id": 338804,
  "stage_id": 77471148,
  "group_id": null,
  "aggregate_id": null,
  "league_id": 271,
  "season_id": 23584,
  "venue_id": 5659,
  "starting_at_timestamp": 1739556000,
```
- **`id: 19130351`** → Unique fixture ID.
- **`sport_id: 1`** → Sport type (1 = Football).
- **`state_id: 5`** → The match state (e.g., scheduled, ongoing, completed).
- **`starting_at: "2025-02-14 18:00:00"`** → The date and time of the match.
- **`league_id: 271`** → ID of the league in which the match is played.
- **`venue_id: 5659`** → ID of the stadium/venue.

---

#### **Sport & League Details:**
```json
"sport": {
    "id": 1,
    "name": "Football"
},
"league": {
    "id": 271,
    "sport_id": 1,
    "country_id": 320,
    "name": "Superliga",
    "country": {
        "id": 320,
        "continent_id": 1,
        "name": "Denmark"
    }
}
```
- **Sport:** `Football`
- **League:** `Superliga`
- **Country:** `Denmark`

---

#### **Participants (Teams):**
```json
"participants": [
    {
        "id": 293,
        "sport_id": 1,
        "country_id": 320,
        "venue_id": 5659,
        "name": "Brøndby IF",
        "meta": {
            "location": "home",
            "winner": true,
            "position": 5
        }
    },
    {
        "id": 2447,
        "sport_id": 1,
        "country_id": 320,
        "venue_id": 1467,
        "name": "Viborg FF",
        "meta": {
            "location": "away",
            "winner": false,
            "position": 8
        }
    }
]
```
- **Home Team:** `Brøndby IF`
  - Played at home (`location: home`).
  - Won the match (`winner: true`).
- **Away Team:** `Viborg FF`
  - Played away (`location: away`).
  - Lost the match (`winner: false`).

---

#### **Match Scores:**
```json
"scores": [
    {
        "id": 15913771,
        "fixture_id": 19130351,
        "type_id": 48996,
        "participant_id": 293,
        "score": {
            "goals": 3,
            "participant": "home"
        },
        "description": "2ND_HALF_ONLY"
    },
    {
        "id": 15913772,
        "fixture_id": 19130351,
        "type_id": 48996,
        "participant_id": 2447,
        "score": {
            "goals": 0,
            "participant": "away"
        },
        "description": "2ND_HALF_ONLY"
    }
]
```
- **Brøndby IF (Home Team):**  
  - **Scored 3 goals in the 2nd half.**  
  - **Scored 4 goals in total.**
- **Viborg FF (Away Team):**  
  - **Scored 0 goals in the 2nd half.**  
  - **Scored 1 goal in total.**

---

### **Summary of the Response:**
- The API fetches football fixtures for the given date range (Feb 12 - Mar 28, 2025).
- The response includes details about the match, sport, league, country, participants (teams), and scores.
- The match took place on **February 14, 2025, at 18:00 UTC**.
- **Brøndby IF (home team) won the match 4-1 against Viborg FF (away team).**


#### **Request URL:**

##### **GET Last Updated Odds**
```
https://api.sportmonks.com/v3/football/odds/pre-match/latest?include=market:id,name,has_winning_calculations&select=id,fixture_id,label,value,american,name,sort_order,winning,stopped,total,handicap&filters=bookmakers:2&api_token=cvHx9GnLIFjxZ9dqaNOBfQbDnYX67zQZSSMwgx6DmaoiSzLGMv4nVWelZzFv
```

##### **GET Odds and Market by Fixture ID**
```
https://api.sportmonks.com/v3/football/odds/pre-match/fixtures/19130351?include=market:id,name,has_winning_calculations&select=id,fixture_id,label,value,american,name,sort_order,winning,stopped,total,handicap&filters=bookmakers:2&api_token=cvHx9GnLIFjxZ9dqaNOBfQbDnYX67zQZSSMwgx6DmaoiSzLGMv4nVWelZzFv
```


**Query Parameters:**
- `include=market:id,name,has_winning_calculations` → Includes market details such as ID, name, and whether winning calculations apply.
- `select=id,fixture_id,label,value,american,name,sort_order,winning,stopped,total,handicap` → Retrieves specific fields related to the odds.
- `filters=bookmakers:2` → Filters the odds by bookmaker ID 2 (specific betting provider).
- `api_token=<token>` → API authentication.

---

## **Response Breakdown**
The response provides **pre-match betting odds** for the football fixture with ID **19130351**.

### **Odds Details:**
- **Odd ID:** `151205426195`
- **Fixture ID:** `19130351` (Match for which the odds are provided)
- **Label:** `"Away"` (Betting odds for the away team winning)
- **Value:** `"6.00"` (Decimal odds format: a winning bet would return 6 times the stake)
- **American Odds:** `"500"` (Equivalent in American odds format)
- **Sort Order:** `2` (Positioning in the odds list)
- **Winning Status:** `false` (Outcome not determined yet)
- **Stopped:** `false` (Odds are still active)
- **Total & Handicap:** `null` (Not applicable in this case)

### **Market Details:**
- **Market ID:** `1`
- **Market Name:** `"Fulltime Result"` (Standard 1X2 market for match results)
- **Winning Calculations:** `true` (Indicates that winning conditions are defined for this market)

---

## **Summary**
This API provides **pre-match betting odds** for a given fixture. It includes data about the betting market, odds value, bookmaker, and other relevant betting attributes. The response allows integration with betting platforms to display and process betting odds effectively.

