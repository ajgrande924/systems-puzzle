What did I do?

6/27/19

Followed the instructions on the page and tried to run the website on `localhost:8080`. Since I was not able to load the page in the browser I inspected the `docker-compose.yml` file and noticed that the port mapping for the nginx container was reversed `<host>:<container>` should be `8080:80`. 

Notice that the page was still not loading so I inspected the logs for each of the containers. You can inspect by using the  `docker logs <container_name>` but I used a tool called DockStation the inspect the logs of all the containers. I found out that the flask app was serving on `0.0.0.0:5000` inside of the flaskapp container. I changed the `flaskapp.conf` file to reflect this: `http://flaskapp:5000`

7/2/19

Tried to enter an item on `http://localhost:8080` and it redirected to `http://localhost%2Clocalhost:8080/success` instead of `http://localhost:8080/success`. When I removed `proxy_set_header Host $http_host;` from `flaskapp.conf` it redirected to `http://localhost/success`. I reverted my change and removed `proxy_set_header Host $host;` instead and the application redirected properly to `http://localhost:8080/success`. The page (`http://localhost:8080/success`) currently shows `[, , , , , , , ]`.
