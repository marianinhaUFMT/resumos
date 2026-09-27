# Segurança em Redes de Computadores

> Este documento apresenta os conceitos fundamentais de segurança em redes de computadores, incluindo protocolos, técnicas e práticas recomendadas para proteger a integridade e a confidencialidade dos dados transmitidos.

---

## Sumário

1. [Unidade 1: Princípios de Segurança da Informação](#unidade-1-princípios-de-segurança-da-informação)
2. [Unidade 2: Ameaças, Malwares e Controles](#unidade-2-ameaças-malwares-e-controles)
3. [Unidade 3: Identificação de Vulnerabilidades](#unidade-3-identificação-de-vulnerabilidades)
4. [Unidade 4: Gerenciamento de Identidade e Acesso](#unidade-4-gerenciamento-de-identidade-e-acesso)

---

## Unidade 1: Princípios de Segurança da Informação

A **Segurança da Informação** é um conjunto de práticas, políticas, procedimentos e tecnologias projetadas para proteger os ativos de informação. Ela desempenha um papel vital na preservação da tríade CID.

Os dados podem estar vulneráveis devido à:
* Forma como são **armazenados**
* Forma como são **transferidos**
* Forma como são **processados**

---

### Tríade CID

Pilar de sustentação da segurança da informação:

* **Confidencialidade:** Garantir que a informação seja acessível apenas a pessoas autorizadas.
* **Integridade:** Garantir que a informação seja precisa e completa, e que não tenha sido alterada de forma não autorizada.
* **Disponibilidade:** Garantir que a informação esteja disponível sempre que necessário.

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

* **Vulnerabilidade:** Uma fraqueza ou deficiência em um sistema que pode ser explorada por uma ameaça.
* **Ameaça:** Evento com o potencial de causar dano.
* **Risco:** A probabilidade de uma ameaça explorar uma vulnerabilidade e causar dano.

**Estratégias para gerenciamento de riscos:**
* Aceitar
* Mitigar
* Transferir
* Evitar

---

### Matriz de Risco

A matriz de risco é uma ferramenta visual utilizada para classificar e priorizar os riscos de segurança com base na combinação da **probabilidade** e do **impacto** da ocorrência.

| Probabilidade | Impacto: Alto | Impacto: Médio | Impacto: Baixo |
| :--- | :---: | :---: | :---: |
| **Alta** | **Elevado** | **Alto** | **Médio** |
| **Média** | **Alto** | **Médio** | **Baixo** |
| **Baixa** | **Médio** | **Baixo** | **Desprezível** |

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

A **superfície de ataque** compreende o conjunto de todos os pontos de entrada e vulnerabilidades exploráveis por um invasor.
* Ela é significativamente maior quando envolve ameaças internas (*insiders*) em relação a atores externos.
* **Redução da superfície:** Implica em restringir privilégios, limitar endpoints, fechar portas/serviços desnecessários e controlar acessos.

**Exemplos de pontos de entrada:**
* Servidores
* Aplicativos
* Dispositivos (endpoints)
* Usuários

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

* **Phishing:** Mensagens fraudulentas (e-mail, SMS ou sites falsos) para captura de dados confidenciais.
  * **Spear Phishing:** Ataque personalizado e direcionado a um indivíduo ou grupo específico.
  * **Whaling:** Phishing de alta precisão direcionado a executivos de alto nível (*C-level*).
  * **Vishing:** Phishing realizado via chamadas telefônicas/voz.
* **Ameaças Correlatas:**
  * **Spam:** Envio em massa de conteúdo não solicitado ou malicioso.
  * **Hoaxes:** Boatos ou mensagens enganosas desenhadas para espalhar pânico ou desinformação.
  * **Coleta de Credenciais:** Obtenção indevida de acessos por meio de engenharia social.
* **Campanhas de Influência:** Manipulação psicológica e comunicação persuasiva para moldar percepções e comportamentos em grande escala.

---

## Unidade 2: Ameaças, Malwares e Controles

### Classificação de Malware

A classificação de malwares varia de acordo com seu **vetor de infecção**, seu **payload** (carga útil) ou seu **propósito final**.

#### Principais Tipos:

* **Vírus:** Dependem de um arquivo hospedeiro (executáveis, documentos, scripts) e da ação do usuário para execução e propagação.
  * *Modo de Operação:* Anexam-se a arquivos válidos; propagam-se ao compartilhar o arquivo infectado.
  * *Exemplos:* Vírus ILOVEYOU (2000), Vírus Melissa.
* **Worms:** Programas autônomos que se multiplicam automaticamente explorando falhas na rede.
  * *Modo de Operação:* Não precisam de arquivos hospedeiros; varrem redes e USBs para infectar novos sistemas.
  * *Exemplos:* Conficker, SQL Slammer.
* **Trojan Horses (Cavalos de Tróia):** Disfarçam-se de softwares legítimos para enganar o usuário e executar funções maliciosas em segundo plano.
  * *Modo de Operação:* Utilizam engenharia social, anexos de e-mail ou links maliciosos.
  * *Exemplo:* Zeus Trojan, RATs diversos.
* **PUPs (Programas Potencialmente Indesejados):** Instalados frequentemente em conjunto com softwares legítimos (*grayware*).
  * *Modo de Operação:* Exibem anúncios intrusivos e alteram configurações do sistema sem autorização prévia.
  * *Exemplos:* Adwares, barras de ferramentas maliciosas (*toolbars*).

---

### Classificação pelo Propósito

* **Spyware:** Coleta informações do usuário sem consentimento (dados de navegação, credenciais, hábitos pessoais).
  * *Adware:* Exibe anúncios publicitários direcionados.
  * *Superfish:* Software pré-instalado que injetava anúncios e quebrava a segurança HTTPS.
* **Keyloggers:** Subtipo de spyware projetado para registrar todas as teclas digitadas pelo usuário (captura de senhas e cartões).
  * *Exemplos:* Módulos do Trojan Zeus, HawkEye Keylogger.
* **Cookies de Rastreamento:** Mapeiam navegação e consultas entre múltiplos sites para perfilamento de dados.

---

### Classificação pelo Payload (Carga Útil)

* **Backdoors e RATs (*Remote Access Trojans*):** Abrem portas de comunicação ocultas para permitir o controle remoto do sistema por um invasor.
  * *Exemplos:* Back Orifice, DarkComet RAT.
* **Rootkits:** Projetados para se ocultar nas camadas mais profundas do sistema operacional (Kernel/Firmware), dificultando a detecção por antivírus.
  * *Exemplos:* DarkMatter EFI, Sony BMG Rootkit, TDL-4.
* **Ransomware:** Criptografa arquivos ou bloqueia o acesso ao sistema, exigindo resgate (geralmente em criptomoedas) para a liberação da chave de decodificação.
  * *Exemplos:* WannaCry, CryptoLocker, Ryuk.
* **Cripto-Malware:** Utiliza criptografia de forma destrutiva para danificar ou ocultar dados de forma irreversível, sem solicitação de resgate.
* **Bombas Lógicas (*Logic Bombs*):** Códigos maliciosos inseridos em aplicações legítimas que são disparados apenas quando condições específicas são atendidas (datas, eventos ou ações do usuário).
  * *Exemplos:* Stuxnet, Worm MyDoom.

---

### Análise de Indicadores e Prevenção de Malware

*(Conteúdo a ser adicionado)*

---

## Unidade 3: Identificação de Vulnerabilidades

*(Conteúdo a ser adicionado)*

---

## Unidade 4: Gerenciamento de Identidade e Acesso

*(Conteúdo a ser adicionado)*