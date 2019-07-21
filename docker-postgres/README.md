# docker and postgres

docker run postgres:10.6
or, better


docker run -d -p 5432:5432 --name DB_NAME -e POSTGRES_PASSWORD=DB_PASS postgres:10.6

https://medium.com/better-programming/connect-from-local-machine-to-postgresql-docker-container-f785f00461a7

https://www.postgresql.org/docs/9.1/backup-dump.html

