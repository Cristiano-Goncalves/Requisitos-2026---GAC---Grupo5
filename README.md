# Requisitos-2026---GAC---Grupo5

📽️ ProjecTrack: Cadeia de Custódia e Gestão Inteligente de Ativos

Projeto concebido para a disciplina de Engenharia de Requisitos / Modelagem de Sistemas. Focado em resolver o problema de rastreabilidade logística e responsabilização de equipamentos (projetores) no Centro de Ciências Tecnológicas (CCT) da UNIFOR.

📌 O Problema

Atualmente, o controle de empréstimos de projetores sofre com registros manuais vulneráveis. O maior gargalo ocorre nos repasses informais de corredor (quando o Prof. A repassa o equipamento ao Prof. B). A secretaria perde o rastro do ativo, impossibilitando a responsabilização correta em caso de extravios ou avarias, gerando prejuízos institucionais.

💡 A Solução

O ProjecTrack substitui os cadernos por uma Cadeia de Custódia Digital Inquebrável utilizando:

Inteligência Artificial (Biometria): Check-out instantâneo via reconhecimento facial no balcão da secretaria.

Rastreamento Colaborativo (Mobile + GPS): Transferência de responsabilidade nos corredores através da leitura de QR Codes usando o smartphone do próprio docente.

Auditoria de Integridade: Check-in condicionado a um checklist de hardware obrigatório.

🚀 Principais Funcionalidades

👤 Check-out Fricção Zero: Identificação biométrica do docente na secretaria em menos de 3s. O sistema não salva fotos, apenas vetores matemáticos, garantindo conformidade com a LGPD.

🤝 Repasse Oficial (QR Code): O Docente B lê a etiqueta do projetor com o celular. O sistema transfere a posse e isenta o Docente A.

📍 Mapa de Calor & GPS: Durante o repasse, as coordenadas do celular atualizam um mapa do campus ao vivo no painel da secretaria (via Leaflet.js).

⚖️ Termos com Peso Jurídico: Disparo de e-mails automáticos ("Termo de Responsabilidade" e "Recibo de Quitação") a cada alteração de status.

⚠️ Bloqueio por Avaria: Formulário de devolução obriga a conferência de lentes e cabos. Falhas geram ocorrências que bloqueiam o docente para novos empréstimos até regularização.

🛠️ Stack Tecnológica (Proposta)

Backend & Inteligência Artificial

Python (FastAPI): Alta performance e requisições assíncronas.

OpenCV / DeepFace: Processamento de visão computacional e extração da matriz facial.

PostgreSQL + pgvector: Armazenamento relacional e busca vetorial de similaridade para a biometria em milissegundos.

Frontend (Secretaria)

React.js & TailwindCSS: Interface responsiva e design system moderno.

Leaflet.js: Renderização do mapa do campus interativo.

Chart.js: Gráficos de Business Intelligence e disponibilidade.

Mobile & Físico

Integração Webview/Nativa: Módulo inserido no app institucional existente.

Etiquetas Físicas: Vinil destrutível com impressão de QR Code.
