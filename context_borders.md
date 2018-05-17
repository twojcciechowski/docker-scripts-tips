# Pass tarball as context

## Problem
While building an image docker is packaging directory pointed as context. 

Let's go deeper. Would you like to have a structure like this:

```
    |->project
        |->src
            |->file1
            |->file2
    |->docker-images
        |->Dockerfile
```

While building an image you want to run 

```
docker build . 
```

In above command the `.` argument set context to `/docker-images/` so 
you cannot operate on `/project/*` part of the project, and you need 
to pass complicated arguments and paths in docker build command. There is 
other way.

## Solution

To pass files to docker you can use tarball. In that case you need to build 
a tarball with all files which you want to pass as context to docker. Below 
is command showing how you can pass tarball context into docker.

```
cd /docker-images && \
tar czv -f package.tar.gz ../project/src Dockerfile  && \
docker build - < package.tar.gz &&\
rm -rf package.tar.gz
``` 

## What is happening in above command:

* First, we go to `/docker-images` path. This is clear.


* Next we tar all files which we want to pass to docker. 
In this example that will be `../project/src` and `Dockerfile`. 
Read next points to see why **adding dockerfile is necessary**.

* Next, we run docker build process and pass `package.tar.gz` as context.
 Docker will extract this archive and it 
will get structure like 
this as context to build image:
```
    |->src
        |->file1
        |->file2
    |->Dockerfile
```


* Last command is just cleaning.


Main reason why I am publishing this is that sometimes paths `/project` and `/docker-images` are placed in two 
separated repositories. In that way you dont want to 
