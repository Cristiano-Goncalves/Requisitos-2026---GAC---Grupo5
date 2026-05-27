# CDU-09 — Visualizar Mapa de Rastreamento do Patrimônio

---

## 1. Histórico de Versões

| Data | Versão | Descrição | Autor |
|---|---|---|---|
| 03/05/2026 | 1.0 | Criação do CDU-09 | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |
| 24/05/2026 | 1.1 | Inclusão da seção IV1 com tabela de elementos visuais | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |

---

## 2. Identificação do Caso de Uso

### 2.1 Nome
Visualizar Mapa de Rastreamento do Patrimônio

### 2.2 Objetivo
Permitir à Secretaria CCT consultar, em um mapa interativo, o último local de repasse reportado por GPS de cada projetor ativo, a partir da saída da Secretaria do CCT.

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
- Existe ao menos um projetor com status **em_uso** ou **em_repasse** no sistema.

---

## 5. Fluxo Principal (cenário de sucesso)

**P1.** A Secretaria CCT seleciona a opção **Mapa de Rastreamento** no menu lateral do sistema.

**P2.** O sistema exibe o mapa interativo com marcadores posicionados nas últimas coordenadas registradas de cada projetor fora da secretaria.

**P3.** A Secretaria CCT aplica filtros por status: **Todos**, **Em Uso** ou **Em Repasse**. **[A1]**

**P4.** O sistema atualiza a exibição dos marcadores conforme o filtro selecionado.

**P5.** A Secretaria CCT clica no marcador de um projetor. **[A2]**

**P6.** O sistema exibe o painel lateral com: Código do Projetor, Professor Responsável Atual, Última Localização, Sala de Aula Informada e Horário do Último Repasse.

**P7.** O caso de uso é encerrado.

---

## 6. Fluxos Alternativos

**A1. Nenhum resultado para o filtro selecionado**

**A1.1** No passo **P4**, o sistema não encontra projetores para o status filtrado.

**A1.2** O sistema exibe a mensagem: *"Nenhum projetor encontrado para o filtro selecionado."*

**A1.3** O sistema mantém o mapa visível sem marcadores.

---

**A2. Acessar prontuário técnico a partir do mapa**

**A2.1** No passo **P6**, a Secretaria CCT seleciona **Ver Prontuário Completo**.

**A2.2** O sistema navega para o caso de uso **CDU-08 — Consultar Prontuário Técnico via QR Code** do projetor correspondente.

---

## 7. Fluxos de Exceção

Não se aplica. A operação é somente leitura e não envolve persistência.

---

## 8. Pós-condições

Não há persistência. A consulta é somente leitura e não altera o estado de nenhum dado no sistema.

---

## 9. Interface Visual

### IV1. Mapa Interativo de Rastreamento

Nesta tela, a Secretaria CCT visualiza a localização atual de todos os projetores ativos.

| Elemento | Comportamento |
|---|---|
| Mapa interativo | Exibido com zoom centralizado no campus da UNIFOR; permite navegação por arrastar e ampliar. |
| Marcadores de projetores | Cada marcador representa um projetor fora da secretaria; cor indica o status: **Azul** = Em Uso, **Laranja** = Em Repasse. |
| Filtro por status | Botões de alternância no topo: **Todos**, **Em Uso**, **Em Repasse**; atualiza marcadores em tempo real. |
| Painel lateral | Aberto ao clicar em um marcador; exibe: Código, Professor Responsável, Última Localização, Sala de Aula e Horário do Último Repasse. |
| Botão **Ver Prontuário Completo** | Exibido no painel lateral; redireciona para o CDU-08. |
| Atualização em tempo real | O mapa é atualizado via WebSocket conforme novos repasses são registrados. |

---

## 10. Referências

- [CDU-07 — Transferir Posse de Projetor via QR Code](CDU-07-Transferir-Posse-de-Projetor-via-QR-Code.md) (origem dos dados de GPS exibidos no mapa)
- [CDU-08 — Consultar Prontuário Técnico via QR Code](CDU-08-Consultar-Prontuario-Tecnico-via-QR-Code.md)
- Funcionalidades: **F4.1**
