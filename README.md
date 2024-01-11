# Meraki DC Monitoring
## Introdução
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

Para resolver o problema descrito acima, foi proposta a seguinte arquitetura de coleta e apresentação de dados. 
![Arquitetura](arch.png)

As informações coletadas pelos sensores e pela nuvem Meraki são escritas em um broker MQTT e, em seguida, armazenadas no banco de dados InfluxDB. Essas informações são usadas posteriormente pelo Grafana para a apresentação dos dados. A leitura dos dados escritos no broker e a gravação no banco de dados é feita pelo NodeRed.

A linha de sensores da Meraki usa o protocolo Bluetooth BLE para enviar os dados coletados para a nuvem Meraki, por meio de um gateway IoT. Este pode ser uma câmera ou um Access Point Meraki que suporte BLE. Para esta demonstração, as câmeras MV12 funcionam como gateways IoT e suas imagens também são utilizadas para auxiliar na correlação de eventos.

Um exemplo de aplicação é o diagnóstico de falhas elétricas, como uma interrupção de energia em um switch Catalyst 9300. Esse tipo de falha pode ser devido a diversas causas, incluindo ação humana. Combinando as informações de vídeo analítico, status do switch e dados do sensor de energia da Meraki, é possível determinar a origem do problema.

