# Segurança em Redes de Computadores

> Este documento apresenta os conceitos fundamentais de segurança em redes de computadores, incluindo protocolos, técnicas e práticas recomendadas para proteger a integridade e a confidencialidade dos dados transmitidos.

---

## Sumário

- [Unidade 1: Princípios de Segurança da Informação](#unidade-1-principios-de-seguranca-da-informacao)
- [Unidade 2: Ameaças, Malwares e Controles](#unidade-2-ameacas-malwares-e-controles)
- [Unidade 3: Identificação de Vulnerabilidades](#unidade-3-identificacao-de-vulnerabilidades)
- [Unidade 4: Gerenciamento de Identidade e Acesso](#unidade-4-gerenciamento-de-identidade-e-acesso)

---

## Unidade 1: Princípios de Segurança da Informação

A **Segurança da Informação** é um conjunto de práticas, políticas, procedimentos e tecnologias projetadas para proteger os ativos de informação. Ela desempenha um papel vital na preservação da tríade CID.

Os dados podem estar vulneráveis devido à:

- Forma como são **armazenados**
- Forma como são **transferidos**
- Forma como são **processados**

---

### Tríade CID

Pilar de sustentação da segurança da informação:

- **Confidencialidade:** Garantir que a informação seja acessível apenas a pessoas autorizadas.
- **Integridade:** Garantir que a informação seja precisa e completa, e que não tenha sido alterada de forma não autorizada.
- **Disponibilidade:** Garantir que a informação esteja disponível sempre que necessário.

#### Outros Conceitos Fundamentais

1. Não repúdio
2. Autenticidade
3. Controle de acesso
4. Autenticação e Autorização
5. Princípio do menor privilégio (*Least Privilege*)
6. Gestão de riscos
7. Ativos de informação
8. Ameaças à segurança da informação
9. Ataques cibernéticos
10. Criptografia

---

### Relação Entre Vulnerabilidade, Ameaça e Risco

Trata-se de identificar as formas pelas quais os sistemas podem ser atacados através do mapeamento e da análise de:

- **Vulnerabilidade:** Uma fraqueza ou deficiência em um sistema que pode ser explorada por uma ameaça.
- **Ameaça:** Evento com o potencial de causar dano.
- **Risco:** A probabilidade de uma ameaça explorar uma vulnerabilidade e causar dano.

**Estratégias para gerenciamento de riscos:**

- Aceitar
- Mitigar
- Transferir
- Evitar

---

### Matriz de Risco

A matriz de risco é uma ferramenta visual utilizada para classificar e priorizar os riscos de segurança com base na combinação da **probabilidade** e do **impacto** da ocorrência.

<table>
  <tr>
    <th rowspan="2">Probabilidade</th>
    <th colspan="3">Valor</th>
  </tr>
  <tr>
    <th>Alta</th>
    <th>Média</th>
    <th>Baixa</th>
  </tr>
  <tr>
    <th>Alta</th>
    <td style="background-color:#ff5555; color: black;">Elevado</td>
    <td style="background-color:#ffe38a; color: black;">Alto</td>
    <td style="background-color:#ffff75; color: black;">Medio</td>
  </tr>
  <tr>
    <th>Média</th>
    <td style="background-color:#ffe38a; color: black;">Alto</td>
    <td style="background-color:#ffff75; color: black;">Medio</td>
    <td style="background-color:#9bd58a; color: black;">Baixo</td>
  </tr>
  <tr>
    <th>Baixa</th>
    <td style="background-color:#ffff75; color: black;">Medio</td>
    <td style="background-color:#9bd58a; color: black;">Baixo</td>
    <td style="background-color:#e5f0e2; color: black;">Desprezável</td>
  </tr>
</table>

---

### Principais Tarefas de Segurança Cibernética (NIST Framework)

Abordagem institucional para auxílio no gerenciamento da segurança cibernética através de cinco funções centrais:

1. **Identificar:** Reconhecer e catalogar os ativos de informação e os riscos associados.
2. **Proteger:** Implementar medidas de salvaguarda para prevenir acessos não autorizados.
3. **Detectar:** Monitorar o ambiente continuamente para identificar incidentes de segurança.
4. **Responder:** Desenvolver planos e procedimentos para conter e tratar incidentes.
5. **Recuperar:** Restaurar os sistemas e dados afetados após um incidente.

---

### Competências em Segurança da Informação

Diversos profissionais atuam em funções específicas dentro da segurança cibernética:

* **CISO (*Chief Information Security Officer*):** Responsável pela estratégia geral de segurança.
* **DPO (*Data Protection Officer*):** Responsável pela proteção de dados pessoais e conformidade regulatória.
* **Hacker Ético:** Realiza testes de penetração (*pentests*) para identificar vulnerabilidades.
* **Especialista em Segurança na Nuvem:** Protege dados e aplicações em ambientes de nuvem.
* **Analista de Forense Digital:** Investiga incidentes e coleta evidências digitais.
* **Cientista de Dados em Segurança:** Analisa grandes volumes de dados para identificar padrões de ameaças.
* **Analista de Resposta a Incidentes:** Coordena a contenção e mitigação de incidentes.
* **QA Security (Testador de Software):** Valida a segurança de aplicações durante o desenvolvimento.

---

### Unidades de Negócios e Equipes

* **NOC (*Network Operations Center*):** Monitora e gerencia a infraestrutura de rede.
* **SOC (*Security Operations Center*):** Monitora eventos e responde a incidentes de segurança.
* **CSIRT (*Computer Security Incident Response Team*):** Equipe especializada no tratamento imediato de incidentes cibernéticos.

#### Divisão de Equipes de Testes (*Teams*)

* **Blue Team:** Foco em monitoramento, defesa e proteção dos sistemas.
* **Red Team:** Simulação de ataques reais para identificar falhas de segurança.
* **Purple Team:** Integração entre Red e Blue teams para otimização de defesas.
* **White Team:** Gestão, mediação e validação dos resultados dos testes.

---

### Superfície de Ataque e Vetores de Ataque

A **superfície de ataque** compreende o conjunto de todos os pontos de entrada e vulnerabilidades exploráveis por um invasor. Ela é significativamente maior quando envolve ameaças internas (*insiders*) em relação a atores externos. 

- **Redução da superfície:** Implica em restringir privilégios, limitar endpoints, fechar portas/serviços desnecessários e controlar acessos.

**Exemplos de pontos de entrada:**

- Servidores
- Aplicativos
- Dispositivos (endpoints)
- Usuários

#### Vetores de Ataque

Métodos e técnicas utilizados para explorar a superfície de ataque:

* Vetores baseados em software
* Vetores sociais e psicológicos
* Vetores de rede e tráfego
* Vetores de autenticação e senhas

> #### Inteligência de Ameaças (*Threat Intelligence*)
> * **Recursos:** Ferramentas dedicadas, plataformas de TI e compartilhamento comunitário de dados de ameaças.
> * **Agentes:** Equipes internas de segurança, fornecedores cibernéticos, agências de inteligência e pesquisadores independentes.

---

### IA, Análise Preditiva e Machine Learning

A aplicação de Inteligência Artificial e Machine Learning otimiza a segurança ao analisar grandes volumes de dados em tempo real, identificar comportamentos anômalos, prever padrões de ataques cibernéticos e automatizar ações de resposta a incidentes.

---

### Engenharia Social

Técnica de manipulação psicológica utilizada para enganar indivíduos e obter acesso não autorizado a sistemas ou dados sigilosos.

#### Principais Métodos:

- **Phishing:** Mensagens fraudulentas (e-mail, SMS ou sites falsos) para captura de dados confidenciais.
    - **Spear Phishing:** Ataque personalizado e direcionado a um indivíduo ou grupo específico.
    - **Whaling:** Phishing de alta precisão direcionado a executivos de alto nível (*C-level*).
    - **Vishing:** Phishing realizado via chamadas telefônicas/voz.
- **Ameaças Correlatas:**
    - **Spam:** Envio em massa de conteúdo não solicitado ou malicioso.
    - **Hoaxes:** Boatos ou mensagens enganosas desenhadas para espalhar pânico ou desinformação.
    - **Coleta de Credenciais:** Obtenção indevida de acessos por meio de engenharia social.
- **Campanhas de Influência:** Manipulação psicológica e comunicação persuasiva para moldar percepções e comportamentos em grande escala.

---

## Unidade 2: Ameaças, Malwares e Controles

### Classificação de Malware

A classificação de malwares varia de acordo com seu **vetor de infecção**, seu **payload** (carga útil) ou seu **propósito final**.

#### Principais Tipos:

* **Vírus:** Dependem de um arquivo hospedeiro (executáveis, documentos, scripts) e da ação do usuário para execução e propagação.
    - *Modo de Operação:* Anexam-se a arquivos válidos; propagam-se ao compartilhar o arquivo infectado.
    - *Exemplos:* Vírus ILOVEYOU (2000), Vírus Melissa.
* **Worms:** Programas autônomos que se multiplicam automaticamente explorando falhas na rede.
    - *Modo de Operação:* Não precisam de arquivos hospedeiros; varrem redes e USBs para infectar novos sistemas.
    - *Exemplos:* Conficker, SQL Slammer.
* **Trojan Horses (Cavalos de Tróia):** Disfarçam-se de softwares legítimos para enganar o usuário e executar funções maliciosas em segundo plano.
    - *Modo de Operação:* Utilizam engenharia social, anexos de e-mail ou links maliciosos.
    - *Exemplo:* Zeus Trojan, RATs diversos.
* **PUPs (Programas Potencialmente Indesejados):** Instalados frequentemente em conjunto com softwares legítimos (*grayware*).
    - *Modo de Operação:* Exibem anúncios intrusivos e alteram configurações do sistema sem autorização prévia.
    - *Exemplos:* Adwares, barras de ferramentas maliciosas (*toolbars*).

---

### Classificação pelo Propósito

* **Spyware:** Coleta informações do usuário sem consentimento (dados de navegação, credenciais, hábitos pessoais).
    - *Adware:* Exibe anúncios publicitários direcionados.
    - *Superfish:* Software pré-instalado que injetava anúncios e quebrava a segurança HTTPS.
* **Keyloggers:** Subtipo de spyware projetado para registrar todas as teclas digitadas pelo usuário (captura de senhas e cartões).
    - *Exemplos:* Módulos do Trojan Zeus, HawkEye Keylogger.
* **Cookies de Rastreamento:** Mapeiam navegação e consultas entre múltiplos sites para perfilamento de dados.

---

### Classificação pelo Payload (Carga Útil)

- **Backdoors e RATs (*Remote Access Trojans*):** Abrem portas de comunicação ocultas para permitir o controle remoto do sistema por um invasor.
    - *Exemplos:* Back Orifice, DarkComet RAT.
- **Rootkits:** Projetados para se ocultar nas camadas mais profundas do sistema operacional (Kernel/Firmware), dificultando a detecção por antivírus.
    - *Exemplos:* DarkMatter EFI, Sony BMG Rootkit, TDL-4.
- **Ransomware:** Criptografa arquivos ou bloqueia o acesso ao sistema, exigindo resgate (geralmente em criptomoedas) para a liberação da chave de decodificação.
    - *Exemplos:* WannaCry, CryptoLocker, Ryuk.
- **Cripto-Malware:** Utiliza criptografia de forma destrutiva para danificar ou ocultar dados de forma irreversível, sem solicitação de resgate.
- **Bombas Lógicas (*Logic Bombs*):** Códigos maliciosos inseridos em aplicações legítimas que são disparados apenas quando condições específicas são atendidas (datas, eventos ou ações do usuário).
    - *Exemplos:* Stuxnet, Worm MyDoom.

---

### Análise de Indicadores e Prevenção de Malware

**Indicadores de Malware**: representam traços, pistas ou evidências deixadas pelo software malicioso, usadas como impresões digitais virtuais. Os formatos são:

- Notificações de antivírus
- Execução de sandbox
- Consumo de recursos
- Mudanças no sistema de arquivos

**Notificações de antivírus**: sempre que um antivírus identifica um malware, ele envia um alerta ao usuário, informando sobre a ameaça detectada e as ações recomendadas para mitigá-la.

- Falso positivo: arquivo legítimo identificado erroneamente como malware.
- Falso negativo: malware não detectado pelo antivírus, permanecendo ativo no sistema.

**Execução de sandbox**: ambiente isolado e controlado onde o malware é executado para observar seu comportamento sem risco de infecção ao sistema principal.

- Permite analisar a forma como o malware se propaga, quais arquivos ele altera e quais conexões de rede ele tenta estabelecer.
- Ferramentas como Cuckoo Sandbox e Any.Run são exemplos de plataformas de análise de malware.

**Consumo de recursos**: monitoramento do uso de CPU, memória e rede para identificar atividades suspeitas que podem indicar a presença de malware.

- Picos de uso de CPU ou memória sem motivo aparente podem ser sinais de malware em execução.
- Ferramentas como Process Explorer e Resource Monitor ajudam a identificar processos suspeitos.

**Mudanças no sistema de arquivos**: monitoramento de alterações em arquivos e diretórios do sistema, como criação de arquivos desconhecidos, modificação de arquivos críticos ou exclusão de arquivos importantes.

- Ferramentas como Tripwire e File Integrity Monitoring (FIM) ajudam a detectar alterações não autorizadas no sistema de arquivos.

### Análise de Processos
Técnica para examinar o comportamento de programas e processos em execução. Necessidade de conhecer o comportamento de apps, processos e serviços. São ações que auxiliam na detecção de malware, como:

- Análise de tráfego de rede
- Análise de registros
- Monitoramento e comportamento

> Ferramentas de análise: Wireshark, Systinternals Suite

### Prevenção de Malware
A prevenção de malware envolve a implementação de medidas proativas para reduzir o risco de infecção por malware, com potencial de reduzir a superfície de ataque e aumentar a resiliência do sistema. Algumas estratégias incluem:

- Atualizações de software
- Políticas de acesso
- Conscientização do usuário

**Mitigação de Riscos**
Visa minimizar o impacto de uma infecção por malware, caso ocorra. Ações de mitigação:

- Estratégias de isolamento e contenção
- Recuperação e restauração
- Investigção pós-incidente

### Categorias de Controles de Segurança
Refere-se a um conjunto de políticas, procedimentos, processos, atividades e ferramentas que moldam as estruturas de serviços na organização. Um controle de segurança é projetado para garantir confidencialidade, integridade, disponibilidade e não repúdio a um sistema ou ativo de dados. Os controles se dividem em:

- **Controles Técnicos**: também chamados de controles lógicos, implementados por meio de sistemas, dispositivos e softwares, e operam em tempo real com respostas rápidas a ameaças e ataques. Exemplos incluem firewalls, antivírus, sistemas de detecção de intrusão (IDS) e criptografia.
- **Controles Operacionais**: refere-se às práticas, procedimentos e ações diárias para garantir a segurança da informação, garante a implementação das políticas de segurança. Garante que os funcionários estejam cientes das melhores práticas de segurança. Exemplos:
    - Políticas de senhas: regras para criação e gerenciamento de senhas
    - Gerenciamento de acesso de usuário: controle de quem tem acesso a determinado sistemas e dados
    - Treinamento em segurança da informação: proporcionar treinamento aos funcionários sobre ameaças cibernéticas
- **Controles Gerenciais**: desenvolvimento de estratégias de longo prazo para lidar com ameaças e risco, foco na alta administração. Contempla a definição de políticas e diretrizes gerais de segurança, estabelecem a estrutura e a governança da informação. Exemplos:
    - Políticas de segurança da informação: definição de regras e diretrizes gerais da segurança da organização
    - Análise de risco: define avaliações periódicas e regulares de riscos
    - Plano de continuidade de negócios: estabelece planos para garantir a continuidade das operações

Além dessas 3 catergorias, também existem os tipos funcionais, que contém abordagens específicas para a proteção de ativos e a gestão de riscos:

- **Controles Preventivos**: projetados para impedir que incidentes de segurança ocorram, como firewalls, antivírus e políticas de acesso.
- **Controles Detectivos**: projetados para identificar e alertar sobre incidentes de segurança em tempo real, como sistemas de detecção de intrusão (IDS) e monitoramento de logs.
- **Controles Corretivos**: projetados para corrigir ou mitigar os efeitos de incidentes de segurança após sua ocorrência, como backups, planos de recuperação de desastres e atualizações de software.
- **Outros tipos**: são aqueles controles quenão se encaixam claramente como preventivos, detectivos ou corretivos, podem ser utilizadospara reforçar outros tipos de controle. Nessa classificação são os seguintes: físicos, dissuasores, controle de compensação.

### Seleção de controles de segurança
A seleção de controles de segurança envolve a identificação e implementação de medidas específicas para proteger os ativos de informação de uma organização. A escolha dos controles deve ser baseada em uma análise de risco, considerando a probabilidade e o impacto de ameaças potenciais.

As etapas para seleção dos controles:

1. **Avaliação de riscos**
2. **Definição de requisistos de segurança**
3. **Seleção de controles adequados**
4. **Implementação e testes**

Critérios de seleção:

1. **Relevância para riscos**
2. **Custo-benefício**
3. **Conformidade regulatória**
4. **Viabilidade técnica e operacional**

Revisão contínua

1. **Mudanças no ambiente de ameaças**
2. **Mudanças na organização**
3. **Resultados de testes e incidentes**
4. **Alterações em regulamentações**

### Fontes de ameaça
Representam os pontos de onde as ameaças podem surgir, podendo ser internas ou externas à organização. A identificação das fontes de ameaça é essencial para a implementação de controles de segurança eficazes.

- **Ameaças internas:** originam-se de dentro da organização, como funcionários, contratados ou parceiros. Podem ser intencionais (roubo de dados, sabotagem) ou acidentais (erros humanos, falhas de procedimento).
    - Atributos dos atores internos
        - Motivação: financeira, política, pessoal ou coerção por terceiros
        - Nível de sofisticação: depende do conhecimento e autorizações do funcionário
        - Recursos: nível de acesso aos sistemas ou apoio de fontes externas
        - Estratégias comuns: abuso de privilégios de acesso, roubo, destruição, divulgação e vazamento de dados
- **Ameaças externas:** originam-se de fora da organização, como hackers, grupos de cibercriminosos, concorrentes ou agentes patrocinados por estados. Podem ser motivadas por lucro, espionagem, ativismo ou sabotagem.
    - Representadas por atores externos
        - Hackers individuais
        - Grupos de cibercriminosos
        - Concorrentes
        - Hackativistas
        - Governos estrangeiros
    - Atributos dos atores externos
        - Motivação: financeira, política, pessoal
        - Nível de sofisticação: pode variar significativamente, desde amadores até grupos altamente organizados
        - Recursos: hackers individuais podem ter recursos limitados, enquanto grupos organizados podem ter acesso a financiamentos
        - Estratégias comuns: ataques de engenharia social, malware, roubo de identidade, ransomware, DDoS e outros

### Deep Web, Dark Web e Dark Net

- **Surface Web**: parte visível da internet, indexada por mecanismos de busca comuns.
- **Deep Web**: parte substancial, porém menos visível da internet, não indexada por mecanismos de busca comuns, tem um papel na ciberseurança e atividade legítimas, exige conhecimento técnico para acessar e navegar.
- **Dark Web**: parte da Deep Web, utiliza redes criptografadas e sistemas de anonimato, costuma hospedar mercados clandestinos, fóruns de hackers e atividades ilegais, mas também pode ser usada para comunicação segura e proteção da privacidade.
---
- **Funcionamento/Acesso a Dark Web**: Trede de anonimato que permite acessar a Dark Web, utilizando o navegador Tor com roteamento criptografado, difícil rastreabilidade.
- **Organização da Dark Web**: descentralizada e baseada em comunidades, composta por várias redes independentes (dark nets) como a rede Onion
- **Potencial de utilização como contra-ameaças**: Pode ser usada de forma legítima para uma comunicação segura

### Dark Web
Opera por meio de redes privadas e sistemas criptografados, garante um alto grau de anonimato, organização descentralizada e tem potencial tanto para atividades maliciosas quanto para ações legítimas.

## Unidade 3: Identificação de Vulnerabilidades

### Avaliações de Segurança
Primeiro a varredura identifica hosts, topologia da rede e serviços/portas. É estabelecida uma superfície de ataque geral. As avaliações são utilizadas para testar vulnerabilidades do ambiente. O NIST identificou três atividades principais:

- Testear o objeto em avaliação
- Examinar objetos de avaliação
- Entrevistar pessoal

Os principais tipos são:

- Verificação de vulnerabilidade
- Caça a ameaças
- Testes de penetração

A verificação de vulnerabilidades trata-se do processo de identificação, avaliação e análise das fraquezas e falhas de segurança. Podem ser manuais ou automatizadas. Apontam para a necessidade de ajustes na segurança e correções de software.

Suas técnicas incluem:

- Varreduras automatizadas: wireshark, burp suite, nessus, etc
- Testes manuais: análise minuciosa feita por profissionais especializados, utilizando ferramentas automáticas, úteis para avaliar a segurança de sistemas complexos.

### CVE (Common Vulnerabilities and Exposures)

Mantido pela MITRE Corporation, o CVE é um banco de dados público que fornece uma lista padronizada de vulnerabilidades conhecidas em softwares e sistemas. Cada vulnerabilidade recebe um identificador único (CVE ID) para facilitar a comunicação e o rastreamento.

### Abordagens de Varredura
- **Intrusiva**: realiza conexões diretas e explorações reais que podem causar travamentos (deve ser feita em ambiente controlado).
- **Não intrusiva**: analisa apenas evidências indiretas (como dados públicos e análise de tráfego), sendo segura contra interrupções
- **Credenciada**: utiliza senhas de acesso fornecidos pelos proprietários para auditar configurações profundas e atualizações internas.
- **Não credenciada**: examina a rede externamente, simulando a visão de um atacante sem privilégios

### Defesa em profundidade
Estratégia de proteção estruturada em múltiplas camadas (segurança física, identidade/acesso, perimetro, rede, computação, aplicação e dados), garantindo que o comprometimento de um nível não exponha o sistema completo.

### Estratégias de engano
Uso de iscas e chamarizes para atrair e detectar invasores, dividadas em Honeypot (sistema isolado), Honeynet (rede simulada) e Honeyfile (arquivo falso).

### Gerência de configuração
Controle de versões, mudanças e integração contínua para assegurar a conformidade e correção de falhas.

---

## Unidade 4: Gerenciamento de Identidade e Acesso

### Conceito de IAM
IAM (Identity and Access Management) é um conjutno de controles técnicos que estabelecem como os sujeitos (usuários, processos ou dispositivos) interagem com os objetos (recursos como redes, servidores e arquivos).

** Os 4 processos principais do IAM**:
1. **Identificação**: atribuição de uma conta ou ID exclusivo para diferenciar o sujeito na rede
2. **Autenticação**: verificação da legitimidade da identidade declarada (comprova que o sujeito é quem diz ser)
3. **Autorização**: concessão de permissões e privilégios específicos com base nas políticas da organização.
4. **Contabilidade**: registro e rastreamento das ações e consumo de recursos efetuados pelo usuário.

### Mecanismos e boas práticas de auth

- Aplicação da tríade CID no login: confidencialidade das credencias, integridade contra acessos forjados e disponibilidade do serviço
- Emprego de MDA combinando dois ou mais fatores de verificação (ex.: senha + token ou biometria)
- Armazenamento de senhas em bancos de dados exclusivamente na forma de hashes criptográficos.

## Autenticação em SOs

- **Windows**: gerenciada pelo Local Security Authority (LSA), localmente e pelo Active Directory (AD) com protocolos Kerberos na rede, utilizando SSTP e certificados para acessos remotos via VPN.
- **Linux**: guarda contas no arquivo ```/etc/passwd``` e os hashes de senhas no ```/etc/shadow```. Para rede e acesso remoto, emprega SSH com chaves criptográficas, PAM (Pluggable Authentication Modules) para flexibilidade e integração com LDAP.

### Protocolos de Autenticação
- **PAP**: envia credenciais em texto puro (obsoleto e inseguro)
- **CHAP**: utiliza esquema de desafio e resposta criptografada
- **MS-CHAP**: variante desenvolvida pela Microsoft com suporte à troca de senhas criptogradas em ambientes Windows e VPNs.

### Ataques a senhas
Interceptação de dados em texto simples (como em conexões HTTP/Telnet sem criptografia) e ataques online (força bruta ou dicionário diretamente na interface de login)