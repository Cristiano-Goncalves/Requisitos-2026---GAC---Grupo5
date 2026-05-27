# CDU-02 — Cadastrar Projetor

---

## 1. Histórico de Versões

| Data | Versão | Descrição | Autor |
|---|---|---|---|
| 03/05/2026 | 1.0 | Criação do CDU-02 | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |
| 24/05/2026 | 1.1 | Inclusão da seção IV1 com tabela de campos e validações | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |

---

## 2. Identificação do Caso de Uso

### 2.1 Nome
Cadastrar Projetor

### 2.2 Objetivo
Permitir que a Secretaria CCT registre um novo projetor no inventário da instituição, associando um identificador único ao equipamento e abrindo seu prontuário técnico com histórico de manutenções.

### 2.3 Tipo
Concreto.

---

## 3. Atores

### 3.1 Primário
Secretaria CCT.

### 3.2 Secundários
- Coordenação CCT (pode solicitar o cadastro de novo equipamento).
- Sistema de autenticação (garante que apenas usuários autorizados acessem a função).

---

## 4. Pré-condições

- Usuário autenticado no sistema com perfil `secretaria`.
- O código de patrimônio do projetor não pode estar previamente cadastrado no sistema.

---

## 5. Fluxo Principal (cenário de sucesso)

**P1.** A Secretaria CCT seleciona a opção **Cadastros / Novo Projetor** no menu lateral do sistema.

**P2.** O sistema exibe o formulário de cadastro com os campos: Código de Patrimônio, Marca/Modelo, Número de Série, Ano de Fabricação, Estado Inicial e Observações.

**P3.** A Secretaria CCT preenche os campos obrigatórios e seleciona **Cadastrar e Gerar QR Code**. **[A1][E1][E2]**

**P4.** O sistema valida os dados conforme **RN5**. **[E1][E2]**

**P4.1** Verifica unicidade do Código de Patrimônio.

**P4.2** Persiste o registro do projetor com status **disponível**.

**P4.3** Abre a linha do tempo de manutenções do equipamento conforme **RN5**.

**P4.4** Gera o QR Code único vinculado ao Código de Patrimônio.

**P5.** O sistema registra a operação no histórico de alterações.

**P6.** O sistema exibe modal de sucesso com o QR Code gerado e as opções **Imprimir Etiqueta** e **Fechar**.

**P7.** A Secretaria CCT seleciona **Imprimir Etiqueta** ou **Fechar**.

**P8.** O caso de uso é encerrado.

---

## 6. Fluxos Alternativos

**A1. Cancelar cadastro**

**A1.1** No passo **P3**, a Secretaria CCT seleciona **Voltar**.

**A1.2** O sistema descarta os dados sem persistência.

**A1.3** O caso de uso é encerrado.

---

## 7. Fluxos de Exceção

**E1. Campos obrigatórios não preenchidos**

**E1.1** No passo **P4**, o sistema detecta campo obrigatório ausente.

**E1.2** O sistema bloqueia a conclusão do cadastro.

**E1.3** O sistema destaca os campos com borda vermelha e orienta a correção com mensagem de validação inline.

---

**E2. Código de patrimônio já cadastrado**

**E2.1** No passo **P4.1**, o sistema detecta que o código já existe na base.

**E2.2** O sistema bloqueia a conclusão do cadastro.

**E2.3** O sistema orienta a correção com a mensagem: *"Código de patrimônio já cadastrado. Verifique o número informado."*

---

**E3. Falha de persistência**

**E3.1** Ocorre falha técnica ao salvar os dados no banco.

**E3.2** O sistema informa indisponibilidade temporária ao usuário.

**E3.3** O sistema registra log técnico para suporte.

---

## 8. Pós-condições

- Projetor registrado no inventário com status **disponível**.
- Prontuário técnico criado com linha do tempo de manutenções vazia conforme **RN5**.
- QR Code único gerado e disponível para impressão.
- Operação registrada no histórico de alterações.

---

## 9. Interface Visual

### IV1. Formulário de Cadastro de Projetor

Nesta tela, a Secretaria CCT informa os dados do equipamento. A tabela abaixo define os campos, obrigatoriedade e regras de validação.

| Campo | Obrigatório | Formato | Regra de Validação |
|---|---|---|---|
| Código de Patrimônio | Sim | `P-XXXXX` (alfanumérico) | Único no sistema; bloquear duplicidade conforme **RN5**. |
| Marca / Modelo | Sim | Texto (3 a 100 caracteres) | Não aceitar campo vazio. |
| Número de Série | Não | Alfanumérico | Campo opcional; se preenchido, aceitar apenas caracteres alfanuméricos. |
| Ano de Fabricação | Sim | `AAAA` (lista de seleção) | Permitir somente anos entre 2000 e o ano atual. |
| Estado Inicial | Sim | Lista de seleção | Permitir somente: Novo, Bom, Regular ou Danificado. |
| Observações | Não | Texto livre (até 500 caracteres) | Campo opcional; se preenchido, limitar tamanho e sanitizar conteúdo. |
| QR Code | Não (automático) | Imagem gerada pelo sistema | Gerado após salvar o cadastro com sucesso; vinculado permanentemente ao Código de Patrimônio. |

---

## 10. Referências

- [CDU-05 — Realizar Devolução de Projetor](CDU-05-Realizar-Devolucao-de-Projetor.md)
- [CDU-08 — Consultar Prontuário Técnico via QR Code](CDU-08-Consultar-Prontuario-Tecnico-via-QR-Code.md)
- Regras de Negócio: **RN5**
- Funcionalidades: **F5.2**
