# Sistema de Reserva de Hoteis

## Introdução

Nesse cenário eu estou tentando criar a arquitetura de um sistema de hoteis que funcione e possa ser minimamente escalável. 
<br>
Nesse documento estou explicando os componentes e o por que estou utilizando eles.

## Solução Proposta

![image](https://github.com/user-attachments/assets/41ae7ad5-970b-4dca-9b35-5541a7c7a301)


## Serviços

**Hotel Management Service** -> Serviço responsável por cadastrar, editar, buscar e deletar os quartos de hotéis cadastrados em nosso sistema.
<br>
**Booking Service** -> Serviço responsável por gerar reservas de quartos, edições nas reservas, cancelamentos e etc.
<br>
**Pricing Service** -> Serviço responsável por calcular de forma dinamica o preço da reserva.
<br>
**Notification Service** -> Serviço responsável por emitir comunicações como por exemplo emails e sms's.
<br>
**Payment Service** -> Serviço responsável por gerar cobranças e realizar reembolsos.


## Escolhas de Banco de Dados

**PostgreSQL - CA** -> Escolhido para os serviços de **Hotel Management Service** e **Payment Service** devido a sua confiabilidade e disponibilidade. Eu considerei esses serviços sendo cruciais para o sistema e confiar de que sempre terão meus dados atualizados e de facil acesso é imprescindivel.
<br>
**MongoDB - CP** -> Escolhido para o serviço de **Booking Service**. O serviço consumidor é um dos mais importantes do meu sistema e provavelmente será o serviço mais consumido em escrita e edição porem são dados em que ainda assim requerem um alto nivel de confiabilidade. Alem disso o mongo tambem pode garantir transações ACID o que me fez escolher ele para aguentar essa grande carga de requisições.
<br>
**Redis - CA** -> Escolhido como nosso sistema de cache. Ele foi plugado a todos os serviços em que eu acredito ser importante o uso de cache para facilitar a escalabilidade do meu sistema.

## Brokers / Mensageria

**SQS** -> Utilizado para garantiar a confiabilidade de que meu sistema não perderiam essas mensagens por instabilidade e são recursos em que eu posso adiar e processar de forma asyncrona.
<br>
<br>
De inicio eu havia acoplado minhas filas em um tópico do SNS, porem depois encontrei dificuldades pois encontrei diversos cenários onde eu precisaria acionar somente uma das filas e prefiri por esse motivo remover meu SNS.

## Recursos de segurança

**Web Application Firewall - WAF** -> Recurso fornecido pela AWS que oferece uma camada de segurança para nossas web-applications.
<br>
**SHIELD** -> Recurso de segurança fornecido pela AWS que visa evitar ataques DDOS e proteger seu sistema de sobrecargas inesperadas.
