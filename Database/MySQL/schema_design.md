# Sportsbook Database Schema

## Issue of the Old SQL Database Schema
### 1. Data Insert Time Issue
- The primary ID is assigned first, and then the foreign key is set afterward. This sequencing creates a significant issue, especially when foreign key references are required before the primary key is established. It results in delays and inconsistency in data insertion, which can lead to failed or delayed insertions of related records.

### 2. Relation Issues
- The old schema involves complex relationships between entities (e.g., sports, locations, leagues, fixtures, odds and markets) that can lead to data inconsistency. Foreign keys are not always enforced, which might result in orphaned records or improperly linked data. The lack of normalized relationships can cause redundancy, performance issues, and difficulty maintaining referential integrity

### 3. Extra Fields and Improper Data Types
- The schema includes unnecessary fields or uses fields that do not conform to best practices, such as incorrect or overly broad data types. For example, not using the `UNSIGNED` attribute for fields like IDs or status flags can lead to unexpected values and poor database performance. The fields also may not align with the actual use case or business logic, making the schema harder to maintain and scale.


### 1. Sports
| Field          | Type        | Attributes           |
|---------------|------------|----------------------|
| id            | INT        | Primary Key, AUTO_INCREMENT |
| sportId       | VARCHAR(191) | Unique Identifier |
| name          | VARCHAR(100) | Not NULL |
| icon          | VARCHAR(255) | Optional |
| status        | BOOLEAN    | Not NULL, Default: TRUE |
| created_at    | TIMESTAMP  | Default: CURRENT_TIMESTAMP |
| updated_at    | TIMESTAMP  | Default: CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP |

### 2. Locations
| Field           | Type        | Attributes          |
|----------------|------------|---------------------|
| id             | INT        | Primary Key, AUTO_INCREMENT |
| locationId     | VARCHAR(191) | Unique Identifier |
| name           | VARCHAR(100) | Not NULL |
| countryflagimage | VARCHAR(255) | Optional |
| status         | BOOLEAN    | Not NULL, Default: TRUE |
| created_at     | TIMESTAMP  | Default: CURRENT_TIMESTAMP |
| updated_at     | TIMESTAMP  | Default: CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP |

### 3. Leagues
| Field        | Type        | Attributes          |
|-------------|------------|---------------------|
| id          | INT        | Primary Key, AUTO_INCREMENT |
| leagueId    | VARCHAR(191) | Unique Identifier |
| name        | VARCHAR(100) | Not NULL |
| season      | VARCHAR(50)  | Optional |
| sport_id    | INT        | Foreign Key (Sports) |
| location_id | INT        | Foreign Key (Locations) |
| status      | BOOLEAN    | Not NULL, Default: TRUE |
| created_at  | TIMESTAMP  | Default: CURRENT_TIMESTAMP |
| updated_at  | TIMESTAMP  | Default: CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP |

### 4. Fixtures
| Field                | Type        | Attributes          |
|----------------------|------------|---------------------|
| id                  | INT        | Primary Key, AUTO_INCREMENT |
| fixtureId           | VARCHAR(191) | Unique Identifier |
| name                | VARCHAR(100) | Not NULL |
| total_markets       | INT        | Default: 0 |
| start_date          | DATETIME   | Not NULL |
| end_date            | DATETIME   | NULLABLE |
| is_odds             | BOOLEAN    | Default: FALSE |
| fixtures_status     | VARCHAR(50)  | Not NULL |
| subscription_type   | VARCHAR(50)  | Optional |
| subscription_status | BOOLEAN    | Default: FALSE |
| league_id          | INT        | Foreign Key (Leagues) |
| locations_id       | INT        | Foreign Key (Locations) |
| participants_home_id | INT        | Foreign Key (Participants) |
| participants_away_id | INT        | Foreign Key (Participants) |
| sport_id           | INT        | Foreign Key (Sports) |
| actual_start_date  | DATETIME   | NULLABLE |
| actual_end_date    | DATETIME   | NULLABLE |
| last_updated_date  | TIMESTAMP  | Default: CURRENT_TIMESTAMP |

### 5. Markets
| Field      | Type        | Attributes          |
|-----------|------------|---------------------|
| id        | INT        | Primary Key, AUTO_INCREMENT |
| marketId  | VARCHAR(191) | Unique Identifier |
| name      | VARCHAR(100) | Not NULL |
| status    | BOOLEAN    | Not NULL, Default: TRUE |
| created_at | TIMESTAMP  | Default: CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP  | Default: CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP |

### 6. Odds
| Field             | Type        | Attributes          |
|------------------|------------|---------------------|
| id               | INT        | Primary Key, AUTO_INCREMENT |
| oddId           | VARCHAR(191) | Unique Identifier |
| name            | VARCHAR(100) | Not NULL |
| line            | FLOAT       | Default: 0.0 |
| baseline        | FLOAT       | Default: 0.0 |
| bet_status      | VARCHAR(50)  | Not NULL |
| startprice      | FLOAT       | Default: 0.0 |
| price           | FLOAT       | Default: 0.0 |
| betsettlement_status | VARCHAR(50)  | Not NULL |
| suspension_reason | VARCHAR(255) | Optional |
| fixture_id      | INT        | Foreign Key (Fixtures) |
| market_id       | INT        | Foreign Key (Markets) |
| created_at      | TIMESTAMP  | Default: CURRENT_TIMESTAMP |
| updated_at      | TIMESTAMP  | Default: CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP |

### 7. Live Scores
| Field            | Type        | Attributes          |
|-----------------|------------|---------------------|
| id              | INT        | Primary Key, AUTO_INCREMENT |
| livescoreId     | VARCHAR(191) | Unique Identifier |
| fixture_id      | INT        | Foreign Key (Fixtures) |
| status          | VARCHAR(50)  | Not NULL |
| participant_home_id | INT        | Foreign Key (Participants) |
| home_score      | INT        | Default: 0 |
| participant_away_id | INT        | Foreign Key (Participants) |
| away_score      | INT        | Default: 0 |
| periods         | JSON       | Optional |
| statistics      | JSON       | Optional |
| created_at      | TIMESTAMP  | Default: CURRENT_TIMESTAMP |
| updated_at      | TIMESTAMP  | Default: CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP |


## Benefits of the New Database Schema

## Overview
- This new schema structure improves flexibility, simplifies external data handling, and enhances performance while avoiding primary key dependency.

### 1. Faster Data Insertion Without Foreign Key Dependencies
- By eliminating traditional primary key relationships and using only `external_id`, the new schema removes dependency on internal IDs, enabling faster insert operations without waiting for primary key lookups.

### 2. Simplified Relationships Without Strict Foreign Keys
- The schema avoids direct foreign key constraints, reducing relational complexity and making it easier to manage external data sources. This prevents cascading failures and allows independent table updates.


### 3. Optimized Storage with Proper Data Types
- Fields are correctly defined using `UNSIGNED` for IDs and status values, ensuring efficient storage and improved query performance.

### 4. Reduced Database Load by Avoiding Excessive Joins
- Without strict foreign key constraints, the system can perform lookups independently using `external_id`, reducing unnecessary joins and improving query efficiency.

## Table: `matches`
This table stores match-related data including teams, sport, location league, and score availability.

| Column Name              | Data Type           | Attributes                                       | Description |
|--------------------------|---------------------|-------------------------------------------------|-------------|
| `id`                     | BIGINT(20) UNSIGNED | PRIMARY KEY, AUTO_INCREMENT                     | Unique identifier for the match |
| `external_id`            | VARCHAR(191)        | NOT NULL, UNIQUE                                | External match ID from a third-party provider |
| `start_date`             | DATETIME            | NOT NULL                                        | Scheduled start date and time of the match |
| `end_date`               | DATETIME            | NULLABLE                                        | Actual end date and time of the match |
| `status`                 | TINYINT(2) UNSIGNED | NOT NULL                                        | Match status: 1 = Not Started, 2 = Running, 3 = Finished |
| `type`                   | TINYINT(1) UNSIGNED | DEFAULT 2                                       | 1 = In-play, 2 = Pre-match |
| `sport_id`               | VARCHAR(191)        | NOT NULL, INDEX                                 | External ID sport identifier |
| `sport_name`             | VARCHAR(100)        | NOT NULL                                        | Name of the sport (e.g., Football, Tennis) |
| `location_id`            | VARCHAR(191)        | NOT NULL, INDEX                                 | External ID location identifier |
| `location_name`          | VARCHAR(100)        | NOT NULL                                        | Name of the match location (e.g., country, stadium) |
| `league_id`              | VARCHAR(191)        | NOT NULL, INDEX                                 | External ID league identifier |
| `league_name`            | VARCHAR(150)        | NOT NULL                                        | Name of the league (e.g., Premier League) |
| `participant_home_id`    | VARCHAR(191)        | NOT NULL, INDEX                                 | External ID for the home team |
| `participant_home_name`  | VARCHAR(150)        | NOT NULL                                        | Name of the home team |
| `participant_home_score` | TINYINT(4) UNSIGNED | DEFAULT 0                                       | Score of the home team |
| `participant_away_id`    | VARCHAR(191)        | NOT NULL, INDEX                                 | External ID for the away team |
| `participant_away_name`  | VARCHAR(150)        | NOT NULL                                        | Name of the away team |
| `participant_away_score` | TINYINT(4) UNSIGNED | DEFAULT 0                                       | Score of the away team |
| `total_markets`          | SMALLINT(5) UNSIGNED| DEFAULT 0                                       | Number of betting markets available |
| `created_at`             | DATETIME            | NOT NULL, DEFAULT CURRENT_TIMESTAMP            | Timestamp when the match was created |
| `updated_at`             | DATETIME            | NOT NULL, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP | Timestamp of the last update |

## Key Changes & Optimizations

### ✅ Changed `sport_id`, `location_id`, `league_id`, `participant_home_id`, and `participant_away_id` to `VARCHAR(191)`
- This allows flexibility for numeric or alphanumeric values from third-party providers.
- 191 characters are chosen for MySQL indexing efficiency (especially for utf8mb4).

### ✅ Kept indexing for `sport_id`, `location_id`, `league_id`, `participant_home_id`, and `participant_away_id`
- Even though they're strings, indexing improves lookup performance.

### ✅ Ensured CHECK constraints for status and type
- Prevents invalid values from being inserted.

### ✅ Optimized text lengths
- VARCHAR(191) for IDs (flexible for different providers).
- VARCHAR(100-150) for names (to save space).

## Indexes

- **Primary Key**: id
- **Foreign Key-like Indexes**: sport_id, location_id, league_id, participant_home_id, participant_away_id
- **Unique Index**: external_id


## Table: `odds`
This table stores information about betting odds, including pricing, status, and related markets.

| Column Name            | Data Type           | Attributes                                       | Description |
|------------------------|---------------------|-------------------------------------------------|-------------|
| `id`                   | BIGINT(20) UNSIGNED | PRIMARY KEY, AUTO_INCREMENT                     | Unique identifier for the odds |
| `external_id`          | VARCHAR(191)        | NOT NULL, UNIQUE                                | External odds ID from a third-party provider |
| `name`                 | VARCHAR(150)        | NULLABLE                                        | Name of the odd type (e.g., "Over 2.5 Goals") |
| `line`                 | VARCHAR(150)        | NULLABLE                                        | Line of the odds (e.g., "2.5") |
| `baseline`             | VARCHAR(150)        | NULLABLE                                        | Baseline for the odds (if applicable) |
| `price_decimal`        | DOUBLE(6,2) UNSIGNED | NOT NULL CHECK (price_decimal >= 1.00) DEFAULT 1.00 | The decimal odds price (e.g., 2.50), always a positive value |
| `price_american`       | SMALLINT(5)         | NULLABLE                                        | The American odds price (e.g., +200, -150, +350) |
| `bet_status`           | TINYINT(1) UNSIGNED | DEFAULT 1                                       | Status of the bet: 1 = Open, 2 = Suspended, 3 = Settled |
| `bet_settlement_status` | TINYINT(2) UNSIGNED | DEFAULT 1                                       | Settlement status of the bet: 1 = Open, 2 = Winner, 3 = Half-Won, 4 = Lost, 5 = Half-Lost, 6 = Canceled/Refunded |
| `market_id`            | VARCHAR(191)        | NOT NULL, INDEX                                 | External ID market identifier |
| `market_name`          | VARCHAR(150)        | NOT NULL                                        | Name of the betting market (e.g., "Match Winner") |
| `match_id`            | BIGINT(20) UNSIGNED | NOT NULL, INDEX                                 | Reference to the match table |
| `created_at`           | DATETIME            | NOT NULL, DEFAULT CURRENT_TIMESTAMP            | Timestamp when the odds were created |
| `updated_at`           | DATETIME            | NOT NULL, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP | Timestamp of the last update |

## Usage
This schema is designed for a sportsbook application, optimizing data storage and access to ensure efficient queries for match details, betting odds, and related information.

## Key Changes & Optimizations

### ✅ Changed price to DOUBLE(6,2) with a minimum value check (>= 1.00)
- Ensures that odds prices remain valid.

### ✅ Kept `market_id` as VARCHAR(191) for flexibility
- Some providers may use alphanumeric market IDs.

### ✅ Added CHECK constraints for `bet_status` and `bet_settlement_status`
- Ensures only valid statuses are stored in the database.

### ✅ Indexed `market_id` and `match_id` for faster lookups
- Queries filtering by market_id or match_id will be more efficient.

### ✅ Added default values for `created_at` and `updated_at`
- Automates timestamp tracking.

## Indexes
- **Primary Key**: id
- **Foreign Key-like Indexes**: market_id, match_id
- **Unique Index**: external_id


