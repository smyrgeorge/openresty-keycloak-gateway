# Original:
https://eclipsesource.com/blogs/2018/01/11/authenticating-reverse-proxy-with-keycloak/

# Links
https://eclipsesource.com/blogs/2018/01/11/authenticating-reverse-proxy-with-keycloak/
https://github.com/zmartzone/lua-resty-openidc
https://openresty.org/en/
https://jwt.io/

# Dockerfile
See Dockerfile

# docker build
`docker build -t authproxy .`

# nginx.conf
See nginx.conf

# docker run
`docker run --name authproxy -d -it -p 8000:8000 -v $PWD/nginx.conf:/nginx.conf authproxy -c /nginx.conf`

# Test http server, for debug perposes
`npm i -g http-echo-server`