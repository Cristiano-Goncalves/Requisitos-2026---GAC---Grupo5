# Especificação de Casos de Uso — ProjecTrack

> **Projeto:** ProjecTrack — Sistema de Gestão de Projetores do CCT/UNIFOR
> **Versão:** 1.2.0
> **Autores:** Iandeyara Farias, Cristiano Gonçalves e Myrla Rodrigues
> **Última atualização:** 21/05/2026
> **Metodologia:** Baseada no processo LAPIS (ProfBezerra/LAPIS)

---

## Histórico de Versões

| Data | Versão | Descrição | Autor |
|---|---|---|---|
| 03/05/2026 | 1.0.0 | Criação inicial da especificação de casos de uso. | Iandeyara Farias, Cristiano Gonçalves e Myrla Rodrigues |
| 17/05/2026 | 1.1.0 | Inclusão de personas e diagrama de casos de uso. | Iandeyara Farias, Cristiano Gonçalves e Myrla Rodrigues |
| 21/05/2026 | 1.2.0 | Detalhamento completo dos casos de uso no padrão LAPIS. | Iandeyara Farias, Cristiano Gonçalves e Myrla Rodrigues |

---

## Sumário

- [Atores](#atores)
- [CDU-01 — Cadastrar Professor e Capturar Biometria](#cdu-01--cadastrar-professor-e-capturar-biometria)
- [CDU-02 — Cadastrar Projetor](#cdu-02--cadastrar-projetor)
- [CDU-03 — Realizar Empréstimo de Projetor (Check-out)](#cdu-03--realizar-empréstimo-de-projetor-check-out)
- [CDU-04 — Identificar Professor por Reconhecimento Facial](#cdu-04--identificar-professor-por-reconhecimento-facial)
- [CDU-05 — Realizar Devolução de Projetor (Check-in)](#cdu-05--realizar-devolução-de-projetor-check-in)
- [CDU-06 — Registrar Ocorrência de Avaria](#cdu-06--registrar-ocorrência-de-avaria)
- [CDU-07 — Transferir Posse de Projetor via QR Code](#cdu-07--transferir-posse-de-projetor-via-qr-code)
- [CDU-08 — Consultar Prontuário Técnico via QR Code](#cdu-08--consultar-prontuário-técnico-via-qr-code)
- [CDU-09 — Visualizar Mapa de Rastreamento do Patrimônio](#cdu-09--visualizar-mapa-de-rastreamento-do-patrimônio)
- [CDU-10 — Bloquear ou Desbloquear Professor](#cdu-10--bloquear-ou-desbloquear-professor)

---

## Atores

| Ator | Tipo | Descrição |
|---|---|---|
| **Secretaria CCT** | Primário | Colaborador do balcão de atendimento. Opera empréstimos, devoluções, cadastros e auditorias. |
| **Professor** | Primário | Docente do CCT. Retira e devolve projetores, recebe termos de responsabilidade e realiza repasses. |
| **Coordenação CCT** | Primário | Gestor responsável pelo patrimônio. Supervisiona ocorrências e autoriza bloqueios/desbloqueios. |
| **Sistema de Horários UNIFOR** | Secundário | Sistema externo que fornece a grade horária dos professores para sincronização de contexto de aula. |
| **Serviço de E-mail** | Secundário | Sistema externo responsável pelo envio de termos de responsabilidade e notificações aos professores. |

---

## CDU-01 — Cadastrar Professor e Capturar Biometria

### Objetivo

Permitir o cadastro inicial e único do professor no sistema, coletando os dados obrigatórios de identificação e realizando a captura da biometria facial para extração e salvamento do vetor numérico (*embedding*).

### Tipo

Concreto.

### Atores

- **Primário:** Secretaria CCT
- **Secundário:** Professor (presente fisicamente no balcão para captura facial)

### Pré-condições

- O professor não pode possuir cadastro ativo com o mesmo CPF ou mesma assinatura biométrica no sistema.
- A webcam do balcão de atendimento deve estar conectada e operacional.

### Fluxo Principal

**P1.** A Secretaria CCT seleciona a opção **Cadastros / Novo Professor** no menu lateral do sistema.

**P2.** O sistema exibe o formulário de cadastro com os campos: Nome Completo, Matrícula UNIFOR, CPF, E-mail Institucional e Departamento.

**P3.** A Secretaria CCT preenche todos os campos obrigatórios e seleciona **Próximo**. **[A1][E1][E2]**

**P4.** O sistema valida os dados preenchidos conforme a regra de negócio **RN3**. **[E1][E3]**

**P4.1** Realiza consulta na base de dados para verificar unicidade do CPF conforme **RN4**. **[A2]**

**P4.2** Exibe a tela de captura biométrica com o feed ao vivo da webcam, o guia oval de posicionamento e as instruções de captura.

**P5.** O Professor posiciona o rosto dentro do guia oval exibido na tela.

**P6.** O sistema detecta o rosto do Professor e altera o guia oval de cinza para **verde**, habilitando o botão **Capturar Biometria**.

**P7.** A Secretaria CCT seleciona **Capturar Biometria**.

**P8.** O sistema extrai o vetor numérico (*embedding*) do rosto capturado e realiza busca de similaridade na base de dados conforme **RN4**. **[A3][E4]**

**P9.** O sistema exibe a mensagem de sucesso: *"Biometria capturada com sucesso"* e habilita o botão **Concluir Cadastro**.

**P10.** A Secretaria CCT seleciona **Concluir Cadastro**.

**P11.** O sistema persiste os dados do professor e o embedding na base de dados.

**P12.** O sistema exibe a mensagem de confirmação: *"Professor cadastrado com sucesso"* e retorna ao Dashboard da Secretaria.

**P13.** O caso de uso é encerrado.

### Fluxos Alternativos

**A1. Cancelar cadastro**

**A1.1** No passo **P3**, a Secretaria CCT seleciona **Voltar**.

**A1.2** O sistema descarta os dados preenchidos sem persistência.

**A1.3** O caso de uso é encerrado.

---

**A2. CPF já cadastrado no sistema**

**A2.1** No passo **P4.1**, o sistema identifica CPF já vinculado a outro cadastro ativo conforme **RN4**.

**A2.2** O sistema exibe modal de alerta: *"CPF já vinculado ao professor [Nome]. Deseja visualizar o cadastro existente?"* com as opções **Ver Cadastro** e **Cancelar**.

**A2.3** Se a Secretaria CCT selecionar **Ver Cadastro**, o sistema navega para a tela de visualização do cadastro existente.

**A2.4** O caso de uso é encerrado.

---

**A3. Recapturar biometria**

**A3.1** No passo **P8**, a captura resulta em baixa qualidade de imagem (rosto parcialmente detectado, iluminação insuficiente).

**A3.2** O sistema exibe a mensagem: *"Qualidade insuficiente para extração do vetor. Tente novamente."*

**A3.3** O sistema retorna ao passo **P5**.

### Fluxos de Exceção

**E1. Campos obrigatórios não preenchidos**

**E1.1** No passo **P3** ou **P4**, o sistema identifica campo obrigatório ausente.

**E1.2** O sistema destaca o(s) campo(s) com borda vermelha e exibe a mensagem de validação inline: *"Campo obrigatório."*

**E1.3** O sistema retorna ao passo **P2**, mantendo os dados já preenchidos.

---

**E2. Formato de CPF inválido**

**E2.1** No passo **P4**, o sistema identifica que o CPF não passa na validação do algoritmo de dígitos verificadores.

**E2.2** O sistema exibe a mensagem inline: *"CPF inválido. Verifique o número informado."*

**E2.3** O sistema retorna ao passo **P2**, mantendo os demais dados.

---

**E3. E-mail fora do domínio institucional**

**E3.1** No passo **P4**, o sistema identifica que o e-mail informado não pertence ao domínio `@unifor.br`.

**E3.2** O sistema exibe a mensagem inline: *"Use um e-mail institucional @unifor.br."*

**E3.3** O sistema retorna ao passo **P2**, mantendo os demais dados.

---

**E4. Assinatura biométrica já cadastrada**

**E4.1** No passo **P8**, o sistema identifica similaridade acima do limiar com um vetor já armazenado na base conforme **RN4**.

**E4.2** O sistema exibe modal de bloqueio: *"Assinatura facial já vinculada ao professor [Nome]. Possível duplicidade. Verifique com a Coordenação."*

**E4.3** O sistema impede a conclusão do cadastro.

**E4.4** O caso de uso é encerrado.

### Pós-condições

- O professor está cadastrado na base de dados com status **ativo**.
- O vetor de embedding facial está armazenado e disponível para uso nos fluxos de reconhecimento.
- Nenhuma imagem facial (`.jpg`/`.png`) é persistida, garantindo conformidade com a **LGPD**.

### Requisitos Não Funcionais Específicos

- O tempo de extração e comparação do embedding não deve ultrapassar **2,5 segundos** (RNF — Performance de Reconhecimento).
- O sistema deve armazenar apenas o vetor matemático, nunca a imagem facial (RNF — Conformidade LGPD).

### Frequência de Utilização

Baixa. Realizado apenas uma vez por professor, no primeiro acesso ao sistema.

---

## CDU-02 — Cadastrar Projetor

### Objetivo

Permitir o registro de um novo projetor no inventário da instituição, associando um identificador único ao equipamento e abrindo seu prontuário técnico com histórico de manutenções.

### Tipo

Concreto.

### Atores

- **Primário:** Secretaria CCT
- **Secundário:** Coordenação CCT (pode solicitar o cadastro)

### Pré-condições

- O código de patrimônio do projetor não pode estar previamente cadastrado no sistema.

### Fluxo Principal

**P1.** A Secretaria CCT seleciona a opção **Cadastros / Novo Projetor** no menu lateral do sistema.

**P2.** O sistema exibe o formulário de cadastro com os campos: Código de Patrimônio, Marca/Modelo, Número de Série, Ano de Fabricação, Estado Inicial e Observações.

**P3.** A Secretaria CCT preenche os campos obrigatórios e seleciona **Cadastrar e Gerar QR Code**. **[A1][E1][E2]**

**P4.** O sistema valida os dados conforme **RN6**. **[E1][E2]**

**P4.1** Verifica unicidade do Código de Patrimônio.

**P4.2** Persiste o registro do projetor com status **disponível**.

**P4.3** Abre a linha do tempo de manutenções do equipamento conforme **RN6**.

**P4.4** Gera o QR Code único vinculado ao Código de Patrimônio.

**P5.** O sistema exibe modal de sucesso com o QR Code gerado e as opções **Imprimir Etiqueta** e **Fechar**.

**P6.** A Secretaria CCT seleciona **Imprimir Etiqueta** ou **Fechar**.

**P7.** O caso de uso é encerrado.

### Fluxos Alternativos

**A1. Cancelar cadastro**

**A1.1** No passo **P3**, a Secretaria CCT seleciona **Voltar**.

**A1.2** O sistema descarta os dados sem persistência.

**A1.3** O caso de uso é encerrado.

### Fluxos de Exceção

**E1. Campos obrigatórios não preenchidos**

**E1.1** No passo **P4**, o sistema identifica campo obrigatório ausente.

**E1.2** O sistema destaca os campos com borda vermelha e exibe mensagem de validação inline.

**E1.3** O sistema retorna ao passo **P2**, mantendo os dados já preenchidos.

---

**E2. Código de patrimônio já cadastrado**

**E2.1** No passo **P4.1**, o sistema identifica que o código já existe na base.

**E2.2** O sistema exibe a mensagem: *"Código de patrimônio já cadastrado. Verifique o número informado."*

**E2.3** O sistema retorna ao passo **P2**, mantendo os demais dados.

### Pós-condições

- O projetor está registrado no inventário com status **disponível**.
- O prontuário técnico do equipamento foi criado com linha do tempo de manutenções vazia.
- O QR Code único foi gerado e está disponível para impressão.

### Requisitos Não Funcionais Específicos

- O QR Code gerado deve ser único e imutável, vinculado permanentemente ao Código de Patrimônio.

### Frequência de Utilização

Baixa. Realizado somente na incorporação de novos equipamentos ao inventário.

---

## CDU-03 — Realizar Empréstimo de Projetor (Check-out)

### Objetivo

Permitir o empréstimo de um projetor disponível a um professor identificado, gerando o Termo de Responsabilidade digital e associando o contexto de aula ao equipamento.

### Tipo

Concreto.

### Atores

- **Primário:** Secretaria CCT
- **Secundário:** Professor (presente no balcão), Sistema de Horários UNIFOR, Serviço de E-mail

### Pré-condições

- Deve existir ao menos um projetor com status **disponível** no inventário.
- O professor solicitante deve estar cadastrado e sem pendências de avaria conforme **RN2**.

### Fluxo Principal

**P1.** A Secretaria CCT seleciona a opção **Check-out** no menu lateral do sistema.

**P2.** O sistema exibe a lista de projetores com status **disponível**, com campo de busca por código ou modelo.

**P3.** A Secretaria CCT seleciona um projetor da lista e clica em **Selecionar Projetor**. **[E1]**

**P4.** O sistema navega para a etapa de identificação do professor e exibe o feed ao vivo da webcam com guia oval de posicionamento.

**P5.** O Professor posiciona o rosto diante da câmera.

**P6.** O sistema realiza o reconhecimento facial, incluindo o caso de uso **CDU-04 — Identificar Professor por Reconhecimento Facial**. **[A1][E2][E3]**

**P7.** O sistema exibe o card de confirmação do professor identificado com: Nome, Matrícula, Departamento.

**P7.1** Consulta o **Sistema de Horários UNIFOR** para obter a grade horária atual do professor conforme **RN5**.

**P7.2** Exibe a sala de destino sugerida, a disciplina em curso e o horário previsto de término.

**P8.** A Secretaria CCT confirma a identificação selecionando **Confirmar e Prosseguir**. **[A2]**

**P9.** O sistema exibe o resumo do empréstimo: Professor, Projetor, Sala de Destino, Data/Hora de Retirada e Previsão de Devolução.

**P10.** A Secretaria CCT seleciona **Confirmar e Gerar Termo**. **[A3]**

**P11.** O sistema persiste o registro do empréstimo.

**P11.1** Atualiza o status do projetor para **em_uso**.

**P11.2** Aciona o **Serviço de E-mail** para envio do Termo de Responsabilidade ao e-mail institucional do professor.

**P12.** O sistema exibe a mensagem de sucesso: *"Empréstimo registrado. Termo enviado para [e-mail]."* e retorna ao Dashboard.

**P13.** O caso de uso é encerrado.

### Fluxos Alternativos

**A1. Identificação manual do professor**

**A1.1** No passo **P6**, a Secretaria CCT opta por usar o campo de busca manual (por nome ou matrícula) em vez do reconhecimento facial.

**A1.2** A Secretaria CCT digita o nome ou matrícula e seleciona o professor na lista de resultados.

**A1.3** O sistema segue para o passo **P7**.

---

**A2. Professor identificado incorretamente**

**A2.1** No passo **P8**, a Secretaria CCT seleciona **Não, buscar outro**.

**A2.2** O sistema retorna ao passo **P4**, reiniciando o feed da webcam.

---

**A3. Cancelar empréstimo**

**A3.1** Nos passos **P9** ou **P10**, a Secretaria CCT seleciona **Cancelar**.

**A3.2** O sistema descarta o empréstimo sem persistência.

**A3.3** O status do projetor permanece **disponível**.

**A3.4** O caso de uso é encerrado.

### Fluxos de Exceção

**E1. Nenhum projetor disponível**

**E1.1** No passo **P2**, o sistema não encontra projetores com status **disponível**.

**E1.2** O sistema exibe a mensagem: *"Não há projetores disponíveis no momento."*

**E1.3** O caso de uso é encerrado.

---

**E2. Professor com ocorrência de avaria em aberto**

**E2.1** No passo **P6**, após a identificação, o sistema verifica que o professor possui status **bloqueado** conforme **RN2**.

**E2.2** O sistema exibe modal de bloqueio: *"Prof. [Nome] possui ocorrência de avaria em aberto. Empréstimo bloqueado. Contate a Coordenação CCT."*

**E2.3** O sistema impede o avanço no fluxo.

**E2.4** O caso de uso é encerrado.

---

**E3. Falha na consulta ao Sistema de Horários UNIFOR**

**E3.1** No passo **P7.1**, o sistema não obtém resposta do **Sistema de Horários UNIFOR**.

**E3.2** O sistema exibe aviso: *"Não foi possível recuperar o horário do professor. A sala de destino deverá ser informada manualmente."*

**E3.3** O sistema habilita campo de texto para inserção manual da sala de destino.

**E3.4** O sistema segue para o passo **P9**.

### Pós-condições

- O empréstimo está registrado com status **ativo**.
- O projetor está com status **em_uso**, vinculado ao professor.
- O Termo de Responsabilidade foi enviado ao e-mail institucional do professor.
- A custódia legal do equipamento recai sobre o professor conforme **RN1**.

### Requisitos Não Funcionais Específicos

- O tempo total do fluxo de reconhecimento facial não deve ultrapassar **2,5 segundos** (RNF — Performance de Reconhecimento).

### Frequência de Utilização

Alta. Realizado múltiplas vezes ao dia, com pico nos inícios de turno (manhã, tarde e noite).

---

## CDU-04 — Identificar Professor por Reconhecimento Facial

### Objetivo

Permitir a identificação automática de um professor por meio da comparação do rosto capturado pela webcam com os vetores biométricos armazenados na base de dados.

### Tipo

Abstrato. Instanciado pelo **CDU-03** (Check-out).

### Atores

Não se aplica (caso de uso abstrato).

### Pré-condições

- O caso de uso deve ter sido instanciado por outro caso de uso.
- A webcam deve estar operacional e com boa iluminação frontal.
- Devem existir embeddings cadastrados na base para comparação.

### Fluxo Principal

**P1.** O sistema ativa o feed ao vivo da webcam e exibe o guia oval de posicionamento na cor **cinza**.

**P2.** O sistema inicia o processo de detecção facial em tempo real.

**P3.** O sistema detecta o rosto dentro do guia oval e altera a cor do guia para **verde**.

**P4.** O sistema captura o frame com melhor qualidade e extrai o vetor numérico (*embedding*) do rosto.

**P5.** O sistema envia o embedding para o serviço de reconhecimento e realiza busca de similaridade (distância de cosseno) na base de dados. **[A1][A2][E1]**

**P6.** O sistema identifica o professor com grau de confiança acima do limiar configurado.

**P7.** O sistema retorna ao caso de uso chamador com os dados do professor identificado (Nome, Matrícula, Departamento).

**P8.** O caso de uso é encerrado.

### Fluxos Alternativos

**A1. Baixa confiança no reconhecimento**

**A1.1** No passo **P5**, o sistema encontra correspondência, porém com grau de confiança abaixo do limiar máximo, mas acima do limiar mínimo.

**A1.2** O sistema exibe o card do candidato mais provável com a pergunta: *"É este professor?"* e as opções **Sim** e **Não, buscar outro**.

**A1.3** Se a Secretaria CCT confirmar, o sistema segue para o passo **P7**.

**A1.4** Se a Secretaria CCT negar, o sistema retorna ao passo **P2**.

---

**A2. Múltiplos rostos detectados**

**A2.1** No passo **P2** ou **P3**, o sistema detecta mais de um rosto no campo da câmera.

**A2.2** O sistema altera o guia oval para **vermelho** e exibe o aviso: *"Múltiplos rostos detectados. Certifique-se de que apenas o professor está em frente à câmera."*

**A2.3** O sistema retorna ao passo **P2**.

### Fluxos de Exceção

**E1. Rosto não reconhecido na base**

**E1.1** No passo **P5**, o sistema não encontra nenhuma correspondência acima do limiar mínimo.

**E1.2** O sistema exibe a mensagem: *"Rosto não reconhecido. Use a identificação manual."*

**E1.3** O sistema retorna ao caso de uso chamador sinalizando falha no reconhecimento automático.

### Pós-condições

- Os dados do professor identificado estão disponíveis para o caso de uso chamador.

### Frequência de Utilização

Alta. Executado a cada Check-out de projetor.

---

## CDU-05 — Realizar Devolução de Projetor (Check-in)

### Objetivo

Permitir o registro da devolução de um projetor, com a execução do checklist obrigatório de integridade física do equipamento antes da finalização do empréstimo.

### Tipo

Concreto.

### Atores

- **Primário:** Secretaria CCT

### Pré-condições

- Deve existir ao menos um empréstimo com status **ativo** ou **em_repasse** para o projetor que está sendo devolvido.

### Fluxo Principal

**P1.** A Secretaria CCT seleciona a opção **Check-in** no menu lateral do sistema.

**P2.** O sistema exibe a lista de empréstimos com status **ativo** ou **em_repasse**, com campo de busca por código do projetor ou nome do professor.

**P3.** A Secretaria CCT localiza e seleciona o empréstimo correspondente e clica em **Iniciar Checklist de Devolução**. **[E1]**

**P4.** O sistema exibe o checklist de integridade com os itens: Lente, Controle Remoto, Cabo HDMI, Cabo de Energia, Corpo/Tampa e Lâmpada/LED. Cada item possui as opções **OK** e **Avaria**, com campo de observação livre.

**P5.** A Secretaria CCT avalia cada item fisicamente e marca o status correspondente no checklist. **[E2]**

**P6.** Após marcar todos os itens, a Secretaria CCT seleciona **Finalizar — Sem Avarias**. **[A1]**

**P7.** O sistema persiste o resultado do checklist.

**P7.1** Atualiza o status do empréstimo para **encerrado**.

**P7.2** Atualiza o status do projetor para **disponível**.

**P8.** O sistema exibe a mensagem de sucesso: *"Devolução registrada com sucesso."* e retorna ao Dashboard.

**P9.** O caso de uso é encerrado.

### Fluxos Alternativos

**A1. Avaria identificada no checklist**

**A1.1** No passo **P5**, a Secretaria CCT marca ao menos um item como **Avaria**.

**A1.2** O sistema desabilita o botão **Finalizar — Sem Avarias** e habilita apenas o botão **Registrar Ocorrência de Avaria**.

**A1.3** A Secretaria CCT seleciona **Registrar Ocorrência de Avaria**.

**A1.4** O sistema inclui o caso de uso **CDU-06 — Registrar Ocorrência de Avaria**.

**A1.5** O caso de uso é encerrado.

### Fluxos de Exceção

**E1. Nenhum empréstimo ativo encontrado**

**E1.1** No passo **P2**, o sistema não localiza empréstimos ativos para o critério de busca informado.

**E1.2** O sistema exibe a mensagem: *"Nenhum empréstimo ativo encontrado para este critério."*

**E1.3** O sistema retorna ao passo **P2**.

---

**E2. Checklist incompleto**

**E2.1** No passo **P6**, a Secretaria CCT tenta finalizar sem ter marcado todos os itens do checklist.

**E2.2** O sistema destaca os itens não preenchidos e exibe a mensagem: *"Todos os itens devem ser avaliados antes de finalizar."*

**E2.3** O sistema retorna ao passo **P4**.

### Pós-condições

- O empréstimo está com status **encerrado**.
- O projetor está com status **disponível** (se sem avarias) ou **em_manutencao** (se com avaria registrada).
- O resultado do checklist está persistido no prontuário técnico do projetor conforme **RN6**.

### Frequência de Utilização

Alta. Realizado a cada devolução de projetor no balcão.

---

## CDU-06 — Registrar Ocorrência de Avaria

### Objetivo

Permitir o registro formal de uma avaria em um projetor, vinculando a ocorrência ao último responsável pela custódia e bloqueando o professor para novos empréstimos até regularização.

### Tipo

Abstrato. Instanciado pelo **CDU-05** (Check-in com avaria identificada).

### Atores

Não se aplica (caso de uso abstrato). Acionado pela Secretaria CCT via **CDU-05**.

### Pré-condições

- O caso de uso deve ter sido instanciado por outro caso de uso.
- Ao menos um item do checklist de devolução deve ter sido marcado como **Avaria**.

### Fluxo Principal

**P1.** O sistema exibe a tela de registro de avaria com: o nome do último responsável pela custódia, os itens marcados como avaria no checklist e campo para descrição detalhada da ocorrência.

**P2.** A Secretaria CCT preenche a descrição detalhada da ocorrência e seleciona o nível de gravidade: **Leve**, **Moderada** ou **Grave**. **[E1]**

**P3.** A Secretaria CCT, opcionalmente, captura uma fotografia da avaria via webcam. **[A1]**

**P4.** A Secretaria CCT seleciona **Registrar Avaria e Bloquear Professor**.

**P5.** O sistema exibe modal de confirmação: *"Prof. [Nome] será BLOQUEADO para novos empréstimos até a regularização da ocorrência. Deseja prosseguir?"* **[A2]**

**P6.** A Secretaria CCT confirma a ação.

**P7.** O sistema persiste a ocorrência de avaria vinculada ao último responsável conforme **RN1**.

**P7.1** Atualiza o status do professor para **bloqueado** conforme **RN2**.

**P7.2** Atualiza o status do projetor para **em_manutencao**.

**P7.3** Registra a ocorrência no prontuário técnico do projetor conforme **RN6**.

**P7.4** Envia notificação por e-mail ao professor e à Coordenação CCT informando a ocorrência.

**P8.** O sistema retorna ao caso de uso chamador com status de ocorrência registrada.

**P9.** O caso de uso é encerrado.

### Fluxos Alternativos

**A1. Não capturar fotografia**

**A1.1** No passo **P3**, a Secretaria CCT pula a etapa de captura fotográfica.

**A1.2** O sistema segue para o passo **P4** sem anexo fotográfico.

---

**A2. Cancelar registro de avaria**

**A2.1** No passo **P5**, a Secretaria CCT seleciona **Cancelar** no modal de confirmação.

**A2.2** O sistema retorna ao passo **P1** sem persistência.

### Fluxos de Exceção

**E1. Descrição da ocorrência não preenchida**

**E1.1** No passo **P4**, o sistema identifica que o campo de descrição está vazio.

**E1.2** O sistema exibe a mensagem: *"A descrição da ocorrência é obrigatória."*

**E1.3** O sistema retorna ao passo **P2**.

### Pós-condições

- A ocorrência de avaria está registrada na base de dados e no prontuário do projetor.
- O professor está com status **bloqueado**.
- O projetor está com status **em_manutencao**.
- O professor e a Coordenação CCT foram notificados por e-mail.

### Frequência de Utilização

Baixa. Ocorre apenas quando há identificação de avaria na devolução.

---

## CDU-07 — Transferir Posse de Projetor via QR Code

### Objetivo

Permitir que um professor transfira a custódia oficial de um projetor para outro professor por meio da leitura do QR Code fixado no equipamento, registrando a geolocalização no momento do repasse.

### Tipo

Concreto.

### Atores

- **Primário:** Professor (que assume a custódia — receptor)
- **Secundário:** Professor (que cede a custódia — cedente)

### Pré-condições

- O projetor deve estar com status **em_uso** ou **em_repasse**, sob custódia de um professor ativo.
- O Professor receptor deve estar cadastrado no sistema e com status **ativo** (sem bloqueio por avaria).
- O Professor receptor deve possuir o aplicativo PWA carregado no smartphone com câmera disponível.

### Fluxo Principal

**P1.** O Professor receptor acessa o aplicativo PWA do ProjecTrack no smartphone e seleciona a opção **Escanear QR Code**.

**P2.** O sistema exibe o leitor de QR Code via câmera do smartphone.

**P3.** O Professor receptor aponta a câmera para o QR Code fixado no projetor. **[E1]**

**P4.** O sistema decodifica o QR Code e identifica o projetor correspondente.

**P5.** O sistema verifica que o projetor está com custódia ativa de outro professor conforme **RN1**. **[A1][E2]**

**P6.** O sistema exibe a tela de autenticação facial no smartphone para identificar o Professor receptor.

**P7.** O Professor receptor posiciona o rosto diante da câmera frontal do smartphone.

**P8.** O sistema realiza o reconhecimento facial, incluindo o caso de uso **CDU-04 — Identificar Professor por Reconhecimento Facial**. **[E3][E4]**

**P9.** O sistema exibe a tela de confirmação da transferência com: Projetor, Professor cedente (atual responsável) e Professor receptor (novo responsável).

**P10.** O sistema captura as coordenadas de geolocalização do dispositivo do Professor receptor.

**P11.** O Professor receptor seleciona **Confirmar Transferência**. **[A2]**

**P12.** O sistema persiste a transferência de custódia.

**P12.1** Atualiza o responsável pelo projetor para o Professor receptor.

**P12.2** Atualiza o status do projetor para **em_repasse**.

**P12.3** Registra as coordenadas de GPS, o horário e os identificadores de ambos os professores no histórico de custódia conforme **RN1**.

**P13.** O sistema exibe a mensagem de sucesso: *"Transferência registrada. Você é agora o responsável pelo projetor [Código]."*

**P14.** O caso de uso é encerrado.

### Fluxos Alternativos

**A1. Projetor já disponível (sem custódia ativa)**

**A1.1** No passo **P5**, o sistema identifica que o projetor está com status **disponível** (sem professor responsável).

**A1.2** O sistema exibe a mensagem: *"Este projetor não possui empréstimo ativo. Procure a Secretaria para formalizar um novo empréstimo."*

**A1.3** O caso de uso é encerrado.

---

**A2. Cancelar transferência**

**A2.1** No passo **P11**, o Professor receptor seleciona **Cancelar**.

**A2.2** O sistema descarta a operação sem persistência.

**A2.3** A custódia permanece com o Professor cedente conforme **RN1**.

**A2.4** O caso de uso é encerrado.

### Fluxos de Exceção

**E1. QR Code inválido ou não pertencente ao sistema**

**E1.1** No passo **P4**, o sistema não encontra o projetor correspondente ao QR Code lido.

**E1.2** O sistema exibe a mensagem: *"QR Code não reconhecido. Verifique se a etiqueta pertence a um projetor cadastrado no ProjecTrack."*

**E1.3** O sistema retorna ao passo **P2**.

---

**E2. Professor receptor com bloqueio ativo**

**E2.1** No passo **P5** ou **P8**, o sistema identifica que o Professor receptor possui status **bloqueado** conforme **RN2**.

**E2.2** O sistema exibe a mensagem: *"Você possui uma ocorrência de avaria em aberto. Novas custódias estão bloqueadas. Regularize com a Coordenação CCT."*

**E2.3** O caso de uso é encerrado.

---

**E3. Geolocalização negada pelo dispositivo**

**E3.1** No passo **P10**, o dispositivo do Professor receptor não concede permissão de geolocalização.

**E3.2** O sistema exibe o aviso: *"Permissão de localização necessária para registrar o repasse. Habilite o GPS e tente novamente."*

**E3.3** O sistema retorna ao passo **P9**.

---

**E4. Rosto não reconhecido**

**E4.1** No passo **P8**, o reconhecimento facial retorna falha.

**E4.2** O sistema exibe a mensagem: *"Não foi possível identificar você. Tente novamente em local com melhor iluminação."*

**E4.3** O sistema retorna ao passo **P7**.

### Pós-condições

- A custódia do projetor foi transferida oficialmente para o Professor receptor.
- O Professor cedente está isento da responsabilidade pelo equipamento a partir do momento do repasse conforme **RN1**.
- A geolocalização e o horário do repasse estão registrados no histórico imutável de custódia.

### Frequência de Utilização

Média. Ocorre conforme a necessidade de repasse entre professores nos corredores.

---

## CDU-08 — Consultar Prontuário Técnico via QR Code

### Objetivo

Permitir que a Secretaria ou a Coordenação CCT acesse o prontuário técnico completo de um projetor por meio da leitura do QR Code fixado no equipamento, visualizando histórico de manutenções, avarias e tempo de vida útil.

### Tipo

Concreto.

### Atores

- **Primário:** Secretaria CCT
- **Primário:** Coordenação CCT

### Pré-condições

- O usuário deve estar autenticado com `role: secretaria` ou `role: coordenacao`.
- O projetor deve estar cadastrado no sistema.

### Fluxo Principal

**P1.** O ator seleciona a opção **Escanear Projetor** no sistema ou acessa diretamente via leitura do QR Code.

**P2.** O sistema exibe o leitor de QR Code.

**P3.** O ator lê o QR Code fixado no projetor. **[E1]**

**P4.** O sistema valida o perfil de acesso do usuário autenticado conforme **RN7**. **[A1]**

**P5.** O sistema exibe o prontuário técnico completo do projetor com: Código de Patrimônio, Marca/Modelo, Ano de Fabricação, Vida Útil Estimada, Status Atual e Linha do Tempo de Manutenções. **[A2]**

**P6.** O ator navega pelo histórico de manutenções e avarias.

**P7.** O ator, opcionalmente, seleciona **Registrar Nova Manutenção** para adicionar um registro ao prontuário. **[PE1]**

**P8.** O caso de uso é encerrado.

### Fluxos Alternativos

**A1. Usuário autenticado como Professor**

**A1.1** No passo **P4**, o sistema identifica que o usuário possui `role: professor`.

**A1.2** O sistema redireciona para o caso de uso **CDU-07 — Transferir Posse de Projetor via QR Code**, exibindo apenas as ações de custódia conforme **RN7**.

**A1.3** O caso de uso é encerrado.

---

**A2. Registrar nova manutenção**

**A2.1** No passo **P7**, o ator seleciona **Registrar Nova Manutenção**.

**A2.2** O sistema exibe formulário com os campos: Data, Tipo de Manutenção, Descrição e Técnico Responsável.

**A2.3** O ator preenche os dados e confirma.

**A2.4** O sistema persiste o registro de forma cumulativa no prontuário conforme **RN6** e exibe mensagem de sucesso.

**A2.5** O sistema retorna ao passo **P5**.

### Fluxos de Exceção

**E1. QR Code inválido**

**E1.1** No passo **P3**, o sistema não localiza o projetor correspondente ao QR Code lido.

**E1.2** O sistema exibe a mensagem: *"Equipamento não encontrado. Verifique se o QR Code pertence ao ProjecTrack."*

**E1.3** O sistema retorna ao passo **P2**.

### Pós-condições

- O ator visualizou o prontuário técnico do projetor.
- Se nova manutenção foi registrada, o registro está persistido de forma cumulativa no prontuário.

### Pontos de Extensão

#### PE1. Registrar Nova Manutenção

No passo **P7**, o ator seleciona a opção **Registrar Nova Manutenção** e o sistema estende o caso de uso para o formulário de registro de manutenção no prontuário técnico.

### Frequência de Utilização

Média. Consultado durante devoluções, auditorias periódicas e ao receber projetores para manutenção.

---

## CDU-09 — Visualizar Mapa de Rastreamento do Patrimônio

### Objetivo

Permitir à Secretaria CCT consultar, em um mapa interativo, o último local de repasse reportado por GPS de cada projetor ativo, a partir da saída da Secretaria do CCT.

### Tipo

Concreto.

### Atores

- **Primário:** Secretaria CCT

### Pré-condições

- Deve existir ao menos um projetor com status **em_uso** ou **em_repasse** no sistema.

### Fluxo Principal

**P1.** A Secretaria CCT seleciona a opção **Mapa de Rastreamento** no menu lateral do sistema.

**P2.** O sistema exibe o mapa interativo com marcadores para cada projetor fora da secretaria, posicionados nas últimas coordenadas registradas.

**P3.** A Secretaria CCT aplica filtros por status: **Todos**, **Em Uso** ou **Em Repasse**. **[A1]**

**P4.** O sistema atualiza a exibição dos marcadores conforme o filtro selecionado.

**P5.** A Secretaria CCT clica no marcador de um projetor. **[A2]**

**P6.** O sistema exibe o painel lateral com: Código do Projetor, Professor Responsável Atual, Última Localização, Sala de Aula Associada (via grade horária) e Horário do Último Repasse.

**P7.** O caso de uso é encerrado.

### Fluxos Alternativos

**A1. Nenhum resultado para o filtro selecionado**

**A1.1** No passo **P4**, o sistema não encontra projetores para o status filtrado.

**A1.2** O sistema exibe a mensagem: *"Nenhum projetor encontrado para o filtro selecionado."*

**A1.3** O sistema mantém o mapa visível sem marcadores.

---

**A2. Acessar histórico completo de um projetor**

**A2.1** No passo **P6**, a Secretaria CCT seleciona **Ver Histórico Completo**.

**A2.2** O sistema navega para o prontuário técnico do projetor correspondente (CDU-08).

### Fluxos de Exceção

Não se aplica.

### Pós-condições

Não há persistência. A consulta é somente leitura.

### Frequência de Utilização

Alta. Consultado diariamente pela Secretaria, especialmente durante os turnos de aula.

---

## CDU-10 — Bloquear ou Desbloquear Professor

### Objetivo

Permitir à Coordenação CCT bloquear ou desbloquear um professor para empréstimos, com base na análise de ocorrências de avaria em aberto ou na regularização com a instituição.

### Tipo

Concreto.

### Atores

- **Primário:** Coordenação CCT

### Pré-condições

- Para **bloquear**: o professor deve estar com status **ativo** e possuir ocorrência de avaria registrada.
- Para **desbloquear**: o professor deve estar com status **bloqueado**.

### Fluxo Principal

**P1.** A Coordenação CCT seleciona a opção **Gestão de Professores** no Dashboard da Coordenação.

**P2.** O sistema exibe a lista de professores com filtro por status (**Todos**, **Ativos**, **Bloqueados**) e campo de busca por nome ou matrícula.

**P3.** A Coordenação CCT localiza o professor desejado e seleciona **Ver Detalhes**. **[E1]**

**P4.** O sistema exibe o perfil do professor com: dados cadastrais, histórico de empréstimos e lista de ocorrências de avaria vinculadas.

**P5.** A Coordenação CCT seleciona **Bloquear Professor** ou **Desbloquear Professor**, conforme o status atual. **[A1]**

**P6.** O sistema exibe modal de confirmação com o resumo da ação e campo obrigatório de justificativa.

**P7.** A Coordenação CCT preenche a justificativa e seleciona **Confirmar**. **[A2][E2]**

**P8.** O sistema atualiza o status do professor conforme a ação selecionada conforme **RN2**.

**P8.1** Persiste a justificativa e o registro da ação no histórico do professor.

**P8.2** Envia notificação por e-mail ao professor informando o bloqueio ou desbloqueio e a justificativa.

**P9.** O sistema exibe mensagem de sucesso e retorna à lista de professores.

**P10.** O caso de uso é encerrado.

### Fluxos Alternativos

**A1. Professor já está no status desejado**

**A1.1** No passo **P5**, a ação selecionada corresponde ao status atual do professor (ex.: tentar desbloquear um professor já ativo).

**A1.2** O sistema exibe a mensagem: *"Este professor já está [ativo/bloqueado]."*

**A1.3** O sistema retorna ao passo **P4**.

---

**A2. Cancelar ação**

**A2.1** No passo **P7**, a Coordenação CCT seleciona **Cancelar** no modal de confirmação.

**A2.2** O sistema descarta a operação sem persistência.

**A2.3** O sistema retorna ao passo **P4**.

### Fluxos de Exceção

**E1. Professor não encontrado**

**E1.1** No passo **P3**, o sistema não localiza resultado para o critério de busca informado.

**E1.2** O sistema exibe a mensagem: *"Nenhum professor encontrado para este critério."*

**E1.3** O sistema retorna ao passo **P2**.

---

**E2. Justificativa não preenchida**

**E2.1** No passo **P7**, a Coordenação CCT tenta confirmar sem preencher a justificativa.

**E2.2** O sistema exibe a mensagem: *"A justificativa é obrigatória para registrar esta ação."*

**E2.3** O sistema retorna ao passo **P6**.

### Pós-condições

- O professor está com o status atualizado (**ativo** ou **bloqueado**).
- A justificativa e o registro da ação estão persistidos no histórico do professor.
- O professor foi notificado por e-mail.

### Frequência de Utilização

Baixa. Executado pontualmente após ocorrências de avaria ou após regularização com a instituição.

---

## Rastreabilidade Casos de Uso × Funcionalidades × Regras de Negócio

| Caso de Uso | Funcionalidades | Regras de Negócio |
|---|---|---|
| CDU-01 | F5.1 | RN3, RN4 |
| CDU-02 | F5.2 | RN6 |
| CDU-03 | F1.1, F1.2 | RN2, RN5 |
| CDU-04 | F1.1 | — |
| CDU-05 | F3.1, F3.2 | RN6 |
| CDU-06 | F3.2 | RN1, RN2, RN6 |
| CDU-07 | F2.1, F2.2, F5.3 | RN1, RN2 |
| CDU-08 | F5.4 | RN6, RN7 |
| CDU-09 | F4.1 | — |
| CDU-10 | F3.2 | RN2 |

---

## Checklist de Validação dos Artefatos (CDU)

Use este checklist ao revisar cada caso de uso.

### Estrutura mínima

- [ ] O caso de uso possui nome iniciado por verbo no infinitivo.
- [ ] O objetivo está claro, direto e representa um único objetivo principal.
- [ ] O tipo (concreto ou abstrato) está informado.
- [ ] Os atores foram identificados e classificados corretamente (primário/secundário).
- [ ] As pré-condições descrevem condições reais de início do fluxo.
- [ ] O fluxo principal está completo e atende ao objetivo do caso de uso.
- [ ] Existem fluxos alternativos para variações esperadas.
- [ ] Existem fluxos de exceção para falhas e erros relevantes.
- [ ] As pós-condições descrevem o estado esperado ao final do caso de uso.
- [ ] Os requisitos não funcionais específicos estão listados quando aplicável.
- [ ] Os pontos de extensão estão descritos quando aplicável.
- [ ] A frequência de utilização foi estimada.

### Qualidade da escrita

- [ ] Os passos estão escritos com verbos no presente do indicativo (3ª pessoa).
- [ ] Há alternância adequada entre ações do ator e da solução.
- [ ] Os atores são citados por nome.
- [ ] Não há ambiguidade nos passos.
- [ ] A linguagem está simples, objetiva e compreensível.

### Consistência e rastreabilidade

- [ ] Os pontos de entrada e saída dos fluxos alternativos estão explícitos.
- [ ] Os fluxos de exceção estão associados a passos da solução.
- [ ] As regras de negócio citadas estão identificadas de forma rastreável (RNx).
- [ ] As referências entre passos (retorna ao Px, segue para Px) estão corretas.
- [ ] A tabela de rastreabilidade CDU × F × RN está atualizada.

### Revisão final

- [ ] Não há conflitos entre fluxo principal, alternativos e exceções.
- [ ] O artefato está coerente com a Visão da Demanda e as Regras de Negócio.
- [ ] O documento foi revisado por outro integrante do time (revisão por pares).
- [ ] O artefato está pronto para uso em projeto, desenvolvimento e testes.

---

## Referências

- COCKBURN, Alistair. *Escrevendo Casos de Uso Eficazes*. Porto Alegre: Bookman, 2005.
- FOWLER, M. *UML Essencial*. Porto Alegre, 3ª Edição, 2005.
- BEZERRA, Eduardo. [Processo LAPIS — Documentando Requisitos com Caso de Uso](https://github.com/ProfBezerra/LAPIS/blob/main/Requisitos/Especifica%C3%A7%C3%A3o%20de%20Casos%20de%20Uso/Casos%20de%20Usos.md). GitHub, 2026.
