# Especificação de Regras de Negócio

## Regras de Negócio

### RN1. Responsabilidade Intransferível sem Sistema

* Identificador: RN1
* Regra: Se o Docente A repassar fisicamente o equipamento ao Docente B sem a leitura do QR Code no aplicativo, a responsabilidade financeira e administrativa por perdas e danos permanece 100% sobre o Docente A.
* Critério verificável: O sistema deve manter o ativo sob a custódia e responsabilidade legal do Docente A até que uma nova leitura de QR Code seja efetivada e registrada com sucesso.

### RN2. Bloqueio por Pendência

* Identificador: RN2
* Regra: Um docente com uma "Ocorrência de Avaria" em aberto fica bloqueado pelo sistema para realizar novos empréstimos de qualquer equipamento até a regularização com a coordenação.
* Critério verificável: Ao tentar iniciar um fluxo de empréstimo ou checkout, o sistema deve validar o status do docente e impedir o avanço caso conste alguma pendência ativa de avaria.

### RN3. Cadastro de professor exige dados mínimos válidos

* Identificador: RN3
* Regra: O cadastro de professor somente pode ser concluído quando os campos obrigatórios (nome completo, código de acesso/matrícula UNIFOR e CPF) estiverem preenchidos e válidos.
* Critério verificável: O sistema deve impedir o salvamento do registro quando houver campo obrigatório ausente ou em formato inválido.

### RN4. CPF e biometria do professor devem ser únicos

* Identificador: RN4
* Regra: Não pode existir mais de um cadastro ativo de professor com o mesmo CPF ou com a mesma assinatura biométrica facial (matriz de *embeddings*). Além disso, o cadastro biométrico deve ser realizado uma única vez.
* Critério verificável: O sistema deve realizar uma busca na base de dados antes de finalizar o registro; ao detectar CPF ou vetor facial já cadastrado, bloqueia o novo cadastro e orienta a regularização.

### RN5. Sincronização obrigatória de contexto de aula

* Identificador: RN5
* Regra: O sistema deve consumir os dados de horários, disciplinas e salas da UNIFOR no momento da operação para associar a localização prevista do projetor ao professor.
* Critério verificável: O sistema deve cruzar o horário atual com a grade horária importada do docente e sugerir de forma automática e obrigatória o bloco e a sala de destino no ato do empréstimo.

### RN6. Prontuário do projetor deve registrar histórico técnico

* Identificador: RN6
* Regra: Todo projetor cadastrado deve possuir um registro de inventário contendo o ano de fabricação e uma linha do tempo dedicada a manutenções e consertos.
* Critério verificável: Ao registrar uma manutenção ou conserto na secretaria, o sistema deve salvar e persistir as informações de forma cumulativa atreladas ao código de identificação do ativo.

### RN7. Leitura do QR Code deve filtrar dados por perfil de acesso

* Identificador: RN7
* Regra: A exibição de dados ao acessar o QR Code do projetor deve variar de acordo com o tipo de usuário logado. O professor visualiza apenas ações de posse e transferência, enquanto a secretaria visualiza o prontuário técnico completo.
* Critério verificável: O sistema deve validar as permissões do perfil autenticado antes de renderizar a página do QR Code, omitindo o histórico de manutenções e informações internas para o perfil de professor.
