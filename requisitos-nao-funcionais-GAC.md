# Especificação de Requisitos Não Funcionais

## 1. Requisitos de Produto

### 1.1. Eficiência de Desempenho

#### 1.1.1. Comportamento temporal
O tempo de resposta entre a captura da imagem no balcão e a identificação do docente não deve ultrapassar 2.5 segundos.

### 1.2. Privacidade

#### 1.2.1. Necessidade
O banco de dados jamais armazenará imagens (arquivos .jpg/.png) das faces dos docentes. O sistema armazenará apenas a representação matemática (matriz de *embeddings*), impossibilitando a engenharia reversa para reconstrução do rosto.

### 1.3. Capacidade de Interação (UX + usabilidade + acessibilidade)

#### 1.3.1. Operabilidade
O repasse pelo smartphone (RF04) deve utilizar o token de autenticação (SSO) já ativo no *Unifor Mobile*, sem exigir que o docente digite login e senha novamente.
