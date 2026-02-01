# Docker
# install image of postgres with postgis extension 
docker pull postgis/postgis:15-3.3

# run images postgis 
docker run -d --name postgis-container \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  -e POSTGRES_DB=gisdb \
  -p 5432:5432 \
  postgis/postgis:15-3.3

# projet road in Madagascar 
wget https://download.geofabrik.de/africa/madagascar-latest.osm.pbf

# installer  convert ^.pbf into tables in container postgis
sudo apt install osm2pgsql
 
 # Connect to the PostgreSQL instance inside your container
docker exec -it postgis-container psql -U postgres -d gisdb

-- Inside the psql shell, run:
CREATE EXTENSION IF NOT EXISTS postgis;
CREATE EXTENSION IF NOT EXISTS hstore; -- Useful for storing OpenStreetMap tags as key-value pairs[citation:1][citation:6]


# 1. Get a bash shell inside your container
docker exec -it postgis-container bash

# 2. Once inside the container shell, update packages and install osm2pgsql
apt-get update && apt-get install -y osm2pgsql


# This command is run INSIDE the container's bash shell
osm2pgsql -d gisdb -U postgres -H localhost -P 5432 --create --slim --hstore /tmp/madagascar-latest.osm.pbf