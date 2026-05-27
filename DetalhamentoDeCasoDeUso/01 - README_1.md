# Especificação de Casos de Uso — ProjecTrack

> **Projeto:** ProjecTrack — Sistema de Gestão de Projetores do CCT/UNIFOR
> **Versão:** 1.2.0
> **Autores:** Iandeyara Farias, Cristiano Gonçalves, Myrla Rodrigues e Sophia Cavalcante
> **Última atualização:** 24/05/2026

---

## Índice

| ID | Nome | Tipo | Ator Primário | Frequência |
|---|---|---|---|---|
| [CDU-01](CDU-01-Cadastrar-Professor-e-Capturar-Biometria.md) | Cadastrar Professor e Capturar Biometria | Concreto | Secretaria CCT | Baixa |
| [CDU-02](CDU-02-Cadastrar-Projetor.md) | Cadastrar Projetor | Concreto | Secretaria CCT | Baixa |
| [CDU-03](CDU-03-Realizar-Emprestimo-de-Projetor.md) | Realizar Empréstimo de Projetor (Check-out) | Concreto | Secretaria CCT | Alta |
| [CDU-04](CDU-04-Identificar-Professor-por-Reconhecimento-Facial.md) | Identificar Professor por Reconhecimento Facial | Abstrato | — | Alta |
| [CDU-05](CDU-05-Realizar-Devolucao-de-Projetor.md) | Realizar Devolução de Projetor (Check-in) | Concreto | Secretaria CCT | Alta |
| [CDU-06](CDU-06-Registrar-Ocorrencia-de-Avaria.md) | Registrar Ocorrência de Avaria | Abstrato | — | Baixa |
| [CDU-07](CDU-07-Transferir-Posse-de-Projetor-via-QR-Code.md) | Transferir Posse de Projetor via QR Code | Concreto | Professor | Média |
| [CDU-08](CDU-08-Consultar-Prontuario-Tecnico-via-QR-Code.md) | Consultar Prontuário Técnico via QR Code | Concreto | Secretaria / Coordenação CCT | Média |
| [CDU-09](CDU-09-Visualizar-Mapa-de-Rastreamento.md) | Visualizar Mapa de Rastreamento do Patrimônio | Concreto | Secretaria CCT | Alta |
| [CDU-10](CDU-10-Bloquear-ou-Desbloquear-Professor.md) | Bloquear ou Desbloquear Professor | Concreto | Coordenação CCT | Baixa |

---

## Rastreabilidade — CDU × Funcionalidades × Regras de Negócio

| Caso de Uso | Funcionalidades | Regras de Negócio |
|---|---|---|
| CDU-01 | F5.1 | RN3, RN4 |
| CDU-02 | F5.2 | RN5 |
| CDU-03 | F1.1, F1.2 | RN1, RN2 |
| CDU-04 | F1.1 | — |
| CDU-05 | F3.1, F3.2 | RN5 |
| CDU-06 | F3.2 | RN1, RN2, RN5 |
| CDU-07 | F2.1, F2.2, F5.3 | RN1, RN2 |
| CDU-08 | F5.4 | RN5, RN6 |
| CDU-09 | F4.1 | — |
| CDU-10 | F3.2 | RN2 |

---

## Dependências entre Casos de Uso

```
CDU-03 ──inclui──► CDU-04
CDU-05 ──inclui──► CDU-06
CDU-07 ──inclui──► CDU-04
CDU-08 ──estende─► PE1 (Registrar Nova Manutenção)
```
