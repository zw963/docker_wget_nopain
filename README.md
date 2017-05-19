# docker_wget_nopain
Create a wget function in docker, and use it download static compile wget.


Does you use Dockerfile like this:

```Dockerfile

# install wget
RUN  apt-get update && apt-get install -qq -y --no-install-recommends wget && rm -rf /var/lib/apt/lists/*

#####################

# do whatever tasks which need wget to download somethings ..

######################

# remove wget
RUN apt-get purge -y --auto-remove wget

```

This is not the correct way to do this.
If run `docker history your_image``, you will see

```sh
...
41753cb4a6be        4 hours ago         /bin/sh -c apt-get purge -y --auto-remove ...   445 kB              
1eae68198132        17 hours ago        /bin/sh -c apt-get update && apt-get insta...   34.4 MB
...
```

Because you `install wget` and `remove wget` in seperate step, you got about `34MB` unuseful data
into docker layout.

Another way to do this is, in every step you need wget, write as followings

```dockerfile
RUN apt-get update && apt-get install -qq -y --no-install-recommends wget && rm -rf /var/lib/apt/lists/* && \
         do-what-ever-process-need-wget && \
         RUN apt-get purge -y --auto-remove wget
```

This method exist too much redundant code to process wget, not perfect.


## Why use bash to download a static compiled wget, and just use it?

This project is the attempt to do this.











   
