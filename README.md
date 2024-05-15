A workout tracking web application for personal use with everything included.

## Getting started

### Docker

Run the latest image from GitHub Container Registry (latest and release images
are available for amd64 and arm64). The current directory is mounted as the data
directory.

```bash
# Latest master build
docker run -p 8080:8080 -v .:/data ghcr.io/jovandeginste/workout-tracker:latest

# Tagged release
docker run -p 8080:8080 -v .:/data ghcr.io/jovandeginste/workout-tracker:0.11.3
docker run -p 8080:8080 -v .:/data ghcr.io/jovandeginste/workout-tracker:0.11
docker run -p 8080:8080 -v .:/data ghcr.io/jovandeginste/workout-tracker:0

# Latest release
docker run -p 8080:8080 -v .:/data ghcr.io/jovandeginste/workout-tracker:release

# Run as non-root user; make sure . is owned by uid 1000
docker run -p 8080:8080 -v .:/data -u 1000:1000 ghcr.io/jovandeginste/workout-tracker
```

Open your browser at `http://localhost:8080`

To persist data and sessions, run:

```bash
docker run -p 8080:8080 \
    -e WT_JWT_ENCRYPTION_KEY=my-secret-key \
    -v $PWD/data:/data \
    ghcr.io/jovandeginste/workout-tracker:master
```

or use docker compose

```bash
# Create directory that stores your data
mkdir -p /opt/workout-tracker
cd /opt/workout-tracker

# Download the compose.yaml
curl https://raw.githubusercontent.com/jovandeginste/workout-tracker/master/compose.yaml --output compose.yaml

# Start the server
docker compose up -d
```


After starting the server, you can access it at <http://localhost:8080> (the
default port). A login form is shown.

If no users are in the database (eg. when starting with an empty database), a
default `admin` user is created with password `admin`. You should change this
password in a production environment.

## API usage

The API is documented using [swagger](https://swagger.io/). You must enable API access for your user, and copy the API key. You can use the API key as a query parameter (`api-key=${API_KEY}`) or as a header (`Authorization: Bearer ${API_KEY}`).

You can configure some tools to automatically upload files to Workout Tracker, using the `POST /api/v1/import/$program` API endpoint.

### FitoTrack

Read [their documentation](https://codeberg.org/jannis/FitoTrack/wiki/Auto-Export) before you continue.

The path to POST to is: `/api/v1/import/fitotrack?api-key=${API_KEY}`

## Development

### Build and run it yourself

- install go
- clone the repository

```bash
go build ./
./workout-tracker
```

This does not require npm or Tailwind, since the compiled css is included in the
repository.

### Do some development

You need to install Golang and npm.

Because I keep forgetting how to build every component, I created a Makefile.

```bash
# Make everything. This is also the default target.
make all # Run tests and build all components

# Install system dependencies
make install-deps

# Testing
make test # Runs all the tests
make test-assets test-go # Run tests for the individual components
