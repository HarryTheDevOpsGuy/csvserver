
### Lets Start To complete challenge.

### Pull docker images

```bash
docker pull infracloudio/csvserver:latest
docker pull prom/prometheus:v2.22.0
```

# Clone Repo
```bash
  git clone git@github.com:HarryTheDevOpsGuy/csvserver.git
  cd csvserver/solution
```

### Create shell script as required
Create shell script `solution/getcsv.sh` to generated `inputFile`.

```bash
# vim getcsv.sh

#!/usr/bin/env bash

for (( i = 0; i < ${1:-10}; i++ )); do
echo "$i, $RANDOM" >> inputFile
done
chmod +r inputFile
```
Run getcsv.sh to generate data
```
# By default it will generate 10 records
 bash getcsv.sh

# generate 10000 records : pass argument as below.
 bash getcsv.sh 10000
```

### Start container with inputFile.

* Please replace `/tmp/csvserver/solution/inputFile` path according to your machine.
* Run below command to start container.

```bash
docker run -d -p 9393:9300 -e CSVSERVER_BORDER=Orange -v /tmp/csvserver/solution/inputFile:/csvserver/inputdata infracloudio/csvserver:latest
```
Try to access http://localhost:9393/
