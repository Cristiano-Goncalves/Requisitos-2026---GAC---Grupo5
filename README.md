# 1. Objetivo

Definir a proposta de valor e o escopo do Sistema de Gestão de Projetores (ProjecTrack) do Centro de Ciências Tecnológicas (CCT) da UNIFOR, detalhando as necessidades da Secretaria, dos professores e da gestão universitária.

# 2. Proposta de Valor

O sistema permitirá modernizar e digitalizar o controle de empréstimos de equipamentos audiovisuais, facilitando a retirada no balcão e a auditoria de devoluções. Espera-se eliminar as filas através de reconhecimento facial, garantir a rastreabilidade via repasse digital nos corredores e reduzir o extravio e prejuízos por avarias não responsabilizadas.

# 3. Descrição da Demanda

O sistema apoiará a Secretaria da UNIFOR na organização do inventário de projetores, identificação ágil de professores (biometria facial), registro de transferências de posse fora da secretaria (via QR Code e GPS) e auditoria de avarias. Todo o processo será digital, com autenticação segura, adequação à LGPD e histórico imutável de alterações de custódia.

# 4. Partes Interessadas

| Nome | Papel | Responsabilidades | Representante |
| :--- | :--- | :--- | :--- |
| UNIFOR | Cliente | Financiar e adotar o sistema, estabelecer as políticas institucionais de patrimônio e proteção de dados. | Reitoria / TI |
| CCT (Centro de Ciências e Tecnologia) | Patrocinador (Área Foco) | Fornecer o ambiente de validação do projeto, gerenciar o inventário local de projetores. | Direção do CCT |
| Secretaria da UNIFOR (CCT) | Especialista do Domínio | Fornecer as regras de negócio de como o processo funciona hoje, operar os empréstimos, devoluções e fiscalizar equipamentos. | Atendente Chefe |
| Professor | Usuário final | Solicitar o projetor, assinar o termo digitalmente, oficializar repasses no corredor via celular e zelar pelo equipamento. | - |
| Equipe de Desenvolvimento | Desenvolvimento | Levantar requisitos, desenhar a arquitetura, implementar e testar o sistema ProjecTrack. | Equipe de Alunos |

# 5. Personas

### 5.1 Professor Solicitante

* **Descrição:** Docente do CCT que necessita do projetor multimídia para lecionar suas aulas.

* **Objetivo:** Retirar o equipamento na secretaria de forma imediata (sem preencher planilhas físicas) e conseguir repassar o ativo para um colega no corredor de forma oficial, isentando-se da responsabilidade.

### 5.2 Atendente da Secretaria

* **Descrição:** Colaborador que opera o balcão de atendimento e cuida da reserva de laboratórios e equipamentos.

* **Objetivo:** Liberar a fila de professores rapidamente nos inícios de turno e garantir que não seja injustamente responsabilizado caso um projetor volte danificado da sala de aula.

# 6. Necessidades e Funcionalidades

### Necessidade 1: Identificação ágil e retirada de equipamentos

> **Nota:** O processo de retirada deve ser sem atrito, dispensando a assinatura em papel.

**F1.1 Reconhecimento facial no balcão (Check-out)**

* **Descrição:** Permite capturar a face do professor via webcam, cruzar com a base de dados e identificá-lo automaticamente.
* **Incluída**
* **Atores:** Secretaria da UNIFOR, Professor
* **Frequência:** Alta
* **Valor:** Alto

**F1.2 Geração e envio de Termo de Responsabilidade**

* **Descrição:** Permite registrar o empréstimo vinculando o projetor ao professor e dispara o comprovante por e-mail.
* **Incluída**
* **Atores:** Secretaria da UNIFOR, Professor
* **Frequência:** Alta
* **Valor:** Alto

### Necessidade 2: Rastreabilidade e repasse nos corredores

**F2.1 Transferência de posse P2P via QR Code (PWA)**

* **Descrição:** Permite que um professor leia a etiqueta do projetor com o celular e assuma a custódia do equipamento que estava com outro professor.
* **Incluída**
* **Atores:** Professor
* **Frequência:** Média
* **Valor:** Alto

**F2.2 Captura de geolocalização**

* **Descrição:** Permite capturar as coordenadas de GPS do navegador do smartphone no momento exato em que ocorre o repasse entre professores.
* **Incluída**
* **Atores:** Professor
* **Frequência:** Média
* **Valor:** Alto

### Necessidade 3: Auditoria de devolução e gestão de avarias

**F3.1 Checklist de devolução (Check-in)**

* **Descrição:** Permite ao atendente preencher um checklist de integridade obrigatório (verificar lente, controle, cabos) antes de finalizar o empréstimo.
* **Incluída**
* **Atores:** Secretaria da UNIFOR
* **Frequência:** Alta
* **Valor:** Alto

**F3.2 Registro de ocorrência e bloqueio**

* **Descrição:** Permite registrar avarias vinculadas ao último responsável e bloquear automaticamente novos empréstimos para o docente até a regularização.
* **Incluída**
* **Atores:** Secretaria da UNIFOR, CCT
* **Frequência:** Baixa
* **Valor:** Médio

### Necessidade 4: Segurança, monitoramento e conformidade

**F4.1 Mapa de rastreamento do patrimônio**

* **Descrição:** Permite à secretaria consultar em um mapa interativo o último local de repasse reportado pelo QR Code de cada projetor ativo a partir da saída da Secretaria do CCT.
* **Incluída**
* **Atores:** Secretaria da UNIFOR
* **Frequência:** Alta
* **Valor:** Alto

### Necessidade 5: Gestão de Cadastros e Prontuário de Ativos

#### F5.1 Cadastro Unificado de Docente e Captura Biométrica
* **Descrição:** Permite realizar o cadastro inicial e único do docente, coletando obrigatoriamente Nome Completo, Código de Acesso/Matrícula UNIFOR e CPF. No mesmo fluxo, realiza a captura biométrica facial inicial para extração e salvamento do vetor numérico (*embedding*).
* **Status:** Incluída
* **Atores:** Secretaria da UNIFOR, Professor
* **Frequência:** Baixa (Apenas no primeiro acesso)
* **Valor:** Alto

#### F5.2 Cadastro e Prontuário do Projetor com Histórico
* **Descrição:** Permite o registro de novos projetores no inventário da instituição, associando o identificador único ao ano de fabricação do aparelho e abrindo uma linha do tempo digital para registro de manutenções preventivas e consertos anteriores.
* **Status:** Incluída
* **Atores:** Secretaria da UNIFOR, CCT
* **Frequência:** Baixa
* **Valor:** Alto

#### F5.3 Leitura de QR Code Informativa e de Custódia (Visão do Professor)
* **Descrição:** Permite que o professor escaneie o QR Code do projetor via smartphone para registrar/confirmar que o equipamento está sob sua posse direta ou aceitar uma transferência, exibindo para ele apenas a tela de confirmação de posse.
* **Status:** Incluída
* **Atores:** Professor
* **Frequência:** Alta
* **Valor:** Alto

#### F5.4 Consulta ao Prontuário Técnico via QR Code (Visão da Secretaria)
* **Descrição:** Permite que a secretaria escaneie o QR Code de um projetor para acessar instantaneamente as informações restritas da instituição, como o histórico completo de consertos, avarias passadas e tempo de vida útil restante do equipamento.
* **Status:** Incluída
* **Atores:** Secretaria da UNIFOR, CCT
* **Frequência:** Média
* **Valor:** Médio
  
# 7. Arquitetura da Demanda

O sistema será composto por módulos de Identificação Biométrica (Inteligência Artificial), Gestão de Inventário, Transferência Web Responsiva (PWA) e Auditoria de Devolução. Utilizará banco de dados relacional adaptado para buscas de similaridade (PostgreSQL com pgvector) e será acessível via navegadores web modernos tanto no desktop (Secretaria) quanto nos smartphones dos docentes. 
