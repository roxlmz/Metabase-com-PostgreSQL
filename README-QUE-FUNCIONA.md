1° Cria um rede com qualquer nome. No meu caso, foi "postgres-network"
docker network create -d bridge postgres-network

2° Subir um container com o bando de dados. Nesse caso foi o PosgreSQL

docker run --name teste-postgres --network=postgres-network -e "POSTGRES_PASSWORD=123456" -p 5432:5432 -v /home/rodolfo/PostgreSQL:/var/lib/postgr

3° Criar um banco de dados para o Metabase. Eu usei o PgAdmin4

docker run --name teste-pgadmin --network=postgres-network -p 15432:80 -e "PGADMIN_DEFAULT_EMAIL=localhost@local.local" -e "PGADMIN_DEFAULT_PASSWORD=123456" -d dpage/pgadmin4

http://<IP_DA_MAQUINA_AONDE_TA_O_CONTAINER>:15432/
login: "PGADMIN_DEFAULT_EMAIL"
Senha: "PGADMIN_DEFAULT_PASSWORD"

criar um novo banco de dados com o nome: "metabase"

4° Subir o container com o Metabase

docker run -d -p 3000:3000 --network=postgres-network -e MB_DB_CONNECTION_URI="jdbc:postgresql://<IP_DA_MAQUINA_AONDE_TA_O_CONTAINER>:5432/metabase?user=postgres&password=123456" --name metabase metabase/metabase:latest

5° Acompanhar os logs

docker logs -f metabase

