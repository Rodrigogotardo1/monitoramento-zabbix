# Projeto Zabbix

Base inicial de um ambiente de monitoramento de rede com Zabbix usando Docker Compose.

## O que esta base sobe

- PostgreSQL para o banco do Zabbix
- Zabbix Server
- Zabbix Web
- Zabbix Agent 2 para testes locais

## Requisitos

- Docker
- Docker Compose

## Como iniciar

1. Copie o arquivo de exemplo de variaveis:

```powershell
Copy-Item .env.example .env
```

2. Suba o ambiente:

```powershell
docker compose up -d
```

3. Acesse a interface web:

```text
http://localhost:8080
```

## Credenciais iniciais do Zabbix

- Usuario: `Admin`
- Senha: `zabbix`

## Proximos passos sugeridos

1. Alterar a senha padrao do usuario `Admin`.
2. Ajustar o arquivo `.env` para o ambiente real.
3. Cadastrar hosts de rede, SNMP e agentes a serem monitorados.
4. Adicionar templates, triggers e dashboards conforme o seu cenario.

## Estrutura

- `docker-compose.yml`: servicos do ambiente
- `.env.example`: configuracoes iniciais
- `.gitignore`: arquivos locais ignorados

## Observacoes

- A porta da interface web pode ser alterada em `ZABBIX_WEB_PORT` no arquivo `.env`.
- As portas publicadas do server e agent podem ser alteradas em `ZABBIX_SERVER_PORT` e `ZABBIX_AGENT_PORT`.
- O ambiente ja inclui um `zabbix-agent` para validacao inicial da instalacao.
