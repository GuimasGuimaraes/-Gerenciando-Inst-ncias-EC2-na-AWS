Projeto: Agendador Econômico de Instâncias EC2
Este projeto implementa uma solução serverless para automatizar o processo de iniciar e parar instâncias EC2 em horários pré-determinados. O objetivo principal é a redução de custos, garantindo que recursos (como ambientes de desenvolvimento ou homologação) só fiquem ligados durante o horário comercial.

Fluxo da Arquitetura
Identificação por Tags: As instâncias EC2 que devem ser gerenciadas (ex: ambiente de "Dev") recebem uma Tag específica (ex: Auto-Stop-Start = true).

Agendamento de "Ligar": Um Amazon EventBridge (Rule) é configurado com uma regra de agendamento (expressão cron) para disparar todo dia útil às 08:00.

Função Lambda "Start": O EventBridge aciona uma função AWS Lambda (Start-Instances). Esta função, usando o AWS SDK, procura por todas as instâncias EC2 com a tag Auto-Stop-Start = true e executa o comando para iniciá-las.

Agendamento de "Desligar": Um segundo Amazon EventBridge (Rule) é configurado com outra regra (expressão cron) para disparar todo dia útil às 18:00.

Função Lambda "Stop": Este evento aciona a função AWS Lambda (Stop-Instances). Esta função procura as mesmas instâncias (pela tag) e executa o comando para pará-las, economizando custos durante a noite.

Permissões: O IAM (Identity and Access Management) concede as permissões necessárias para que as funções Lambda possam descrever e alterar o estado (start/stop) das instâncias EC2.

Serviços Utilizados
Amazon EC2: As instâncias de computação que serão gerenciadas (o alvo da automação).

Amazon EventBridge (Scheduler): O "relógio" que dispara os eventos em horários agendados (ex: cron(0 8 ? * MON-FRI *) para 08:00, de segunda a sexta).

AWS Lambda: As funções serverless que contêm a lógica para encontrar as instâncias por tag e executar as ações de start ou stop.

IAM (Identity and Access Management): Fornece as "Roles" e "Policies" que dão permissão ao Lambda para interagir com o serviço EC2.
