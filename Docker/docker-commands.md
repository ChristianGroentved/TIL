# Docker commands

To mount a file you can use the following
```shell
docker run --mount type=bind,src="$(pwd)"/some_file.json,target=/some_file.json <IMAGE>
```
And the file will be avaliable inside the docker container.