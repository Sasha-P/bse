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

Specify path to folder with books by replacing `/path/to/folder/` in docker-compose.yml

```
...

  web:
    build: .
    command: ./run_web.sh
    volumes:
      - ./src:/falcon/bse
      - /path/to/folder/:/opt/books/
    ports:
      - "8000:8000"
    depends_on:
      - elasticsearch

...
```
Run build command

```
docker-compose build
```

Run
---

Run composition

```
docker-compose up
```

Install mapper-attachments plugin on elasticsearch container

```
docker exec bse_elasticsearch_1 bin/plugin install mapper-attachments
```

Restart elasticsearch container

```
docker restart bse_elasticsearch_1
```

Add books to Elasticsearch

```
docker exec bse_web_1 /bin/bash -c 'python es.py /opt/books/'
```

Go to page and do search

```
http://localhost:8000/
```

Observe logs

```
docker exec bse_web_1 cat requests.log
```

Cleanup

```
docker-compose down && docker rmi bse_web bse_celery
```

License
-------

MIT