# CDU-06 — Registrar Ocorrência de Avaria

---

## 1. Histórico de Versões

| Data | Versão | Descrição | Autor |
|---|---|---|---|
| 03/05/2026 | 1.0 | Criação do CDU-06 | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |
| 24/05/2026 | 1.1 | Inclusão da seção IV1 com tabela de campos e validações | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |

---

## 2. Identificação do Caso de Uso

### 2.1 Nome
Registrar Ocorrência de Avaria

### 2.2 Objetivo
Permitir o registro formal de uma avaria em um projetor, vinculando a ocorrência ao último responsável pela custódia e bloqueando o professor para novos empréstimos até regularização.

### 2.3 Tipo
Abstrato. Instanciado pelo **CDU-05** (Check-in com avaria identificada).

---

## 3. Atores

### 3.1 Primário
Não se aplica (caso de uso abstrato). Acionado pela Secretaria CCT via **CDU-05**.

### 3.2 Secundários
- Serviço de E-mail (envia notificação ao professor e à Coordenação CCT).

---

## 4. Pré-condições

- O caso de uso deve ter sido instanciado por outro caso de uso.
- Ao menos um item do checklist de devolução foi marcado como **Avaria**.

---

## 5. Fluxo Principal (cenário de sucesso)

**P1.** O sistema exibe a tela de registro de avaria com: nome do último responsável pela custódia, itens marcados como avaria no checklist e campo para descrição detalhada da ocorrência.

**P2.** A Secretaria CCT preenche a descrição detalhada da ocorrência e seleciona o nível de gravidade: **Leve**, **Moderada** ou **Grave**. **[E1]**

**P3.** A Secretaria CCT, opcionalmente, captura uma fotografia da avaria via webcam. **[A1]**

**P4.** A Secretaria CCT seleciona **Registrar Avaria e Bloquear Professor**.

**P5.** O sistema exibe modal de confirmação: *"Prof. [Nome] será BLOQUEADO para novos empréstimos até a regularização da ocorrência. Deseja prosseguir?"* **[A2]**

**P6.** A Secretaria CCT confirma a ação.

**P7.** O sistema persiste a ocorrência de avaria vinculada ao último responsável conforme **RN1**.

**P7.1** Atualiza o status do professor para **bloqueado** conforme **RN2**.

**P7.2** Atualiza o status do projetor para **em_manutencao**.

**P7.3** Registra a ocorrência no prontuário técnico do projetor conforme **RN5**.

**P7.4** Aciona o **Serviço de E-mail** para notificação ao professor e à Coordenação CCT.

**P8.** O sistema registra a operação no histórico de alterações.

**P9.** O sistema retorna ao caso de uso chamador com status de ocorrência registrada.

**P10.** O caso de uso é encerrado.

---

## 6. Fluxos Alternativos

**A1. Não capturar fotografia**

**A1.1** No passo **P3**, a Secretaria CCT pula a etapa de captura fotográfica.

**A1.2** O sistema segue para o passo **P4** sem anexo fotográfico.

---

**A2. Cancelar registro de avaria**

**A2.1** No passo **P5**, a Secretaria CCT seleciona **Cancelar** no modal de confirmação.

**A2.2** O sistema descarta a operação sem persistência.

**A2.3** O sistema retorna ao passo **P1**.

---

## 7. Fluxos de Exceção

**E1. Descrição da ocorrência não preenchida**

**E1.1** No passo **P4**, o sistema detecta que o campo de descrição está vazio.

**E1.2** O sistema bloqueia o avanço.

**E1.3** O sistema orienta a correção com a mensagem: *"A descrição da ocorrência é obrigatória."*

**E1.4** O sistema retorna ao passo **P2**.

---

## 8. Pós-condições

- Ocorrência de avaria registrada na base de dados e no prontuário do projetor.
- Professor com status **bloqueado** conforme **RN2**.
- Projetor com status **em_manutencao**.
- Professor e Coordenação CCT notificados por e-mail.
- Operação registrada no histórico de alterações.

---

## 9. Interface Visual

### IV1. Formulário de Registro de Avaria

Nesta tela, a Secretaria CCT formaliza a ocorrência detectada no checklist. A tabela abaixo define os campos, obrigatoriedade e regras de validação.

| Campo | Obrigatório | Formato | Regra de Validação |
|---|---|---|---|
| Último Responsável | Não (informativo) | Texto | Preenchido automaticamente pelo sistema com o nome do professor. |
| Itens com Avaria | Não (informativo) | Lista | Preenchido automaticamente pelo sistema com base no checklist. |
| Descrição da Ocorrência | Sim | Texto livre (até 1000 caracteres) | Não aceitar campo vazio; limitar tamanho e sanitizar conteúdo. |
| Nível de Gravidade | Sim | Lista de seleção | Permitir somente: Leve, Moderada ou Grave. |
| Fotografia da Avaria | Não | Imagem capturada via webcam | Campo opcional; se preenchido, capturar via webcam integrada. |

---

## 10. Referências

- [CDU-05 — Realizar Devolução de Projetor](CDU-05-Realizar-Devolucao-de-Projetor.md) (caso de uso chamador)
- [CDU-10 — Bloquear ou Desbloquear Professor](CDU-10-Bloquear-ou-Desbloquear-Professor.md)
- Regras de Negócio: **RN1**, **RN2**, **RN5**
- Funcionalidades: **F3.2**
