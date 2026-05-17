# Especificação de Requisitos Não Funcionais

## 1. Requisitos de Produto

### 1.1. Eficiência de Desempenho

#### 1.1.1. Performance de Reconhecimento
O tempo de resposta entre a captura da imagem no balcão e a identificação do docente não deve ultrapassar 2.5 segundos.

### 1.2. Privacidade

#### 1.2.1. Conformidade LGPD (Privacidade Privilegiada)
O banco de dados jamais armazenará imagens (arquivos .jpg/.png) das faces dos docentes. O sistema armazenará apenas a representação matemática (matriz de *embeddings*), impossibilitando a engenharia reversa para reconstrução do rosto.

### 1.3. Capacidade de Interação (UX + usabilidade + acessibilidade)

#### 1.3.1. Usabilidade (Fricção Zero)
O repasse via smartphone deverá ocorrer por meio da leitura do QR Code disponibilizado no projetor. Após a leitura, o docente será direcionado para uma URL de autenticação com reconhecimento facial, permitindo a transferência de custódia diretamente pelo navegador do dispositivo móvel, de forma rápida e segura.
