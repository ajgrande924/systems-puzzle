What did I do?

Followed the instructions on the page and tried to run the website on `localhost:8080`. Since I was not able to load the page in the browser I inspected the `docker-compose.yml` file and noticed that the port mapping for the nginx container was reversed `<host>:<container>` should be `8080:80`. 

Notice that the page was still not loading so I inspected the logs for each of the containers. You can inspect by using the  `docker logs <container_name>` but I used a tool called DockStation the inspect the logs of all the containers. I found out that the flask app was serving on `0.0.0.0:5000` inside of the flaskapp container. I changed the `flaskapp.conf` file to reflect this: `http://flaskapp:5000`