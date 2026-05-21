# 📋 UC01 — Realizar Check-out de Equipamento via Biometria Facial

> **Sistema:** ProjecTrack — CCT/UNIFOR  
> **Módulo:** Atendimento Balcão — Secretaria  
> **Versão:** 1.0

---

## Índice

- [Descrição Sumária](#-descrição-sumária)
- [Atores](#-atores)
- [Pré-condições](#-pré-condições)
- [Fluxo Principal](#-fluxo-principal-de-interação)
- [Fluxos Alternativos e de Exceção](#-fluxos-alternativos-e-de-exceção)
- [Interfaces e Telas](#-especificação-das-interfaces-e-telas)
- [Regras de Negócio](#-regras-de-negócio)

---

## 📌 Descrição Sumária

Permite ao **Atendente da Secretaria** identificar o Professor por meio de **reconhecimento facial** na webcam do balcão, validar a elegibilidade do docente, cruzar o horário com a grade acadêmica da UNIFOR e efetivar o empréstimo do projetor — **sem assinaturas físicas em papel**.

---

## 👥 Atores

| Ator | Tipo | Responsabilidade |
|------|------|-----------------|
| Atendente da Secretaria | Principal | Opera o sistema no desktop do balcão |
| Professor | Secundário | Posiciona-se para captura biométrica e recebe o equipamento |

---

## ✅ Pré-condições

- [ ] O **Atendente** está autenticado no ProjecTrack com perfil `Secretaria`
- [ ] O **Professor** possui cadastro unificado com vetor biométrico (*embedding*) salvo no banco *(F5.1 / RN4)*
- [ ] O **projetor** está cadastrado com status `Disponível` no inventário *(F5.2)*
- [ ] A **webcam** do balcão está operacional

---

## 🔄 Fluxo Principal de Interação

```
[Atendente da Secretaria]        [Sistema ProjecTrack]            [Professor]
          |                               |                             |
          |-- 1. Inicia Check-out ------->|                             |
          |                               |-- 2. Ativa webcam/overlay ->|
          |                               |                             | [Posiciona-se]
          |                               |<- 3. Captura frame da face -|
          |                               |                             |
          |                               |-- 4. Processa vetor / ID -->|
          |                               |-- 5. Consulta restrições -->|
          |                               |-- 6. Consome grade UNIFOR ->|
          |<- 7. Formulário pré-preench. -|                             |
          |                               |                             |
          |-- 8. Seleciona ativo/confirma>|                             |
          |                               |-- 9. Atualiza custódia ---->|
          |                               |      gera termo (F1.2)      |
          |<- 10. Alerta de sucesso -------|---------------------------->| [Recebe e-mail]
```

### Detalhamento dos Passos

| # | Ator | Ação |
|---|------|------|
| 1 | Atendente | Clica em **"Iniciar Novo Empréstimo (Check-out)"** na tela inicial |
| 2 | Sistema | Abre modal, ativa webcam e exibe overlay com silhueta-guia de posicionamento |
| 3 | Professor | Posiciona-se em frente à câmera; sistema captura automaticamente o melhor frame |
| 4 | Sistema | Extrai *embeddings*, realiza varredura vetorial (PostgreSQL + pgvector) e identifica o docente em **< 2,5 s** *(Req. 1.1.1)* |
| 5 | Sistema | Valida pendências ou bloqueios por avaria em aberto *(RN2)* |
| 6 | Sistema | Consome API UNIFOR e cruza horário atual com a grade para identificar disciplina, bloco e sala *(RN5)* |
| 7 | Sistema | Preenche automaticamente: Nome, Matrícula, CPF (mascarado), Bloco e Sala de Destino |
| 8 | Atendente | Digita ou bipa o código patrimonial do projetor e clica em **"Confirmar e Emitir Termo"** |
| 9 | Sistema | Altera status para `Emprestado`, vincula custódia ao CPF *(RN1)*, grava log imutável e envia Termo Digital por e-mail *(F1.2)* |
| 10 | Sistema | Exibe mensagem de sucesso e limpa o formulário |

---

## ⚠️ Fluxos Alternativos e de Exceção

### FA01 — Ajuste Manual de Sala/Destino
> **Ativado no Passo 7** — professor vai para sala diferente da prevista na grade

1. Atendente observa o campo **"Sala de Destino"** preenchido automaticamente
2. Clica em **"Alterar Destino"**
3. Sistema exibe *dropdown* com todos os blocos e salas do CCT
4. Atendente seleciona a sala correta informada pelo professor
5. ↩️ Fluxo retorna ao **Passo 8**

---

### FE01 — Professor com Pendências Ativas
> **Ativado no Passo 5** — docente possui avaria em aberto no prontuário *(RN2)*

1. Sistema **bloqueia o fluxo** e exibe alerta crítico:

   ```
   ⛔ BLOQUEIO - RN2: Docente possui pendência de avaria em aberto com a Coordenação do CCT.
   ```

2. Botão de confirmação é ocultado; apenas **"Cancelar Operação"** permanece visível
3. Atendente informa o docente verbalmente e clica em **"Cancelar"**
4. 🚫 Caso de uso encerrado **sem efetuar o empréstimo**

---

### FE02 — Biometria Não Identificada
> **Ativado no Passo 4** — similaridade abaixo do limiar mínimo de confiança

1. Sistema exibe na modal:
   ```
   Não foi possível identificar o docente automaticamente.
   Tentar novamente ou Buscar por Matrícula?
   ```
2. Atendente clica em **"Buscar por Matrícula"**
3. Sistema abre campo de texto para digitação da matrícula
4. ✅ Dados válidos → avança para o **Passo 5** | ❌ Inválidos → operação abortada

---

## 🖥️ Especificação das Interfaces e Telas

### Painel de Check-out — Desktop (Secretaria)

---

#### Componente 01 — Barra Superior de Contexto Acadêmico

| Elemento | Valor |
|----------|-------|
| Título | `ProjecTrack // CCT - Módulo de Atendimento Balcão` |
| Breadcrumb | `Home > Empréstimos > Check-out Biométrico` |
| Metadados | Turno atual em tempo real · ex: `Manhã [M1/M2]` · Data e Hora |

---

#### Componente 02 — Área de Identificação (Webcam)

| Elemento | Descrição |
|----------|-----------|
| Container de Vídeo | Proporção 16:9 com feed em tempo real da webcam do balcão |
| Overlay de Detecção | Moldura oval: 🔴 piscando (rosto fora de foco) → 🟢 sólido (captura confirmada) |
| Barra de Progresso | Completada em 2,5 s com texto: *"Processando assinaturas matemáticas faciais (Vetor Embeddings)..."* |

---

#### Componente 03 — Formulário Dinâmico de Custódia
> 🔒 Somente leitura após reconhecimento

| Campo | Comportamento |
|-------|--------------|
| Docente Identificado | Input desabilitado — exibe nome completo em **negrito** |
| Vínculo Institucional | Input desabilitado — exibe matrícula UNIFOR |
| CPF do Responsável | Mascarado por LGPD — ex: `***.452.***-**` |
| Destino do Ativo | Pré-carregado — ex: `Bloco K - Sala 04` + botão `[Alterar Local manualmente]` |

> 🔵 **Alerta de sincronização:** *"Grade UNIFOR sincronizada com sucesso para este horário."*

---

#### Componente 04 — Entrada do Patrimônio (Ativo)

| Elemento | Descrição |
|----------|-----------|
| Destaque Visual | Contorno azul brilhante para foco do atendente |
| Input | Focado por padrão — aceita teclado ou leitura óptica (código de barras / QR Code) |
| Status em tempo real | `✔ Ativo PRJ-1042 localizado — Modelo: Epson X39 (Status: Disponível)` |

---

#### Componente 05 — Rodapé de Ações

| Botão | Posição | Estilo | Comportamento |
|-------|---------|--------|---------------|
| Cancelar Operação | Esquerda | 🔴 Vermelho | Reseta o formulário e desativa a webcam sem salvar |
| Efetivar Check-out & Disparar Termo Digital | Direita | 🟢 Verde | **Desabilitado** por padrão; ativa após ID de projetor válido |

---

## 📏 Regras de Negócio

| Regra | Descrição | Ponto de Aplicação |
|-------|-----------|-------------------|
| **RN1** | Custódia legal vinculada ao CPF do docente | Passo 9 |
| **RN2** | Bloqueio automático por pendência de avaria em aberto | Passo 5 — validação no backend antes de renderizar o formulário |
| **RN4** | Unicidade facial: um vetor deve corresponder a exatamente um CPF no pgvector | Passo 4 |
| **RN5** | Sincronização com grade acadêmica UNIFOR baseada na hora da requisição | Passo 6 — busca assíncrona que injeta destino no Componente 03 |
