# Chapter 8 — Environment Variables (-e) Inside Containers (Node.js Example)

Many apps need configuration at startup: usernames, passwords, database names, etc. Docker supports this using **environment variables**.

## Setting environment variables with docker run
Syntax:
```bash
docker run -e KEY=value <image>
```

You can repeat `-e` multiple times.

Example shown in the course (MySQL style):
- MYSQL_USER
- MYSQL_PASSWORD
- MYSQL_ROOT_PASSWORD
- MYSQL_DATABASE

Environment variables are usually UPPERCASE with underscores.

## Practice with Node.js image

Pull and run a Node container (Alpine tag is smaller):
```bash
docker run -d -it node:alpine
```

Important detail: if you run `docker run node:alpine` without a long-running process, it may exit immediately. That’s why we often use `-it` for interactive containers.

Check containers:
```bash
docker ps
docker ps -a
```

### Enter the container
```bash
docker exec -it <CONTAINER_ID> sh
```

### Start Node REPL and read env variables
Inside container:
```bash
node
```

Then inside Node:
```js
process.env
```

You will see many environment variables.

Exit Node REPL:
- Ctrl+C twice

Exit container shell:
```bash
exit
```

## Inject your own environment variable
Stop/remove the old container:
```bash
docker stop <ID>
```

Run with `-e`:
```bash
docker run -d -it -e MY_TEST_ENV=myname node:alpine
```

Enter container again:
```bash
docker exec -it <ID> sh
node
process.env.MY_TEST_ENV
```

You should see your value.

Note: environment variable values are strings. If your app needs numbers, it must convert them.

Next, we learn volumes (volume mapping) and use nginx HTML override as the demo.
