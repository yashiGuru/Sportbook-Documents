# Sportsbook Database Schema

## Overview
The **MatchesSchema** is designed to store match-related information, integrating details about **sports**, **locations**, **leagues**, **participants**, and _scores_ details. Each match record is uniquely identified by an `external_match_id`. The schema supports timestamps for creation and updates.


## Key Features
- **Unique Identifier:** Each match has a unique `external_match_id`.
- **Time Tracking:** `start_date`, `end_date`, and automatic timestamps (`created_at`, `updated_at`).
- **Match Status:** Enum field indicating if a match is **Not Started (1), Running (2), or Finished (3)**.
- **Sports, Locations, and Leagues:** Each match is linked to its respective **sport, location, and league**.
- **Participants:** Stores **home and away** participant details along with their scores.
- **Market Count:** Keeps track of the **total available betting markets**.

## Indexes
- `sport.external_sport_id`, `location.external_location_id`, `league.external_league_id`, `participant_home.external_participant_home_id`, and `participant_away.external_participant_away_id` are indexed for faster querying.
external_id is unique to prevent duplicate matches.


## Match Schema Definition
```javascript
const MatchesSchema = new mongoose.Schema(
    {
        external_match_id: { type: String, required: true, unique: true },
        start_date: { type: Date, required: true },
        end_date: { type: Date, default: null },
        status: { type: Number, required: true, enum: [1, 2, 3] }, // 1 = Not Started, 2 = Running, 3 = Finished
        type: { type: Number, default: 2, enum: [1, 2] },  // 1 = In-play, 2 = Pre-match
        sport: {
            external_sport_id: { type: String, required: true, index: true },
            name: { type: String, required: true },
        },
        location: {
            external_location_id: { type: String, required: true, index: true },
            name: { type: String, required: true },
        },
        league: {
            external_league_id: { type: String, required: true, index: true },
            name: { type: String, required: true },
        },
        participant_home: {
            external_participant_home_id: { type: String, required: true, index: true },
            home_name: { type: String, required: true },
            home_score: { type: Number, default: 0 },
        },
        participant_away: {
            external_participant_away_id: { type: String, required: true, index: true },
            name: { type: String, required: true },
            score: { type: Number, default: 0 },
        },
        total_markets: { type: Number, default: 0 },
    },
    {
        timestamps: { createdAt: 'created_at', updatedAt: 'updated_at' },
    }
);
```

# MarketsSchema

## Description
The `MarketsSchema` maintains betting odds associated with a match. It contains an array of odds, each with pricing details and statuses.

## Indexes
- `external_market_id`, `external_match_id`, and `odds.external_odd_id` are indexed for performance.
- `odds.external_odd_id` is unique to prevent duplicate odds entries.

## Performance Considerations
- **Indexing**: The key fields are indexed to improve query performance, especially for filtering and searching.
- **Storage Optimization**: Using default values reduces storage overhead for missing fields.
- **Denormalization**: Related information (e.g., sport, location, league) is embedded within the `MatchSchema` to minimize joins and enhance retrieval speed.
- **Scalability**: Designed to accommodate high write operations with minimal locking issues by using sharded collections if necessary.

## Schema Definition
```javascript
const MarketsSchema = new mongoose.Schema(
   {
      external_market_id: { type: String, required: true, index: true },
      name: { type: String, required: true },
      external_match_id: { type: String, ref: 'Match', required: true, index: true },
      odds: [
         {
            external_odd_id: { type: String, required: true, unique: true },
            name: { type: String, default: null },
            line: { type: String, default: null },
            baseline: { type: String, default: null },
            price_decimal: { type: Number, required: true, min: 1.00, default: 1.00 },
            price_american: { type: Number, default: null },
            odd_status: { type: Number, default: 1, enum: [1, 2, 3] },  
            // 1 = Open, 2 = Suspended, 3 = Settled
            odd_settlement_status: { type: Number, default: 1, enum: [1, 2, 3, 4, 5, 6] },
            // 1 = Open, 2 = Winner, 3 = Half-Won, 4 = Lost, 5 = Half-Lost, 6 = Canceled/Refunded
         },
      ],
   },
   {
      timestamps: { createdAt: 'created_at', updatedAt: 'updated_at' },
   }
);
```

## Considerations
- Ensure `external_odd_id` remains unique to avoid duplicate entries.
- Use indexing effectively for better performance.
- Consider sharding strategies for large-scale applications.

