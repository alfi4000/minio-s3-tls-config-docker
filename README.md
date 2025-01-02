TLS:

mkdir -p ${HOME}/minio/data

mkdir -p ${HOME}/minio/certs

cd minio

cd certs

openssl req -newkey rsa:4096 -x509 -days 730 -nodes -keyout private.key -out public.crt



sudo chown -R $(id -u):$(id -g) ${HOME}/minio/data
sudo chmod -R u+rw ${HOME}/minio/data



sudo docker run \
  -p 443:9000 \
  -p 9001:9001 \
  --user $(id -u):$(id -g) \
  --name minio1 \
  -e "MINIO_ROOT_USER=kali" \
  -e "MINIO_ROOT_PASSWORD=hack4000" \
  -e "MINIO_HTTP_SERVER=localhost:9000" \
  -e "MINIO_HTTPS_SERVER=localhost:443" \
  -v ${HOME}/minio/data:/data \
  -v ${HOME}/minio/certs:/data1/.minio/certs \
  quay.io/minio/minio server /data --console-address ":9001" --certs-dir /data1/.minio/certs &
