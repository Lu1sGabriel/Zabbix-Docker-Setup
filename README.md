# Zabbix Docker Compose Setup

Este repositório contém a configuração do **Zabbix** usando **Docker Compose**, facilitando o processo de criação e gerenciamento de containers para os componentes do Zabbix.

## Componentes

Este setup utiliza os seguintes serviços Docker:

- **PostgreSQL**: Banco de dados para armazenar os dados do Zabbix.
- **Zabbix Server**: O servidor principal que coleta e processa dados de monitoramento.
- **Zabbix Web**: Interface web para acessar e gerenciar o Zabbix.
- **Zabbix Agent**: Agente para monitorar os hosts.

## Pré-requisitos

- Docker
- Docker Compose

## Como Usar

### 1. Clone o repositório:

```bash
git clone https://github.com/seu-usuario/zabbix-docker-compose.git
cd zabbix-docker-compose
```

````

### 2. Crie a rede no Docker

Antes de subir os containers, crie a rede personalizada `zabbix` (ou qualquer outro nome que você preferir). Para isso, execute o seguinte comando:

```bash
docker network create zabbix
```

Caso tenha optado por um nome diferente para a rede, modifique a variável `NETWORK` no arquivo `.env` com o nome escolhido. Exemplo:

```env
NETWORK=zabbix
```

### 3. Crie o arquivo `.env`

Crie um arquivo `.env` na raiz do projeto baseado no exemplo abaixo:

```env
POSTGRES_DB=zabbix
POSTGRES_USER=zabbix
POSTGRES_PASSWORD=zabbix
ZBX_SERVER_HOST=zabbix-server
DB_SERVER_HOST=zabbix-postgres
DB_SERVER_PORT=5432
PHP_TZ=America/Sao_Paulo
ZBX_ENABLE_SNMP_TRAPS=true
NETWORK=zabbix  # Nome da rede, ajuste se necessário
```

### 4. Suba os containers

Suba os containers usando o Docker Compose:

```bash
docker-compose up -d
```

### 5. Verifique o IP do `zabbix-agent`

Depois de subir os containers, é necessário verificar o IP atribuído ao container do `zabbix-agent` dentro da rede `zabbix`. Para isso, execute o seguinte comando para fazer o `inspect` na rede Docker:

```bash
docker network inspect zabbix
```

Procure pela seção correspondente ao container `zabbix-agent` e localize o campo `IPv4Address`. Exemplo:

```json
"Containers": {
    "zabbix-agent": {
        "Name": "zabbix-agent",
        "EndpointID": "c12345678d1234b12345678d1234b12",
        "MacAddress": "02:42:ac:11:00:02",
        "IPv4Address": "172.18.0.4/16",
        "IPv6Address": ""
    }
}
```

No exemplo acima, o IP do `zabbix-agent` seria `172.18.0.4`.

### 6. Adicione o IP do `zabbix-agent` na interface web

Acesse a interface web do Zabbix em [http://localhost](http://localhost) e vá até a aba **Host**. Clique para adicionar um novo host e, no campo **Agent interfaces**, adicione o IP que você obteve do `zabbix-agent` (por exemplo, `172.18.0.4`).

### 7. Acesse a interface web do Zabbix

Após o processo acima, acesse a interface web do Zabbix:

```bash
http://localhost
```

## Volumes

Este setup usa volumes persistentes para garantir que os dados do Zabbix e do PostgreSQL não sejam perdidos quando os containers forem removidos:

- `zabbix_postgres_data`: Armazenamento do banco de dados PostgreSQL.
- `zabbix_server_data`: Armazenamento para o Zabbix Server.
- `zabbix_web_data`: Armazenamento para a interface web do Zabbix.
````
