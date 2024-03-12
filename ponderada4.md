# Avaliação de Segurança do Projeto

A segurança da informação é um aspecto fundamental em qualquer projeto tecnológico, especialmente em iniciativas que lidam com a coleta, processamento e compartilhamento de dados sensíveis. Continuando nossa jornada na disciplina de Segurança da Informação, esta segunda atividade ponderada expande nosso escopo para identificar e classificar vulnerabilidades em um contexto específico: nosso projeto de ecovigilância com sensores de poluição, ruído e radiação solar.↳

Nosso projeto, um metaprojeto de ecovigilância, visa monitorar e analisar os níveis de poluição ambiental, ruído e radiação solar. Para isso, utilizamos sensores conectados que coletam dados em tempo real, os quais são publicados em um broker MQTT local ou clusterizado do HiveMQ. Essa arquitetura não apenas permite a coleta eficiente de informações ambientais, mas também estabelece uma base para análises e tomadas de decisão informadas.

Entretanto, nosso projeto não está imune a vulnerabilidades. Nesta atividade, nosso foco está na identificação e classificação dessas vulnerabilidades inerentes aos ambientes de ecovigilância. A análise de riscos será realizada através de uma matriz que considera a probabilidade e o impacto de cada vulnerabilidade, permitindo-nos entender melhor as potenciais ameaças e priorizar ações para mitigá-las.

Dessa forma, esta atividade não apenas aprofunda nosso entendimento sobre segurança da informação, mas também destaca a importância de considerar a proteção de dados em todas as fases de um projeto, desde a concepção até a implementação e operação. Vamos agora explorar as vulnerabilidades em nosso ambiente de ecovigilância e desenvolver estratégias para garantir a integridade, confidencialidade e disponibilidade dos dados coletados.

<img width="3683" alt="Risk Matrix Template (Copy)" src="https://github.com/elisaflemer/relatoriop3/assets/99259251/1d54a49f-396f-41b6-8424-7cfc9e4d16f3">

## Ataque de força bruta

Um ataque de força bruta é uma técnica utilizada por invasores para tentar obter acesso não autorizado a sistemas ou contas através da tentativa repetida de combinações de credenciais, como nomes de usuário e senhas. Essa abordagem, embora simples, pode ser extremamente eficaz quando não são implementadas medidas de segurança adequadas.

### O que é?
Em um ataque de força bruta às credenciais locais ou do HiveMQ, um invasor tenta acessar um sistema ou broker MQTT como o HiveMQ através de uma abordagem de tentativa e erro, testando diversas combinações de nomes de usuário e senhas. O objetivo é encontrar as credenciais corretas que concedam acesso não autorizado ao sistema alvo.

### Passo a passo do ataque
1. Coleta de Informações: O primeiro passo para o invasor é coletar informações sobre o sistema alvo, como possíveis nomes de usuário padrão, políticas de senha, e quaisquer outras informações disponíveis que possam facilitar o ataque.
2. Geração de Combinações: Com base nas informações coletadas, o invasor gera uma lista de combinações possíveis de nomes de usuário e senhas. Isso pode ser feito manualmente ou automatizado através de ferramentas específicas de ataque de força bruta.
3. Teste de Credenciais: Utilizando uma ferramenta automatizada ou script, o invasor começa a testar cada combinação de nome de usuário e senha na tentativa de encontrar as credenciais corretas que concedam acesso ao sistema alvo.
4. Iteração e Persistência: O invasor continua iterando sobre as combinações de credenciais até que encontre um conjunto válido. Em alguns casos, pode até mesmo tentar técnicas de persistência, como tentativas repetidas ao longo do tempo, para evitar bloqueios de segurança temporários.

### Mitigação do ataque
Para mitigar um ataque de força bruta às credenciais locais ou do HiveMQ, são necessárias várias medidas de segurança:
Políticas de Senha Fortes: Implementar políticas de senha fortes que exijam combinações de caracteres complexas, como letras maiúsculas e minúsculas, números e caracteres especiais.

- Bloqueio de Conta após Tentativas Malsucedidas: Configurar sistemas para bloquear contas temporariamente após um número específico de tentativas de login malsucedidas, a fim de evitar tentativas repetidas de força bruta.
- Monitoramento de Atividades Suspeitas: Implementar sistemas de monitoramento de atividades que alertem os administradores sobre tentativas de login suspeitas ou padrões incomuns de acesso.
- Autenticação de Dois Fatores (2FA): Utilizar autenticação de dois fatores sempre que possível, adicionando uma camada extra de segurança além das credenciais de login padrão.

Ao adotar essas medidas de segurança, é possível mitigar significativamente o risco de um ataque de força bruta e proteger as credenciais locais ou do HiveMQ contra acesso não autorizado por parte de invasores.

## SQL Injection

SQL Injection é uma técnica de ataque comum em que um invasor insere código SQL malicioso em campos de entrada de um aplicativo web para manipular a execução de comandos SQL pela aplicação.

### Passo a passo do ataque

1. Identificação dos Pontos de Injeção: No nosso dashboard do Metabase, os campos de entrada que permitem aos usuários filtrar dados, como região ou tipo de sensor, podem ser identificados como pontos de injeção. Especial atenção deve ser dada se esses campos permitirem entrada de texto livre em vez de opções predefinidas.
2. Inserção de Código Malicioso: O atacante pode inserir consultas SQL manipuladas nos campos de entrada do dashboard. Por exemplo, ao inserir uma região maliciosa ou um tipo de sensor manipulado, o invasor pode tentar executar comandos SQL prejudiciais.
3. Execução de Consultas Manipuladas: Quando os dados manipulados são submetidos, a aplicação pode inadvertidamente executar as consultas SQL inseridas pelo invasor. Isso pode levar à exposição de dados sensíveis, manipulação de dados ou até mesmo comprometer a integridade do sistema.

### Mitigação do ataque
- Utilização de Parâmetros Preparados: Implementar consultas parametrizadas ou parâmetros preparados no Metabase, em vez de concatenar diretamente os valores dos campos de entrada em consultas SQL, pode prevenir a execução de código malicioso.
- Validação e Sanitização de Entradas: Validar e sanitizar os dados inseridos pelos usuários nos campos de entrada do dashboard pode ajudar a prevenir a execução de consultas SQL maliciosas. Restrições de caracteres e o uso de listas de permissões podem ser implementados para aceitar apenas entradas específicas.
- Restrição de Privilégios de Banco de Dados: Limitar os privilégios de acesso do usuário do banco de dados somente ao necessário pode reduzir o impacto de um ataque de SQL injection. Por exemplo, restringir o acesso apenas a leitura nos bancos de dados utilizados pelo Metabase.

## DDoS

DDoS (Distributed Denial of Service) é um tipo de ataque cibernético em que múltiplos dispositivos, frequentemente distribuídos geograficamente, são coordenados para inundar um sistema alvo com tráfego de rede, sobrecarregando-o e tornando-o inacessível para usuários legítimos.

### Passo a passo do ataque

1. Identificação do Alvo: O dashboard do Metabase, utilizado para visualização dos dados coletados em nosso projeto de ecovigilância, pode ser identificado como o alvo do ataque DDoS. Este é um componente crítico para a análise e tomada de decisões baseadas nos dados ambientais coletados.
2. Inundação de Tráfego: Os atacantes lançam uma grande quantidade de tráfego malicioso em direção ao servidor onde o dashboard está hospedado. Esse tráfego pode ser gerado por uma rede de dispositivos comprometidos (botnets) ou por meio de técnicas de spoofing de IP.
3. Sobrecarga do Sistema: O volume massivo de tráfego recebido pelo servidor do dashboard do Metabase sobrecarrega os recursos de rede, CPU e/ou memória, tornando o sistema indisponível para os usuários legítimos. Como resultado, os usuários não conseguem acessar o dashboard para visualizar os dados ambientais.

### Mitigação do ataque

- Implementação de Filtros de Tráfego: Utilizar firewalls e sistemas de detecção de intrusões (IDS/IPS) para filtrar e bloquear o tráfego malicioso antes que ele atinja o servidor do dashboard do Metabase.
- Utilização de Serviços de Mitigação de DDoS: Contratar serviços de mitigação de DDoS de provedores especializados pode ajudar a proteger o servidor do dashboard contra ataques volumétricos, reduzindo o impacto do tráfego malicioso.
- Balanceamento de Carga e Escalabilidade: Implementar estratégias de balanceamento de carga e escalabilidade no ambiente de hospedagem do dashboard pode ajudar a distribuir o tráfego de forma mais equilibrada e lidar com picos de demanda durante um ataque DDoS.
- Monitoramento e Alertas: Estabelecer sistemas de monitoramento contínuo do tráfego de rede e da utilização dos recursos do servidor pode ajudar a identificar rapidamente um ataque DDoS em andamento e acionar alertas para uma resposta imediata.

## Man-In-The-Middle

O ataque Man-in-the-Middle (MitM) é uma técnica em que um atacante intercepta e potencialmente altera a comunicação entre dois dispositivos ou sistemas sem o conhecimento dos usuários legítimos. O atacante se posiciona entre a comunicação, podendo ler, modificar e até mesmo injetar novos dados na troca de informações.

### Passo a passo do ataque

1. Interceptação da Comunicação: O atacante se posiciona entre o cliente (usuário) e o servidor que hospeda o dashboard do Metabase. Isso pode ser realizado através de diversas técnicas, como ARP spoofing, DNS spoofing ou interceptação de redes sem fio não seguras.
2. Manipulação dos Dados: Uma vez que o atacante está intermediando a comunicação, ele pode ler, modificar ou até mesmo injetar dados na troca de informações entre o cliente e o servidor do dashboard. Isso pode incluir a visualização de dados sensíveis ou a inserção de informações falsas para enganar os usuários.
3. Redirecionamento de Tráfego: Em alguns casos, o atacante pode redirecionar o tráfego da comunicação legítima para seus próprios servidores controlados, onde os dados podem ser manipulados antes de serem encaminhados para o servidor real do dashboard. Isso pode ocorrer de forma transparente para o usuário, sem que ele perceba a alteração na comunicação.

### Mitigação do ataque

- Utilização de Conexões Criptografadas: Implementar conexões criptografadas, como SSL/TLS, entre o cliente e o servidor do dashboard do Metabase pode ajudar a proteger a integridade e confidencialidade das informações transmitidas, dificultando a interceptação e manipulação por parte do atacante.
- Verificação de Certificados SSL/TLS: Certificar-se de que os certificados SSL/TLS utilizados pelo servidor do dashboard são válidos e confiáveis. Isso ajuda a prevenir ataques de spoofing de certificados, onde o atacante pode tentar enganar os usuários com certificados falsificados.
- Monitoramento de Tráfego: Estabelecer sistemas de monitoramento de tráfego de rede pode ajudar a identificar atividades suspeitas, como alterações no padrão de comunicação ou tentativas de interceptação de pacotes, permitindo uma resposta rápida a possíveis ataques MitM.
- Educação e Conscientização dos Usuários: Educar os usuários sobre os riscos de ataques MitM e instruí-los sobre como identificar sinais de comunicação insegura ou suspeita pode ajudar a evitar que sejam vítimas desse tipo de ataque.

