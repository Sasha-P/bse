Books Search Engine
===================

Books search engine powered by Elasticserch.

Built on Falcon minimalistic Python framework.

Requirements
------------

For Python requirements see

```
./requirements.txt
```

Mapper Attachments Type for Elasticsearch plugin

Redis

Build
-----

```
docker build -t falcon .
```

Run Step-by-step
----------------

#### Run containers

```
docker run --name redis -d redis

docker run --name elasticsearch -d elasticsearch

docker exec elasticsearch bin/plugin install mapper-attachments

docker stop elasticsearch && docker start elasticsearch
```

Replace `/path/to/folder/` to real path

```
docker run --name falcon -v /path/to/folder/:/opt/books/ --link redis:REDIS --link elasticsearch:ELASTICSEARCH -it -p 8000:8000 falcon /bin/bash

gunicorn -b 0.0.0.0:8000 bse:app &
```

#### Add books

```
python es.py /opt/books/
```

#### Go to page and do search

```
http://localhost:8000/
```

#### Run workers

```
python search_task.py

python log_task.py
```

#### Observe logs

```
cat requests.log
```

#### Cleanup

```
docker stop elasticsearch redis falcon

docker rm elasticsearch redis falcon
```

Run shortcut
------------

#### Run containers

```
docker run --name redis -d redis && docker run --name elasticsearch -d elasticsearch && docker exec elasticsearch bin/plugin install mapper-attachments && docker stop elasticsearch && docker start elasticsearch
```

Replace `/path/to/folder/` to real path

```
docker run --name falcon -v /path/to/folder/:/opt/books/ --link redis:REDIS --link elasticsearch:ELASTICSEARCH -it -p 8000:8000 falcon /bin/bash 
```

#### Run Gunicorn and Add books

```
gunicorn -b 0.0.0.0:8000 bse:app & python es.py /opt/books/
```

#### Go to page and do search

```
http://localhost:8000/
```

#### Run workers and Observe logs

```
python search_task.py && python log_task.py && cat requests.log
```


Debug
-----

```
docker run --name kibana --link elasticsearch:elasticsearch -d kibana

docker exec kibana /opt/kibana/bin/kibana plugin --install elastic/sense

docker stop kibana && docker start kibana
```

License
-------

MIT