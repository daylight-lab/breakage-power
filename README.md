# breakage power

## install

First, run Postgres in a container:

```sh
docker pull postgres

mkdir postgres-data

sudo docker run -d \
	--name dev-postgres \
	-e POSTGRES_PASSWORD=your-password-here\
	-v `pwd`/postgres-data/:/var/lib/postgresql/data \
    -p 5432:5432 \
    postgres
```

Then, restore from a recent taraaxtak dump:

```sh
mv taaraxtak_2021-06-20-00.00.01.sql postgres-data/
docker exec -it dev-postgres bash
$ cd /var/lib/postgresql/data
$ psql -U postgres < taaraxtak_2021-06-20-00.00.01.sql
```

Finally, set up the notebook:

```sh
python3 -m venv venv
source venv/bin/activate
pip3 install -r requirements.txt
```

Find the IP address of your docker container:

```sh
docker inspect dev-postgres -f "{{json .NetworkSettings.Networks.bridge.IPAddress }}" 
```

You'll need this to connect to the DB in the Jupyter notebook.
