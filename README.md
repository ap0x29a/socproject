# socproject
My self challange for develop some practical skills within cibersecurity

Documentação Detalhada do Diagrama de Rede
1. Descrição Geral:
Este diagrama descreve uma arquitetura de rede interna que incorpora componentes de segurança da informação e monitoramento centralizado de logs. A rede utiliza a faixa de IP privada 192.168.10.0/24, que é comum para redes locais. O diagrama ilustra a comunicação entre diversos dispositivos alocados em máquinas virtuais, incluindo um servidor Splunk, uma máquina Windows 10, um servidor Active Directory (AD), e uma máquina Kali Linux, que simula um atacante. A função central do Splunk é coletar, indexar e correlacionar os logs de diversas fontes, incluindo o sistema Sysmon, para identificar possíveis anomalias ou intrusões na rede.

O ambiente também está conectado à internet, permitindo a troca de informações externas, mas também introduzindo uma superfície de ataque que requer monitoramento.

2. Componentes do Diagrama:
2.1. Rede:
Faixa de IP: 192.168.10.0/24
A rede local segue o padrão de endereçamento /24, o que permite um total de 254 hosts disponíveis.
Cada dispositivo conectado possui um endereço IP único dentro dessa faixa, garantindo que não haja conflitos de IP.
Função da Rede:
A rede interna conecta os dispositivos internos (servidor Splunk, AD, Windows 10) ao roteador, que por sua vez se conecta à internet.
Permite que o Splunk colete logs de cada dispositivo conectado, como o servidor AD e a máquina Windows 10.
2.2. Servidor Splunk:
IP: 192.168.10.10
Este é o endereço IP do servidor central de logs responsável por coletar, analisar e visualizar os eventos de toda a rede.
Função:
O Splunk é uma plataforma de gestão de logs e segurança que indexa dados de eventos e permite aos administradores monitorar a rede em tempo real. Ele ajuda a detectar ataques, monitorar o desempenho do sistema e gerar alertas em caso de anomalias.
Monitoramento Centralizado: Coleta dados de outros dispositivos, centralizando as informações para análise.
Aplicações/Serviços:
Splunk forwarder: Um agente instalado nas máquinas clientes que coleta logs e os encaminha para o servidor Splunk.
Sysmon (System Monitor): Ferramenta da Microsoft usada para monitorar eventos detalhados do sistema, como criação de processos, alteração de arquivos e tráfego de rede. Ele gera eventos detalhados que são enviados para o Splunk para uma análise mais profunda.
2.3. Active Directory (AD):
IP: 192.168.10.7

O controlador de domínio AD (Active Directory) gerencia as identidades, políticas de segurança e autenticação para os usuários e máquinas na rede.
Função:

O AD é essencial para autenticação e controle de acesso, fornecendo segurança para a rede ao gerenciar quem pode acessar os recursos da rede e como.
Ele fornece suporte para políticas de segurança, como controle de senha, permissões de grupo, e autenticação via Kerberos ou LDAP.
Aplicações/Serviços:

Splunk forwarder: O agente instalado no servidor AD coleta logs de eventos relacionados a autenticações de usuários, tentativas de login malsucedidas, e outras atividades críticas de segurança. Esses logs são enviados para o Splunk para análise centralizada.
Sysmon: Além dos eventos padrões do Windows, o Sysmon no servidor AD coleta dados específicos de segurança, como criação de processos suspeitos, tráfego de rede incomum e alterações nos arquivos críticos do sistema.
2.4. Máquina Windows 10:
Configuração de Rede: A máquina Windows recebe seu IP via DHCP, o que significa que o servidor de DHCP da rede atribui dinamicamente o endereço IP.

Função:

A máquina Windows representa um cliente típico da rede. Ela pode ser usada por um usuário regular que acessa os recursos da rede, como o servidor AD, ou pode ser alvo de testes de segurança para simular ataques.
Aplicações/Serviços:

Splunk forwarder: Similar às outras máquinas na rede, o Windows 10 também possui um agente Splunk forwarder instalado. Este agente coleta logs do sistema, tais como registros de login, execução de processos e tentativas de conexão de rede.
Sysmon: O Sysmon no Windows 10 monitora eventos como a execução de programas, criação de conexões de rede, modificações de arquivos críticos e logs de segurança. Esses dados são enviados para o Splunk, permitindo uma visão completa da atividade da máquina.
2.5. Kali Linux (Atacante):
IP: 192.168.10.250

A máquina Kali Linux é usada para testes de intrusão ou simulação de ataques. O Kali é amplamente utilizado por pentesters e hackers éticos devido à sua vasta gama de ferramentas para explorar vulnerabilidades.
Função:

Simulação de ataques: Esta máquina pode estar realizando varreduras de rede, exploração de vulnerabilidades, ataques de força bruta contra o AD ou outros sistemas, ou análise de segurança.
Ela é configurada como uma máquina de atacante e pode ser usada para testar a capacidade da rede de detectar e responder a ataques.
Ferramentas Usadas:

Nmap: Para varredura de portas e descoberta de serviços.
Metasploit: Para exploração de vulnerabilidades em sistemas Windows, AD e outros.
Wireshark: Para análise de tráfego de rede.
2.6. Internet:
A rede local tem conexão direta com a internet, que é uma fonte de dados, serviços, mas também de potenciais ameaças externas.
O tráfego entre a rede local e a internet pode ser monitorado por ferramentas de firewall ou IDS/IPS, dependendo das configurações de segurança implementadas.
3. Fluxo de Dados:
Coleta de Logs:

Todos os dispositivos, incluindo o servidor AD, a máquina Windows 10 e o servidor Splunk, estão configurados para enviar logs ao servidor Splunk central. Isso inclui:
Logs de segurança, como tentativas de login e falhas de autenticação.
Eventos de sistema, como criação de processos, modificações em arquivos e tráfego de rede monitorado pelo Sysmon.
A máquina Kali Linux não está enviando logs para o Splunk, pois está configurada como atacante.
Análise de Logs:

O servidor Splunk indexa e correlaciona os logs de todos os dispositivos, permitindo que os administradores monitorem a rede em tempo real e respondam a incidentes de segurança rapidamente.
Qualquer atividade suspeita detectada pelo Sysmon ou pelos logs de autenticação do AD pode gerar alertas automáticos no Splunk.
4. Segurança:
Monitoramento Contínuo:

O uso de Sysmon em todas as máquinas críticas (AD, Windows 10) indica uma abordagem de segurança baseada no monitoramento proativo. Sysmon gera eventos detalhados sobre processos e tráfego de rede, o que ajuda a identificar comportamentos anômalos ou tentativas de invasão.
Splunk como SIEM:

A presença do Splunk indica que a organização está utilizando um SIEM (Security Information and Event Management) para centralizar a coleta e análise de logs. O SIEM é essencial para a detecção e resposta a ameaças em tempo real.
Ameaças Potenciais:

A máquina Kali Linux está simulando um atacante. Possíveis ataques incluem:
Varredura de rede para encontrar portas abertas.
Tentativas de login bruteforce no AD.
Exploits conhecidos contra sistemas desatualizados.
Respostas a Incidentes:

Qualquer atividade suspeita detectada no Splunk, como tentativas de login repetidas ou a execução de processos maliciosos, pode gerar alertas para a equipe de segurança agir rapidamente.
