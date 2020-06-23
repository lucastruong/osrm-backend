docker container stop $(docker container ls -aq)
docker container rm $(docker container ls -aq)
docker rmi $(docker images -qa --digests)

wget https://download.geofabrik.de/asia/vietnam-latest.osm.pbf

docker run -t -v "./osrm-backend/data:/data" osrm/osrm-backend osrm-extract -p /opt/car.lua /data/vietnam-latest.osm.pbf

docker run -t -v "./osrm-backend/data:/data" osrm/osrm-backend osrm-partition /data/vietnam-latest.osrm

docker run -t -v "./osrm-backend/data:/data" osrm/osrm-backend osrm-customize /data/vietnam-latest.osrm

docker run -t -i -p 5000:5000 -v "./osrm-backend/data:/data" osrm/osrm-backend osrm-routed --algorithm mld /data/vietnam-latest.osrm

coordinates: String of format {longitude},{latitude};
http://127.0.0.1:5000/route/v1/driving/106.774328,10.828934;106.673375,10.797622

https://developers.google.com/maps/documentation/utilities/polylineutility

sudo apt-get install osmium-tool
osmium cat new-york.osm.pbf new-jersey.osm.pbf connecticut.osm.pbf -o ny-nj-ct.osm.pbf
