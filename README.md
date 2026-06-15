# inLaudo Docker

Esta pasta contém arquivos auxiliares para executar a imagem Docker do inLaudo em VPS, especialmente via Portainer.

## Imagem Docker

A imagem publicada no Docker Hub é:

```text
wallesonnn/inlaudo
```

## Tags publicadas

A primeira versão validada foi:

```text
wallesonnn/inlaudo:v0.0.1
wallesonnn/inlaudo:latest
```

## Uso com Portainer

Use o arquivo `docker-compose.yml` desta pasta como base para criar uma stack no Portainer.

```yaml
services:
  inlaudo-web:
    image: wallesonnn/inlaudo:v0.0.1
    container_name: inlaudo-web
    restart: unless-stopped
    expose:
      - "80"
    networks:
      - proxy

networks:
  proxy:
    external: true
```

## Domínio

O domínio principal planejado para produção é:

```text
inlaudo.pro
```

Configure no provedor DNS:

```text
Tipo: A
Nome: @
Valor: IP público da VPS
```

Opcionalmente, configure também:

```text
Tipo: A
Nome: www
Valor: IP público da VPS
```

## Proxy reverso

O compose atual foi preparado para uso atrás de proxy reverso, sem publicar porta diretamente na VPS.

Antes de subir a stack, confirme que a rede externa usada pelo proxy existe:

```bash
docker network ls
```

Se a rede do seu proxy tiver outro nome, altere o trecho:

```yaml
networks:
  proxy:
    external: true
```

## HTTPS

Para funcionamento adequado como PWA em produção, configure HTTPS para:

```text
https://inlaudo.pro
```

## Supabase

No Supabase, revise as URLs de autenticação:

```text
Site URL: https://inlaudo.pro
Redirect URLs:
https://inlaudo.pro/*
https://www.inlaudo.pro/*
```

## Atualização de versão

Para atualizar a produção:

1. Gere e publique uma nova imagem com `./build.sh vX.Y.Z`.
2. Atualize a tag da imagem na stack do Portainer.
3. Reimplante a stack.
