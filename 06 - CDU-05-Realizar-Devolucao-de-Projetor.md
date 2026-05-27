# CDU-05 — Realizar Devolução de Projetor (Check-in)

---

## 1. Histórico de Versões

| Data | Versão | Descrição | Autor |
|---|---|---|---|
| 03/05/2026 | 1.0 | Criação do CDU-05 | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |
| 24/05/2026 | 1.1 | Inclusão da seção IV1 com tabela de campos e validações | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |

---

## 2. Identificação do Caso de Uso

### 2.1 Nome
Realizar Devolução de Projetor (Check-in)

### 2.2 Objetivo
Permitir o registro da devolução de um projetor, com a execução do checklist obrigatório de integridade física do equipamento antes da finalização do empréstimo.

### 2.3 Tipo
Concreto.

---

## 3. Atores

### 3.1 Primário
Secretaria CCT.

### 3.2 Secundários
- Sistema de autenticação (garante que apenas usuários autorizados acessem a função).

---

## 4. Pré-condições

- Usuário autenticado no sistema com perfil `secretaria`.
- Existe ao menos um empréstimo com status **ativo** ou **em_repasse** para o projetor que está sendo devolvido.

---

## 5. Fluxo Principal (cenário de sucesso)

**P1.** A Secretaria CCT seleciona a opção **Check-in** no menu lateral do sistema.

**P2.** O sistema exibe a lista de empréstimos com status **ativo** ou **em_repasse**, com campo de busca por código do projetor ou nome do professor.

**P3.** A Secretaria CCT localiza e seleciona o empréstimo correspondente e clica em **Iniciar Checklist de Devolução**. **[E1]**

**P4.** O sistema exibe o checklist de integridade com os itens: Lente, Controle Remoto, Cabo HDMI, Cabo de Energia, Corpo/Tampa e Lâmpada/LED. Cada item possui as opções **OK** e **Avaria**, com campo de observação livre.

**P5.** A Secretaria CCT avalia cada item fisicamente e marca o status correspondente no checklist. **[E2]**

**P6.** Após marcar todos os itens, a Secretaria CCT seleciona **Finalizar — Sem Avarias**. **[A1]**

**P7.** O sistema valida que todos os itens foram avaliados.

**P7.1** Persiste o resultado do checklist no prontuário técnico do projetor conforme **RN5**.

**P7.2** Atualiza o status do empréstimo para **encerrado**.

**P7.3** Atualiza o status do projetor para **disponível**.

**P8.** O sistema registra a operação no histórico de alterações.

**P9.** O sistema exibe a mensagem de sucesso: *"Devolução registrada com sucesso."* e retorna ao Dashboard.

**P10.** O caso de uso é encerrado.

---

## 6. Fluxos Alternativos

**A1. Avaria identificada no checklist**

**A1.1** No passo **P5**, a Secretaria CCT marca ao menos um item como **Avaria**.

**A1.2** O sistema desabilita o botão **Finalizar — Sem Avarias** e habilita apenas o botão **Registrar Ocorrência de Avaria**.

**A1.3** A Secretaria CCT seleciona **Registrar Ocorrência de Avaria**.

**A1.4** O sistema inclui o caso de uso **CDU-06 — Registrar Ocorrência de Avaria**.

**A1.5** O caso de uso é encerrado.

---

## 7. Fluxos de Exceção

**E1. Nenhum empréstimo ativo encontrado**

**E1.1** No passo **P2**, o sistema não localiza empréstimos ativos para o critério de busca informado.

**E1.2** O sistema exibe a mensagem: *"Nenhum empréstimo ativo encontrado para este critério."*

**E1.3** O sistema retorna ao passo **P2**.

---

**E2. Checklist incompleto**

**E2.1** No passo **P6**, a Secretaria CCT tenta finalizar sem ter marcado todos os itens do checklist.

**E2.2** O sistema bloqueia a finalização.

**E2.3** O sistema destaca os itens não avaliados e exibe a mensagem: *"Todos os itens devem ser avaliados antes de finalizar."*

**E2.4** O sistema retorna ao passo **P4**.

---

## 8. Pós-condições

- Empréstimo com status **encerrado**.
- Projetor com status **disponível** (sem avarias) ou **em_manutencao** (com avaria registrada).
- Resultado do checklist persistido no prontuário técnico do projetor conforme **RN5**.
- Operação registrada no histórico de alterações.

---

## 9. Interface Visual

### IV1. Lista de Empréstimos Ativos

Nesta tela, a Secretaria CCT localiza o empréstimo a ser encerrado.

| Elemento | Comportamento |
|---|---|
| Campo de busca | Filtra por código do projetor ou nome do professor em tempo real. |
| Lista de empréstimos | Exibe somente empréstimos com status **ativo** ou **em_repasse**; mostra código do projetor, professor responsável e horário de retirada. |
| Botão **Iniciar Checklist** | Habilitado somente após seleção de um empréstimo na lista. |

### IV2. Checklist de Integridade

Nesta tela, a Secretaria CCT avalia fisicamente cada componente do projetor. A tabela abaixo define os itens, status possíveis e comportamentos.

| Item do Checklist | Obrigatório | Status Possíveis | Regra de Validação |
|---|---|---|---|
| Lente | Sim | OK / Avaria | Campo de observação habilitado ao selecionar **Avaria**. |
| Controle Remoto | Sim | OK / Avaria | Campo de observação habilitado ao selecionar **Avaria**. |
| Cabo HDMI | Sim | OK / Avaria | Campo de observação habilitado ao selecionar **Avaria**. |
| Cabo de Energia | Sim | OK / Avaria | Campo de observação habilitado ao selecionar **Avaria**. |
| Corpo / Tampa | Sim | OK / Avaria | Campo de observação habilitado ao selecionar **Avaria**. |
| Lâmpada / LED | Sim | OK / Avaria | Campo de observação habilitado ao selecionar **Avaria**. |
| Botão **Finalizar — Sem Avarias** | — | Habilitado / Desabilitado | Habilitado somente se todos os itens estiverem marcados como **OK**. |
| Botão **Registrar Ocorrência** | — | Habilitado / Desabilitado | Habilitado somente se ao menos um item estiver marcado como **Avaria**. |

---

## 10. Referências

- [CDU-06 — Registrar Ocorrência de Avaria](CDU-06-Registrar-Ocorrencia-de-Avaria.md) (incluído neste CDU)
- [CDU-03 — Realizar Empréstimo de Projetor](CDU-03-Realizar-Emprestimo-de-Projetor.md)
- Regras de Negócio: **RN5**
- Funcionalidades: **F3.1**, **F3.2**
