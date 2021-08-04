# WIFI

## WINDOWS 10: GESTÃO DE REDES WIRELESS ATRAVÉS DA LINHA DE COMANDOS
##### NUNO SILVA

## Introdução

No Windows 10, os utilizadores podem fazer a gestão e configuração das redes Wireless a partir de dois locais distintos: Painel de Controlo e App Definições. Aqui podemos fazer configurações básicas, como alterar definições da placa, ativar endereços aleatório de hardware, esquecer redes Wireless às quais já não fazemos ligação, etc.

Windows 10: App Definições - Wi-FiContudo, existem inúmeras tarefas que estas aplicações não permitem executar, como por exemplo obter informações detalhadas sobre a placa de rede e respetivos drivers, informações sobre os diversos perfis de redes armazenados no PC e respetivas passwords, gerar relatórios que permitem despistar problemas de conectividade, entre outros.

Para os casos referidos anteriormente, normalmente recorremos a um utilitário da Linha de Comandos disponível desde o Windows 2000 e que se chama Netsh. O Netsh é utilizado para gerir redes com e sem fios e a através de um conjunto de parâmetros, podemos configurar, obter informações e resolver problemas de conectividade em praticamente todos os tipos de adaptadores.

Vejamos então em detalhe, quais os comandos Netsh WLAN disponíveis e que nos permitem executar algumas das tarefas referidas anteriormente.

## Aceder à Linha de Comandos no Windows 10

Antes de começar a analisar cada um dos comandos, é necessário abrir uma janela da Linha de Comandos com privilégios de administrador.

Uma das formas mais rápidas de o fazer no Windows 10, é através da combinação de teclas WIN + X abrir o Menu Power User e em seguida, selecionar a opção Linha de Comandos (admin).Windows 10: Linha de Comandos (admin)

## Informações sobre os drivers dos adaptadores

Para obter informações pormenorizadas sobres os drivers da placa de rede Wireless, vamos escrever o seguinte comando: Netsh WLAN show drivers

Este comando mostra as principais informações sobre os drivers instalados no PC, e que normalmente incluem o nome do driver e fabricante, data e versão, tipo e rádios suportados, entre outras.Netsh WLAN show drivers

## Informações sobre as capacidades do adaptador Wireless

Para além das informações gerais sobre os drivers, é possível obter informações sobre outras capacidades suportadas pela placa de rede.

Para isso utilizamos o comando: Netsh WLAN show wirelesscapabilitiesNetsh WLAN show wirelesscapabilities

## Informações sobre as definições do adaptador

Para obtermos informações especificas de todas as placas de rede instaladas no PC, como por exemplo Nome, Tipo de Rede, Tipo de Radio, SSID, Autenticação, etc., utilizamos o seguinte comando: Netsh WLAN show interfacesNetsh WLAN show interfacesPara obter informações sobre apenas uma das placas de rede instaladas, podemos usar o seguinte comando: Netsh WLAN show interface name="nome do interface"Netsh WLAN show interface name nome do interface

## Informações sobre os perfis de redes wireless armazenadas no PC

Quando nos ligamos a determinado Access Point Wireless, o Windows 10 cria automaticamente um novo perfil de rede wireless que guarda localmente no PC.

Para visualizar os perfis que estão associados a determinada placa de rede e quais os utilizadores com permissões para fazer ligações através destes, utilizamos o comando: Netsh WLAN show profilesNetsh WLAN show profilesTal como acontece com os interfaces, podemos obter estas informações para apenas uma placa de rede. Para isso, utilizamos o comando Netsh WLAN show profiles interface="nome do interface"Netsh WLAN show profiles interface nome do interface

## Visualizar as passwords dos perfis das redes wireless

No Windows 10, acedendo às propriedades da placa de rede no Painel de Controlo, conseguimos visualizar a password da rede wireless à qual estamos ligados, contudo, existindo mais do que um perfil guardado, não conseguimos visualizar as passwords definidas nos restantes perfis.

Para visualizar as passwords de todos os perfis guardados no PC, utilizamos o comando: Netsh WLAN show profile name="nome do perfil" key=clear

O parâmetro key=clear, vai mostrar a password associada ao perfil em questão no formato de texto.Netsh WLAN show profile name nome do perfil key clear

## Importar/Exportar perfis de redes wireless

No Windows 10 e também Windows 8.x, a utilização da Microsoft Account vem permitir a sincronização de determinadas definições entre os vários dispositivos que usamos, nomeadamente as definições wireless. Com este novo cenário, a funcionalidade no Painel de Controlo que permitia exportar/importar perfis de redes wireless e que ainda se encontra disponível no Windows 7, foi descontinuada.

Ainda assim e caso seja necessário, os utilizadores através do comando Netsh WLAN podem continuar a exportar/importar estes perfis.

Para isso, utilizamos o seguinte comando: Netsh WLAN export profile key=clear folder="caminho da pasta"Netsh WLAN export profile keyclear folder caminho da pastaEste comando, cria um ficheiro .xml por cada perfil guardado no PC e contém todas as informações relacionadas com os mesmos, incluindo as passwords. Caso não queiramos que esta informação inclua as passwords por uma questão de segurança, devemos excluir o parâmetro key=clear.Windows 10: perfis de redes wirelessNo caso de queremos exportar apenas um perfil, vamos acrescentar ao comando anterior o parâmetro name="nome perfil": Netsh WLAN export profile name="nome perfil" key=clear folder="caminho da pasta"Netsh WLAN export profile name nome perfil key clear folder caminho da pastaQuando for necessário fazer a importação destes perfis noutro dispositivo, podemos considerar 2 cenários:

Importar o ficheiro .xml dos vários perfis para que fiquem disponíveis em todos os interfaces e para todos os utilizadores.

Importar o ficheiro .xml dos vários perfis para determinado perfil e apenas para o utilizador com sessão iniciada.

Assim, usamos para o primeiro cenário o comando: Netsh WLAN add profile filename="caminho_ficheiro.XML"Netsh WLAN add profile filenamecaminho ficheiro XMLPara o segundo cenário, utilizamos o comando: Netsh WLAN add profile filename=" caminho_ficheiro.xml" Interface="nome do interface" user=currentNetsh WLAN add profile filename caminho ficheiro xml Interface nome do interface user current

## Desativar a ligação automática a redes wireless indisponíveis

Quando configuramos no PC ligações a redes wireless que usamos com frequência, normalmente optamos por definir que as próximas ligações sejam feitas de forma automática. Contudo, se uma destas redes oferece uma qualidade de ligação inferior ou simplesmente deixamos de a usar com frequência, podemos definir novamente que a ligação seja feita de forma manual sem ter que remover o perfil.

Para isso, vamos usar o comando: Netsh WLAN set profileparameter name="Profile_Name" connectionmode=manualNetsh WLAN set profileparameter name Profile_Name connectionmode manualNos perfis cuja ligação está definida como automática, podemos ainda indicar qual o perfil prioritário, ou seja, qual é o que estabelece uma ligação em primeiro ligar.

Para tal utilizamos o comando: Netsh WLAN set profileparameter name=" Profile_Name" connectionmode=autoNetsh WLAN set profileparameter name Profile_Name connectionmode auto

## Eliminar perfis de redes wireless

Ao fim de algum tempo a fazer ligações a diversas redes wireless, vamos acumulando perfis que usamos apenas uma ou duas vezes e que não pensamos voltar a utilizar. Nestes casos, podemos apagar permanente estes perfis do PC. Em termos de sincronização e ao contrario do que acontece quando adicionamos novos perfis, quando eliminamos um perfil de rede num PC com Windows 10, o mesmo não é eliminado nos restantes dispositivos que possuímos e onde usamos a mesma MSA.

Para eliminar um perfil, vamos utilizar o seguinte comando: Netsh WLAN delete profile name="nome perfil"Netsh WLAN delete profile name Profile_Name

## Criar relatórios sobre o adaptador e ligações

Quando estamos perante problemas de conectividade, nem sempre é fácil fazer a despistagem e avançar para a resolução. Para nos ajudar nesta tarefa, o Windows 10 disponibiliza uma ferramenta que permite gerar um relatório da rede wireless. Este relatório detalhado, apresenta em forma de gráficos inúmeras informações que nos podem ajudar a identificar eventuais problemas.

Para gerar um relatório, vamos utilizar o seguinte comando: Netsh WLAN show WLANreportNetsh WLAN show WLANreportO relatório é então gerado no formato .HTML e guardado na pasta C:\ProgramData\Microsoft\Windows\WlanReport\wlan-report-latest.htmlWindows 10: relatório de redes wirelessNeste relatório vamos encontrar informações gerais do sistema e do utilizador, informações sobre os adaptadores disponíveis, sumário de ligações sucedidas/falhadas, duração das sessões, eventos, erros, entre outras.Windows 10: relatório detalhado de redes wireless

## Recursos

Para obter informações sobre outros comandos disponíveis, os utilizadores podem escrever na janela da linha de comandos “Netsh WLAN”.

Para saber mais sobre o comando Netsh e a sua utilização noutros cenários, como por exemplo LAN, WINSOCK, IPv4 e IPv6, entre outros, poderá consultar a lista disponível no site Microsoft TechNet e acessível através do link: Netsh Command Reference

Para obter ajuda na resolução de problemas de conectividade no Windows 10, poderá consultar a informação disponível no site do Windows e acessível através do link: Corrigir problemas de ligação de rede.

## Conclusão

Em conclusão, apesar das configurações que os utilizadores podem fazer através do Painel de Controlo e App Definições, o comando Netsh WLAN dá-nos outras opções sem as quais não era possível executar tarefas igualmente importantes na gestão das redes wireless, como por exemplo, a recuperação das passwords ou a exportação/importação de perfis.

A possibilidade de criar um relatório detalhado sobre o estado das ligações wireless, é uma mais valia no que diz respeito ao despiste e resolução de problemas.

[Artigo original](https://www.revista-programar.info/artigos/windows-10-gestao-de-redes-wireless-atraves-da-linha-de-comandos/)