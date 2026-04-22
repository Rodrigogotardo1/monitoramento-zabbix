# Projeto Zabbix

Ambiente base de monitoramento de rede com Zabbix usando Docker Compose.

O projeto foi montado para servir como ponto de partida para evolucao do monitoramento, com versionamento em Git e publicacao no GitHub.

## Objetivo

Esta base permite iniciar rapidamente um ambiente Zabbix para:

- monitorar hosts locais ou remotos
- testar monitoramento via agente
- preparar descoberta e monitoramento SNMP de equipamentos de rede
- evoluir dashboards, triggers, mapas e alertas

## Arquitetura

O ambiente sobe quatro servicos principais:

- `postgres-server`: banco de dados do Zabbix
- `zabbix-server`: coleta metricas, processa eventos e gera alertas
- `zabbix-web`: interface web de administracao e visualizacao
- `zabbix-agent`: agente local para validacao inicial do ambiente

Fluxo resumido:

```text
Hosts/Equipamentos -> Zabbix Server -> PostgreSQL
                            |
                            v
                       Zabbix Web
```

## Requisitos

- Docker Desktop ou Docker Engine
- Docker Compose
- Git

## Estrutura do projeto

- `docker-compose.yml`: definicao dos servicos do ambiente
- `.env.example`: modelo de configuracao local
- `.env`: configuracao ativa local, nao versionada
- `.gitignore`: arquivos ignorados pelo Git
- `README.md`: documentacao do projeto

## Como iniciar

1. Crie o arquivo de configuracao local:

```powershell
Copy-Item .env.example .env
```

2. Suba os containers:

```powershell
docker compose up -d
```

3. Verifique o status dos servicos:

```powershell
docker compose ps
```

4. Acesse a interface web:

```text
http://localhost:8080
```

## Credenciais iniciais

- Usuario: `Admin`
- Senha: `zabbix`

Troque essa senha no primeiro acesso.

## Portas publicadas

Este projeto usa portas ajustadas para evitar conflito com outros ambientes locais de Zabbix:

- `8080`: interface web
- `10061`: Zabbix Server publicado no host
- `10060`: Zabbix Agent publicado no host

As portas internas entre containers continuam sendo as padroes do Zabbix.

## Variaveis de ambiente

Principais variaveis no arquivo `.env`:

- `POSTGRES_DB`: nome do banco de dados
- `POSTGRES_USER`: usuario do banco
- `POSTGRES_PASSWORD`: senha do banco
- `ZABBIX_WEB_PORT`: porta web publicada
- `ZABBIX_SERVER_PORT`: porta do Zabbix Server publicada no host
- `ZABBIX_AGENT_PORT`: porta do Zabbix Agent publicada no host
- `PHP_TZ`: timezone da interface web
- `ZBX_AGENT_HOSTNAME`: nome do host do agente local
- `ZBX_STARTPOLLERS`: quantidade inicial de pollers
- `ZBX_STARTPINGERS`: quantidade inicial de pingers
- `ZBX_CACHESIZE`: cache principal do servidor
- `ZBX_HISTORYCACHESIZE`: cache de historico
- `ZBX_TIMEOUT`: timeout de verificacao

## Comandos uteis

Subir o ambiente:

```powershell
docker compose up -d
```

Parar o ambiente:

```powershell
docker compose down
```

Ver logs do servidor Zabbix:

```powershell
docker compose logs zabbix-server
```

Ver logs da interface web:

```powershell
docker compose logs zabbix-web
```

Recriar os servicos apos alteracoes:

```powershell
docker compose up -d --remove-orphans
```

## Persistencia de dados

O projeto usa volumes Docker para persistir:

- dados do PostgreSQL
- scripts e dados auxiliares do Zabbix Server
- chaves SSH e MIBs do servidor

Isso permite reiniciar os containers sem perder a base configurada.

## Primeiro uso recomendado

1. Acessar a interface web.
2. Alterar a senha do usuario `Admin`.
3. Confirmar que o host do agente local aparece no ambiente.
4. Adicionar um host de teste para ping ou agente.
5. Iniciar o cadastro de dispositivos SNMP da rede.

## Equipamento ja configurado

O ambiente ja foi validado com um switch Cisco real na rede:

- Host no Zabbix: `SW-3750X`
- IP: `172.16.17.10`
- Modelo detectado: `WS-C3750X-48PF-S`
- Template aplicado: `Cisco IOS by SNMP`
- Protocolo: `SNMPv3`
- Security level: `authPriv`
- Usuario SNMP: `admin`

Validacoes executadas:

- conectividade IP
- acesso SSH ao equipamento
- habilitacao de `SNMPv3`
- resposta em `161/udp`
- coleta SNMP no Zabbix com interface disponivel

Se esse equipamento for reutilizado como padrao para outros switches Cisco, a mesma estrategia pode ser aplicada ajustando IP, usuario e credenciais SNMP.

## Evolucao esperada do projeto

Proximos incrementos naturais para este repositorio:

1. cadastrar equipamentos de rede via SNMP
2. aplicar templates padrao do Zabbix
3. criar triggers para indisponibilidade, CPU, memoria e disco
4. configurar canais de alerta
5. montar dashboards operacionais
6. preparar deploy em nuvem ou VPS

## Git e GitHub

Repositorio remoto:

- `https://github.com/Rodrigogotardo1/monitoramento-zabbix`

Comandos Git uteis:

```powershell
& "C:\Program Files\Git\cmd\git.exe" status
& "C:\Program Files\Git\cmd\git.exe" add .
& "C:\Program Files\Git\cmd\git.exe" commit -m "mensagem"
& "C:\Program Files\Git\cmd\git.exe" push
```

## Observacoes

- O arquivo `.env` nao deve ser versionado.
- Se houver outro Zabbix rodando na maquina, mantenha portas diferentes no `.env`.
- A interface web pode levar alguns instantes para ficar pronta apos o primeiro `up`.
- O banco e inicializado automaticamente pelo container do Zabbix Server.
