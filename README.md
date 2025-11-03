# Gator - RSS Feed Aggregator CLI

A command-line RSS feed aggregator built with Go and PostgreSQL. Gator allows you to follow RSS feeds, fetch posts automatically, and browse them in your terminal.

## Prerequisites

- **Go** (version 1.19 or higher) - [Install Go](https://golang.org/doc/install)
- **PostgreSQL** - [Install PostgreSQL](https://www.postgresql.org/download/)

## Installation

Install the gator CLI using Go:

```bash
go install github.com/kostyakozko/gator@latest
```

## Setup

### 1. Create PostgreSQL Database

```bash
createdb gator
```

### 2. Configure Gator

Create a configuration file at `~/.gatorconfig.json`:

```json
{
  "db_url": "postgres://your_username:@localhost:5432/gator?sslmode=disable",
  "current_user_name": ""
}
```

Replace `your_username` with your PostgreSQL username.

### 3. Run Migrations

Install goose for database migrations:

```bash
go install github.com/pressly/goose/v3/cmd/goose@latest
```

Navigate to the project directory and run migrations:

```bash
cd sql/schema
goose postgres "postgres://your_username:@localhost:5432/gator" up
```

## Usage

### User Management

**Register a new user:**
```bash
gator register <name>
```

**Login as a user:**
```bash
gator login <username>
```

**List all users:**
```bash
gator users
```

### Feed Management

**Add a new feed:**
```bash
gator addfeed <name> <url>
```

Example:
```bash
gator addfeed "Boot.dev Blog" https://blog.boot.dev/index.xml
```

**List all feeds:**
```bash
gator feeds
```

**Follow an existing feed:**
```bash
gator follow <url>
```

**Unfollow a feed:**
```bash
gator unfollow <url>
```

**List feeds you're following:**
```bash
gator following
```

### Aggregation

**Start the aggregator (fetches feeds continuously):**
```bash
gator agg <time_between_requests>
```

Example (fetch every minute):
```bash
gator agg 1m
```

The aggregator runs continuously. Press `Ctrl+C` to stop.

### Browse Posts

**View recent posts from feeds you follow:**
```bash
gator browse [limit]
```

Examples:
```bash
gator browse      # Shows 2 most recent posts (default)
gator browse 10   # Shows 10 most recent posts
```

### Database Reset

**Delete all users (and their feeds/follows):**
```bash
gator reset
```

## Example Workflow

```bash
# Register and login
gator register alice
gator login alice

# Add some feeds
gator addfeed "TechCrunch" https://techcrunch.com/feed/
gator addfeed "Hacker News" https://news.ycombinator.com/rss

# Start aggregating in the background
gator agg 5m &

# Browse posts
gator browse 5
```

## Recommended RSS Feeds

- TechCrunch: https://techcrunch.com/feed/
- Hacker News: https://news.ycombinator.com/rss
- Boot.dev Blog: https://blog.boot.dev/index.xml

## Project Structure

```
gator/
├── main.go                    # Main application code
├── internal/
│   ├── config/               # Configuration management
│   └── database/             # Generated database code (sqlc)
├── sql/
│   ├── schema/               # Database migrations
│   └── queries/              # SQL queries
└── sqlc.yaml                 # SQLC configuration
```

## Technologies Used

- **Go** - Programming language
- **PostgreSQL** - Database
- **SQLC** - Type-safe SQL code generation
- **Goose** - Database migrations

## License

MIT
