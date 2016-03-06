# simple nginx docker image

## Install
First clone this repositoriy and build the image:
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

## PHP

If you need php-support I recommend you to use my compatible [php-fpm docker image](https://github.com/jkuettner/fpm):

Run the fpm-container (replace the webroot path with yours):
```sh
docker run -d \
    --name fpm \
    -v /path/to/your/webroot:/www \
    jkuettner/fpm
```

Now add or edit the php-handling section of your nginx-site-config(s):
```
server {
    [...]
    root /www/your.website;
    [...]

    location ~ \.php$ {
        fastcgi_keep_conn on;
        fastcgi_pass   fpm:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include        fastcgi_params;
    }
    [...]
}
```

Run the nginx container and link it to the fpm-container:
```sh
docker run -d \
    --name nginx \
    --link fpm:fpm \
    --volumes-from fpm \
    -p 80:10080 \
    -p 443:10443 \
    -v /path/to/your/nginx/configs:/nginx/conf.d \
    -v /path/to/your/ssl/certificates:/nginx/ssl \
    jkuettner/nginx
```