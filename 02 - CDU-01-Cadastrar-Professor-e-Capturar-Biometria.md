# CDU-01 — Cadastrar Professor e Capturar Biometria

---

## 1. Histórico de Versões

| Data | Versão | Descrição | Autor |
|---|---|---|---|
| 03/05/2026 | 1.0 | Criação do CDU-01 | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |
| 24/05/2026 | 1.1 | Inclusão da seção IV1 com tabela de campos e validações | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |

---

## 2. Identificação do Caso de Uso

### 2.1 Nome
Cadastrar Professor e Capturar Biometria

### 2.2 Objetivo
Permitir que a Secretaria CCT registre um novo professor no sistema, coletando os dados obrigatórios de identificação e realizando a captura da biometria facial para extração e salvamento do vetor numérico (*embedding*).

### 2.3 Tipo
Concreto.

---

## 3. Atores

### 3.1 Primário
Secretaria CCT.

### 3.2 Secundários
- Professor (presente fisicamente no balcão para captura facial).
- Sistema de autenticação (garante que apenas usuários autorizados acessem a função).

---

## 4. Pré-condições

- Usuário autenticado no sistema com perfil `secretaria`.
- A webcam do balcão de atendimento está conectada e operacional.
- O professor não possui cadastro ativo com o mesmo CPF ou mesma assinatura biométrica no sistema.

---

## 5. Fluxo Principal (cenário de sucesso)

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

**P12.** O sistema registra a operação no histórico de alterações.

**P13.** O sistema exibe a mensagem de confirmação: *"Professor cadastrado com sucesso."* e retorna ao Dashboard da Secretaria.

**P14.** O caso de uso é encerrado.

---

## 6. Fluxos Alternativos

**A1. Cancelar cadastro**

**A1.1** No passo **P3**, a Secretaria CCT seleciona **Voltar**.

**A1.2** O sistema descarta os dados preenchidos sem persistência.

**A1.3** O caso de uso é encerrado.

---

**A2. CPF já cadastrado no sistema**

**A2.1** No passo **P4.1**, o sistema identifica CPF já vinculado a outro cadastro ativo conforme **RN4**.

**A2.2** O sistema não cria novo cadastro.

**A2.3** O sistema exibe modal de alerta: *"CPF já vinculado ao professor [Nome]. Deseja visualizar o cadastro existente?"* com as opções **Ver Cadastro** e **Cancelar**.

**A2.4** Se a Secretaria CCT selecionar **Ver Cadastro**, o sistema exibe os dados existentes e oferece opção de atualização.

**A2.5** O caso de uso é encerrado.

---

**A3. Recapturar biometria**

**A3.1** No passo **P8**, a captura resulta em baixa qualidade de imagem (rosto parcialmente detectado, iluminação insuficiente).

**A3.2** O sistema exibe a mensagem: *"Qualidade insuficiente para extração do vetor. Tente novamente."*

**A3.3** O sistema retorna ao passo **P5**.

---

## 7. Fluxos de Exceção

**E1. Campos obrigatórios não preenchidos**

**E1.1** No passo **P3** ou **P4**, o sistema detecta campo obrigatório ausente.

**E1.2** O sistema bloqueia a conclusão do cadastro.

**E1.3** O sistema destaca o(s) campo(s) com borda vermelha e orienta a correção com a mensagem inline: *"Campo obrigatório."*

---

**E2. Formato de CPF inválido**

**E2.1** No passo **P4**, o sistema detecta que o CPF não passa na validação do algoritmo de dígitos verificadores.

**E2.2** O sistema bloqueia a conclusão do cadastro.

**E2.3** O sistema orienta a correção do campo com a mensagem inline: *"CPF inválido. Verifique o número informado."*

---

**E3. E-mail fora do domínio institucional**

**E3.1** No passo **P4**, o sistema detecta que o e-mail informado não pertence ao domínio `@unifor.br`.

**E3.2** O sistema bloqueia a conclusão do cadastro.

**E3.3** O sistema orienta a correção do campo com a mensagem inline: *"Use um e-mail institucional @unifor.br."*

---

**E4. Assinatura biométrica já cadastrada**

**E4.1** No passo **P8**, o sistema detecta similaridade acima do limiar com um vetor já armazenado na base conforme **RN4**.

**E4.2** O sistema bloqueia a conclusão do cadastro.

**E4.3** O sistema exibe modal: *"Assinatura facial já vinculada ao professor [Nome]. Possível duplicidade. Verifique com a Coordenação."*

**E4.4** O caso de uso é encerrado.

---

## 8. Pós-condições

- Cadastro criado com status **ativo**.
- Vetor de *embedding* facial armazenado e disponível para os fluxos de reconhecimento.
- Operação registrada no histórico de alterações.
- Nenhuma imagem facial (`.jpg`/`.png`) é persistida, garantindo conformidade com a **LGPD**.

---

## 9. Interface Visual

### IV1. Formulário de Cadastro de Professor

Nesta tela, a Secretaria CCT informa os dados básicos do professor. A tabela abaixo define os campos, obrigatoriedade e regras de validação.

| Campo | Obrigatório | Formato | Regra de Validação |
|---|---|---|---|
| Nome Completo | Sim | Texto (3 a 150 caracteres) | Não aceitar campo vazio; exigir ao menos 3 palavras. |
| Matrícula UNIFOR | Sim | Alfanumérico | Único no sistema; bloquear duplicidade. |
| CPF | Sim | `999.999.999-99` | Validar formato e dígitos verificadores; bloquear duplicidade conforme **RN4**. |
| E-mail Institucional | Sim | `texto@unifor.br` | Validar domínio `@unifor.br`; bloquear outros domínios. |
| Departamento | Sim | Lista de seleção | Permitir somente opções do catálogo institucional. |

### IV2. Tela de Captura Biométrica

Nesta tela, a webcam é ativada para captura do rosto do professor. A tabela abaixo detalha os elementos visuais e seus comportamentos.

| Elemento | Comportamento |
|---|---|
| Feed ao vivo da webcam | Exibido em tempo real assim que a tela é carregada. |
| Guia oval de posicionamento | Cinza = aguardando rosto; Amarelo = rosto fora do centro; Verde = rosto posicionado corretamente; Vermelho = múltiplos rostos. |
| Botão **Capturar Biometria** | Desabilitado até o guia oval ficar verde. |
| Aviso de privacidade | Exibido fixo na tela: *"Nenhuma imagem será armazenada. Apenas o vetor matemático é salvo."* |

---

## 10. Referências

- [CDU-03 — Realizar Empréstimo de Projetor](CDU-03-Realizar-Emprestimo-de-Projetor.md) (utiliza o cadastro biométrico gerado neste CDU)
- Regras de Negócio: **RN3**, **RN4**
- Funcionalidades: **F5.1**
- Requisitos Não Funcionais: RNF — Performance de Reconhecimento, RNF — Conformidade LGPD
