
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


### Part 2

Create `docker-compose.yml`

```bash
version: "3.9"

services:
  #Part - 2
  csvserver:
    hostname: csvserver
    image: infracloudio/csvserver:latest
    ports:
      - "9393:9300"
    volumes:
      - ./inputFile:/csvserver/inputdata
    environment:
      - CSVSERVER_BORDER=Orange
```

Start container

```bash
# To start
docker-compose up -d

# To down
docker-compose down
```


### Part 3

update prometheus configs in `docker-compose.yml`

```bash
version: "3.9"

services:
  #Part - 2
  csvserver:
    hostname: csvserver
    image: infracloudio/csvserver:latest
    ports:
      - "9393:9300"
    volumes:
      - ./inputFile:/csvserver/inputdata
    environment:
      - CSVSERVER_BORDER=Orange

  # Part - 3
  prometheus:
    image: prom/prometheus:v2.22.0
    hostname: prometheus
    ports:
      - 9090:9090
    volumes:
      - ./prometheus:/etc/prometheus
    command: --web.enable-lifecycle  --config.file=/etc/prometheus/prometheus.yml      
```

Create `prometheus/prometheus.yml`
```bash
global:
  scrape_interval: 30s
  scrape_timeout: 10s

scrape_configs:
  - job_name: csvserver
    metrics_path: /metrics
    static_configs:
      - targets:
          - 'prometheus:9090'
          - 'csvserver:9300'
```


Lets Start Container again.

```bash
# To start
docker-compose up -d

# To down
docker-compose down
```

 After starting the container. you will able to access prometheus page. http://localhost:9090/graph





## Thanks
**H**arry - **T**he **D**ev**O**ps **G**uy

Please visit following links to explore more about me
- **Portfolio** : https://harrythedevopsguy.github.io/
- **Github** : https://github.com/HarryTheDevOpsGuy?tab=repositories
- **Medium** : https://harrythedevopsguy.medium.com/
