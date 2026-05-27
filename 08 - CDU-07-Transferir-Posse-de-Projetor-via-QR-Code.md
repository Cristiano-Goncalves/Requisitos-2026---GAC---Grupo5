# CDU-07 — Transferir Posse de Projetor via QR Code

---

## 1. Histórico de Versões

| Data | Versão | Descrição | Autor |
|---|---|---|---|
| 03/05/2026 | 1.0 | Criação do CDU-07 | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |
| 24/05/2026 | 1.1 | Inclusão da seção IV1 com tabela de elementos visuais | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |

---

## 2. Identificação do Caso de Uso

### 2.1 Nome
Transferir Posse de Projetor via QR Code

### 2.2 Objetivo
Permitir que um professor transfira a custódia oficial de um projetor para outro professor por meio da leitura do QR Code fixado no equipamento, registrando a geolocalização no momento do repasse.

### 2.3 Tipo
Concreto.

---

## 3. Atores

### 3.1 Primário
Professor (que assume a custódia — receptor).

### 3.2 Secundários
- Professor (que cede a custódia — cedente).

---

## 4. Pré-condições

- Professor receptor autenticado no PWA do ProjecTrack com perfil `professor`.
- O projetor está com status **em_uso** ou **em_repasse**, sob custódia de um professor ativo.
- O Professor receptor está cadastrado no sistema e com status **ativo** (sem bloqueio por avaria conforme **RN2**).
- O smartphone do Professor receptor possui câmera disponível e GPS habilitado.

---

## 5. Fluxo Principal (cenário de sucesso)

**P1.** O Professor receptor acessa o aplicativo PWA do ProjecTrack no smartphone e seleciona a opção **Escanear QR Code**.

**P2.** O sistema exibe o leitor de QR Code via câmera do smartphone.

**P3.** O Professor receptor aponta a câmera para o QR Code fixado no projetor. **[E1]**

**P4.** O sistema decodifica o QR Code e identifica o projetor correspondente.

**P5.** O sistema verifica que o projetor está com custódia ativa de outro professor conforme **RN1**. **[A1][E2]**

**P6.** O sistema exibe a tela de autenticação facial no smartphone para identificar o Professor receptor.

**P7.** O Professor receptor posiciona o rosto diante da câmera frontal do smartphone.

**P8.** O sistema realiza o reconhecimento facial, incluindo o caso de uso **CDU-04 — Identificar Professor por Reconhecimento Facial**. **[E3][E4]**

**P9.** O sistema exibe a tela de confirmação da transferência com: Projetor, Professor cedente (atual responsável) e Professor receptor (novo responsável).

**P10.** O sistema captura as coordenadas de geolocalização do dispositivo do Professor receptor. **[E3]**

**P11.** O Professor receptor seleciona **Confirmar Transferência**. **[A2]**

**P12.** O sistema persiste a transferência de custódia.

**P12.1** Atualiza o responsável pelo projetor para o Professor receptor.

**P12.2** Atualiza o status do projetor para **em_repasse**.

**P12.3** Registra coordenadas de GPS, horário e identificadores de ambos os professores no histórico de custódia conforme **RN1**.

**P13.** O sistema registra a operação no histórico de alterações.

**P14.** O sistema exibe a mensagem de sucesso: *"Transferência registrada. Você é agora o responsável pelo projetor [Código]."*

**P15.** O caso de uso é encerrado.

---

## 6. Fluxos Alternativos

**A1. Projetor sem custódia ativa**

**A1.1** No passo **P5**, o sistema identifica que o projetor está com status **disponível**.

**A1.2** O sistema exibe a mensagem: *"Este projetor não possui empréstimo ativo. Procure a Secretaria para formalizar um novo empréstimo."*

**A1.3** O caso de uso é encerrado.

---

**A2. Cancelar transferência**

**A2.1** No passo **P11**, o Professor receptor seleciona **Cancelar**.

**A2.2** O sistema descarta a operação sem persistência.

**A2.3** A custódia permanece com o Professor cedente conforme **RN1**.

**A2.4** O caso de uso é encerrado.

---

## 7. Fluxos de Exceção

**E1. QR Code inválido ou não pertencente ao sistema**

**E1.1** No passo **P4**, o sistema não localiza o projetor correspondente ao QR Code lido.

**E1.2** O sistema exibe a mensagem: *"QR Code não reconhecido. Verifique se a etiqueta pertence a um projetor cadastrado no ProjecTrack."*

**E1.3** O sistema retorna ao passo **P2**.

---

**E2. Professor receptor com bloqueio ativo**

**E2.1** No passo **P5** ou **P8**, o sistema identifica que o Professor receptor possui status **bloqueado** conforme **RN2**.

**E2.2** O sistema bloqueia o avanço no fluxo.

**E2.3** O sistema exibe a mensagem: *"Você possui uma ocorrência de avaria em aberto. Novas custódias estão bloqueadas. Regularize com a Coordenação CCT."*

**E2.4** O caso de uso é encerrado.

---

**E3. Geolocalização negada pelo dispositivo**

**E3.1** No passo **P10**, o dispositivo do Professor receptor não concede permissão de geolocalização.

**E3.2** O sistema bloqueia a confirmação da transferência.

**E3.3** O sistema exibe o aviso: *"Permissão de localização necessária para registrar o repasse. Habilite o GPS e tente novamente."*

**E3.4** O sistema retorna ao passo **P9**.

---

**E4. Rosto não reconhecido**

**E4.1** No passo **P8**, o reconhecimento facial retorna falha.

**E4.2** O sistema exibe a mensagem: *"Não foi possível identificar você. Tente novamente em local com melhor iluminação."*

**E4.3** O sistema retorna ao passo **P7**.

---

## 8. Pós-condições

- Custódia do projetor transferida oficialmente para o Professor receptor.
- Professor cedente isento da responsabilidade pelo equipamento a partir do repasse conforme **RN1**.
- Geolocalização e horário do repasse registrados no histórico imutável de custódia.
- Operação registrada no histórico de alterações.

---

## 9. Interface Visual

### IV1. Tela de Leitura de QR Code (PWA — Mobile)

Nesta tela, o Professor receptor escaneia o QR Code fixado no projetor.

| Elemento | Comportamento |
|---|---|
| Leitor de QR Code | Ativado via câmera traseira do smartphone; exibe frame de leitura centralizado. |
| Indicador de status | Texto dinâmico: *"Aponte para o QR Code do projetor..."*. |
| Feedback de leitura | Vibração e som de confirmação ao decodificar o QR Code com sucesso. |

### IV2. Tela de Confirmação da Transferência

Nesta tela, o Professor receptor visualiza o resumo da transferência e confirma a operação.

| Campo | Obrigatório | Formato | Regra de Validação |
|---|---|---|---|
| Projetor | Não (informativo) | Texto | Preenchido automaticamente com código e modelo do projetor. |
| Professor Cedente | Não (informativo) | Texto | Preenchido automaticamente com o nome do responsável atual. |
| Professor Receptor | Não (informativo) | Texto | Preenchido automaticamente com o nome do usuário autenticado. |
| Geolocalização | Não (automático) | Coordenadas GPS | Capturada automaticamente pelo sistema; obrigatória para confirmar a transferência. |
| Botão **Confirmar Transferência** | — | — | Habilitado somente após geolocalização capturada com sucesso. |

---

## 10. Referências

- [CDU-04 — Identificar Professor por Reconhecimento Facial](CDU-04-Identificar-Professor-por-Reconhecimento-Facial.md) (incluído neste CDU)
- [CDU-09 — Visualizar Mapa de Rastreamento do Patrimônio](CDU-09-Visualizar-Mapa-de-Rastreamento.md)
- Regras de Negócio: **RN1**, **RN2**
- Funcionalidades: **F2.1**, **F2.2**, **F5.3**
