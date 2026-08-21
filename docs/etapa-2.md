# Etapa 2 — Usuários, Privilégios e Controle de Acesso

## Objetivo

Implementar e validar o controle de acesso do servidor Linux, definindo quais usuários, grupos e serviços podem acessar recursos e quais operações podem realizar.

## Atividades realizadas

### 1. Usuários

- Criação, alteração e remoção de usuários.
- Gerenciamento de senhas.
- Identificação de usuários por UID.
- Configuração de diretórios pessoais e shell.
- Análise dos arquivos `/etc/passwd` e `/etc/shadow`.
- Uso dos comandos `useradd`, `adduser`, `usermod`, `passwd`, `userdel`, `id`, `whoami`, `who` e `w`.

### 2. Grupos

- Criação, alteração e remoção de grupos.
- Associação de usuários a grupos.
- Configuração de grupos primários e secundários.
- Uso dos comandos `groupadd`, `groupmod`, `groupdel`, `groups` e `gpasswd`.

### 3. Sudo

- Administração de privilégios através do `sudo`.
- Uso do `visudo` para validação das configurações.
- Análise do arquivo `/etc/sudoers`.
- Organização de regras específicas em `/etc/sudoers.d/`.
- Aplicação de privilégios administrativos de forma controlada.

### 4. Princípio do menor privilégio

Foram definidas diferentes contas de acordo com suas funções:

- **admin:** conta administrativa com acesso ao `sudo`.
- **svc-backup:** conta de serviço destinada às operações de backup, sem privilégios administrativos desnecessários.
- **auditor:** conta destinada à consulta dos dados necessários para auditoria.
- **usuário comum:** conta sem privilégios administrativos e sem acesso a recursos protegidos.

### 5. Permissões Linux

- Gerenciamento de permissões com `chmod`.
- Alteração de proprietário com `chown`.
- Alteração de grupo com `chgrp`.
- Análise das permissões através do `ls -l`.
- Compreensão dos níveis `owner`, `group` e `others`.
- Estudo das permissões `read (r)`, `write (w)` e `execute (x)`.
- Aplicação dos modos `400`, `600`, `640`, `644`, `700`, `750`, `755` e `770`.

### 6. ACL

- Configuração de permissões específicas utilizando `setfacl`.
- Consulta das ACLs utilizando `getfacl`.
- Uso de ACL para conceder permissões específicas a usuários ou grupos sem alterar o modelo tradicional de `owner/group/others`.

### 7. Permissões especiais

- Estudo e aplicação de **SUID**.
- Estudo e aplicação de **SGID**.
- Estudo e aplicação do **Sticky Bit**.
- Análise dos impactos dessas permissões na segurança do sistema.

### 8. Umask

- Análise da configuração atual de `umask`.
- Entendimento de como a `umask` influencia as permissões padrão de arquivos e diretórios.
- Testes de criação de arquivos e diretórios para validar o comportamento.

### 9. Testes de acesso

Foram realizados testes para comprovar que as permissões configuradas estão funcionando corretamente.

| Usuário | Acesso esperado |
|---|---|
| `admin` | Acesso administrativo através do `sudo` |
| `svc-backup` | Acesso somente aos recursos necessários para backup |
| `auditor` | Acesso somente aos dados definidos para auditoria |
| Usuário comum | Acesso negado aos recursos restritos |

Os testes devem validar tanto os acessos permitidos quanto os acessos negados.

## Resultado

Ao final da etapa, o servidor possui um modelo de controle de acesso baseado em usuários, grupos, permissões, ACLs e privilégios administrativos.

A configuração segue o **princípio do menor privilégio**, garantindo que cada usuário ou serviço tenha somente as permissões necessárias para executar sua função.

Todas as configurações devem ser validadas através de testes antes de serem consideradas concluídas.
