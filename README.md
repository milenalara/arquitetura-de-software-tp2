# 🏥 Documentação Oficial – Design System: Sistema de Consultas Médicas - Clínica Vida+Saúde

Trabalho desenvolvido para a disciplina Arquitetura de Software, ministrada pela Profª Angélica Guimarães no Bacharelado em Engenharia de Software da PUC Minas.

__Alunos:__
- Guilerme Cantoni
- Lívia Bontempo
- Milena Lara
- Pedro Gonçalves


__Cenário:__
```
Uma clínica médica chamada Vida+ Saúde deseja modernizar seu sistema de gestão de
consultas, que atualmente é manual. O objetivo é permitir que os pacientes agendem consultas
online, os médicos visualizem seus horários e o setor administrativo gerencie os atendimentos.

Para isso, será desenvolvido um Sistema de Gestão de Consultas Médicas, baseado na
arquitetura em 3 camadas (Apresentação, Negócio e Dados).

1️. Camada de Apresentação (Front-end - Interface do Usuário)
Esta camada é responsável pela interação com os usuários e pode ser implementada como um
sistema web responsivo.

Elementos da interface:
• Tela de Login: Pacientes e médicos devem se autenticar para acessar o sistema.
• Tela de Agendamento: Os pacientes escolhem a data e o médico disponível.
• Tela do Médico: Visualização dos agendamentos do dia.
• Tela Administrativa: O setor administrativo pode visualizar e gerenciar as consultas.

2️. Camada de Negócio (Back-end - Regras de Negócio)
Esta camada processa as regras de negócio do sistema, garantindo que os dados sejam
manipulados corretamente.

Regras de negócio principais:
• Validação de login e perfis de acesso (Paciente, Médico, Administrador).
• Restrição de agendamento para horários disponíveis.
• Cancelamento de consultas com aviso prévio ao médico.
• Relatórios sobre a quantidade de consultas realizadas por período.

3. Camada de Dados (Banco de Dados - Persistência de Informações)
Esta camada armazena os dados do sistema, garantindo integridade e segurança.

Principais tabelas:
• Usuários: Pacientes, médicos e administradores.
• Consultas: Informações do agendamento (data, hora, médico, paciente).
• Especialidades: Lista de especialidades médicas disponíveis na clínica.
```

## 📌 Visão Geral
Este documento descreve a arquitetura e os componentes principais do Sistema de Consultas Médicas.

O foco está em **escalabilidade**, **segurança** e **robustez** para garantir um serviço eficiente e confiável para usuários (pacientes, médicos e administradores).

## 📐 Arquitetura Geral

O sistema adota uma arquitetura de microsserviços orientada a serviços, seguindo boas práticas de escalabilidade.

### 🔁 Fluxo de Funcionamento
1. Usuário acessa o sistema via front-end (web/mobile).
2. Requisições são roteadas por um **load balancer** para os servidores de aplicação.
3. As **APIs** processam as requisições, interagindo com:
   - Banco de dados relacional;
   - Fila de mensagens para tarefas assíncronas;
   - Armazenamento de arquivos;
   - Serviços externos (e-mails, etc).
4. O sistema monitora todas as interações para garantir disponibilidade e detectar falhas rapidamente.

## 🧱 Componentes da Arquitetura

### 🎯 Front-end
- Interface Web/Mobile;
- Envia requisições HTTP (JSON/HTML) à API;
- Integrado com autenticação/autorização.

### ⚖ Load Balancer
- **Tecnologia:** NGINX;
- Balanceamento de tráfego entre instâncias;
- Suporte à escalabilidade horizontal e alta disponibilidade.

### 🧠 API (Back-end)
- **Tecnologia:** Spring Boot (Java);
- Endpoints RESTful;
- Regras de negócio e autenticação via JWT;
- Comunicação com banco, filas, e armazenamento.

### 🔒 Camada de Segurança
- **Framework:** Spring Security;
- Autenticação via `/login`;
- Tokens JWT;
- Autorização baseada em roles (paciente, médico, admin).

### 🗃 Banco de Dados
- **Tecnologia:** PostgreSQL;
- Armazena usuários, agendamentos, prontuários;
- Acesso via JPA/Hibernate.

### ✉ Message Broker
- **Tecnologia:** RabbitMQ;
- Processa tarefas assíncronas (e-mails, relatórios);
- Reduz carga dos servidores.

### ⚙ Queue Worker
- **Tecnologia:** Java ou Node.js;
- Consome mensagens da fila;
- Executa tarefas em segundo plano (notificações, exames);
- Integra com e-mail e armazenamento.

### 📤 Serviço de E-mail
- **Tecnologia:** SMTP via SendGrid;
- Envio de e-mails transacionais (confirmação, lembretes, etc).

### 📂 Armazenamento de Arquivos
- **Tecnologia:** Amazon S3 (ou compatível);
- Armazena documentos médicos, RX, exames;
- Proteção com links temporários e ACLs.

### 📈 Monitoramento e Observabilidade
- **Logs:** ELK Stack (Elasticsearch, Logstash, Kibana);
- **Métricas:** Prometheus + Grafana;
- **Alertas:** Alertmanager;
- Health checks via Spring Boot Actuator.

## 🔄 Escalabilidade
- Múltiplas instâncias da API com load balancer;
- Filas desacoplam tarefas pesadas;
- Banco replicável/particionável;
- Serviços stateless para scaling horizontal (containers/Kubernetes).

## 🔐 Segurança
- Autenticação com JWT;
- Senhas com hashing (BCrypt);
- Controle de acesso por roles;
- CORS configurado;
- Firewall e segurança em nível de rede.
