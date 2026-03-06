# Chapter 9 — Volumes (-v): Share Files Between Host and Container (Nginx HTML Demo)

Volumes are one of the most used Docker features.

They let you map a folder:
- from your host to a container (common for development)
- or from the Docker host to a container (common for databases / persistence)

## Two common volume use cases

### 1) Development: map your project folder into a container
You write code on your computer, but run it inside a container.  
So you map your code folder into the container.

### 2) Persistence: map database data folder to Docker host storage
Containers are disposable. If you remove a DB container, data inside it is gone.  
To keep DB data, you map container’s DB folder to a host volume.

## Volume mapping syntax (-v)
Basic syntax:
```bash
docker run -v <hostPath>:<containerPath> <image>
```

On macOS/Linux you often use `$(pwd)` to get the current folder:
```bash
docker run -v $(pwd)/html:/usr/share/nginx/html nginx
```

In the course, the example uses `${PWD}` (works in many shells):
```bash
-v ${PWD}/html:/usr/share/nginx/html
```

## Practice: override nginx default page

### Step 1: Create files on your computer
Create this folder structure:

- docker-examples/
  - nginx/
    - html/
      - index.html

Example `index.html`:
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Nginx Demo</title>
  </head>
  <body>
    <h1>Welcome to custom nginx webpage</h1>
  </body>
</html>
```

### Step 2: Run nginx with port mapping + volume mapping
In your terminal, `cd` into the nginx folder (so `${PWD}` points correctly):

```bash
cd docker-examples/nginx
docker run -d -p 8888:80 -v ${PWD}/html:/usr/share/nginx/html nginx
```

If port 8888 is taken, choose another host port.

### Step 3: Test in browser
Open:
- `http://localhost:8888`

You should see your custom page.

### Step 4: Live editing
Edit `index.html` on your host and refresh the browser. Changes appear immediately because the container is reading from the mapped folder.

If you rename index.html, nginx may show Forbidden because it can’t find the default index page.

### Run two nginx containers using same volume (optional)
You can run another container with a different host port but same volume:
```bash
docker run -d -p 8889:80 -v ${PWD}/html:/usr/share/nginx/html nginx
```

Both serve the same files because they share the same host folder.

Next, we run a Python app inside Docker using volume mapping, then move to Dockerfiles and custom images.
