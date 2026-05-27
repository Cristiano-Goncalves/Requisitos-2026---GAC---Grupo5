# CDU-08 — Consultar Prontuário Técnico via QR Code

---

## 1. Histórico de Versões

| Data | Versão | Descrição | Autor |
|---|---|---|---|
| 03/05/2026 | 1.0 | Criação do CDU-08 | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |
| 24/05/2026 | 1.1 | Inclusão da seção IV1 com tabela de elementos visuais | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |

---

## 2. Identificação do Caso de Uso

### 2.1 Nome
Consultar Prontuário Técnico via QR Code

### 2.2 Objetivo
Permitir que a Secretaria ou a Coordenação CCT acesse o prontuário técnico completo de um projetor por meio da leitura do QR Code fixado no equipamento, visualizando histórico de manutenções, avarias e tempo de vida útil.

### 2.3 Tipo
Concreto.

---

## 3. Atores

### 3.1 Primários
- Secretaria CCT.
- Coordenação CCT.

### 3.2 Secundários
- Sistema de autenticação (valida o perfil de acesso antes de renderizar o conteúdo conforme **RN6**).

---

## 4. Pré-condições

- Usuário autenticado com perfil `secretaria` ou `coordenacao`.
- O projetor está cadastrado no sistema.

---

## 5. Fluxo Principal (cenário de sucesso)

**P1.** O ator seleciona a opção **Escanear Projetor** no sistema ou acessa diretamente via leitura do QR Code.

**P2.** O sistema exibe o leitor de QR Code.

**P3.** O ator lê o QR Code fixado no projetor. **[E1]**

**P4.** O sistema valida o perfil de acesso do usuário autenticado conforme **RN6**. **[A1]**

**P5.** O sistema exibe o prontuário técnico completo: Código de Patrimônio, Marca/Modelo, Ano de Fabricação, Vida Útil Estimada, Status Atual e Linha do Tempo de Manutenções.

**P6.** O ator navega pelo histórico de manutenções e avarias.

**P7.** O ator, opcionalmente, seleciona **Registrar Nova Manutenção**. **[PE1][A2]**

**P8.** O caso de uso é encerrado.

---

## 6. Fluxos Alternativos

**A1. Usuário autenticado como Professor**

**A1.1** No passo **P4**, o sistema identifica que o usuário possui `role: professor`.

**A1.2** O sistema redireciona para o caso de uso **CDU-07 — Transferir Posse de Projetor via QR Code**, exibindo apenas as ações de custódia conforme **RN6**.

**A1.3** O caso de uso é encerrado.

---

**A2. Registrar nova manutenção**

**A2.1** No passo **P7**, o ator seleciona **Registrar Nova Manutenção**.

**A2.2** O sistema exibe formulário com os campos: Data, Tipo de Manutenção, Descrição e Técnico Responsável.

**A2.3** O ator preenche os dados e confirma.

**A2.4** O sistema valida os campos obrigatórios. **[E2]**

**A2.5** O sistema persiste o registro de forma cumulativa no prontuário conforme **RN5**.

**A2.6** O sistema exibe mensagem de sucesso e retorna ao passo **P5**.

---

## 7. Fluxos de Exceção

**E1. QR Code inválido**

**E1.1** No passo **P3**, o sistema não localiza o projetor correspondente ao QR Code lido.

**E1.2** O sistema exibe a mensagem: *"Equipamento não encontrado. Verifique se o QR Code pertence ao ProjecTrack."*

**E1.3** O sistema retorna ao passo **P2**.

---

**E2. Campos obrigatórios da manutenção não preenchidos**

**E2.1** No passo **A2.4**, o sistema detecta campo obrigatório ausente no formulário de manutenção.

**E2.2** O sistema bloqueia o salvamento.

**E2.3** O sistema destaca os campos e orienta a correção com mensagem de validação inline.

**E2.4** O sistema retorna ao passo **A2.2**.

---

## 8. Pós-condições

- Prontuário técnico do projetor visualizado pelo ator.
- Se nova manutenção registrada: registro persistido de forma cumulativa no prontuário conforme **RN5**.

---

## 9. Interface Visual

### IV1. Prontuário Técnico do Projetor

Nesta tela, o ator visualiza as informações restritas e o histórico técnico do equipamento.

| Elemento | Comportamento |
|---|---|
| Código de Patrimônio | Exibido em destaque no topo da tela. |
| Marca / Modelo | Informativo; preenchido no cadastro. |
| Ano de Fabricação | Informativo; exibe também a estimativa de vida útil restante. |
| Status Atual | Exibido com indicador colorido: Verde = Disponível; Amarelo = Em Uso; Vermelho = Em Manutenção. |
| Linha do Tempo de Manutenções | Exibida em ordem cronológica decrescente; cada entrada mostra data, tipo, descrição e técnico responsável. |
| Botão **Registrar Nova Manutenção** | Visível apenas para perfis `secretaria` e `coordenacao`. |

### IV2. Formulário de Nova Manutenção

Nesta tela, o ator registra uma manutenção no prontuário do equipamento.

| Campo | Obrigatório | Formato | Regra de Validação |
|---|---|---|---|
| Data | Sim | `DD/MM/AAAA` | Não aceitar datas futuras. |
| Tipo de Manutenção | Sim | Lista de seleção | Permitir somente: Preventiva, Corretiva ou Substituição de Peça. |
| Descrição | Sim | Texto livre (até 1000 caracteres) | Não aceitar campo vazio; sanitizar conteúdo. |
| Técnico Responsável | Sim | Texto (3 a 100 caracteres) | Não aceitar campo vazio. |

---

## 10. Pontos de Extensão

### PE1. Registrar Nova Manutenção

No passo **P7**, o ator seleciona a opção **Registrar Nova Manutenção** e o sistema estende o caso de uso para o formulário de registro descrito em **IV2**.

---

## 11. Referências

- [CDU-02 — Cadastrar Projetor](CDU-02-Cadastrar-Projetor.md)
- [CDU-07 — Transferir Posse de Projetor via QR Code](CDU-07-Transferir-Posse-de-Projetor-via-QR-Code.md)
- Regras de Negócio: **RN5**, **RN6**
- Funcionalidades: **F5.4**
