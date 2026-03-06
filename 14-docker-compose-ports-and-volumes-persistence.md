# Chapter 14 — Docker Compose: Ports + Volumes + Persistence (MongoDB Links Demo)

Now we make the example more realistic:
- our Python app writes a document into MongoDB
- we expose MongoDB port so we can connect from our host using MongoDB Compass
- we persist MongoDB data using a volume

## Updated Python app: insert + read links
Example `main.py`:

```python
from pymongo import MongoClient
from pprint import pprint

url = "mongodb://mongo:27017"
client = MongoClient(url)

db = client["records"]
links = db["links"]

links.insert_one({"link": "https://stashchuk.com"})
all_links = links.find()

for link in all_links:
    pprint(link)
```

This inserts one record and prints all documents.

## Expose MongoDB port using docker-compose.yml
Add ports to the mongo service:

```yaml
version: "3"
services:
  app:
    build: ./app
  mongo:
    image: mongo
    ports:
      - "27017:27017"
```

Now you can connect from your host to MongoDB at:
- `mongodb://localhost:27017`

### Run with rebuild when code changed
If you changed app code, run:
```bash
docker compose up --build
```

You should see Python app output and it exits.

## Why data disappears without volume
If you do:
```bash
docker compose down
docker compose up
```
MongoDB container is recreated, so data disappears (new empty DB).

## Persist MongoDB data with a named volume
Mongo stores data inside the container at:
- `/data/db`

We will mount a named volume there.

Update docker-compose.yml:

```yaml
version: "3"
services:
  app:
    build: ./app
  mongo:
    image: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db

volumes:
  mongo_data:
```

Meaning:
- create a named volume `mongo_data` on the Docker host
- mount it into the container at `/data/db`

Now data survives container recreation.

### Demo behavior
1) `docker compose up` inserts one record → you see 1 record printed
2) `docker compose down`
3) `docker compose up` inserts again → now you see 2 records (previous persisted + new)
4) repeat → records keep increasing

## Check volumes
List volumes:
```bash
docker volume ls
```

Compose names volumes with a prefix (project folder name), so you may see something like:
`<project>_mongo_data`

That volume lives on the Docker host, not in the container.

## Quick recap of Compose
- `docker compose up`: create network, start services
- `docker compose down`: remove containers and network
- named volumes keep persistent data across container recreation

That’s the end of this crash course. Next, I’ll add optional chapter: “GitHub publish” (how the instructor pushed examples).
