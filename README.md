# Description

**openresty-keycloak-gateway** is  a fully working example of a reverse proxy, that supports JWT authentication.

Technologies used:

* Keycloak, provides authentication, authorization, user management, etc
* OpenResty (with `lua-resty-openidc`  module), web platform (like nginx)

Note that the reverse proxy needs to validate a JWT token in order to forward the request. In this case we need to provide an `Authorization: Bearer bearer_token_here` header.

Also in every request the gateway, replaces the `Authorization` with  a `X-Real-Name` header to the request.

## Original request

```
GET /api/v1/test HTTP/1.0
Accept: */*
Accept-Encoding: gzip, deflate
Authorization: Bearer bearer_token_here
Accept-Language: en-us
```



## Forwarded request

```
GET /api/v1/test HTTP/1.0
Host: localhost
X-Real-IP: 172.17.0.1
X-Forwarded-For: 172.17.0.1
X-Forwarded-Proto: http
Connection: close
Accept: */*
User-Agent: Rested/2009 CFNetwork/1128.0.1 Darwin/19.6.0 (x86_64)
Accept-Language: en-us
Accept-Encoding: gzip, deflate
X-Real-Name: ewoJImZpcnN0X25hbWUiOiAidXNlciIsCgkidXVpZCI6ICIwYmM5MTMyNS0xNjk3LTRiNmQtYjRiZi01MGYxZmNmZGMzZWIiLAoJInJvbGVzIjogWyJST0xFX1RFU1RfMSIsICJST0xFX1RFU1RfMiJdLAoJImxhc3RfbmFtZSI6ICJ1c2VyIiwKCSJ1c2VybmFtZSI6ICJ1c2VyIiwKCSJlbWFpbCI6ICJ1c2VyQHRlc3QudGVzdCIKfQ==
```

## X-Real-Name

Is a base64 encoded `json object`, which contains user information.

```json
{
	"first_name": "user",
	"uuid": "0bc91325-1697-4b6d-b4bf-50f1fcfdc3eb",
	"roles": ["ROLE_TEST_1", "ROLE_TEST_2"],
	"last_name": "user",
	"username": "user",
	"email": "user@test.test"
}
```



# Prerequisites

* A working Keycloak identity `server`. [See here](https://www.keycloak.org/docs/latest/getting_started/index.html).
* A keycloak `realm`. [See here](https://www.keycloak.org/docs/latest/getting_started/index.html#creating-a-realm-and-a-user)
* A keycloak client with `Access type: public`. 
* A simple `http echo server` (Optional)

# Docker

## Dockerfile

See [Dockerfile](https://github.com/smyrgeorge/openresty-keycloak-gateway/blob/master/Dockerfile)

## docker build

`docker build -t authproxy .`

## docker run

`docker run --name authproxy -d -it -p 8000:8000 -v $PWD/nginx.conf:/nginx.conf authproxy -c /nginx.conf`

# nginx.conf
See [nginx.conf](https://github.com/smyrgeorge/openresty-keycloak-gateway/blob/master/nginx.conf)

## Notes

### keycloak

* Change the `client_id`.
* Change `keycloak_base_url` to locate your `Keycloak` server.

### proxy_pass

In the `location` section, `proxy_pass` is used to forward request to another service. In this case we forward the request to a simple echo http server. 

# Test http server, for debug purposes

*Note: in each request http-echo-server adds 2 sec timeout.*

`npm i -g http-echo-server`

# Links

https://eclipsesource.com/blogs/2018/01/11/authenticating-reverse-proxy-with-keycloak/

https://www.keycloak.org/docs/latest/getting_started/index.html

https://github.com/zmartzone/lua-resty-openidc

https://openresty.org/en/

https://jwt.io/
