# Example: Football Analytics with PromptQL

This is a simple example of exposing [PromptQL](https://promptql.hasura.io/) to the most recent play-by-play data available for the NFL.
This example illustrates taking a large data set and conversing with it using PromptQL.

You can request access to a deployed version of this project [here](https://console.hasura.io/project/saved-kitten-4996)!

## Getting Started

> [!NOTE]
> Before you begin, make sure you have the latest version of the DDN CLI installed. You can install the CLI by
> following the instructions in the [CLI documentation](https://promptql.hasura.io/docs/installation).

### Step 1. Update the CLI to the latest release

```bash
ddn update-cli
```

### Step 2. Clone this repo

Using http:

```bash
git clone https://github.com/robertjdominguez/example-promptql-football-analytics.git
```

or using ssh:

```bash
git clone git@github.com:robertjdominguez/example-promptql-football-analytics.git
```

Add your Anthropic API key to the `.env` file:

```bash
echo "ANTHROPIC_API_KEY=your-anthropic-api-key" >> .env
```

> [!NOTE]
> You can generate an API key [here](https://console.anthropic.com/settings/keys).

### Step 3. Initialize a DDN project

```bash
ddn project init
```

### Step 4. Load the data into PostgreSQL

```bash
cd example-promptql-football-analytics
psql "<YOUR_DB_STRING>" -c "\copy plays FROM './pbp-2024.csv' DELIMITER ',' CSV HEADER;"
```

### Step 5. Add the PostgreSQL connector

```bash
ddn connector init mypostgres -i
```

```bash
HINT To access the local Postgres database:
- Run: docker compose -f app/connector/mypostgres/compose.postgres-adminer.yaml up -d
- Open Adminer in your browser at http://localhost:9822 and create tables
- To connect to the database using other clients use postgresql://user:password@local.hasura.dev:8105/dev
```

### Step 6. Introspect the data source

```bash
ddn connector introspect mypostgres
```

### Step 7. Add the `Plays` table as a model

```bash
ddn model add mypostgres '*'
```

### Step 8. Start PromptQL

First, create a build:

```bash
ddn supergraph build local
```

Then, start your local services:

```bash
ddn run docker-start
```

### Step 9. Open the console

In another terminal window, run the following command from your project's directory to open the console:

```bash
ddn console --local
```

### Step 10. Talk to the data

From the console, try asking a question, like:

```plaintext
> What formation are the Saints most likely to use on 3rd down?
```
