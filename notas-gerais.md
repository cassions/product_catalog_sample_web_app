# TEMPLATE INPUT VARS
DOCDB_USERNAME="admin"
DOCDB_PASSWORD="localdev123"
PROJECT_URL=https://github.com/aws-samples/amazon-documentdb-samples/tree/master/usecases/product_catalog
APP_INSTALL_FOLDER=/home/ec2-user/amazon-documentdb-samples/usecases/product_catalog

# ENV
export DOCDB_HOST="localhost"
export DOCDB_PORT="27017"
export DOCDB_USERNAME="admin"
export DOCDB_PASSWORD="localdev123"
export DOCDB_TLS_CA=/etc/mongo-tls/ca.crt
export APP_INSTALL_FOLDER=/home/ec2-user/amazon-documentdb-samples/usecases/product_catalog

# INSTALL
sudo yum install pip
sudo yum install docker
pip install -r requirements.txt
sudo systemctl enable --now docker



mkdir -p ~/product_catalog

curl -L https://github.com/aws-samples/amazon-documentdb-samples/archive/refs/heads/master.tar.gz | tar -xz -C ~/product_catalog --strip-components=3 amazon-documentdb-samples-master/usecases/product_catalog


# START
sudo docker start docdb-local
cd /home/ec2-user/amazon-documentdb-samples/usecases/product_catalog
nohup python3 $APP_INSTALL_FOLDER/app.py &


## Connection
export DOCDB_HOST="localhost"
export DOCDB_PORT="27017"
export DOCDB_USERNAME="admin"
export DOCDB_PASSWORD="localdev123"
export DOCDB_TLS_CA="./global-bundle.pem"


export DOCDB_TLS=false
sudo docker run -d --name docdb-local -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=localdev123 mongo:7


# STATUS
sudo systemctl status docker  --no-pager
sudo docker ps -a --filter name=docdb-local

# INSTALL DOCUMENTDB
wget https://truststore.pki.rds.amazonaws.com/global/global-bundle.pem


# START

export APP_INSTALL_FOLDER=/home/ec2-user/amazon-documentdb-samples/usecases/product_catalog
export DOCDB_HOST="localhost"
export DOCDB_PORT="27017"
export DOCDB_USERNAME="admin"
export DOCDB_PASSWORD="localdev123"
export DOCDB_TLS_CA=$APP_INSTALL_FOLDER/mongo-tls/ca.crt
sudo docker start docdb-local
cd $APP_INSTALL_FOLDER
nohup python3 app.py &

# CHECK
App Runner to deploy docker
ECR
ECS

# PROMPT CLOUD FORMATION
I want to create a cloud formation template that does the following 

 https://github.com/aws-samples/amazon-documentdb-samples/tree/master/usecases/product_catalog






# DOCKER MONGODB

#NO SSL
sudo docker run -d --name docdb-local -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=$DOCDB_USERNAME -e MONGO_INITDB_ROOT_PASSWORD=$DOCDB_PASSWORD mongo:7

sudo usermod -aG docker ec2-user
sudo docker ps -a
docker start docdb-local
sudo docker logs docdb-local --tail 20
sudo docker rm -f docdb-local

sudo docker run -d --name docdb-local -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=$DOCDB_USERNAME -e MONGO_INITDB_ROOT_PASSWORD=$DOCDB_PASSWORD mongo:7

sudo docker run -d --name docdb-local -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=$DOCDB_USERNAME -e MONGO_INITDB_ROOT_PASSWORD=$DOCDB_PASSWORD -v /etc/mongo-tls:/etc/mongo-tls:ro mongo:7 --auth --tlsMode requireTLS --tlsCertificateKeyFile /etc/mongo-tls/server.pem --tlsCAFile /etc/mongo-tls/ca.crt --tlsAllowConnectionsWithoutCertificates

`sudo docker run -d --name docdb-local -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=$DOCDB_USERNAME -e MONGO_INITDB_ROOT_PASSWORD=$DOCDB_PASSWORD -v $APP_INSTALL_FOLDER/mongo-tls:/etc/mongo-tls:ro mongo:7 --auth --tlsMode requireTLS --tlsCertificateKeyFile /etc/mongo-tls/server.pem --tlsCAFile /etc/mongo-tls/ca.crt --tlsAllowConnectionsWithoutCertificates`




## CERT SCRIPT
sudo bash -s <<'OUTER'
set -euo pipefail
D=/etc/mongo-tls
mkdir -p "$D"; cd "$D"

openssl req -x509 -newkey rsa:2048 -days 3650 -nodes \
  -keyout ca.key -out ca.crt -subj "/CN=local-docdb-ca"

openssl req -newkey rsa:2048 -nodes \
  -keyout server.key -out server.csr -subj "/CN=localhost"

cat > ext.cnf <<'INNER'
subjectAltName = DNS:localhost,IP:127.0.0.1
basicConstraints = critical,CA:FALSE
keyUsage = critical,digitalSignature,keyEncipherment
extendedKeyUsage = serverAuth
INNER

openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
  -out server.crt -days 825 -sha256 -extfile ext.cnf

cat server.key server.crt > server.pem

chown 999:999 server.pem
chmod 600 server.pem
chmod 644 ca.crt
rm -f server.csr ext.cnf
echo "OK - certs in $D"
OUTER

## STATUS
openssl x509 -in /etc/mongo-tls/server.crt -noout -ext subjectAltName

## PERM
sudo chown 999:999 /etc/mongo-tls/server.pem && sudo chmod 600 /etc/mongo-tls/server.pem && sudo chmod 644 /etc/mongo-tls/ca.crt
