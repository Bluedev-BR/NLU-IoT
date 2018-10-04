# Controlando Dispositivos IoT por Linguagem Natural

Esse é o repositório oficial da demonstração "Controlando Dispositivos IoT por Linguagem Natural" criada por Eduardo Petecof Mattoso e João Pedro Poloni Ponce. Aqui você encontra todos os códigos e recursos utilizados, bem como um tutorial para reproduzir a aplicação em sua conta na IBM Cloud.

O objetivo da demonstração é construir um assistente virtual, que entende a voz ou o texto de um interlocutor, e envia comandos para os dispositivos IoT quando for necessário. Os comandos indicam que o dispositivo deve acender uma lâmpada ou obter o valor registrado por um sensor.

Tudo que você precisa para reproduzir essa demonstração é [criar a sua conta](https://bluemix.net). Todos os serviços utilizados nessa demonstração são gratuitos.

## Componentes

* IBM Watson IoT Platform
* IBM Cloud Functions
* IBM Watson Assistant
* Devices plausíveis
    * Node-RED (Simulação)
    * NodeMCU
    * Omega2+
* Opcional
    * Mobile App


## IBM Watson IoT Platform 

A Watson IoT Platform é um serviço que permite conectar dispositivos IoT à nuvem. Dessa forma, eles podem receber e enviar dados de forma segura e gerenciada, usando o protocolo MQTT.

Usuários e aplicações podem utilizar a plataforma pelo Dashboard ou através de APIs para acompanhar, em tempo real, a situação de cada dispostivo e enviar comandos a eles.

Siga o tutorial em [PDF](./presentationv1-03-10.pdf) para saber como criar e configurar o serviço na sua conta.

## IBM Cloud Functions

IBM Cloud Functions é o nome do serviço de FaaS (Functions as a Service) na IBM Cloud. Através dele você pode executar o código de uma função diretamente na Nuvem e disponibilizar essa rotina através de APIs para outras pessoas e aplicações. Você não precisa se preocupar com a infraestrutura necessária para executar o código, mesmo que centenas (ou até milhares) de pessoas decidam executar aquela a sua rotina ao mesmo tempo. 

Toda vez que a sua Cloud Function é acionada a nuvem já está preparada com um conjunto de containers em estado de hibernação, ela seleciona um deles para executar a sua rotina e o substitui assim que a execução termina. Ao invés de acionar a sua função manualmente você também pode definir gatilhos para execução, como um agendamento por exemplo. 

Nessa demonstração vamos utilizar duas funções que enviam comandos aos dispositivos conectados na Watson IoT Platform e serão engatilhadas pelo Assistente Virtual. O código delas está disponível nesse [repositório](./Functions) e você pode entender como criar e executar uma Cloud Function no tutorial em [PDF](./presentationv1-03-10.pdf).

Exemplo de função:

```javascript
function main(param){
    if (param.example != undefined) return {msg: param.example}
    else return {msg: 'param is null'}
}
```

Exemplo de parâmetros de entrada:

```json
{
    "example": "Hello Wordl",
    "example01": 23,
    "example02":{
        "inside01":32,
        "inside02": "Inside Wordl"
    }
}
```

## IBM Watson Assistant

IBM Watson Assitant é o serviço da IBM Cloud capaz de criar Assistentes Virtuais que conversam em linguagem natural com um usuário. Para utilizá-lo você precisa inserir na ferramenta exemplos de frases que o usuário do assistente pode dizer, bem como a intenção na qual aquela frase deve ser classificada. Com essa informação, algoritmos de aprendizado de máquina serão utilizados para treinar o seu Assistente, o qual passará a reconhecer aquela intenção quando frases parecidas (não necessáriamente iguais) forem ditas pelo usuário.

Quando for detectada uma intenção, você também pode, na ferramenta, informar a resposta que o seu Assistente deveria devolver ao usuário. Partindo desse princípio básico a ferramenta disponibiliza uma série de outros recursos para que o Assistente Virtual simule cada vez melhor uma conversa humana. 

Além disso, no caso dessa demo, usaremos o reconhecimento de certas intenções como um gatilho para a ativação das Cloud Functions, as quais enviarão comandos para os nossos devices conectados na Watson IoT Platform. Para ter uma ideia geral da solução, consulte esse diagrama de arquitetura:

## Diagrama de Arquitetura

![Diagrama de Arquitetura](/Img/diagrama.jpg)

## Dispositivos

Essa demonstração foi planejada utilizando os dois tipos de dispositivos reais descritos abaixo: 

### NodeMCU Files
![NodeMCU](/Img/NodeMCU.jpeg)

[NodeMCU](http://nodemcu.com/index_en.html) is "An open-source firmware and development kit that helps you to prototype your IOT product within a few Lua script lines. {...} The Development Kit based on ESP8266, integates GPIO, PWM, IIC, 1-Wire and ADC all in one board. Power your developement in the fastest way combinating with NodeMcu Firmware!"

Se você tiver um NodeMCU basta baixar o código disponível nessa [pasta](./Device%20Code/ESP8266) e seguir as indicações dos comentários.

### Omega2+ Files
![Omega2+Img](/Img/Omega2.jpeg)

[Omega2+](http://onion.io) é uma placa de desenvolvimento especificamente projetada para IoT e de acordo com seus produtores "World's smallest Linux server".

Por possuir o kernel do linux, ele pode rodar qualquer linguagem de programação. Para este projeto foi usado especificamente Node.js para conectar e executar os comandos enviados pela plataforma de IoT.

Se você possui um Omega2+ ou alguma placa de desenvolvimento similar, basta você copiar o conteúdo da pasta do [Omega](./Device\ Code/Omega2+) para a sua placa e seguir os seguintes passos:
1. Execute `npm install`;
2. Preencha o arquivo .env com suas credenciais da plataforma de IoT
   ```env
   IOT_ORG=<YOUR_IOT_ORG_HERE>
   IOT_ID=<YOUR_IOT_ID_HERE>
   IOT_DEVICE_TYPE=<YOUR_IOT_DEVICE_TYPE>
   IOT_AUTH_KEY=<YOUR_IOT_AUTH_KEY>
   IOT_AUTH_TOKEN=<YOUR_IOT_AUTH_TOKEN>
   ```
3. Execute `npm start`

### Dispositivo Simulado

Se você não tiver um dispositivo em mãos você pode criar um dispositivo simulado usando um servidor na nuvem. Nós mostramos como isso é feito no tutorial em [PDF](./presentationv1-03-10.pdf).

### Mobile app

Em breve.

## Contributors
* [Eduardo Petecof Mattoso](https://github.com/epetecof)
* [Joao Pedro Poloni Ponce](https://github.com/JoaoPedroPP)

## Special Thanks
* [Danilo Paschon](https://www.linkedin.com/in/danilo-paschon/) -> without him there will be no project
  

![Node+Omega](/Img/MCU+Omega.jpeg)
