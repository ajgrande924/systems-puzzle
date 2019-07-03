# What did I do?

### 6/27/19

Followed the instructions on the page and tried to run the website on `localhost:8080`. Since I was not able to load the page in the browser I inspected the `docker-compose.yml` file and noticed that the port mapping for the nginx container was reversed `<host>:<container>` should be `8080:80`. 

Notice that the page was still not loading so I inspected the logs for each of the containers. You can inspect by using the  `docker logs <container_name>` but I used a tool called DockStation the inspect the logs of all the containers. I found out that the flask app was serving on `0.0.0.0:5000` inside of the flaskapp container. I changed the `flaskapp.conf` file to reflect this: `http://flaskapp:5000`

### 7/2/19

Tried to enter an item on `http://localhost:8080` and it redirected to `http://localhost%2Clocalhost:8080/success` instead of `http://localhost:8080/success`. When I removed `proxy_set_header Host $http_host;` from `flaskapp.conf` it redirected to `http://localhost/success`. I reverted my change and removed `proxy_set_header Host $host;` instead and the application redirected properly to `http://localhost:8080/success`. The page (`http://localhost:8080/success`) currently shows `[, , , , , , , ]`.

### 7/3/19

When I tried to enter an item on `http://localhost:8080` I checked the logs for `flaskapp` and get a POST with a 302 code and no indication from the `db` logs that the entry was received from `flaskapp`. I suspect that there is an issue with the connection between the `flaskapp` and `db`.

I checked the postgres container to see if the entries were added to the database and I found out that they were.

```sh
# ssh into db container by name
dex <db_container> sh # docker exec -i -t

# inside db container
psql -U postgres
\l # list all databases
\c flaskapp_db # connect to flaskapp_db database
select * from public.items; # list all contents of items table
\q # exit psql
```

The issue is retrieving the data from the postgres db and displaying all the entries to the `/success` route.

I checked this [flaskapp example](https://realpython.com/flask-by-example-part-2-postgres-sqlalchemy-and-alembic/).

Looks like the `model.py` is not complete so I added methods to the Items class. 

I updated the `/success` route to send back the results array as a formatted string; the method for the string format can be found in the Items class.
