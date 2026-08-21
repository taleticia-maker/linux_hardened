Etapa 2 — Usuários, privilégios e SSH

## Objetivo

Implementar controle básico de acesso administrativo
e hardening inicial do serviço SSH.

## Alterações realizadas

- criação do usuário administrativo
- inclusão no grupo sudo
- validação de privilégios
- verificação das permissões dos diretórios pessoais
- instalação/configuração do OpenSSH Server
- desabilitação de login SSH direto como root
- validação da configuração com sshd -t
- validação do serviço SSH

## Comandos importantes

Foi criado um usuário administrativo separado da conta root, com privilégios elevados controlados pelo sudo. O acesso SSH direto ao root foi desabilitado e a configuração foi validada antes da aplicação.
