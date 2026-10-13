# Introdução ao Processamento de Imagens

> Você sabia que quando vemos um quadrado cinza sobre um fundo preto, nosso cérebro te engana fazendo-o perceber que ele é visivelmente mais brilhante do que o mesmo quadrado colocado sobre um fundo branco? Ou que antes mesmo que um único sinal visual atinja o córtex visual do seu cérebro, a retina do seu olho já realizou uma filtragem espacial complexa nos raios de luz que atingem a sua fóvea? O processamento digital de imagens preenche a lacuna entre a percepção humana e os algoritmos computacionais, permitindo que as máquinas limpem, transformem e analisem dados visuais.

---

Nesta lição, exploraremos os princípios fundamentais do processamento de imagens comparando como os humanos percebem a luz visual com a forma como os computadores digitalizam e manipulam imagens. Ao final desta lição, você será capaz de:

* **Diferenciar** entre Processamento Digital de Imagens e Visão Computacional com base em suas entradas, saídas e objetivos analíticos.

* **Analisar** a mecânica da percepção visual humana, incluindo a funcionalidade dos fotorreceptores (cones vs. bastonetes) e fenômenos perceptivos, como bandas de Mach e contraste simultâneo.

* **Comparar** os modelos de cores RGB e HSI para explicar por que desacoplar a intensidade da informação cromática simplifica o aprimoramento da imagem.

* **Avaliar** os processos matemáticos de amostragem espacial e quantização de intensidade, identificando os artefatos visuais causados pela degradação da resolução.

---

## 1. Processamento de Imagens vs. Visão Computacional e Arquitetura de Sistemas

Para construir uma base sólida, precisamos primeiro esclarecer uma fonte comum de confusão: **Qual a diferença entre Processamento Digital de Imagens e Visão Computacional?**

Embora esses campos estejam intimamente interligados, eles desempenham papéis distintos no pipeline da computação visual:

* **Processamento Digital de Imagens:** Concentra-se em algoritmos onde **a entrada e a saída são imagens**. O objetivo principal é transformar ou melhorar uma imagem para posterior visualização humana ou análise de máquina. Exemplos incluem remover ruído de fundo, afiar bordas borradas, inverter valores de pixels para criar mamografias negativas ou ajustar o contraste geral.

* **Visão Computacional:** Concentra-se em **interpretar** e **compreender** o conteúdo de uma imagem ou vídeo. A entrada é uma imagem visual, mas a saída é informação de alto nível ou metadados. Exemplos incluem detectar pontos de referência faciais, dirigir veículos autônomos identificando obstáculos na estrada ou realizar classificação binária para determinar se uma imagem contém um gato ou um cachorro.

### O Pipeline de Hardware e Software

Como uma cena visual abstrata se transforma em uma matriz digital na tela do seu computador? Um **sistema completo de processamento de imagens** depende de seis componentes principais interconectados:

1\. **Sensores:** Dispositivos físicos que capturam a energia eletromagnética recebida (luz) refletida em uma cena.

2\. **Hardwares especializados:** Módulos de hardware dedicados que lidam com conversão analógico-digital, captura rápida de quadros e supressão preliminar de ruído em nível de hardware.

3\. **Computador:** O mecanismo de processamento, que vai de computadores pessoais a servidores distribuídos em nuvem, executa a lógica algorítmica.

4\. **Software:** Bibliotecas de desenvolvimento, como OpenCV, NumPy e Matplotlib em Python, que realizam operações matemáticas em matrizes de imagens.

5\. **Armazenamento em massa e memória:** Infraestrutura de armazenamento de alta capacidade necessária para lidar com matrizes massivas de pixels não compactados.

6\. **Exibições e dispositivos de saída:** Monitores de alta resolução ou sistemas de impressão calibrados para exibir resultados visuais a observadores humanos.

Crucialmente, o aprimoramento da imagem é subjetivo e dependente do contexto. O que conta como uma imagem **melhorada** depende inteiramente de quem ou do que está consumindo a saída final. Na mamografia digital, o aprimoramento significa aumentar o contraste em torno das microcalcificações para que um radiologista possa detectar lesões precoces; na fotografia de consumo, pode significar simplesmente fazer com que o azul do céu pareça mais vívido.

---

## 2. O Sistema Visual Humano e Fenômenos da Percepção Visual

Como as imagens digitais são, em última análise, projetadas para o consumo humano, algoritmos eficazes de processamento de imagens devem levar em conta a mecânica biológica da visão humana.

### Arquitetura Anatômica do Olho

A luz entra no olho humano através da córnea e passa pela pupila, uma abertura cujo diâmetro é expandido ou contraído dinamicamente pela íris. O cristalino em seguida refrata os raios de luz incidentes, concentrando-os na retina na parte de trás do globo ocular. A maior concentração de nitidez visual ocorre em uma pequena região especializada da retina chamada **fóvea**.

### Fotorreceptores: Cones vs. Bastonetes

A retina contém dois tipos distintos de células fotorreceptoras que convertem energia luminosa em sinais neurais elétricos:

* **Cones (Visão fotópica):** Funcionam em luz brilhante, mediam a visão de cores e fornecem alta acuidade espacial porque cada cone se conecta individualmente à sua própria terminação nervosa dedicada.

* **Bastonetes (Visão escotópica):** Lidam com visão em pouca luz e percepção periférica. Eles não conseguem distinguir cores e fornecem detalhes espaciais muito menores porque vários bastonetes compartilham uma única fibra nervosa.

Antes que os sinais viajem pelo nervo óptico para o córtex visual no cérebro, os neurônios locais (células bipolares e ganglionares) realizam a integração preliminar do sinal e a filtragem diretamente na retina.

### Ilusões Psicológicas e Perceptivas

Como a visão humana depende do processamento neural em vez de simples medidores de luz, nosso sistema visual é suscetível a distorções perceptivas previsíveis:

* **Adaptação de brilho logarítmico:** O olho humano se adapta a uma vasta gama de intensidades de luz, mas nossa percepção de brilho ($B$) é logarítmica e não linear em relação à energia luminosa incidente. Abaixo de um certo limite de adaptação ($B_b$), todos os estímulos escuros são percebidos como pretos indistinguíveis.

* **Bandas de Mach:** Ao visualizar barras verticais adjacentes de cinza uniforme com aumento gradual da intensidade, o sistema visual humano exagera o brilho nos limites. Isso cria faixas brilhantes e escuras percebidas ao longo das bordas, demonstrando que nossos olhos melhoram ativamente o contraste das bordas.

* **Contraste simultâneo:** O brilho percebido de um objeto é fortemente influenciado pelo seu fundo. Um quadrado cinza médio fixo parece significativamente mais claro quando cercado por um fundo escuro e mais escuro quando colocado sobre um fundo brilhante.

---

## 3. Física da Luz e Modelos de Cores: RGB vs. HSI

Compreender as cores requer combinar a mecânica física das ondas com a percepção visual.

### A Física da Luz Visível

A luz visível é a radiação eletromagnética dentro de uma estreita faixa de comprimento de onda, entre aproximadamente $430\text{ nm}$ e $790\text{ nm}$. A luz exibe dualidade onda-partícula:

* As **propriedades ondulatórias** explicam comportamentos físicos como reflexão, refração e interferência.

* As **propriedades corpusculares** explicam a quantização da energia dos fótons através da equação de Planck:

$$E = h \cdot \nu$$

onde $h = 6{,}63 \times 10^{-34} \text{ m}^2\text{kg/s}$ (constante de Planck) e $\nu$ é a frequência, relacionada à velocidade da onda $c \approx 2{,}998 \times 10^8 \text{ m/s}$ por $c = \lambda \cdot \nu$.

A **luz branca** consiste em uma mistura equilibrada de todos os comprimentos de onda visíveis. Quando a luz atinge um objeto, os comprimentos de onda refletidos determinam a cor percebida do objeto. Se um objeto reflete a luz uniformemente em todos os comprimentos de onda, ele aparece **acromático** (branco, cinza ou preto, dependendo da intensidade refletida).

A visão humana das cores depende de três tipos distintos de cones sensíveis a ondas curtas, médias e longas:

* **Cones S:** \~2% dos cones (sensíveis ao azul)

* **Cones M:** \~33% dos cones (sensíveis ao verde)

* **Cones L:** \~65% dos cones (sensíveis ao vermelho)

Mesmo que os cones S estejam em menor número, eles exibem a maior sensibilidade.

### O Modelo RGB Aditivo

Os computadores modelam a mistura de aditivos primários coloridos usando o modelo de cores RGB, que representa cores dentro de um cubo cartesiano 3D:

* Os eixos primários representam **Vermelho (R)**, **Verde (G)** e **Azul (B)** normalizados de $0{,}0$ a $1{,}0$ (ou inteiros de $0$ a $255$).

* A origem $(0, 0, 0)$ representa o preto.

* O vértice oposto $(1, 1, 1)$ representa o branco.

* As cores secundárias ficam nos vértices restantes: **Ciano** $(0, 1, 1)$, **Magenta** $(1, 0, 1)$ e **Amarelo** $(1, 1, 0)$.

* A linha diagonal reta que conecta o preto ao branco representa o **espectro em escala de cinza**.

### O Modelo HSI (Hue, Saturation, Intensity)

Embora o RGB seja ótimo para hardware de exibição, ele não é intuitivo para interpretação humana. Não descrevemos um carro vermelho especificando porcentagens de luz vermelha, verde e azul; descrevemos sua tonalidade, saturação e brilho. O modelo de cores HSI resolve isso desacoplando a intensidade do conteúdo cromático:

* **Matiz ($H$ - Hue):** Descreve o tom da cor pura (um ângulo que representa vermelho, amarelo, verde, ciano, azul ou magenta).

* **Saturação ($S$ - Saturation):** Mede a pureza da cor ou a quantidade de luz branca que dilui o tom puro.

* **Intensidade ($I$ - Intensity):** A medida acromática do brilho geral.

---

## 4. Digitalização: Amostragem, Quantização e Representação de Imagens

Para processar uma cena física e contínua em um computador, devemos converter sinais ópticos contínuos em uma matriz matemática discreta.

### Amostragem Espacial e Quantização de Intensidade

A digitalização envolve duas operações fundamentais:

1\. **Amostragem:** Discretiza as coordenadas espaciais contínuas $(x, y)$ de uma cena em uma grade regular de pontos discretos chamados **pixels**. A amostragem determina a resolução espacial da imagem (por exemplo, $1920 \times 1080$ pixels).

2\. **Quantização:** Discretiza os valores contínuos de intensidade luminosa (amplitude) em cada pixel em um conjunto finito de níveis discretos. A quantização determina a resolução de intensidade ou profundidade de bits (por exemplo, a quantização de 8 bits produz $2^8 = 256$ níveis de cinza variando de $0$ a $255$).

### Artefatos de Degradação

O que acontece quando a taxa de amostragem ou os níveis de quantização são reduzidos em excesso?

* **Resolução espacial reduzida:** Causa serrilhamento espacial (**aliasing**), bordas irregulares em forma de "dente de serra" em curvas e diagonais, perda de detalhes finos de textura e pixelização perceptível quando ampliada por interpolação do vizinho mais próximo.

* **Quantização de intensidade reduzida:** Causa contornos falsos, limites abruptos e não naturais em gradientes suaves (como céus ou tons de pele) porque o sistema não possui níveis de intensidade intermediários suficientes para renderizar transições contínuas.

### Representação Matemática em Código

Matematicamente, uma imagem acromática 2D (monocromática/escala de cinza) é representada como uma função matricial escalar $f(x, y)$, onde $x$ representa o índice da linha e $y$ o índice da coluna, retornando um único valor de nível de cinza.

Uma imagem colorida (RGB) é representada matematicamente como uma função vetorial:

$$f(x, y) = \big(f_r(x, y), \\, f_g(x, y), \\, f_b(x, y)\big)$$

Em código (como Python com NumPy e OpenCV), isso corresponde a uma matriz tensorial 3D de formato \`(altura, largura, 3)\`.

### Vizinhança de Pixel

Ao aplicar filtros espaciais (como suavização, nitidez ou detecção de bordas), os algoritmos operam em um pixel $p$ nas coordenadas $(x, y)$ utilizando seus vizinhos espaciais circundantes:

* **4-vizinhos ($N_4(p)$):** Os 4 pixels ortogonais que compartilham uma aresta: $(x+1, y)$, $(x-1, y)$, $(x, y+1)$ e $(x, y-1)$.

* **Vizinhos diagonais ($N_d(p)$):** Os 4 pixels nos cantos que se tocam diagonalmente: $(x+1, y+1)$, $(x+1, y-1)$, $(x-1, y+1)$ e $(x-1, y-1)$.

* **8-vizinhos ($N_8(p)$):** A união de $N_4(p)$ e $N_d(p)$, formando a vizinhança completa $3 \times 3$, totalizando 8 pixels circundantes.

---

## Guia de Revisão – Processamento de Imagens (Aulas 1–6)

**12 de outubro de 2026**

## Como Priorizar

Foque em entender, para cada técnica: **o objetivo, como funciona, quando usar e como interpretar o resultado.** Os Capítulos 3 e 6 do **Gonzalez & Woods** cobrem as Aulas 2 a 6 (transformações de intensidade, histogramas, filtragem espacial) e a parte de cores (Aula 1 e tonalidade de imagens coloridas). Os slides e o cronograma são o guia principal; o livro é o complemento.

| Nível | O que estudar | Onde |
| --- | --- | --- |
| **Prioridade Alta** | Transformações pontuais (negativo, brilho, log, exponencial, gamma, lineares por partes), limiarização, histograma e equalização, reamostragem (vizinho mais próximo e bilinear), filtragem espacial linear (correlação, convolução, padding, separabilidade), filtros box, gaussiano, mediana, laplaciano, gradiente/Sobel. | Aulas 2 a 6 |
| **Entender sem decorar** | Modelo HSI: o que são H, S e I, por que separar intensidade de cor e quando usar. **Não** memorizar as equações de conversão RGB $\leftrightarrow$ HSI. | Aulas 1 e 2 |
| **Menor aprofundamento** | Especificação de histogramas. | Aula 3 |
| **Contexto** | Sistema visual humano, espectro eletromagnético, sensores, bibliotecas (NumPy, Matplotlib, OpenCV). | Aula 1 |

Revise também os exercícios resolvidos em aula:

* Tabela de equalização (imagem $64 \times 64$ com 3 bits).
* Filtragem com o kernel de Sobel.
* Correlação vs. Convolução ($285$ vs. $165$).
* Decomposição de kernel separável.
* Filtro da mediana $3 \times 3$.

---

## Aula 1 – Fundamentos, Visão, Cor e Representação de Imagens

**Aula de contexto: focar no vocabulário e na base conceitual.**

* **Processamento de imagens vs. Visão computacional:** Processamento recebe imagem e devolve imagem (contraste, ruído, bordas). Visão computacional interpreta o conteúdo (detectar faces, classificar gato/cachorro).

* **Sistema visual humano:** Cones (6–7 milhões, cor, visão fotópica, concentrados na fóvea) vs. Bastonetes (75–150 milhões, sem cor, visão periférica e escotópica). O brilho percebido é uma função logarítmica da intensidade, justificando as transformações logarítmicas e gamma.

* **Ilusões relevantes para realce:** Bandas de Mach (o olho exagera diferenças perto de bordas) e contraste simultâneo (o brilho percebido depende do fundo). Por isso, a avaliação do realce é subjetiva.

* **Cor:** A percepção de cor vem da ativação relativa de 3 tipos de cones (L \~65%, M \~33%, S \~2%), definindo R, G, B como cores primárias.

* **Modelo RGB:** Cubo cartesiano com preto em $(0,0,0)$, branco em $(1,1,1)$ e escala de cinza na diagonal principal. Ciano, magenta e amarelo são as cores secundárias.

* **Modelo HSI:** Matiz ($H$), Saturação ($S$) e Intensidade ($I$). Sua principal utilidade é **desacoplar a intensidade da cor**, permitindo alterar brilho e contraste sem distorcer os tons.

* **Amostragem vs. Quantização:** Amostragem discretiza o espaço (resolução espacial, grade de pixels); quantização discretiza a intensidade (profundidade de bits). Reduzir a primeira causa pixelização; reduzir a segunda gera falsos contornos.

* **Vizinhança de pixel:** 4-vizinhos, vizinhos diagonais ($d$) e 8-vizinhos.

* **Ferramentas:** NumPy (manipulação de matrizes), Matplotlib (exibição) e OpenCV (leitura e processamento).

---

## Aula 2 – Transformações Pontuais (Intensidade e Brilho)

**Ideia central:** No domínio espacial, cada pixel de saída depende exclusivamente do pixel de entrada na mesma posição:

$$g(x,y) = T[f(x,y)] \quad\Longleftrightarrow\quad s = T(r)$$

O objetivo é o **melhoramento para visualização** (subjetivo e dependente da aplicação).

| Técnica | Como funciona | Quando usar / Interpretação |
| --- | --- | --- |
| **Negativo** | $s = 255 - r$. Em RGB, aplica-se a cada canal (vermelho $\leftrightarrow$ ciano, azul $\leftrightarrow$ amarelo, verde $\leftrightarrow$ magenta). | Realçar detalhes claros em regiões predominantemente escuras. |
| **Brilho (Soma/Subtração)** | $s = r \pm c$ | Mantém a diferença absoluta entre pixels. Em RGB, pode alterar a tonalidade das cores. |
| **Brilho (Multiplicação)** | $s = c \cdot r$ | Amplia a diferença proporcional. Em RGB, preserva as relações de cor. |
| **Brilho em HSI** | Altera apenas o canal $I$. | Preserva matiz ($H$) e saturação ($S$), evitando distorção de cor. |
| **Logarítmica** | $s = c \cdot \log(1 + r)$ | Expande tons escuros e comprime tons claros (clareia imagens **escuras**). |
| **Exponencial** | $s = c \cdot r^\gamma$ (com $\gamma > 1$) | Comprime tons escuros e expande claros (escurece imagens **claras**). |
| **Gamma** | $s = c \cdot r^\gamma$ | $\gamma < 1$ clareia (efeito log); $\gamma > 1$ escurece (efeito exponencial). |
| **Linear por partes** | Combina trechos lineares mapeados por pontos $(r_1, s_1), (r_2, s_2)$. | Expande o contraste em uma faixa específica e comprime no restante. |

> **Leitura do gráfico $T(r)$:** Curva **acima** da diagonal clareia a imagem; **abaixo** escurece. Trecho com **inclinação $> 1$** aumenta o contraste naquela faixa; **inclinação $< 1$** reduz.

**Exercício da aula (Alargamento de contraste):** Mapear $[r_{\min}, r_{\max}]$ para $[0, 255]$ através de uma reta:

$$s = \frac{255}{r_{\max} - r_{\min}} \cdot (r - r_{\min})$$

---

## Aula 3 – Cor, Limiarização e Histogramas

* **Correções tonais em imagens coloridas:** Para ajustar brilho/contraste preservando as cores, em RGB deve-se aplicar a **mesma transformação nos três canais**; em HSI, modifica-se **apenas o canal $I$**.

* **Limiarização de intensidade:**

* **Binária:** Transforma uma faixa de interesse em branco e o restante em preto.

* **Não-binária:** Altera apenas a faixa de interesse e preserva os demais níveis de cinza.

* **Limiarização de cor:** Define uma região no espaço RGB delimitada por um **cubo** (largura $W$) ou **esfera** (raio $R$ via distância euclidiana).

* **Histograma:** Contagem da frequência de pixels por nível de intensidade. A versão normalizada representa a PMF: $p(r_k) = \frac{n_k}{M \cdot N}$.

* **Imagem escura:** Histograma deslocado para a esquerda.

* **Imagem clara:** Histograma deslocado para a direita.

* **Baixo contraste:** Histograma concentrado/estreito.

* **Alto contraste:** Histograma bem distribuído e aproximadamente uniforme.

### Equalização de Histograma

Aumenta o contraste distribuindo os níveis de intensidade de forma aproximadamente uniforme usando a CDF escalada:

$$s_k = T(r_k) = (L-1) \sum_{j=0}^{k} p_r(r_j)$$

**Passo a passo do cálculo:**

1\. Calcular $p(r_k) = \frac{n_k}{\text{total}}$.

2\. Acumular os valores (CDF).

3\. Multiplicar por $L-1$ (para 3 bits, $L-1 = 7$).

4\. Arredondar para o inteiro mais próximo.

#### Exemplo Prático (Imagem $64 \times 64$, 3 bits, total = 4096 pixels):

| $r$ | $n_r$ | $p(r)$ | CDF | $7 \times \text{CDF}$ | Nível Arredondado |
| --- | --- | --- | --- | --- | --- |
| **0** | 790 | 0,19 | 0,19 | 1,33 | **1** |
| **1** | 1023 | 0,25 | 0,44 | 3,08 | **3** |
| **2** | 850 | 0,21 | 0,65 | 4,55 | **5** |
| **3** | 656 | 0,16 | 0,81 | 5,67 | **6** |
| **4** | 329 | 0,08 | 0,89 | 6,23 | **6** |
| **5** | 245 | 0,06 | 0,95 | 6,65 | **7** |
| **6** | 122 | 0,03 | 0,98 | 6,86 | **7** |
| **7** | 81 | 0,02 | 1,00 | 7,00 | **7** |

**Interpretação:** Níveis adjacentes podem se fundir (ex: $3$ e $4$ viram $6$). Em dados discretos, o resultado não fica perfeitamente plano, mas sim melhor distribuído.

* **Equalização em cores:** Converter para HSI, equalizar apenas o canal $I$ e reconstruir o RGB.

* **Equalização localizada:** Processa vizinhanças locais ($5 \times 5$, $11 \times 11$) usando janelas deslizantes para preservar detalhes em regiões menores.

---

## Aula 4 – Reamostragem e Fundamentos da Filtragem Espacial

### Reamostragem (Interpolação)

Processo de alterar a dimensão da imagem (zoom, redução ou transformações geométricas).

| Método | Funcionamento | Vantagens e Desvantagens |
| --- | --- | --- |
| **Vizinho Mais Próximo** | Copia o valor do pixel mais próximo da coordenada mapeada. | Rápido e simples; gera serrilhamento (**aliasing**) e pixelização. |
| **Bilinear** | Média ponderada dos 4 vizinhos mais próximos. | Transições suaves e boa qualidade; causa leve borramento. |
| **Bicúbica** | Utiliza uma janela de 16 vizinhos. | Maior precisão e nitidez; maior custo computacional (\`INTER_CUBIC\`). |

**Equação da Interpolação Bilinear:**

Com $a = r_m - r_0$ e $b = c_m - c_0$ (deslocamentos fracionários):

$$J = (1-a)(1-b)I_{00} + (1-a)b I_{01} + a(1-b)I_{10} + ab I_{11}$$

### Filtragem Espacial

Substitui o valor de um pixel por uma função calculada sobre sua vizinhança.

* **Filtros Passa-Baixas:** Suavizam/borram a imagem; atenuam altas frequências (ruído e bordas).

* **Filtros Passa-Altas:** Destacam variações abruptas (bordas e detalhes).

* **Tratamento de Bordas (Padding):** Zero padding, replicação de borda (**replicate**), espelhamento (**reflect**) e valor constante.

---

## Aula 5 – Correlação, Convolução, Separabilidade e Filtro Box

### Correlação vs. Convolução

* **Correlação:** Deslocamento do kernel sobre a imagem multiplicando e somando os elementos correspondentes.

* **Convolução:** Idêntica à correlação, porém com o kernel **rotacionado em 180°**.

* **Kernels Simétricos:** A correlação e a convolução produzem resultados idênticos (ex: Box, Gaussiano).

> **Propriedade fundamental:** A resposta ao impulso de uma convolução gera uma cópia idêntica do kernel na posição do impulso; na correlação, gera uma cópia rotacionada.

### Kernels Separáveis

Um kernel $W$ de dimensões $m \times n$ é separável se puder ser decomposto no produto de um vetor coluna por um vetor linha:

$$W = v_1 \cdot v_2^T$$

Isso reduz a complexidade computacional de $m \cdot n$ operações por pixel para $m + n$.

### Filtro Box (Média)

Kernel com todos os coeficientes iguais a $1$, normalizado pela soma (ex: $1/9$ em um kernel $3 \times 3$).

* Aumentar o tamanho da janela aumenta o borramento e reduz detalhes finos.

* Apresenta limitações direcionais (favorece eixos horizontais/verticais).

---

## Aula 6 – Filtro Gaussiano, Mediana e Filtros Passa-Altas

### Filtros Passa-Baixas (Continuação)

#### Filtro Gaussiano

Média ponderada baseada em uma distribuição normal 2D centrada:

$$G(x,y) = K \cdot e^{-\frac{x^2+y^2}{2\sigma^2}}$$

* $\sigma$ (desvio padrão) controla o grau de suavização.

* Regra prática para o tamanho da janela: $N = \lceil 6\sigma \rceil$.

* É **separável**, **circularmente simétrico** e suaviza a imagem de forma mais natural que o filtro Box.

#### Filtro da Mediana (Não Linear)

Substitui o pixel central pelo valor mediano da vizinhança ordenada.

* **Aplicação principal:** Remoção de ruído do tipo "sal e pimenta" (impulsivo).

* Preserva bordas com muito mais precisão do que os filtros de média e Gaussiano.

---

### Filtros Passa-Altas (Derivativos)

| Propriedade | 1ª Derivada | 2ª Derivada |
| --- | --- | --- |
| **Expressão Discreta** | $f(x+1) - f(x)$ | $f(x+1) + f(x-1) - 2f(x)$ |
| **Região Constante** | Zero | Zero |
| **Rampa de Intensidade** | Valor constante não-zero longo da rampa | Responde apenas no início e no fim da rampa |
| **Característica Visual** | Produz bordas mais largas | Destaca detalhes finos; mais sensível ao ruído |

#### Laplaciano (2ª Ordem - Aguçamento)

Operador isotrópico definido pela soma das segundas derivadas:

$$\nabla^2 f = f(x+1,y) + f(x-1,y) + f(x,y+1) + f(x,y-1) - 4f(x,y)$$

Para aguçar uma imagem usando o Laplaciano:

$$g(x,y) = f(x,y) + c \cdot \nabla^2 f(x,y)$$

**(onde $c = -1$ se o elemento central do kernel for negativo, e $c = +1$ se for positivo).**

#### Operador de Gradiente (1ª Ordem - Sobel)

Mede a variação espacial de intensidade por meio da magnitude do vetor gradiente:

$$M(x,y) = \sqrt{g_x^2 + g_y^2} \approx \vert{}g_x\vert{} + \vert{}g_y\vert{}$$

* O operador **Sobel** utiliza máscaras $3 \times 3$ com ponderação central para aproximar as derivadas parciais $g_x$ e $g_y$, integrando uma leve suavização perpendicular para mitigar o ruído.