### Nginx Reverse Proxy to ASP.NET Core – Separate Docker Containers
Kestrel is the web server that is included by default in ASP.NET Core new-project templates. If your application accepts requests only from an internal network, you can use Kestrel by itself. 


![alt text](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/overview/_static/kestrel-to-internal.png "kestrel to internal traffic")

But If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a reverse proxy server. A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram

![alt text](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/overview/_static/kestrel-to-internet.png "kestrel to internet traffic")

This project is a sample how to setup a reverse proxy between Nginx and an ASP.NET Core application in the same box.

The sample creates two separate containers: one for the application and one for the reverse proxy. Then It uses Docker compose to bring them up together and handle the network bridge between them.

#### Docker configuration

That configuration defines two services: app and proxy.

##### Docker-Compose.yml:

``` 
version: '2'
 
services:
  app:
    build:
      context:  ./app
      dockerfile: Dockerfile
    expose:
      - "5000"
 
  proxy:
    build:
      context:  ./nginx
      dockerfile: Dockerfile
    ports:
      - "8084:80"
    links:
      - app
```

##### App Dockerfile:
```
FROM microsoft/aspnetcore:1.1
 
WORKDIR /app
COPY bin/Debug/netcoreapp1.1/publish .
 
ENV ASPNETCORE_URLS http://+:5000
EXPOSE 5000
 
ENTRYPOINT ["dotnet", "app.dll"]

```
##### Proxy Dockerfile:

```
FROM nginx
COPY nginx.conf /etc/nginx/nginx.conf

```