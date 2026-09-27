# Segurança em redes de computadores

> Este documento apresenta os conceitos fundamentais de segurança em redes de computadores, incluindo protocolos, técnicas e práticas recomendadas para proteger a integridade e a confidencialidade dos dados transmitidos.

---

## Sumário

[Unidade 1: Princípios de Segurança da Informação](#unidade-1-principios-de-seguranca-da-informacao)

[Unidade 2: Ameaças, Malwares e Controles](#unidade-2-ameacas-malwares-e-controles)

[Unidade 3: Identificação de Vunerabilidades](#unidade-3-identificacao-de-vulnerabilidades)

[Unidade 4: Gerenciamento de Identidade e Acesso](#unidade-4-gerenciamento-de-identidade-e-acesso)

---

# Unidade 1: Princípios de Segurança da Informação

A **segurança da informação** é um conjunto de práticas, políticas, procedimentos e tecnologias projetadas para proteger os ativos de informação.
A segurança da informação desempenha um papel vital na proteção da CID.

Os dados podem estar vulneráveis devido à:
- forma como são armazenados
- forma como são transferidos
- forma como são processados
---
### TRÍADE CID

-> Este é considerado o pilar de sustentação da segurança da informação:
- **Confidencialidade**: garantir que a informação seja acessível apenas a pessoas autorizadas.
- **Integridade**: garantir que a informação seja precisa e completa, e que não tenha sido alterada de forma não autorizada.
- **Disponibilidade**: garantir que a informação esteja disponível quando necessário.

-> Outros conceitos importantes incluem:
1. Não repúdio
2. Autenticidade
3. Controle de acesso
4. Autenticação e Autorização
5. Princípio do menor privilégio
6. Gestão de riscos
7. Ativos de informação
8. Ameaças à segurança da informação
9. Ataques cibernéticos
10. Criptografia

### RELAÇÃO ENTRE VULNERABILIDADE, AMEAÇA E RISCO
-> Trata-se de indentificar as formas pelas quais os sitemas podem ser atacados.
Envolve o mapeamento e a análise das relações entre:
- **Vulnerabilidade**: uma fraqueza ou deficiência em um sistema que pode ser explorada por uma ameaça.
- **Ameaça**: evento com o potencial de causar dano.
- **Risco**: a probabilidade de uma ameaça explorar uma vulnerabilidade e causar dano.

Algumas estratégias para gerenciar riscos incluem:
- Aceitar
- Mitigar
- Transferir
- Evitar

### MATRIZ DE RISCO
-> A matriz de risco é uma ferramenta visual que ajuda a avaliar e priorizar os riscos de segurança, utilizada para classicar e priorizar com base da probabilidade e no impacto da ocorrência.

Esta matriz é uma **matriz de avaliação de risco**, utilizada para classificar e priorizar riscos através da combinação de dois eixos fundamentais:

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

### Principais tarefas de segurança cibernética com base no NIST Framework
-> Trata-se de uma abordagem institucional que auxilia as organizações a gerencia desegurança cibernética. O framework é composto por quatro elementos:
1. **Identificação**: reconhecer e catalogar os ativos de informação e os riscos associados.
2. **Proteger**: implementar medidas de segurança para prevenir acessos não autorizados.
3. **Detectar**: monitorar o ambiente para identificar atividades suspeitas ou incidentes de segurança.
4. **Responder**: desenvolver planos e procedimentos para lidar com incidentes de segurança.
5. **Recuperar**: restaurar os sistemas e dados afetados após um incidente de segurança.

### Competências de segurança da informação
-> Há uma diversidade de profissionais atuando em segurança da informação, cada um com funções específicas. Alguns exemplos incluem:
-**CISO** (Chief Information Security Officer): responsável pela estratégia de segurança da informação.
- **DPO** (Data Protection Officer): responsável pela proteção de dados pessoais e conformidade com regulamentações de privacidade.
- **Hacker ético**: profissional que realiza testes de penetração para identificar vulnerabilidades.
- **Especialista em segurança da nuvem**: responsável por proteger dados e sistemas em ambientes de nuvem.
- **Analista de forense digital**: investiga incidentes de segurança e coleta evidências digitais.
- **Cientista de dados em segurança**: analisa grandes volumes de dados para identificar padrões e ameaças.
- **Analista de resposta a incidentes**: coordena a resposta a incidentes de segurança e implementa medidas corretivas.
- **Testador de software** (QA Security): realiza testes de segurança em aplicativos e sistemas para identificar vulnerabilidades.

### Unidades de negócios de segurança da informação
-> NOC (Network Operations Center): responsável por monitorar e gerenciar a infraestrutura de rede.
-> SOC (Security Operations Center): responsável por monitorar e responder a incidentes de segurança.
-> CSIRT (Computer Security Incident Response Team): equipe especializada em responder a incidentes de segurança cibernética.

**Times de testes**
-> Esses times são parte do SOC e CSIRT, e são responsáveis por realizar testes de penetração, análise de vulnerabilidades e avaliação de segurança em sistemas e aplicativos.

Papéis de cada time:
- **Blue Team**: monitoramento e defesa.
- **Red Team**: simulação de ataques para identificar vulnerabilidades.
- **Purple Team**: avaliação e aprimoramento.
- **White Team**: validação de resultados de testes.

### Superfície de ataque e vetores de ataque
-> Refere-se ao conjunto de todos os pontos de entrada e vulnerabilidades que um invasor pode explorar para comprometer um sistema ou rede. É supostamente maior quando se trata de atores internos em comparação com atores externos.
-> Reduzir a superficie de ataque implica em restringir acessos, limitar acesso a endpoints, protocolos/portas e serviços/métodos conhecidos.

Os pontos de entrada podem incluir:
- Servidores
- Aplicativos
- Dispositivos
- Usuários

**VETORES DE ATAQUE**
-> São métodos e técnicas usadas pelos atores de ameaças para explorar vulnerabilidades na superfície de ataque. Categorias comuns de vetores de ataque incluem:
- Vetores baseados em software
- Vetores de ataque sociais e psicológicos
- Vetores de ataque de redes e tráfego
- Vetores de ataque de autenticação e senhas

> ### Como pesquisar inteligência de ameaças
> - Ferramentas e plataformas de inteligência de ameaças
> - Colaboração e compartilhamento de informações
>
> -> Quem pesquisa: equipes de segurança, fornecedores de segurança cibernética, agências de inteligência, comunidade de segurança cibernética.


### IA, Análise Preditiva e Machine Learning
-> A inteligência artificial (IA), a análise preditiva e o aprendizado de máquina (machine learning) são tecnologias que podem ser aplicadas à segurança da informação para melhorar a detecção de ameaças, identificar padrões de comportamento suspeito e automatizar respostas a incidentes. Essas tecnologias podem ajudar a analisar grandes volumes de dados, identificar anomalias e prever possíveis ataques cibernéticos com base em padrões históricos.

### Engenharia Social
-> A engenharia social é uma técnica de manipulação psicológica usada por atacantes para enganar indivíduos e obter acesso não autorizado a informações, sistemas ou recursos. Os ataques de engenharia social exploram a confiança, a curiosidade ou o medo das pessoas para induzi-las a revelar informações confidenciais ou realizar ações que comprometam a segurança. Alguns métodos são:
- **Phishing**: utiliza do envio de mensagens fraudulentas (e-mail ou SMS) ou a criação de sites falsos para enganar pessoas e fazê-las fornecer informações confidenciais, como senhas ou dados bancários.
    - **Spear Phishing**: é uma forma mais direcionada de phishing, onde o atacante personaliza a mensagem para um indivíduo ou grupo específico, aumentando a probabilidade de sucesso.
    - **Whaling**: é uma forma de phishing que visa executivos ou pessoas de alto nível em uma organização, geralmente com mensagens sofisticadas e convincentes.
    - **Vishing**: é uma forma de phishing que utiliza chamadas telefônicas para enganar as vítimas e obter informações confidenciais.
- **Spam, Hoaxes e coleta de credenciais**:
    - **Span**: envio de mensagens em massa, geralmente com conteúdo indesejado ou malicioso.
    - **Hoaxes**: são mensagens falsas ou enganosas que circulam na internet, muitas vezes com o objetivo de assustar ou enganar as pessoas.
    - **Coleta de credenciais**: refere-se à obtenção não autorizada de informações de login, como nomes de usuário e senhas, por meio de técnicas de engenharia social ou ataques cibernéticos.
- **Campanha de influência**: é uma estratégia que pode influenciar opiniões, moldar percepções e promover agendas políticas ou comerciais, utilizando técnicas de manipulação psicológica e comunicação persuasiva.

# Unidade 2: Ameaças, Malwares e Controles

### Classificação de Malware
-> Tipos de Malware
A classificação de malware não segue um padrão pré-estabelecido, pode haver sobreposição na classificação; algumas classificações concentram-se no vetor usado pelo malware, outras são baseadas na carga (payload) entregue pelo malware. Podem tamvém serem baseadas no seu propósito.

- **Vírus**: entre os mais antigos, ocultados em um executável, documentos ou scripts, se espaçham quando são compartilhados. Se for ativado, pode infectar outros arquivos no mesmo sistema.
  - Modo de operação:
    - precisa de um hospedeiro para se propagar
    - se anexam a arquivos executáveis ou documentos
    - são ativados se o arquivo infectado for executado ou aberto
    - pode infectar outros arquivos no mesmo sistema ou em sistemas conectados
    - exemplos: Vírus ILOVEYOU em 2000 e Vírus Melissa
- **Worms**: autônomos, não precisam de um hospedeiro para se propagar, exploram vulnerabilidades em sistemas e redes para se espalhar rapidamente.
  - Modo de operação:
    - projetados para se replicar por redes e sistemas
    - não se anexam a arquivos existentes
    - se transmitem pelas redes, dispositivos USB e Internet
    - se ativado pode se replicar automaticamente e transmitir-se para outros sistemas na rede
    - exemplos: Conficker e SQL Slammer
- **Trojan Horses (Cavalos de Troia)**: se disfarçam como software legítimo, mas contêm código malicioso que pode comprometer a segurança do sistema.
  - Modo de operação:
    - enganam o usuário usando engenharia social
    - disfarçam-se como software legítimo
    - podem estar contidos em anexos maliciosos de e-mails
    - links maliciosos que levam para a instalação do trojan
    - exemplos: Zeus trojan RAT
- **PUPs (Programas Potencialmente Indesejados)**: acompanham a instalação de software legítimo, realizam atividades indesejas, como anúncios. A presença de um PUP não é considerada maliciosa, mas pode afetar a experiência do usuário e a segurança do sistema.
  - Modo de operação:
    - instaladas como parte de um pacote de software legítimo
    - descrito como grayware em vez de malware
    - realizam ações intrusivas sem consentimento do usuário
    - exemplos: barras de ferramentas de navegador, extensões suspeitas

### Classificação de Malware pelo propósito
- **Spyware**: coleta informações do usuário sem consentimento, como hábitos de navegação, dados pessoais e credenciais de login. Pode ser usado para fins de marketing ou espionagem.
  - Exemplos:
    - **Adware**: exibe anúncios indesejados e coleta informações sobre os hábitos de navegação do usuário.
    - **Superfish**: um spyware que foi pré-instalado em alguns laptops Lenovo para fins de publicidade, acabou expondo os usuários a vulnerabilidades de segurança.
- **Keyloggers**: registram teclas, um subconjunto de spyware. Visa registrar todas as teclas digitadas pelo usuário, esperando roubar informações como senhas ou dados do cartão de crédito.
  - Exemplos:
    - **Zeus**: roubo de informações bancárias e credenciais de login.
    - **HawkEye Keylogger**: captura informações de login e credenciais sensíveis.
- **Cookies de rastreamento**: criados por anúncios e widgets analíticos incorporados em muitos sites. Podem registrar páginas visitas, consultas de pesquisa, metadados e endereço IP.

### Classificação de malware de acordo com o payload

- **Backdoors e RAT**: abre "portas de fundo" para que um invasos controle e manipule o sistema remotamente. Se disfarçam como programas legítimos ou são instalados juntos. RAT é um malware backdoor que imita a funcionalidade de programas remotos legítimos.
  - Exemplos:
    - **Back Orifice**: um backdoor que permite controle remoto de sistemas Windows.
    - **DarkComet**: um RAT que permite controle remoto e espionagem de sistemas infectados.
- **Rootkits**: projetados para se enconder no sistema e se fixam no kernel. Conseguem manter-se ativos mesmo após reinicializações do sistema e atualizações de software. Dificultam a detecção por softwares de segurança, residem no firmware de placas e adaptadores.
  - Exemplos:
    - **DarkMatter e QuakMatter EFI**: direcionados ao firmware de laptos Apple Macbook
    - **Sony BMG Rootkit**: um rootkit que se escondia em CDs de música da Sony BMG, permitindo acesso não autorizado ao sistema do usuário.
    - **Tdl-4**: um rootkit notório usados para ocultar botnet e atividades maliciosas.
- **Ransomware**: criptografa os arquivos ou sistemas tornando-os inacessíveis. Exigem um resgate financeiros pela chave de descriptografia, sem garantia de integridade dos dados. Infecação por meio de anexos de e-mails, downloads, sites infectados ou exploração de vulnerabilidades. Pagamento em moedas digitais.
  - Exemplos:
    - **WannaCry**: um ransomware que se espalhou rapidamente em 2017, explorando uma vulnerabilidade no Windows.
    - **CrpytoLocker**: um dos primeiros, extorquiu muito dinheiro das vítimas.
    - **Ryuk**: um ransomware direcionado a organizações, conhecido por ataques de alto perfil.
- **Cripto-Malware**: utiliza técnicas de criptografia para prejudicar ou comprometer sistemas, arquivos ou informações. Não pede resgate mas visa prejudcar ou ocultar informações. Pode causar danos irreparáveis, a infecção ocorre da mesma forma que o ransomware.
- **Bombas lógicas**: malware ativado a partir de um gatilho pré-determinado, causa danos deliberados aos sistemas, ocorre por meio de programa legítimo, código-fonte ou operação criminosa maior. Se ativada suas ações incluem exclusão de arquivos cŕiticos, corrupção de dados, disseminação adicional e outros.
  - Exemplos:
    - **Stuxnet**: usada para atacar sistemas de enriquecimento de urânio no Irã.
    - **mydoom**: um worm que continha uma bomba lógica que se ativava em uma data específica lançando um ataque de negação de serviço (DDoS).

### Análise de Indicadores e Prevenção de malware


# Unidade 3: Identificação de Vunerabilidades


# Unidade 4: Gerenciamento de Identidade e Acesso
