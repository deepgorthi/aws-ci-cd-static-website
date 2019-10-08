# Static Jekyll Website on AWS with CI/CD pipeline


## Jekyll Website

```
                    [CloudFormation Template]
|---------------------------------------------------------------------|
|           (Resolve Domain name)                                     |
|         |-->[Amazon Route53]                                        |
|         |                               (Get contents)              |
|         |                     |-->[Amazon S3 contents Bucket]       |
| Users---|-->[AWS CloudFront]--|                                     |
|         |                     |-->[Amazon S3 Logging Bucket]        |
|         |                               (Save Logs)                 |
|         |-->[AWS Certificate Manager]                               |
|---------------------------------------------------------------------|
```


## AWS CI/CD Pipeline

```
                    [CloudFormation Template]
|---------------------------------------------------------------------------|
|[NewCodeChange] --> [Pushed to master branch] --> [AWS CodePipeline]       |     
|                                                            |              |
|                                                            |              |
|          Published to S3 <-- Build Jekyll Website <-- [AWS CodeBuild]     |
|                    |                                                      |
|                    |                                                      |
|     Lambda function triggered to invalidate CloudFront Distribution       |
|  (Old files removed from CloudFront edge before the actual expiration)    |
|---------------------------------------------------------------------------|
```

# Running Jekyll in Docker container

Jekyll might require a different version of ruby and gem than the one installed on your computer and when working on MacOSX, it is difficult to work with a different version than the one already pre-installed. Rather than corrupting the PATH and ruby versions, it is best to run jekyll in a container. 

Here are the following steps to setup and work with jekyll in a container. 

```bash
$ echo 'JEKYLL_VERSION=4.0' >> ~/.bash_profile
```

Create a project directory to hold the files and directories for jekyll.

```bash
$ mkdir -p ~/jekyll-dev/new-blog
$ cd ~/jekyll-dev/new-blog
```

Run docker using the following command:

```bash
$ docker run --rm --volume="$PWD:/srv/jekyll" -it jekyll/jekyll:$JEKYLL_VERSION jekyll new .
```

To build the site, use the following command:

```bash
$ docker run --rm --volume="$PWD:/srv/jekyll" -it jekyll/jekyll:$JEKYLL_VERSION jekyll build
```

To run the site locally:

```bash
$ docker run --name deepgorthi --volume="$PWD:/srv/jekyll" -p 4000:4000 -it jekyll/jekyll:$JEKYLL_VERSION jekyll serve --watch --drafts
```

The website can be locally accessed using `http://localhost:4000`


To execute commands in the container:

```bash
$ docker exec -ti deepgorthi gem install "jekyll-theme-example"
```

Executing a shell in the container:

```bash
$ docker exec -ti deepgorthi /bin/sh
```

Removing container:

```bash
docker rm -f deepgorthi
```

After making any changes to the jekyll website, you can either build jekyll within docker using:

```bash
$ docker run --name deepgorthi --volume="$PWD:/srv/jekyll" -p 4000:4000 -it jekyll/jekyll:$JEKYLL_VERSION jekyll serve --watch --drafts
```

Or, you can restart docker container using:

```bash
$ docker restart deepgorthi
```
