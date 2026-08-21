# Etapa 5 — Logs e Monitoramento

## Objetivo

Complementar o projeto com procedimentos básicos de operação, diagnóstico e monitoramento do servidor Linux, utilizando ferramentas nativas do Ubuntu.

O objetivo é verificar o estado dos serviços, acompanhar logs, identificar erros, monitorar recursos do sistema, verificar portas de rede e realizar um health check básico.

## 1- Verificar serviços com falha

systemctl --failed - Mostra as unidades do systemd que estão em estado de falha

Esse comando permite identificar rapidamente serviços que apresentaram problemas.

Se nenhum serviço estiver com falha, o sistema deverá informar que não existem unidades carregadas em estado failed.

## 2- Verificar o estado do SSH

systemctl is-active ssh - Verifica se o serviço SSH está ativo

systemctl is-enabled ssh - Verifica se o SSH está configurado para iniciar automaticamente

O primeiro comando verifica o estado atual do serviço. O segundo verifica se o serviço está habilitado para iniciar durante o boot.

## 3- Consultar os logs do sistema

sudo journalctl - Mostra os registros armazenados pelo systemd-journald

sudo journalctl -b - Mostra os logs referentes ao boot atual

sudo journalctl -b -p err - Mostra mensagens de erro registradas desde o boot atual

journalctl é utilizado para consultar os eventos registrados pelo sistema. Os filtros permitem limitar a consulta e facilitar a identificação de problemas.

## 4- Consultar logs do SSH

sudo journalctl -u ssh - Mostra os logs relacionados ao serviço SSH

sudo journalctl -u ssh -n 50 - Mostra as últimas 50 mensagens do SSH

sudo journalctl -u ssh -f - Acompanha novas mensagens do SSH em tempo real

sudo journalctl -u ssh | grep -i "failed" - Procura mensagens relacionadas a falhas de autenticação

sudo journalctl -u ssh | grep -i "accepted" - Procura mensagens relacionadas a autenticações aceitas

O parâmetro -u seleciona uma unidade do systemd. O parâmetro -n limita a quantidade de mensagens exibidas. O parâmetro -f acompanha novas mensagens em tempo real.

grep é utilizado para localizar determinado texto dentro da saída de outro comando. O parâmetro -i ignora diferenças entre letras maiúsculas e minúsculas.

Para sair do modo de acompanhamento em tempo real:

Ctrl + C - Interrompe o comando atual

## 5- Monitorar espaço em disco

df -h - Mostra o uso dos sistemas de arquivos em formato legível

sudo du -sh /var/log - Mostra quanto espaço os logs estão utilizando

sudo du -sh /var/log/* - Mostra o espaço utilizado pelos arquivos e diretórios dentro de /var/log

df permite verificar o espaço disponível nos sistemas de arquivos. O campo Use% deve ser acompanhado principalmente no sistema de arquivos raiz.

du permite identificar quais diretórios estão consumindo espaço.

O parâmetro -h apresenta os valores em unidades mais fáceis de interpretar. O parâmetro -s apresenta apenas um resumo.

## 6- Monitorar memória

free -h - Mostra o uso da memória RAM em formato legível

O campo available é importante para avaliar quanta memória ainda está disponível para utilização pelo sistema e pelos processos.

## 7- Monitorar processos

ps aux - Lista os processos em execução

ps aux | grep ssh - Procura processos relacionados ao SSH

ps permite verificar processos, usuários, consumo de recursos e identificadores de processo.

O símbolo | é o pipe, utilizado para enviar a saída de um comando como entrada para outro comando.

## 8- Monitoramento em tempo real

top - Mostra processos e utilização de recursos em tempo real

O comando top permite acompanhar CPU, memória, processos, usuários, PID e carga do sistema.

Para sair:

q - Sai do top

## 9- Verificar portas e serviços de rede

sudo ss -tulpen - Mostra portas TCP e UDP em estado de escuta e os processos associados

Parâmetros utilizados:

-t - Mostra conexões TCP
-u - Mostra conexões UDP
-l - Mostra portas em estado de escuta
-p - Mostra o processo associado
-e - Mostra informações adicionais
-n - Mostra números diretamente sem resolver nomes

Essa verificação permite comparar as portas que deveriam estar disponíveis com as portas que realmente estão abertas no servidor.

Essa análise complementa a configuração realizada na etapa de firewall.

## 10- Verificar o firewall

sudo ufw status verbose - Mostra o estado do firewall e suas regras com detalhes

sudo ufw status numbered - Mostra as regras do UFW numeradas

A verificação permite confirmar se as regras configuradas anteriormente continuam ativas e se o firewall está funcionando conforme o planejado.

## 11- Health Check básico

Foi realizada uma verificação geral do servidor utilizando:

uptime - Verifica tempo de atividade e carga do sistema

free -h - Verifica utilização da memória

df -h - Verifica utilização do disco

systemctl --failed - Verifica serviços com falha

systemctl is-active ssh - Verifica o estado atual do SSH

sudo ss -tulpen - Verifica portas e serviços em escuta

sudo journalctl -b -p err - Verifica erros registrados no boot atual

sudo ufw status verbose - Verifica o estado do firewall

O conjunto desses comandos permite realizar uma verificação inicial de saúde do servidor sem a necessidade de ferramentas externas.

## 12- Resultado

Com esta etapa foi possível:

* Verificar serviços em estado de falha.
* Verificar o funcionamento e inicialização do SSH.
* Consultar logs do sistema através do journalctl.
* Consultar eventos específicos do SSH.
* Identificar possíveis falhas de autenticação.
* Monitorar utilização de disco e memória.
* Identificar processos em execução.
* Acompanhar recursos em tempo real.
* Verificar portas de rede em estado de escuta.
* Validar o estado do firewall.
* Realizar um health check básico do servidor.

## Conclusão

O servidor passou a contar com procedimentos básicos de logs, diagnóstico e monitoramento utilizando ferramentas nativas do Linux.

Essas verificações complementam as configurações de segurança realizadas nas etapas anteriores, permitindo não apenas configurar o servidor, mas também acompanhar seu funcionamento e identificar possíveis problemas.
