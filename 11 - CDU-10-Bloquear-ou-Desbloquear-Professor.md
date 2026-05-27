# CDU-10 — Bloquear ou Desbloquear Professor

---

## 1. Histórico de Versões

| Data | Versão | Descrição | Autor |
|---|---|---|---|
| 03/05/2026 | 1.0 | Criação do CDU-10 | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |
| 24/05/2026 | 1.1 | Inclusão da seção IV1 com tabela de campos e validações | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |

---

## 2. Identificação do Caso de Uso

### 2.1 Nome
Bloquear ou Desbloquear Professor

### 2.2 Objetivo
Permitir à Coordenação CCT bloquear ou desbloquear um professor para empréstimos, com base na análise de ocorrências de avaria em aberto ou na regularização com a instituição.

### 2.3 Tipo
Concreto.

---

## 3. Atores

### 3.1 Primário
Coordenação CCT.

### 3.2 Secundários
- Serviço de E-mail (notifica o professor sobre o bloqueio ou desbloqueio).
- Sistema de autenticação (garante que apenas a Coordenação acesse a função).

---

## 4. Pré-condições

- Usuário autenticado no sistema com perfil `coordenacao`.
- Para **bloquear**: o professor está com status **ativo** e possui ocorrência de avaria registrada.
- Para **desbloquear**: o professor está com status **bloqueado**.

---

## 5. Fluxo Principal (cenário de sucesso)

**P1.** A Coordenação CCT seleciona a opção **Gestão de Professores** no Dashboard da Coordenação.

**P2.** O sistema exibe a lista de professores com filtro por status (**Todos**, **Ativos**, **Bloqueados**) e campo de busca por nome ou matrícula.

**P3.** A Coordenação CCT localiza o professor desejado e seleciona **Ver Detalhes**. **[E1]**

**P4.** O sistema exibe o perfil do professor com: dados cadastrais, histórico de empréstimos e lista de ocorrências de avaria vinculadas.

**P5.** A Coordenação CCT seleciona **Bloquear Professor** ou **Desbloquear Professor**, conforme o status atual. **[A1]**

**P6.** O sistema exibe modal de confirmação com o resumo da ação e campo obrigatório de justificativa.

**P7.** A Coordenação CCT preenche a justificativa e seleciona **Confirmar**. **[A2][E2]**

**P8.** O sistema atualiza o status do professor conforme **RN2**.

**P8.1** Persiste a justificativa e o registro da ação no histórico do professor.

**P8.2** Aciona o **Serviço de E-mail** para notificação ao professor informando o bloqueio ou desbloqueio com a justificativa.

**P9.** O sistema registra a operação no histórico de alterações.

**P10.** O sistema exibe mensagem de sucesso e retorna à lista de professores.

**P11.** O caso de uso é encerrado.

---

## 6. Fluxos Alternativos

**A1. Professor já está no status desejado**

**A1.1** No passo **P5**, a ação selecionada corresponde ao status atual do professor.

**A1.2** O sistema exibe a mensagem: *"Este professor já está [ativo/bloqueado]."*

**A1.3** O sistema retorna ao passo **P4**.

---

**A2. Cancelar ação**

**A2.1** No passo **P7**, a Coordenação CCT seleciona **Cancelar** no modal de confirmação.

**A2.2** O sistema descarta a operação sem persistência.

**A2.3** O sistema retorna ao passo **P4**.

---

## 7. Fluxos de Exceção

**E1. Professor não encontrado**

**E1.1** No passo **P3**, o sistema não localiza resultado para o critério de busca informado.

**E1.2** O sistema exibe a mensagem: *"Nenhum professor encontrado para este critério."*

**E1.3** O sistema retorna ao passo **P2**.

---

**E2. Justificativa não preenchida**

**E2.1** No passo **P7**, a Coordenação CCT tenta confirmar sem preencher a justificativa.

**E2.2** O sistema bloqueia a confirmação.

**E2.3** O sistema orienta a correção com a mensagem: *"A justificativa é obrigatória para registrar esta ação."*

**E2.4** O sistema retorna ao passo **P6**.

---

## 8. Pós-condições

- Professor com status atualizado (**ativo** ou **bloqueado**) conforme **RN2**.
- Justificativa e registro da ação persistidos no histórico do professor.
- Professor notificado por e-mail com a justificativa da ação.
- Operação registrada no histórico de alterações.

---

## 9. Interface Visual

### IV1. Lista de Professores

Nesta tela, a Coordenação CCT localiza o professor a ser bloqueado ou desbloqueado.

| Elemento | Comportamento |
|---|---|
| Campo de busca | Filtra por nome ou matrícula em tempo real. |
| Filtro por status | Botões de alternância: **Todos**, **Ativos**, **Bloqueados**; atualiza a lista imediatamente. |
| Lista de professores | Exibe nome, matrícula, departamento e status atual de cada professor. |
| Indicador de status | **Verde** = Ativo; **Vermelho** = Bloqueado. |
| Botão **Ver Detalhes** | Navega para o perfil completo do professor selecionado. |

### IV2. Perfil do Professor e Ações de Bloqueio

Nesta tela, a Coordenação CCT analisa o histórico do professor e executa o bloqueio ou desbloqueio.

| Campo | Obrigatório | Formato | Regra de Validação |
|---|---|---|---|
| Dados cadastrais | Não (informativo) | Texto | Exibidos em modo somente leitura. |
| Histórico de empréstimos | Não (informativo) | Lista | Exibido em ordem cronológica decrescente. |
| Ocorrências de avaria | Não (informativo) | Lista | Exibidas com data, gravidade e status (aberta/regularizada). |
| Botão **Bloquear Professor** | — | — | Exibido somente se o professor está com status **ativo**. |
| Botão **Desbloquear Professor** | — | — | Exibido somente se o professor está com status **bloqueado**. |

### IV3. Modal de Confirmação

Nesta tela, a Coordenação CCT confirma a ação e registra a justificativa.

| Campo | Obrigatório | Formato | Regra de Validação |
|---|---|---|---|
| Resumo da ação | Não (informativo) | Texto | Gerado automaticamente pelo sistema. |
| Justificativa | Sim | Texto livre (até 500 caracteres) | Não aceitar campo vazio; sanitizar conteúdo. |

---

## 10. Referências

- [CDU-06 — Registrar Ocorrência de Avaria](CDU-06-Registrar-Ocorrencia-de-Avaria.md)
- Regras de Negócio: **RN2**
- Funcionalidades: **F3.2**
