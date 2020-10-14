#eea.docker.guacamole

run the container with docker-compose:

# git clone <reponame>
# cd <reponame>
# mv docker-compose_example.yml docker-compose.yml
# docker-compose up -d
```

## Release to production

The production deployment is not made with `git clone` and `docker-compose build`.
Instead it pulls a tagged image from Docker Hub.  When you have tested your changes
and are satisfied, then you must push a new image up with a new version number that
follows [semantic versioning](http://semver.org/) principles.  Here is how you do it:

    edit VERSION.txt
    git commit VERSION.txt
    version=$(cat VERSION.txt)
    git tag -a $version -m "Tagging the $version release of the 'guacamole' Docker image."
    git push origin $version

    git push origin master
    
    in case there's no automatic build on docker hub:
    docker build -t eeacms/eea-guacamole:latest guacamole
    docker tag eeacms/eea-guacamole:latest eeacms/eea-guacamole:$version
    docker login --username=<user username> --email=<user email address>
    docker push eeacms/eea-guacamole:latest
    docker push eeacms/eea-guacamole:$version

The purpose of the procedure is to be able to redeploy the exact same image on a
new host, and to be able to roll back one or more versions if the deployment has problems.

