# Chapter 11 — Dockerfile: Build a Custom Image (Python + pymongo example)

If your app needs dependencies that are not in the base image, you build your own image.

We will build a Python image that installs `pymongo` and runs a script that connects to MongoDB.

## The application (main.py)
We need `pymongo`, which is not installed by default.

Example `main.py`:
```python
from pymongo import MongoClient
from pprint import pprint

url = "mongodb://mongo:27017"

client = MongoClient(url)
dbs = client.list_database_names()
pprint(dbs)
```

Notice: the hostname in the URL is `mongo`. That will be the *container name / service name* of MongoDB.

If you try to run this locally without installing pymongo, you get:
`ModuleNotFoundError: No module named 'pymongo'`

## Build a custom image using Dockerfile

Create `Dockerfile`:

```dockerfile
FROM python:alpine

WORKDIR /app

RUN pip install pymongo

COPY . /app

CMD ["python", "main.py"]
```

Meaning:
- Start from base image `python:alpine`
- Set working directory `/app`
- Install dependency `pymongo`
- Copy your app files into the image
- Set default command: run `python main.py`

## Build the image
From the folder containing Dockerfile:
```bash
docker build . -t python-custom
```

Then:
```bash
docker images
```

You should see:
- `python-custom:latest`

Docker build creates a new layer per instruction (WORKDIR, RUN, COPY, etc.) on top of base image layers.

## Run the container (it will fail without MongoDB)
Try:
```bash
docker run -it python-custom
```

You’ll likely get:
- “name does not resolve”
because `mongo` hostname is not resolvable unless we run MongoDB and connect containers properly.

Next, we start MongoDB and connect the containers using a Docker network.
