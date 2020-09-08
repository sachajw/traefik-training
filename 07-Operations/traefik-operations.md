# Traefik Operations Lab

<img src="../img/Traefik_training.png" alt="Traefik Logo" height="350"> 

## 1. Enable Healthcheck with CLI & Ping
1. Before we begin, lets cleanup any running Docker stack `docker stack rm traefik` If you named you stack something else use your specified name. If you don't remember run `docker stack ls`
2. Change to the `06-Observability` folder
3. Open the `traefik.yml` file in your favorite editor and review the `Traefik Logging` section
4. Change the Log level from `INFO` to `DEBUG`

```yml
log:
  level: DEBUG
```

5. Start Traefik and the `catapp` `docker stack deploy -c docker-compose.log.yml traefik`
6.  View the Debug logs `docker service logs traefik_traefik`
7.  Once we are done with Debug switch Traefik back to default logging. Open `traefik.yml` and change the Log level from `DEBUG` to `INFO`

```yml
log:
  level: INFO
```

8. Remove Traefik `docker stack rm traefik`
9. Redeploy Traefik with the updated Log Level `docker stack deploy -c docker-compose.log.yml traefik`
10. View the `ERROR` Log level for Traefik `docker service logs traefik_traefik`


## 2. Dashboard Configurations
1. Before we begin, lets cleanup the HTTP stack  `docker stack rm traefik` If you named you stack something else use your specified name. If you don't remember run `docker stack ls`
2. Change to the `06-Observability` folder
3. Open the `traefik.access-log.yml` file in your favorite editor and review the `Access Logging` section
4. Uncomment the `accessLog` line to enable Access Logging

```yml
# enable Access logs
accessLog: {}
```

5. Start Traefik with `Access Log` enabled `docker stack deploy -c docker-compose.access-log.yml traefik`
6. Open [http://catapp.localhost](http://catapp.localhost) and refresh the page a couple times
7. View the `Access Log` using `docker service logs traefik_traefik` see the example below of a `200` status message


```apache
traefik_traefik.1.zaxcbu7aplii@docker-desktop    | 10.0.0.2 - traefik [04/Sep/2020:07:52:41 +0000] "GET / HTTP/1.1" 200 760 "-" "-" 1 "catapp@docker" "http://10.0.8.6:5000" 14ms
```

8. Let's enable filtering to see just `404` status messages. Edit the `Access Log` filter configuration in `traefik.access-log.yml`and comment the `Access Log` and uncomment the filtering section
   
```yml
# accessLog: {}
#Configuring Multiple Filters
accessLog:
  filters:    
    statusCodes:
      - "404"
    retryAttempts: true
    minDuration: "10ms"
```

8. Remove Traefik `docker stack rm traefik`
9. Redeploy Traefik with the updated `Access Log` filter `docker stack deploy -c docker-compose.access-log.yml traefik`
10. Open [http://catapp.localhost](http://catapp.localhost) and refresh the page a couple times
11. View the `Access Log` using `docker service logs traefik_traefik` see the example below of a `200` status message
12. Create a `404` error by opening a missing path [http://catapp.localhost/missing](http://catapp.localhost/missing)
13. View the `Access Log` using `docker service logs traefik_traefik` and we should only see a `404` status message


# Continue to the Next to Advance Tips

### Click here to continue -> [Operations Lab](https://github.com/56kcloud/traefik-training/blob/master/07-operations/traefik-operations.md)