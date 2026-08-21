# Etapa 3 — Hardening de acesso e SSH

## Objetivo

Implementar controle de acesso administrativo e fortalecer
a autenticação SSH do servidor.

## Alterações realizadas

- criação do usuário operador
- inclusão no grupo sudo
- configuração de autenticação por chave SSH
- desabilitação de autenticação SSH por senha
- bloqueio de login direto do root
- validação da configuração sshd
- criação de backup do sshd_config

## Validações

- sudo funcionando
- SSH por chave funcionando
- SSH por senha bloqueado
- login direto do root bloqueado
- serviço SSH ativo
