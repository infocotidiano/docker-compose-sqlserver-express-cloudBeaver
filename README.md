# SQL Server Express + CloudBeaver com Docker Compose

Exemplo utilizado no vídeo do canal **InfoCotidiano** para executar SQL Server Express e CloudBeaver com Docker Compose.

## 🎥 Vídeo

[![SQL Server + CloudBeaver](https://img.youtube.com/vi/ithEr__T-94/maxresdefault.jpg)](https://youtu.be/ithEr__T-94    )
> **Link:** [https://youtu.be/ithEr__T-94 ]


O arquivo `docker-compose.yml` fica separado deste README.

## Estrutura

```text
.
├── docker-compose.yml
├── dados/
│   ├── data/
│   ├── log/
│   └── secrets/
└── cloudbeaver/
```

Os diretórios `dados` e `cloudbeaver` são usados para manter os dados dos containers no computador.

## Configurações

### SQL Server Express

- Imagem: `mcr.microsoft.com/mssql/server:2025-latest`
- Container: `sqlserver_express`
- Porta: `1433`
- Usuário: `sa`
- Senha: `SqlServer@123`
- Edição: `Express`

> A edição **Express** é gratuita, mas tem licença limitada: restringe recursos como CPU, memória RAM utilizada e tamanho máximo do banco de dados. Para detalhes atualizados, consulte a documentação oficial da Microsoft.

### CloudBeaver

- Imagem: `dbeaver/cloudbeaver:latest`
- Container: `cloudbeaver_dev`
- Porta: `8978`
- Acesso: `http://localhost:8978`

## Iniciar os containers

Na pasta onde está o `docker-compose.yml`:

```bash
docker compose up -d
```

O `-d` executa os containers em segundo plano.

## Parar os containers

```bash
docker compose stop
```

Os containers são apenas parados e podem ser iniciados novamente.

## Iniciar novamente containers já criados

```bash
docker compose start
```

## Parar e remover os containers

```bash
docker compose down
```

O `down` remove os containers criados pelo Compose. Os dados permanecem nos diretórios mapeados como volumes.

Depois do `down`, para criar os containers novamente:

```bash
docker compose up -d
```

> Se você apenas alterou o `docker-compose.yml`, normalmente use `docker compose up -d` para aplicar a configuração. Se necessário, o Compose recria os containers alterados.

## Acessar o SQL Server pelo terminal

Entre no SQL Server diretamente pelo container usando o `sqlcmd`:

```bat
docker exec -it sqlserver_express /opt/mssql-tools18/bin/sqlcmd -S localhost,1433 -U sa -P "SqlServer@123" -C
```

Comentários de cada parte:

```text
docker exec -it
```
Executa um comando dentro do container.

```text
sqlserver_express
```
Nome do container do SQL Server.

```text
/opt/mssql-tools18/bin/sqlcmd
```
Programa utilizado para conectar ao SQL Server.

```text
-S localhost,1433
```
Servidor e porta do SQL Server.

```text
-U sa
```
Usuário utilizado na conexão.

```text
-P "SqlServer@123"
```
Senha do usuário `sa`.

```text
-C
```
Confia no certificado utilizado pela conexão.

Após conectar, o prompt será:

```text
1>
```

Depois de entrar no `sqlcmd`, alguns comandos úteis:

```sql
SELECT name FROM sys.databases;
GO
```

Lista os bancos de dados.

```sql
USE master;
GO
```

Seleciona o banco `master`.

Para sair:

```text
EXIT
```

### Criar tabela Cliente

```sql
CREATE TABLE Cliente (
    codigo INT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    telefone VARCHAR(20)
);
GO
```

### Inserir registro

```sql
INSERT INTO Cliente (codigo, nome, telefone)
VALUES (1, 'João da Silva', '(19) 99999-9999');
GO
```

### Excluir registro

```sql
DELETE FROM Cliente
WHERE codigo = 1;
GO
```

## Acessar pelo Windows

Com o SQL Server publicado na porta `1433`, aplicações instaladas no Windows podem utilizar:

```text
Servidor: localhost,1433
Usuário: sa
Senha: SqlServer@123
```

Exemplo usando o **SQL Server Management Studio (SSMS)**:

```text
Server type: Database Engine
Server name: localhost,1433
Authentication: SQL Server Authentication
Login: sa
Password: SqlServer@123
```

## Acessar pelo Beekeeper Studio

No Beekeeper Studio, crie uma nova conexão **SQL Server** e informe:

```text
Host: localhost
Port: 1433
User: sa
Password: SqlServer@123
Database: master
```

Depois, teste a conexão.

## Acessar o CloudBeaver pelo navegador

Abra:

```text
http://localhost:8978
```

### Conectar o CloudBeaver ao SQL Server

Dentro do CloudBeaver, ao cadastrar o servidor SQL Server, como os dois serviços estão na mesma rede do Docker Compose, utilize o nome do serviço como host:

```text
Host: sqlserver
Port: 1433
User: sa
Password: SqlServer@123
Database: master
```

> No CloudBeaver, não utilize `localhost` como host para acessar o SQL Server. Dentro do container do CloudBeaver, `localhost` aponta para o próprio container do CloudBeaver.

## Acessar pelo terminal do container

Para abrir um terminal dentro do container do SQL Server:

```bash
docker exec -it sqlserver_express bash
```

Depois, para acessar o SQL Server:

```bash
/opt/mssql-tools18/bin/sqlcmd -S localhost,1433 -U sa -P "SqlServer@123" -C
```

Para sair do container:

```bash
exit
```

Também é possível abrir um shell diretamente:

```bash
docker exec -it sqlserver_express sh
```

## Verificar os containers

Para verificar se os containers estão em execução:

```bash
docker ps
```

Para visualizar também os containers parados:

```bash
docker ps -a
```

## Logs

Para visualizar os logs do SQL Server:

```bash
docker logs sqlserver_express
```

Para acompanhar os logs em tempo real:

```bash
docker logs -f sqlserver_express
```

Para o CloudBeaver:

```bash
docker logs cloudbeaver_dev
```

## Observação

Este projeto é um **exemplo didático utilizado em vídeo no canal InfoCotidiano**.

As senhas e configurações apresentadas são apenas para ambiente de desenvolvimento/teste. Para utilização em produção, utilize credenciais seguras e revise as configurações de acesso.