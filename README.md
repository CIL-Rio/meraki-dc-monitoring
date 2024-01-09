# Meraki DC Monitoring
Meraki DC Monitoring 
## Descrição
Variações de temperatura, umidade e vazamentos de líquidos podem comprometer a infraestrutura de um Data Center e interromper os serviços hospedados nele. Os sensores da Meraki oferecem uma visão detalhada dessas variáveis ambientais, antes inacessíveis aos times de TI. Esta demonstração tem como objetivo ampliar as funcionalidades dos sensores, fornecendo informações adicionais obtidas de câmeras, redes e plataformas de terceiros.

## Objetivos
1.	Detecção de abertura/fechamento da porta do DC no decorrer do tempo
2.	Detecção da entrada de pessoas (quantidade / local de acesso / tempo de permanência)
3.	Correlação armário acessado x componentes do armário x possíveis áreas afetadas – Gestão de mudanças e riscos 
4.	Medição de Temperatura do ambiente e comportamento após abertura da porta 
5.	Medição da Temperatura e Humidade do Ambiente no decorrer do tempo
6.	Medição de Vazamento de Água no Ambiente no decorrer do tempo
7.	Medição do Consumo Energético no decorrer do tempo
8.	Integração com ServiceNow para abertura de Ticket

## BOM 
| *Dispositivo/Licença* |	*Quantidade* |
|---------------------|------------|
|Sensor Meraki MT40 |	1 |
|Sensor Meraki MT12 |	1|
|Sensor Meraki MT20 |	1|
|Sensor Meraki MT14 |	2|
|Câmera Meraki MV12 |	2|
|Switch Catalyst 9300 |	2|
|Licença ServiceNow | 	1|

## Arquitetura

A linha de sensores Meraki usa o protocolo Bluetooth BLE para transmitir os dados coletados para a nuvem Meraki. Para concretizar essa transmissão, é necessário a utilização de um gateway IoT. Este gateway pode ser uma câmera ou um Access Point Meraki com funcionalidade BLE.

Nesta demonstração, as câmeras MV12 atuam como gateways IoT. As imagens coletadas pelas câmeras também são usadas para ajudar a correlacionar eventos. Um exemplo seria uma falha elétrica do tipo falta de energia em um switch, aqui representado pelos Catalyst 9300. A falta de energia pode ser causada por uma falha entre a PSU e o switch, uma falha elétrica no próprio switch ou no fornecimento de energia da PSU. No primeiro caso o mal contado pode ser provocado por ação humana, intencional ou não. Ao combinarmos as informações de vídeo analítico, o status do switch e as informações do sensor de energia da Meraki conseguimos definir onde defeito foi originado e se tem causa humana ou não.

Para podermos fazer correlação dos eventos de rede, sensores e câmeras precisamos de uma base de dados comum. A base de dados escolhida para isso foi o InfluxDB, que uma base de dados para series temporais.  Os dados são enviados pela nuvem Meraki via protocolo MQTT para um broker MQTT hospedado em nosso DataCenter. O Broker MQTT utilizado é Mosquitto MQTT.  Os dados são enviados do broker para o banco de dados via NodeRed. O NodeRed é uma plataforma LowCode que permite a criação de automações simples sem a necessidade de código.

Uma vez no banco de dados utilizamos o Grafana Dashboard para criar gráficos a partir dos dados coletados. A partir do Grafana os usuários podem ter uma visão instantânea dos dados dos sensores e câmeras, e usá-los para fazer correlações. 

![Arquitetura](arch.png)