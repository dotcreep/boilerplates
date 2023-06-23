# Install

## Command

```
$ docker run -d --name jenkins --restart always -v /var/run/docker.sock:/var/run/docker.sock:ro -v jenkins:/var/jenkins_home <use-your-image>
```

## Compose
```
jenkins:
  image: <use-your-image>
  container_name: jenkins
  restart: always
  volumes:
    - /var/run/docker.sock:/var/run/docker.sock:ro
    - jenkins:/var/jenkins_home
```