# Action to establish a connection with and test Docker Postgres database.

## Prerequisites

### Docker Postgres image
#### Needed:
- Docker image must be present (see example)
- Docker image must have a POSTGRES_PASSWORD variable
#### Optional environment variables:
- POSTGRES_USER (defaults to 'postgres')
- POSTGRES_PASSWORD (defaults to 'postgres')
- POSTGRES_DB (defaults to 'postgres')

### Step action variables
#### Needed:
- POSTGRES_HOST: localhost
- POSTGRES_PORT: 5432
#### Optional action parameters (correspond to those set in Docker image):
- POSTGRES_USER (defaults to 'postgres')
- POSTGRES_PASSWORD (defaults to 'postgres')
- POSTGRES_DB (defaults to 'postgres')

## Usage examples

### Simplest usage:

```yml
name: PostgreSQL test
on: push

jobs:

    myjob:
    
    runs-on: ubuntu-latest

    services:
    
        postgres:
        # Docker Hub image
        image: postgres
        env:
            POSTGRES_PASSWORD: postgres
            # Set health checks to wait until postgres has started
        options: >-
            --health-cmd pg_isready
            --health-interval 10s
            --health-timeout 5s
            --health-retries 5
        ports:
            # Maps tcp port 5432 on service container to the host
            - 5432:5432

    steps:
        # Downloads a copy of the code in your repository
        - name: Check out repository code
        uses: actions/checkout@v2
        - name: Create, populate & query PostgreSQL
        # Runs a script that creates a PostgreSQL client, populates
        # the client with data, and retrieves data
        uses: Oxfos/action-postgres-setup@master
        # Environment variables used by the `index.js` script to create
        # a new PostgreSQL client.
        with:
            # The hostname used to communicate with the PostgreSQL service container
            POSTGRES_HOST: localhost
            # The default PostgreSQL port
            POSTGRES_PORT: 5432

        # whatever additional steps you may want to add here
        - name: name of your step
            # ...

```

### Custom usage (no real need):

```yml
name: PostgreSQL test
on: push

jobs:

    myjob:
    
    runs-on: ubuntu-latest

    services:
    
        postgres:
        # Docker Hub image
        image: postgres
        env:
            POSTGRES_PASSWORD: postgres
            POSTGRES_USER: your_user
            POSTGRES_PASSWORD: my_password
            POSTGRES_DB: my_db
            # Set health checks to wait until postgres has started
        options: >-
            --health-cmd pg_isready
            --health-interval 10s
            --health-timeout 5s
            --health-retries 5
        ports:
            # Maps tcp port 5432 on service container to the host
            - 5432:5432

    steps:
        # Downloads a copy of the code in your repository
        - name: Check out repository code
        uses: actions/checkout@v2
        - name: Create, populate & query PostgreSQL
        # Runs a script that creates a PostgreSQL client, populates
        # the client with data, and retrieves data
        uses: Oxfos/action-postgres-setup@master
        # Environment variables used by the `index.js` script to create
        # a new PostgreSQL client.
        with:
            # The hostname used to communicate with the PostgreSQL service container
            POSTGRES_HOST: localhost
            # The default PostgreSQL port
            POSTGRES_PORT: 5432
            # Other parameters to access database
            POSTGRES_USER: your_user
            POSTGRES_PASSWORD: my_password
            POSTGRES_DB: my_db

        # whatever additional steps you may want to add here
        - name: name of your step
            # ...

```