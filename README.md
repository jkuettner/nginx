# simple nginx docker container

## Install
First clone this repositoriy and build the container:
```sh
git clone https://github.com/jkuettner/nginx.git
cd nginx
docker build -t jkuettner/nginx .
```


test the container with the included example nginx-config if you wish:
```sh
docker run \
    --rm -ti \
    -p 80:10080 \
    -p 443:10443 \
    -v ${PWD}/example/www:/www \
    -v ${PWD}/example/sites:/nginx/conf.d\
    jkuettner/nginx
```
If you open http://localhost you should see **Hello World!**


run the container with your website nginx configs:
```sh
docker run -d \
    --name nginx \
    -p 80:10080 \
    -p 443:10443 \
    -v /path/to/your/webroot:/www \
    -v /path/to/your/nginx/configs:/nginx/conf.d \
    -v /path/to/your/ssl/certificates:/nginx/ssl \
    jkuettner/nginx
```