# Medibank Coding Challenge (API)

## Coding Test Overview

<img src="https://i.imgur.com/7zxoGam.png" width="400" height="380">

An example web app has been setup and expect the following JSON response for the path `/api/article`:

```
{
    "title": "Fake doctor saved thousands of infants and changed medical history (2018)",
    "url": "https://nypost.com/2018/07/23/how-fake-docs-carnival-sideshow-brought-baby-incubators-to-main-stage",
    "author": "Alex3917",
    "imageUrl": "https://images.unsplash.com/photo-1560582861-45078880e48e?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=400&fit=max&ixid=eyJhcHBfaWQiOjE3NDkzNn0"
}
```

## Coding Test Requirements

You need to:

- Write some code and create your own web services to respond with JSON at the following path: `/api/article`.
- Use the hackers news api to randomly pick an article about `medicine` and use it for the returned JSON `title`, `url`, `author` fields.
- Use the unsplash api to randomly pick an image about `medicine` and use it for the returned JSON `imageUrl` field.
- For the duration of 1 hour return the same JSON content.
- Apply consideration if the API services to retrieve news articles and images has crashed or cannot respond.
- We want this service to be running in a docker container. (see the `docker-compose.yml` file)

## APIs

Use the following API to retrieve hacker news articles:

- https://hn.algolia.com/api

Use the following API to retrieve images from unsplash:

- https://unsplash.com/documentation

You can signup for a free account to get an unsplash access key.

## Docker

Here is the `docker-compose.yml` file for you to use (feel free to modify it to your liking):

```yaml
version: '3'

services:
  backend:
    image: 'YOUR_WEB_SERVICE_HERE'
    environment:
      UNSPLASH_ACCESS_KEY: 'YOUR_UNSPLASH_ACCESS_KEY_HERE'
    expose:
      - '3000'
  frontend:
    image: 'medibankdigital/hacker-card-frontend:latest'
    depends_on:
      - backend
    expose:
      - '3001'
  nginx:
    image: nginx:latest
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - frontend
    ports:
      - '4000:4000'
```

Here is the `nginx.conf`:

```
user  nginx;

events {
    worker_connections   1000;
}
http {
    server {
        listen 4000;

        location /api/ {
            proxy_pass http://backend:3000;
        }

        location / {
            proxy_pass http://frontend:3001;
        }
    }
}
```

## Please note:

- It can be written in any language you like, we mostly use Java at Medibank.
- Use any libraries/frameworks/SDKs of your choosing.
- Use industry best practices.
- Use the code to showcase your skill and what you value in a software application.
- Submissions will only be accepted via GitHub or Bitbucket.
- Avoid spending more than 3-5 hours to complete this assignment.
