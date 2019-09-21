# Docker_registry
Deploy Docker_registry
Certs generate
----------------
mkdir /root/certs/
openssl req -newkey rsa:4096 -nodes -sha256 -keyout /root/certs/domain.key -x509 -days 365  -out /root/certs/domain.crt


Pull registry image with TLS 
 docker run -d -p 5000:5000 -v /images:/var/lib/registory -v /root/certs:/certs -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key --restart on-failure --name myregistry docker.io/registry:2

 mkdir auth
$ docker run \
  --entrypoint htpasswd \
  registry:2 -Bbn testuser testpassword > auth/htpasswd
  
docker run -d \
  -p 5000:5000 \
  --restart=always \
  --name registry \
  -v "$(pwd)"/auth:/auth \
  -e "REGISTRY_AUTH=htpasswd" \
  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
  -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
  -v "$(pwd)"/certs:/certs \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  registry:2
