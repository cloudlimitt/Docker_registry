# Docker_registry
Deploy Docker_registry
Certs generate
----------------
mkdir /root/certs/
openssl req -newkey rsa:4096 -nodes -sha256 -keyout /root/certs/domain.key -x509 -days 365  -out /root/certs/domain.crt


Pull registry image with TLS 
 docker run -d -p 5000:5000 -v /images:/var/lib/registory -v /root/certs:/certs -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key --restart on-failure --name myregistry docker.io/registry:2
