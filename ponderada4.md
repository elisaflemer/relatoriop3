# Ponderada 4

A segurança da informação é um aspecto fundamental em qualquer projeto tecnológico, especialmente em iniciativas que lidam com a coleta, processamento e compartilhamento de dados sensíveis. Continuando nossa jornada na disciplina de Segurança da Informação, esta segunda atividade ponderada expande nosso escopo para identificar e classificar vulnerabilidades em um contexto específico: nosso projeto de ecovigilância com sensores de poluição, ruído e radiação solar.↳

Nosso projeto, um metaprojeto de ecovigilância, visa monitorar e analisar os níveis de poluição ambiental, ruído e radiação solar. Para isso, utilizamos sensores conectados que coletam dados em tempo real, os quais são publicados em um broker MQTT local ou clusterizado do HiveMQ. Essa arquitetura não apenas permite a coleta eficiente de informações ambientais, mas também estabelece uma base para análises e tomadas de decisão informadas.
Entretanto, nosso projeto não está imune a vulnerabilidades. Nesta atividade, nosso foco está na identificação e classificação dessas vulnerabilidades inerentes aos ambientes de ecovigilância. A análise de riscos será realizada através de uma matriz que considera a probabilidade e o impacto de cada vulnerabilidade, permitindo-nos entender melhor as potenciais ameaças e priorizar ações para mitigá-las.
Dessa forma, esta atividade não apenas aprofunda nosso entendimento sobre segurança da informação, mas também destaca a importância de considerar a proteção de dados em todas as fases de um projeto, desde a concepção até a implementação e operação. Vamos agora explorar as vulnerabilidades em nosso ambiente de ecovigilância e desenvolver estratégias para garantir a integridade, confidencialidade e disponibilidade dos dados coletados.

## Ataque de força bruta

Um ataque de força bruta é uma técnica utilizada por invasores para tentar obter acesso não autorizado a sistemas ou contas através da tentativa repetida de combinações de credenciais, como nomes de usuário e senhas. Essa abordagem, embora simples, pode ser extremamente eficaz quando não são implementadas medidas de segurança adequadas.

### O que é?
Em um ataque de força bruta às credenciais locais ou do HiveMQ, um invasor tenta acessar um sistema ou broker MQTT como o HiveMQ através de uma abordagem de tentativa e erro, testando diversas combinações de nomes de usuário e senhas. O objetivo é encontrar as credenciais corretas que concedam acesso não autorizado ao sistema alvo.

### Passo a passo do ataque:
1. Coleta de Informações: O primeiro passo para o invasor é coletar informações sobre o sistema alvo, como possíveis nomes de usuário padrão, políticas de senha, e quaisquer outras informações disponíveis que possam facilitar o ataque.
2. Geração de Combinações: Com base nas informações coletadas, o invasor gera uma lista de combinações possíveis de nomes de usuário e senhas. Isso pode ser feito manualmente ou automatizado através de ferramentas específicas de ataque de força bruta.
3. Teste de Credenciais: Utilizando uma ferramenta automatizada ou script, o invasor começa a testar cada combinação de nome de usuário e senha na tentativa de encontrar as credenciais corretas que concedam acesso ao sistema alvo.
4. Iteração e Persistência: O invasor continua iterando sobre as combinações de credenciais até que encontre um conjunto válido. Em alguns casos, pode até mesmo tentar técnicas de persistência, como tentativas repetidas ao longo do tempo, para evitar bloqueios de segurança temporários.

### Mitigação do ataque:
Para mitigar um ataque de força bruta às credenciais locais ou do HiveMQ, são necessárias várias medidas de segurança:
Políticas de Senha Fortes: Implementar políticas de senha fortes que exijam combinações de caracteres complexas, como letras maiúsculas e minúsculas, números e caracteres especiais.

- Bloqueio de Conta após Tentativas Malsucedidas: Configurar sistemas para bloquear contas temporariamente após um número específico de tentativas de login malsucedidas, a fim de evitar tentativas repetidas de força bruta.
- Monitoramento de Atividades Suspeitas: Implementar sistemas de monitoramento de atividades que alertem os administradores sobre tentativas de login suspeitas ou padrões incomuns de acesso.
- Autenticação de Dois Fatores (2FA): Utilizar autenticação de dois fatores sempre que possível, adicionando uma camada extra de segurança além das credenciais de login padrão.

Ao adotar essas medidas de segurança, é possível mitigar significativamente o risco de um ataque de força bruta e proteger as credenciais locais ou do HiveMQ contra acesso não autorizado por parte de invasores.
