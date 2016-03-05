# simple nginx docker container

## Install
First clone this repositoriy:
```sh
git clone https://github.com/jkuettner/nginx.git
cd nginx
```
Then put your website nginx-configs into the **sites** folder and build the container:
```sh
docker build -t jkuettner/nginx .
```
run the container:
```sh
docker run -p 80:10080 -p 443:10443 -v /path/to/your/webroot:/www jkuettner/docker
```

## Example setup
First clone this repositoriy:
```sh
git clone https://github.com/jkuettner/nginx.git
cd nginx
```
copy the example.conf to sites and build the container:
```sh
cp example/sites/example.conf sites/
docker build -t jkuettner/nginx-hellow-world .
```
run the container with the example/www dir mounted:
```sh
docker run --rm -ti -p 80:10080 -p 443:10443 -v ${PWD}/example/www/:/www jkuettner/nginx-hellow-world .
```

If you open http://localhost you should see **Hello World!**

### Notes
If you wish to put ssl certificates into the container use the **/nginx/ssl** volume.