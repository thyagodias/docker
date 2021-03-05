## Executando restauração

Com o ambiente criado e executando, para dar início à restauração é necessário deixar os arquivos de restauração disponíveis para o container do PostgreSQL, para isso, copie o arquivo que será usado na restauração de dados para a pasta _/backup_.

Agora podemos acessar o container de forma interativa, para executarmos os comandos para a restauração.

`docker exec -it <nome_container> /bin/bash`

Já dentro do container do PostgreSQL, acesse a pasta localizada em _/var/lib/postgresql/_, nesse diretório estarão disponíveis os backups.

`cd /var/lib/postgresql/`

A restauração do diretório acontecerá em duas etapas, agora dentro do diretório específico do PostgreSQL, podemos iniciar a restauração com o seguinte comando.

```
pg_restore --no-owner -U postgres --dbname= --create --verbose -c  --schema-only backups/<nome_arquivo>.pgdump
```

Esse comando com várias flags, que são explicadas detalhadamente neste link, basicamente criará um novo banco de dados e em seguida, fará a importação apenas do schema, deixando os dados para o próximo passo.


### Restaurando os dados

Agora que já temos todo a estrutura do banco de dados, precisamos da parte principal, os dados. Usaremos o mesmo comando para importar os dados, entretanto com algumas opções diferentes.

```
pg_restore 
  --no-owner -U postgres 
  --dbname=<nome_bd> 
  --verbose --data-only 
  --superuser=postgres
  --disable-triggers backups/<nome_arquivo>.pgdump
```

Com esse comando apenas dados serão importados para dentro do bd que foi especificado no comando. Um ponto que vale ser destacado trata-se da opção --disable-triggers, que desabilitará a checagem de integridade dos registros, algo que não é muito importante na restauração de um banco de dados, já que esses dados são provindos de outra base, que provavelmente já verificou a integridade desses dados, o que nos livra de vários erros, pois durante o processo, pode-se haver a referência de dados que ainda não foram restaurados.

__Trecho de texto retirado do [artigo](https://dev.to/alexandrel0pes/restauracao-de-bd-postgresql-docker-30oa) de Alexandre Lopes__ 