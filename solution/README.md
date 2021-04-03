
### Lets Start To complete challenge.

### Pull docker images as required.

```bash
docker pull infracloudio/csvserver:latest
docker pull prom/prometheus:v2.22.0
```

### Error
Tried to start docker container but not able to start.
got below error

```bash
command:
docker run -d infracloudio/csvserver:latest
docker logs a767ae74dc55

output :
2021/04/03 06:17:12 error while reading the file "/csvserver/inputdata": open /csvserver/inputdata: no such file or directory

```

### Create shell script as required
i have created simple shell script to generated `inputFile`.

Script :
```bash
#!/usr/bin/env bash

for (( i = 0; i < ${1:-10}; i++ )); do
echo "$i, $RANDOM" >> inputFile
done

chmod +r inputFile
```

### Resolved issue with inputFile.
I have mounted inputFile in container by using `-v /tmp/csvserver/solution/inputFile:/csvserver/inputdata`


```bash
docker run -d -v /tmp/csvserver/solution/inputFile:/csvserver/inputdata infracloudio/csvserver:latest
```
Finally issue resolved. stopped running container and cleaned. as per direction in point 5.

To clean up :  `docker rm -vf $(docker ps -aq)`

### Make it accessible from host machine. (6.)
run the `docker ps` command to check docker ports. and found '9300/tcp'
 - exposed docker port on host machine port `9393`
 - exported environment variable `-e CSVSERVER_BORDER=Orange`

```bash
docker run -d -p 9393:9300 -e CSVSERVER_BORDER=Orange -v /tmp/csvserver/solution/inputFile:/csvserver/inputdata infracloudio/csvserver:latest
```
I executed above command successfully and able to access page on http://localhost:9393/. it is showing csv data and welcome note as exepected.
