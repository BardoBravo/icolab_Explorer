# Hyperledger Explorer

## Pré-Requisitos

- nodejs 8.11.x (9.0 não é suportado)
- PostgreSQL 9.5 ou + ((https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-18-04))
- Jq (https://zoomadmin.com/HowToInstall/UbuntuPackage/jq)

## Clonar repositório
- git clone https://github.com/hyperledger/blockchain-explorer.git.
- cd blockchain-explorer.
- git checkout v0.3.5.1

## Database Setup
- cd blockchain-explorer/app/persistence/postgreSQL/db
- Modifique pgconfig.json com os parâmetros de postgres 
``` json
 "pg": {
		"host": "127.0.0.1",
		"port": "5432",
		"database": "fabricexplorer",
		"username": "hppoc",
		"passwd": "password"
	}
```

- cd blockchain-explorer/app/persistence/postgreSQL/db
- ./createdb.sh

### Test DB
- sudo -u postgres psql
- **\l**, para ver os databass
- **\d**, para ver as tabelas

## Configure Composer Explorer
- cd blockchain-explorer/app/platform/fabric
- Altere o arquivo config.json, para que fique similar a:
``` json
{
  "network-config": {
    "org1": {
      "name": "Org1",
      "mspid": "Org1MSP",
      "peer1": {
        "requests": "grpc://127.0.0.1:7051",
        "events": "grpc://127.0.0.1:7053",
        "server-hostname": "peer0.org1.example.com",
        "tls_cacerts":
          "/home/icolab/fabric-dev-servers/fabric-scripts/hlfv12/composer/crypto-config/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt"
      },      
      "admin": {
        "key":
          "/home/icolab/fabric-dev-servers/fabric-scripts/hlfv12/composer/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/keystore",
        "cert":
          "/home/icolab/fabric-dev-servers/fabric-scripts/hlfv12/composer/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/signcerts"
      }
    }
  },
  "channel": "composerchannel",
  "orderers": [
    {
      "mspid": "OrdererMSP",
      "server-hostname": "orderer.example.com",
      "requests": "grpc://127.0.0.1:7050",
      "tls_cacerts":
        "/home/icolab/fabric-dev-servers/fabric-scripts/hlfv12/composer/crypto-config/ordererOrganizations/example.com/orderers/orderer.example.com/tls/ca.crt"
    }
  ],
  "keyValueStore": "/tmp/fabric-client-kvs",
  "configtxgenToolPath": "/home/icolab/fabric-dev-servers/fabric-scripts/hlfv12/composer",
  "SYNC_START_DATE_FORMAT": "YYYY/MM/DD",
  "syncStartDate": "2018/01/01",
  "eventWaitTime": "30000",
  "license": "Apache-2.0",
  "version": "1.1"
}
```

- Altere o **channel** para seu channel

## Iniciando o Hyperledger Explorer
- cd blockchain-explorer
- npm install
- cd blockchain-explorer/app/test
- npm install
- npm run test
- cd client/
- npm install
- npm test -- -u --coverage
- npm run build

- cd blockchain-explorer
- ./start.sh

Em caso de erro, pode-se verificar os arquivos de log.