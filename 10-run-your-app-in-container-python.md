# Chapter 10 — Run Your Own App Inside a Container (Python Example)

Now we run a simple Python program inside a container. This is a very common workflow:
- you have code on your laptop
- you run it inside a container using an official runtime image

## Example: a simple Python script
Create `main.py` in a folder like `python-app/`.

Example (sum numbers):
```python
n = int(input("Please enter any positive number: "))

if n < 0:
    print("Error: number must be positive")
else:
    total = 0
    i = n
    while i > 0:
        total += i
        i -= 1

    print(f"Sum of all positive numbers till {n} is {total}")
```

You can run it locally if Python is installed:
```bash
python3 main.py
```

## Run it in a container using the official Python image

The idea:
- map your folder into `/app` inside the container
- run python on the file inside `/app`

From inside your project folder:
```bash
docker run -it -v ${PWD}:/app python:alpine python /app/main.py
```

What happens:
- Docker pulls `python:alpine` if missing
- Docker maps your folder into the container at `/app`
- Docker runs `python /app/main.py`
- you see the prompt and enter input
- when the script ends, container stops (single-purpose container)

Check:
```bash
docker ps
# empty (script ended)
docker ps -a
# shows exited container
```

This works because the script has no extra dependencies.

Next, we handle the real case: your app needs dependencies, so we build a custom image using a Dockerfile.
