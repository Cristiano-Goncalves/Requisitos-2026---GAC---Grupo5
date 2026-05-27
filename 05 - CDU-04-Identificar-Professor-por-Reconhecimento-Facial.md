# CDU-04 — Identificar Professor por Reconhecimento Facial

---

## 1. Histórico de Versões

| Data | Versão | Descrição | Autor |
|---|---|---|---|
| 03/05/2026 | 1.0 | Criação do CDU-04 | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |
| 24/05/2026 | 1.1 | Inclusão da seção IV1 com tabela de elementos visuais | Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante |

---

## 2. Identificação do Caso de Uso

### 2.1 Nome
Identificar Professor por Reconhecimento Facial

### 2.2 Objetivo
Permitir a identificação automática de um professor por meio da comparação do rosto capturado pela webcam com os vetores biométricos armazenados na base de dados.

### 2.3 Tipo
Abstrato. Instanciado pelo **CDU-03** (Check-out) e pelo **CDU-07** (Transferência de Posse via QR Code).

---

## 3. Atores

### 3.1 Primário
Não se aplica (caso de uso abstrato).

### 3.2 Secundários
Não se aplica.

---

## 4. Pré-condições

- O caso de uso deve ter sido instanciado por outro caso de uso.
- A webcam deve estar operacional e com boa iluminação frontal.
- Existem *embeddings* cadastrados na base de dados para comparação.

---

## 5. Fluxo Principal (cenário de sucesso)

**P1.** O sistema ativa o feed ao vivo da webcam e exibe o guia oval de posicionamento na cor **cinza**.

**P2.** O sistema inicia o processo de detecção facial em tempo real.

**P3.** O sistema detecta o rosto dentro do guia oval e altera a cor do guia para **verde**.

**P4.** O sistema captura o frame com melhor qualidade e extrai o vetor numérico (*embedding*) do rosto.

**P5.** O sistema envia o *embedding* ao serviço de reconhecimento e realiza busca de similaridade (distância de cosseno) na base de dados. **[A1][A2][E1]**

**P6.** O sistema identifica o professor com grau de confiança acima do limiar configurado.

**P7.** O sistema retorna ao caso de uso chamador com os dados do professor identificado: Nome, Matrícula e Departamento.

**P8.** O caso de uso é encerrado.

---

## 6. Fluxos Alternativos

**A1. Baixa confiança no reconhecimento**

**A1.1** No passo **P5**, o sistema encontra correspondência com grau de confiança abaixo do limiar máximo, mas acima do limiar mínimo.

**A1.2** O sistema exibe o card do candidato mais provável com a pergunta: *"É este professor?"* e as opções **Sim** e **Não, buscar outro**.

**A1.3** Se o usuário confirmar, o sistema segue para o passo **P7**.

**A1.4** Se o usuário negar, o sistema retorna ao passo **P2**.

---

**A2. Múltiplos rostos detectados**

**A2.1** No passo **P2** ou **P3**, o sistema detecta mais de um rosto no campo da câmera.

**A2.2** O sistema altera o guia oval para **vermelho** e exibe o aviso: *"Múltiplos rostos detectados. Certifique-se de que apenas o professor está em frente à câmera."*

**A2.3** O sistema retorna ao passo **P2**.

---

## 7. Fluxos de Exceção

**E1. Rosto não reconhecido na base**

**E1.1** No passo **P5**, o sistema não encontra nenhuma correspondência acima do limiar mínimo.

**E1.2** O sistema exibe a mensagem: *"Rosto não reconhecido. Use a identificação manual."*

**E1.3** O sistema retorna ao caso de uso chamador sinalizando falha no reconhecimento automático.

---

## 8. Pós-condições

- Dados do professor identificado disponíveis para o caso de uso chamador.

---

## 9. Interface Visual

### IV1. Tela de Reconhecimento Facial

Nesta tela, o rosto do professor é capturado e comparado com a base de *embeddings*. A tabela abaixo detalha os elementos visuais e seus comportamentos.

| Elemento | Comportamento |
|---|---|
| Feed ao vivo da webcam | Exibido em tempo real assim que a tela é carregada. |
| Guia oval de posicionamento | **Cinza** = aguardando rosto; **Amarelo** = rosto fora do centro; **Verde** = rosto posicionado corretamente; **Vermelho** = múltiplos rostos detectados. |
| Indicador de status | Texto dinâmico abaixo do feed: *"Aguardando rosto..."*, *"Rosto detectado..."*, *"Identificando..."*. |
| Card de confirmação | Exibido após identificação bem-sucedida: nome, matrícula, departamento e foto do perfil. Contém botões **Sim** e **Não, buscar outro**. |
| Campo de busca manual | Disponível no caso de uso chamador como alternativa; não pertence a esta tela. |

---

## 10. Referências

- [CDU-03 — Realizar Empréstimo de Projetor](CDU-03-Realizar-Emprestimo-de-Projetor.md) (caso de uso chamador)
- [CDU-07 — Transferir Posse de Projetor via QR Code](CDU-07-Transferir-Posse-de-Projetor-via-QR-Code.md) (caso de uso chamador)
- Funcionalidades: **F1.1**
- Requisitos Não Funcionais: RNF — Performance de Reconhecimento (≤ 2,5 s), RNF — Conformidade LGPD
