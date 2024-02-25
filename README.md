# Ataques a MQTT | ponderada 3

No mundo interconectado de hoje, a Internet das Coisas (IoT) desempenha um papel crucial na otimização de processos e na melhoria da qualidade de vida. No entanto, à medida que o número de dispositivos conectados continua a crescer exponencialmente, surgem preocupações significativas sobre a segurança desses dispositivos e das redes que os interligam. Nesse contexto, o protocolo MQTT (Message Queuing Telemetry Transport) destaca-se como uma das tecnologias fundamentais para a comunicação entre dispositivos IoT, oferecendo eficiência e escalabilidade. No entanto, sua ubiquidade também o torna um alvo atrativo para potenciais ataques cibernéticos.

O presente relatório apresenta os resultados e as análises obtidas durante o laboratório realizado no módulo 9 do curso de Engenharia da Computação no Inteli. O objetivo principal deste laboratório foi explorar mais a fundo o funcionamento do protocolo MQTT, compreender suas vulnerabilidades e simular ataques direcionados aos princípios fundamentais da segurança da informação: Confidencialidade, Integridade e Disponibilidade (CIA).

Para atingir esse objetivo, foi configurado um ambiente de laboratório consistindo de um broker local utilizando o software Mosquitto e implementado em Python. A partir desse ambiente controlado, foram realizadas diversas simulações de ataques, visando comprometer os pilares de segurança da informação. As análises detalhadas desses ataques e suas implicações são apresentadas ao longo deste relatório, fornecendo insights valiosos sobre as potenciais ameaças enfrentadas por sistemas IoT que utilizam o protocolo MQTT.

Ao compreender as vulnerabilidades existentes e os possíveis cenários de ataque, os profissionais de segurança da informação podem desenvolver estratégias mais eficazes para proteger infraestruturas IoT contra ameaças cibernéticas. Este relatório visa contribuir para o avanço do conhecimento e da prática em cibersegurança, destacando a importância de uma abordagem proativa na proteção de sistemas críticos e na preservação da confiança digital.

## O protocolo MQTT

O protocolo MQTT (Message Queuing Telemetry Transport) é um protocolo leve de mensagens projetado para dispositivos com recursos limitados de rede e computação, comumente utilizado em ambientes de Internet das Coisas (IoT). Desenvolvido pela IBM na década de 1990, o MQTT se baseia no protocolo TCP/IP (Transmission Control Protocol/Internet Protocol) para garantir a entrega confiável das mensagens entre dispositivos conectados.

O MQTT opera em um modelo de publicação e assinatura, onde os dispositivos podem publicar mensagens em tópicos específicos e se inscrever para receber mensagens de tópicos de interesse. Isso permite uma comunicação assíncrona eficiente e escalável entre os dispositivos na rede IoT.

Em termos de segurança, o MQTT oferece diversos mecanismos padrão para proteger a integridade e a confidencialidade das comunicações entre os clientes e o broker MQTT:

- **Autenticação:** O MQTT suporta autenticação de clientes por meio de diversos mecanismos, incluindo nome de usuário e senha, certificados digitais e tokens de acesso. Esses mecanismos garantem que apenas clientes autorizados possam se conectar ao broker MQTT e participar da troca de mensagens.

- **TLS/SSL (Transport Layer Security/Secure Sockets Layer):** O MQTT pode ser configurado para operar sobre uma conexão segura TLS/SSL, proporcionando criptografia de ponta a ponta entre os clientes e o broker MQTT. Isso protege contra ataques de interceptação de dados e garante a confidencialidade das informações transmitidas.

- **Controle de Acesso:** O MQTT permite a definição de políticas de controle de acesso que regulam quais clientes têm permissão para publicar ou assinar determinados tópicos. Isso permite uma granularidade maior no gerenciamento de permissões e na proteção contra acessos não autorizados.

- **Mensagens Retidas e Limpeza de Sessão:** O MQTT oferece suporte a mensagens retidas, que são armazenadas no broker e entregues a novos clientes que se inscrevem em um tópico específico. Além disso, os clientes podem optar por limpar ou persistir sessões de cliente, controlando o comportamento do broker em relação à entrega de mensagens durante desconexões temporárias.

Esses mecanismos de segurança fornecidos pelo MQTT ajudam a mitigar diversos tipos de ameaças, incluindo ataques de autenticação, interceptação de dados e acesso não autorizado. No entanto, é importante ressaltar que a implementação correta e a configuração adequada desses mecanismos são fundamentais para garantir a robustez da segurança em um ambiente MQTT.

## Pré-requisitos e Setup

Antes de realizar as simulações de ataques, é necessário configurar um ambiente de teste adequado. Os seguintes passos devem ser seguidos:

**Instalação do Mosquitto Broker:** Baixe e instale o Mosquitto Broker em sua máquina local. Este software atuará como o broker MQTT para facilitar a comunicação entre os clientes MQTT.

**Configuração (mosquitto.conf):** É recomendado configurar o arquivo de configuração do Mosquitto (mosquitto.conf) de acordo com as necessidades do teste. Isso inclui a definição de políticas de segurança, autenticação de clientes, entre outros parâmetros relevantes.

**Inicialização do Mosquitto Broker:** Inicie o Mosquitto Broker com base nas configurações especificadas no arquivo de configuração. Isso estabelecerá o broker MQTT local para fins de teste.

Com o ambiente configurado, podemos proceder com as simulações de ataques nos pilares da segurança da informação.

## Ataques

### Confidencialidade (C)

Um possível ataque à confidencialidade no contexto do MQTT envolve a publicação de mensagens em um broker sem autenticação adequada. Existem duas abordagens comuns para esse tipo de ataque:

#### Anonimato no broker

O conceito de anonimato no broker MQTT refere-se à capacidade de permitir que qualquer dispositivo na rede local publique mensagens no broker sem a necessidade de autenticação. Para implementar essa funcionalidade, ativamos a opção `anonymous true` no arquivo de configuração (`mosquitto.conf`). Essa configuração compromete a confidencialidade das informações transmitidas, uma vez que não exige credenciais para realizar publicações. A seguir, são apresentados os passos para configurar o broker com anonimato e o comando para realizar publicações anônimas:

```bash
# Abra o arquivo de configuração do Mosquitto
sudo nano /etc/mosquitto/mosquitto.conf
```
Dentro do arquivo de configuração, adicione a seguinte linha
```bash
anonymous true
```

Salve e saia do editor (no Nano, pressione Ctrl + X, Y e Enter). Reinicie o serviço do Mosquitto para aplicar as alterações.

```bash
sudo service mosquitto restart
```

Para publicar anonimamente, baste rodar:
```bash
mosquitto_pub -h <endereco_do_broker> -t <topico> -m "Sua mensagem aqui"
```

#### Quebra de Senha de Baixa Entropia 

Em ambientes onde a autenticação é essencial, é crucial estar ciente dos riscos associados à quebra de senhas de baixa entropia utilizadas por clientes MQTT. A seguir, apresenta-se um passo a passo explicando como um atacante pode tentar explorar essa vulnerabilidade:

1. Identificação do Alvo: O atacante identifica o cliente MQTT que utiliza senhas de baixa entropia no ambiente alvo.

2. Obtenção de Hash de Senha: Caso o atacante tenha acesso ao hash da senha, ele pode tentar usar técnicas como a utilização de rainbow tables, que são tabelas precomputadas de hashes e suas correspondentes senhas, para encontrar uma correspondência.

3. Ataque de Força Bruta: Se o atacante não tiver acesso ao hash da senha, ele pode realizar um ataque de força bruta. Isso envolve tentar todas as combinações possíveis de caracteres, números e símbolos até encontrar uma senha válida.

4. Uso de Dicionários: O atacante pode empregar dicionários de palavras comuns, combinados com variações e padrões previsíveis, para tentar adivinhar senhas de baixa complexidade.

5. Monitoramento de Tráfego: Em alguns casos, o atacante pode monitorar o tráfego de rede em busca de senhas transmitidas sem criptografia, expondo vulnerabilidades de segurança.

### Integridade (I)

Explorar a integridade no MQTT pode envolver ataques direcionados às mensagens persistentes presentes em tópicos não consumidos por clientes inscritos. Esta técnica representa uma ameaça à integridade dos dados, e a seguir são detalhados os passos que um atacante pode seguir para comprometer a integridade do sistema:

1. Identificação de Tópicos com Mensagens Persistentes: O atacante identifica tópicos que contenham mensagens persistentes não consumidas por clientes inscritos. Isso pode ser realizado através de análise de tráfego ou, em alguns casos, pela subscrição ativa a tópicos relevantes.

2. Inserção de Informações Falsas: Aproveitando a presença de mensagens persistentes, o atacante introduz informações falsas nesses tópicos. Isso pode ser feito modificando diretamente o conteúdo das mensagens persistentes ou injetando novas mensagens falsas.

3. Espera pela Recebimento pelos Clientes Inscritos: O atacante aguarda que os clientes inscritos, em algum momento futuro, recebam e processem as mensagens manipuladas. Este passo é crítico para garantir que as informações falsas alcancem os sistemas dos usuários finais.

4. Distorsão das Informações Originais: Quando os clientes inscritos processam as mensagens manipuladas, as informações falsas são integradas ao sistema, distorcendo a integridade dos dados originais. Dependendo da natureza das informações, isso pode resultar em danos, confusão ou indução a ações indesejadas.

### Disponibilidade (A)

O MQTT, como muitos sistemas de mensagens, está sujeito a ataques de negação de serviço (DoS) que visam comprometer a disponibilidade do serviço. Aqui estão duas maneiras comuns de realizar esse tipo de ataque:

- **Ataque de Excesso de Mensagens:** Um atacante pode sobrecarregar o broker MQTT com um grande volume de mensagens, consumindo assim os recursos do sistema e impedindo que clientes legítimos publiquem ou recebam mensagens. Isso resultaria em uma interrupção no serviço e comprometeria a disponibilidade da rede MQTT.

- **Mensagens Muito Longas:** Enviar mensagens excessivamente longas para o broker MQTT pode causar uma sobrecarga nos recursos de processamento e armazenamento do sistema, resultando em uma diminuição da capacidade de resposta e, eventualmente, na interrupção do serviço para os clientes legítimos. Este tipo de ataque visa explorar as limitações do sistema e comprometer sua disponibilidade.

## Conclusão

A realização das simulações de ataques no ambiente MQTT proporcionou uma visão detalhada das vulnerabilidades e dos riscos associados à segurança da Internet das Coisas (IoT). Ao explorar os pilares fundamentais da segurança da informação - Confidencialidade, Integridade e Disponibilidade (CIA) - foi possível identificar potenciais pontos fracos e ameaças que podem comprometer a integridade e a segurança dos sistemas IoT baseados em MQTT.

Os ataques simulados demonstraram a importância de implementar medidas robustas de segurança para proteger os dispositivos IoT e as redes MQTT contra ameaças cibernéticas. Desde a configuração adequada do broker MQTT até a adoção de práticas de autenticação seguras e o controle de acesso granular, é crucial adotar uma abordagem proativa para mitigar os riscos e fortalecer a segurança dos sistemas IoT.

Além disso, os resultados das simulações destacaram a necessidade contínua de conscientização e educação sobre segurança cibernética, tanto entre os profissionais de TI quanto entre os usuários finais de dispositivos IoT. A segurança da informação deve ser uma preocupação central em todos os estágios do desenvolvimento e implementação de sistemas IoT, desde a concepção até a operação e manutenção contínuas.

Em última análise, este relatório oferece insights valiosos para a comunidade de segurança cibernética e destaca a importância de uma abordagem abrangente e multidisciplinar para proteger os sistemas IoT contra ameaças em constante evolução. Ao adotar uma postura proativa e implementar as melhores práticas de segurança, podemos garantir que a promessa da IoT seja realizada de forma segura e confiável, impulsionando a inovação e o progresso tecnológico de maneira sustentável.
