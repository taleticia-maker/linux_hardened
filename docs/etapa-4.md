# Etapa 4 — Firewall e Controle de Tráfego

## Objetivo

Configurar o firewall do servidor para controlar o tráfego de rede e reduzir a exposição de serviços desnecessários.

Nesta etapa foi utilizado o UFW (Uncomplicated Firewall), adotando uma política de bloqueio das conexões de entrada por padrão e permitindo apenas os acessos necessários para a administração do servidor.

---

## 1. Verificação do UFW

Primeiro foi verificado se o UFW estava instalado e qual era seu estado atual.

    sudo ufw version
    sudo ufw status verbose

O firewall estava inicialmente inativo.

---

## 2. Definição da política padrão

Foi configurado o bloqueio das conexões de entrada e a permissão das conexões de saída:

    sudo ufw default deny incoming
    sudo ufw default allow outgoing

Com isso, novas conexões de entrada são bloqueadas caso não exista uma regra permitindo o acesso.

As conexões de saída iniciadas pelo próprio servidor permanecem permitidas.

---

## 3. Liberação do SSH

Antes de ativar o firewall, foi liberada a porta utilizada pelo SSH:

    sudo ufw allow 22/tcp

A porta 22 utiliza o protocolo TCP e é necessária para manter o acesso administrativo ao servidor.

Essa regra foi configurada antes da ativação do firewall para evitar o bloqueio do acesso administrativo.

---

## 4. Ativação do firewall

Após a criação da regra do SSH, o firewall foi ativado:

    sudo ufw enable

Em seguida, seu estado foi verificado:

    sudo ufw status verbose

---

## 5. Verificação das regras

As regras configuradas foram verificadas utilizando:

    sudo ufw status numbered

A configuração adotada possui:

- Entrada bloqueada por padrão.
- Saída permitida por padrão.
- Porta 22/TCP permitida para SSH.

Também foram verificadas as regras para IPv4 e IPv6.

---

## 6. Verificação das portas em uso

Foi utilizado o comando abaixo para identificar quais serviços estavam escutando em portas de rede:

    sudo ss -tulpen

Essa verificação permite comparar os serviços realmente disponíveis no sistema com as portas permitidas pelo firewall.

Uma porta em estado LISTEN indica que existe um serviço aguardando conexões, mas isso não significa necessariamente que ela esteja acessível externamente. O firewall pode bloquear as conexões destinadas a essa porta.

---

## 7. Resultado

Após a configuração, o servidor passou a utilizar uma política de controle de tráfego baseada no princípio de privilégio mínimo.

As conexões de entrada são bloqueadas por padrão e somente os serviços necessários podem ser liberados através de regras específicas.

O SSH permanece acessível pela porta 22/TCP para permitir a administração do servidor.

### Estado final

    Firewall: ativo
    Entrada: bloqueada por padrão
    Saída: permitida por padrão
    SSH: permitido — TCP/22
    IPv4: configurado
    IPv6: configurado

---

## Conceitos demonstrados

- Configuração de firewall no Linux.
- Utilização do UFW.
- Controle de tráfego de entrada e saída.
- Configuração de políticas padrão.
- Gerenciamento de regras de firewall.
- Proteção do acesso SSH.
- Verificação de portas e serviços com ss.
- Aplicação do princípio de privilégio mínimo.
- Consideração de IPv4 e IPv6.

## 8. Instalação do Fail2ban

O Fail2ban foi instalado para adicionar uma camada de proteção contra tentativas repetidas de autenticação no SSH.

    sudo apt update
    sudo apt install fail2ban -y

Após a instalação, o serviço foi verificado e configurado para iniciar automaticamente com o sistema.

---

## 9. Configuração do SSH no Fail2ban

Foi criado um arquivo de configuração específico para o SSH:

    /etc/fail2ban/jail.d/sshd.local

A configuração utilizada foi:

    [sshd]
    enabled = true
    port = ssh
    backend = systemd
    maxretry = 5
    findtime = 10m
    bantime = 10m

Essa configuração determina que cinco falhas de autenticação dentro de uma janela de dez minutos podem resultar no bloqueio temporário do endereço IP por dez minutos.

---

## 10. Verificação do Fail2ban

A configuração foi validada utilizando:

    sudo fail2ban-client -t

O serviço foi reiniciado e verificado:

    sudo systemctl restart fail2ban
    sudo systemctl status fail2ban

O jail do SSH foi verificado utilizando:

    sudo fail2ban-client status sshd

---

## 11. Teste de bloqueio

O mecanismo de banimento foi testado utilizando um endereço reservado para documentação e testes:

    sudo fail2ban-client set sshd banip 192.0.2.10

O endereço foi identificado como banido através de:

    sudo fail2ban-client status sshd

Após o teste, o bloqueio foi removido:

    sudo fail2ban-client set sshd unbanip 192.0.2.10

Esse procedimento permitiu validar o mecanismo de banimento sem bloquear o endereço utilizado para administrar a máquina.

---

## 12. Resultado

A etapa adicionou duas camadas de proteção ao servidor.

O UFW controla quais conexões podem chegar ao sistema, enquanto o Fail2ban monitora tentativas de autenticação e realiza bloqueios temporários diante de um número excessivo de falhas.

A combinação dos dois mecanismos reduz a exposição do SSH e fornece uma proteção básica contra ataques automatizados de força bruta.
