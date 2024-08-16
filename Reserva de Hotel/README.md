# Sistema de Reserva de Hoteis

## Introdução

Neste documento, estou elaborando a arquitetura de um sistema de reservas de hotéis que seja funcional e minimamente escalável. Apresentarei os componentes e explicarei as escolhas feitas para cada um deles.

## Solução Proposta

![image](https://github.com/user-attachments/assets/41ae7ad5-970b-4dca-9b35-5541a7c7a301)


## Serviços

**Hotel Management Service**: Serviço responsável por cadastrar, editar, buscar e deletar os quartos dos hotéis cadastrados em nosso sistema.

**Booking Service**: Serviço responsável por gerar reservas de quartos, realizar edições nas reservas, cancelamentos e outras operações relacionadas.

**Pricing Service**: Serviço responsável por calcular, de forma dinâmica, o preço das reservas.

**Notification Service**: Serviço responsável por emitir comunicações, como emails e SMS.

**Payment Service**: Serviço responsável por gerar cobranças e realizar reembolsos.


## Escolhas de Banco de Dados

**PostgreSQL (CA)**: Escolhido para os serviços de Hotel Management Service e Payment Service devido à sua confiabilidade e disponibilidade. Considero esses serviços cruciais para o sistema, e a certeza de que os dados estarão sempre atualizados e acessíveis é imprescindível.

**MongoDB (CP)**: Escolhido para o Booking Service. Este serviço provavelmente será o mais consumido em termos de escrita e edição, exigindo um banco de dados que suporte alta demanda de requisições e ainda assim ofereça confiabilidade. O MongoDB, que também oferece suporte a transações ACID, foi selecionado para lidar com essa carga.

**Redis (CA)**: Escolhido como sistema de cache, foi integrado aos serviços onde acredito ser importante o uso de cache para facilitar a escalabilidade do sistema.

## Brokers / Mensageria

**SQS**: Utilizado para garantir a confiabilidade de que meu sistema não perderá mensagens devido a instabilidades. É ideal para processar tarefas de forma assíncrona.

Inicialmente, havia integrado as filas a um tópico do SNS, mas encontrei cenários em que precisava acionar apenas uma das filas. Por esse motivo, optei por remover o SNS e simplificar o sistema.

## Recursos de segurança

**Web Application Firewall (WAF)**: Recurso fornecido pela AWS que oferece uma camada de segurança para as nossas aplicações web.

**AWS Shield**: Recurso de segurança fornecido pela AWS que visa evitar ataques DDoS e proteger o sistema contra sobrecargas inesperadas.
