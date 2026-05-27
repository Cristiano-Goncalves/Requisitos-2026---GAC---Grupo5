# CDU-03 — Realizar Empréstimo de Projetor (Check-out)

---

## 1. Histórico de Versões

| Data | Versão | Descrição | Autor |
|---|---|---|---|
| 03/05/2026 | 1.0 | Criação do CDU-03 | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |
| 24/05/2026 | 1.1 | Inclusão da seção IV1 com tabela de campos e validações | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |

---

## 2. Identificação do Caso de Uso

### 2.1 Nome
Realizar Empréstimo de Projetor (Check-out)

### 2.2 Objetivo
Permitir o empréstimo de um projetor disponível a um professor identificado, gerando o Termo de Responsabilidade digital e associando a sala de destino informada pelo professor ao equipamento.

### 2.3 Tipo
Concreto.

---

## 3. Atores

### 3.1 Primário
Secretaria CCT.

### 3.2 Secundários
- Professor (presente no balcão para identificação e informação da sala).
- Serviço de E-mail (envia o Termo de Responsabilidade ao professor).

---

## 4. Pré-condições

- Usuário autenticado no sistema com perfil `secretaria`.
- Existe ao menos um projetor com status **disponível** no inventário.
- O professor solicitante está cadastrado no sistema e sem pendências de avaria conforme **RN2**.

---

## 5. Fluxo Principal (cenário de sucesso)

**P1.** A Secretaria CCT seleciona a opção **Check-out** no menu lateral do sistema.

**P2.** O sistema exibe a lista de projetores com status **disponível**, com campo de busca por código ou modelo.

**P3.** A Secretaria CCT seleciona um projetor da lista e clica em **Selecionar Projetor**. **[E1]**

**P4.** O sistema navega para a etapa de identificação do professor e exibe o feed ao vivo da webcam com guia oval de posicionamento.

**P5.** O Professor posiciona o rosto diante da câmera.

**P6.** O sistema realiza o reconhecimento facial, incluindo o caso de uso **CDU-04 — Identificar Professor por Reconhecimento Facial**. **[A1][E2]**

**P7.** O sistema exibe o card de confirmação do professor identificado com: Nome, Matrícula e Departamento.

**P7.1** A Secretaria CCT solicita ao professor a sala de destino do equipamento.

**P7.2** O sistema exibe campo para preenchimento manual da sala de destino.

**P7.3** A Secretaria CCT informa a sala de destino indicada pelo professor. **[E3]**

**P8.** A Secretaria CCT seleciona **Confirmar e Prosseguir**. **[A2]**

**P9.** O sistema exibe o resumo do empréstimo: Professor, Projetor, Sala de Destino, Data/Hora de Retirada e Previsão de Devolução.

**P10.** A Secretaria CCT seleciona **Confirmar e Gerar Termo**. **[A3]**

**P11.** O sistema persiste o registro do empréstimo.

**P11.1** Atualiza o status do projetor para **em_uso**.

**P11.2** Aciona o **Serviço de E-mail** para envio do Termo de Responsabilidade ao e-mail institucional do professor.

**P12.** O sistema registra a operação no histórico de alterações.

**P13.** O sistema exibe a mensagem de sucesso: *"Empréstimo registrado. Termo enviado para [e-mail]."* e retorna ao Dashboard.

**P14.** O caso de uso é encerrado.

---

## 6. Fluxos Alternativos

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

---

## 7. Fluxos de Exceção

**E1. Nenhum projetor disponível**

**E1.1** No passo **P2**, o sistema não encontra projetores com status **disponível**.

**E1.2** O sistema exibe a mensagem: *"Não há projetores disponíveis no momento."*

**E1.3** O caso de uso é encerrado.

---

**E2. Professor com ocorrência de avaria em aberto**

**E2.1** No passo **P6**, após a identificação, o sistema verifica que o professor possui status **bloqueado** conforme **RN2**.

**E2.2** O sistema bloqueia o avanço no fluxo.

**E2.3** O sistema exibe modal: *"Prof. [Nome] possui ocorrência de avaria em aberto. Empréstimo bloqueado. Contate a Coordenação CCT."*

**E2.4** O caso de uso é encerrado.

---

**E3. Sala de destino não informada**

**E3.1** No passo **P7.3**, a Secretaria CCT tenta prosseguir sem preencher a sala de destino.

**E3.2** O sistema bloqueia o avanço e exibe a mensagem: *"Informe a sala de destino antes de continuar."*

**E3.3** O sistema retorna ao passo **P7.2**.

---

## 8. Pós-condições

- Empréstimo registrado com status **ativo**.
- Projetor com status **em_uso**, vinculado ao professor.
- Termo de Responsabilidade enviado ao e-mail institucional do professor.
- Custódia legal do equipamento sob responsabilidade do professor conforme **RN1**.
- Operação registrada no histórico de alterações.

---

## 9. Interface Visual

### IV1. Lista de Projetores Disponíveis

Nesta tela, a Secretaria CCT visualiza e seleciona o equipamento a ser emprestado.

| Elemento | Comportamento |
|---|---|
| Campo de busca | Filtra por código de patrimônio ou marca/modelo em tempo real. |
| Lista de projetores | Exibe somente equipamentos com status **disponível**; cada item mostra código, modelo e localização. |
| Botão **Selecionar Projetor** | Desabilitado até que um projetor seja selecionado na lista. |

### IV2. Tela de Identificação do Professor

Nesta tela, o professor é identificado por reconhecimento facial ou busca manual.

| Elemento | Comportamento |
|---|---|
| Feed ao vivo da webcam | Ativado automaticamente ao entrar na tela. |
| Guia oval de posicionamento | Segue o mesmo comportamento definido em CDU-04 / IV1. |
| Campo de busca manual | Disponível como alternativa ao reconhecimento facial; busca por nome ou matrícula. |
| Card de confirmação | Exibido após identificação: Nome, Matrícula, Departamento e foto do perfil. |

### IV3. Formulário de Destino e Confirmação

Nesta tela, a Secretaria CCT informa a sala e confirma o empréstimo.

| Campo | Obrigatório | Formato | Regra de Validação |
|---|---|---|---|
| Sala de Destino | Sim | Texto livre (ex.: `Sala 310`) | Não aceitar campo vazio. |
| Previsão de Devolução | Não (informativo) | Data e hora | Calculado automaticamente com base no horário de retirada. |

---

## 10. Referências

- [CDU-04 — Identificar Professor por Reconhecimento Facial](CDU-04-Identificar-Professor-por-Reconhecimento-Facial.md) (incluído neste CDU)
- [CDU-05 — Realizar Devolução de Projetor](CDU-05-Realizar-Devolucao-de-Projetor.md)
- Regras de Negócio: **RN1**, **RN2**
- Funcionalidades: **F1.1**, **F1.2**
- Requisitos Não Funcionais: RNF — Performance de Reconhecimento
