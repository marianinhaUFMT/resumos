# Introdução ao Processamento de Imagens

> Você sabia que quando vemos um quadrado cinza sobre um fundo preto, seu cérebro na verdade te engana fazendo-o perceber que ele é visivelmnte mais brilhante do que o mesmo quadrado colocado sobre um fundo branco? Ou que antes mesmo que um único sinal visual atinja o córtex visual do seu cérebro, a retina do seu olho já realizou uma filtragem espacial complexa nos raios de luz que atingem a sua fóvea? O processamento digital de imagens preenche a lacuna entre a percepção humana e os algoritmos computacionais, permitindo que as máquinas limpem, transforme e analisem dados visuais.

---

Nesta lição, exploraremos os princípios fundamentais do processamento de imagens comparando como os humanos percebem a luz visual com a forma como os computadores digitalizam e manipulam imagens. Ao final desta lição, você será capaz de:

- **Diferenciar** entr Processamento Digital de Imagens e Visão Computacional com base em suas entradas, saídas e objetivos analíticos.
- **Analisar** a mecânica da percepção visual humana, incluindo a funcionalidade dos fotorreceptores (cones vs. bastonetes) e fenômenos perceptivos, como bandas de Mach e contraste simultâneo.
- **Comparar** os modelos de cores RGB e HSI para explicar por que desacoplar a intensidade da informação cromática simplifica o aprimoramento da imagem.
- **Avaliar** os processos matemáticos de amostragem espacial e quantização de intensidade, identificando os artefatos visuais causados pela degradação da resolução.

## Processamento de imagens vs. visão computacional e arquitetura de sistemas

Para construir uma base sólida, precisamos primeiro esclarecer uma fonte comum de confusão: **Qual a diferença entre Processamento Digital de Imagens e Visão Computacional?**

Embora esses campos estejam intimamente interligados, eles desempenham papéis distintos no pipeline da computação visual:

- **Processamento Digital de Imagens**: Fconcentra-se em algoritmos onde ambos **entrada** e **saída são imagens**. O objetivo principal é transformar ou melhorar uma imagem para posterior visualização humana ou análise de máquina. Exemplos incluem remover ruído de fundo, afiar bordas borradas, inverter valores de pixels para criar mamografias negativas ou ajustar o contraste geral.
- **Visão Computacional**: por outro lado, concentra-se emm **interpretar** e **compreender** o conteúdo de imagem ou vídeo. A entrada é uma imagem visual, mas a saída é informação de alto nível ou metadados. Exemplos incluem detectar pontos de referência faciais, dirigir veículos autônomos identificando obstáculos na estrada ou realizar classificação binária para determinar se uma imagem contém um gato ou um cachorro.

### O pipeline de hardware e software

Como uma cena visual abstrata se transforma em uma matriz digital na tela do seu computador? Um completo **sistema de processamento de imagens** depende de seis componentes principais interconectados:

1. **Sensores**: dispositivos físicos que capturam a energia eletromagnética recebida (luz) refletida em uma cena.
2. **Hardwares especializados**: módulos de hardware dedicados que lidam com conversão analógico-digital, captura rápida de quadros e supressão preliminar de ruído em nível de hardware
3. **Computador**: o mecanismo de processamento, que vai de computadores pessoais a servidores distribuídos em nuvem, executa lógica algorítmica.
4. **Software**: bibliotecas ambientais, como CV aberto, Numpy e Matplotlib em Python, que realizam operações matemáticas em matrizes de imagens.
5. **Armazenamento em massa e memória**: infraestrutura de armazenamento de alta capacidade necessária para lidar com amtrizes massivas de pixels não compactados.
6. **Exibições e dispositivos de saída**: monitores de alta resolução ou sistemas de impressão calibrados para exibir resultados visuais a observadores humanos.

Crucialmente, o aprimoramento da imagem é subjetivo e dependente do contexto. O que conta como uma imagem **melhorada** depende inteiramente de quem ou o que está consumindo a saída final. Na mamografia digital, o aprimoramento signfica aumentar o contraste em torno das microcalcificações para que um radiologista possa detectar lesões precoces; na fotografia do consumidor, pode significar simplesmente fazer com que o azul do céu pareça mais vívido.

## O sistema visual humano e os fenômenos da percepção visual
Como as imagens digitais são, em última análise, projetadas para o consumo humano, algoritmos eficazes de processamento de imagens devem levar me conta a mecânica biológica da visão humana.

### Arquitetura anatômica do olho
A luz entra no olho humano através da córnea e passa pela pupila, uma abertura cujo diâmetro é expandido ou contraído dinamicamente pela íris. O cristalino em seguida refrata os raios de luz incidentes, concentrando-os na retina na parte de trás do globo ocular. A maior concentração de nitidez visual ocorre em uma pequena região especializada da retina chamada retina fóvea.

### Fotorreceptores: cones vs. bastonetes
A retina contém dois tipos distintos de células fotorreceptoras que convertem energia luminosa em sinais neurais elétricos:

- **Cones (Visão fotópica)**: funcionam em luz brilhante, mediam a visão de cores e fornecem alta acuidade espacial porque cada cone se conecta individualmente à sua própria terminação nervosa dedicada.
- **Bastonetes (Visão escotópica)**: as hastes lidam com visão em pouca luz e percepção periférica. Eles não conseguem distinguir cores e fornecem detalhes espaciais muito menores porque vários bastonetes compartilham uma única fibra nervosa.

Antes que os sinais viajem pelo nervo óptico para o córtex visual no cérebro, os neurônios locais (células bipolares e ganglionares) realizam a integração preliminar do sinal e a filtragem diretamente na retina.

### Ilusões psicológicas e perceptivas
Como a visão humana depende do processamento neural em vez de simples medidores de luz, nosso sitesma visual é suscetível a distorções perceptivas previsíveis:

- **Adaptação de brilho logarítmico**: o olho humano se adapta a uma vasta gama de intensidades de luz, mas nossa percepção de brilho (B) é logarítmico e não linear em relação à energia luminosa incidente. Abaixo de um certo limite de adaptação(Bb), todos os estímulos escuros são eprcebidos como pretos indistinguívies.
- **Bandas Mach**: ao visualizar barras verticais adjacentes de cinza uniforme com aumento gradual da intensidade, o sistema visual humano ultrapassa e compreende o brilho nos limites. Isso cria faixas brilhantes e escuras percebidas ao longo das bordas, demonstrando que nossos olhos melhoram ativamente o contraste das bordas.
- **Contraste simultâneo**: o brilho percebido de um objeto é fortemente influenciado pelo seu fundo. Um quadrado cinza médio fixo parece significativamente mais claro quando cercado por um fundo escuro e mais escuro quando colocado sobre um fundo brilhante.

## Física da luz e modelos de cores: RGB vs. HSI
Compreender as cores requer combinar a mecânica física das ondas com a percepção visual.

### A física da luz visível

A luz visível é a radiação eletromagnética dentro de uma estreita faixa de comprimento de onda, entre aproximadamente 430nm e 790nm. Exposições leves dualidade onda-partícula:
- É propridades das ondas explicar comportamento físico, como reflexão, refração e interferência.
- É propriedades das partículas explicar a quantização da energia dos fótons através da equação de Planck:

$$E = h \cdot \nu$$

onde $h = 6.63 \times 10^{-34} \text{ m}^2\text{kg/s}$  (constante de Planck) e  $\nu$ é a frequência, relacionada a velocidade da onda $c \approx 2.998 \times 10^8 \text{ m/s}$ com $c = \lambda \cdot \nu$.

**Luz branca** consiste em uma mistura equilibrada de todos os comprimentos de onda visíveis (como um acorde musical completo). Quando a luz atinge um objeto, os comprimentos de onda refletidos determinam a cor percebida do objeto. Se um objeto reflete a luz uniformemente em todos os comprimentos de onda, ele aparece acroma´tico (branco, cinza ou preto dependendo da intensidade refletida).

A visão humana das cores depende de três tipos distintos de cones sensíveis a ondas curtas, médias e longas: Cones S (2% dos cones, sensíveis ao azul), Cones M (33% dos cones, sensíveis ao verde) e Cones L (65% dos cones, sensíveis ao vermelho). Mesmo queos cones S sejam suficientes em número, eles exibem a maior sensibilidade.

### O modelo RGB aditivo
Os computadores modelam a mistura de aditivos primários coloridos usando o modelo de cores RGB, que representa cores dentro de um cubo cartesiano 3D:

- Os eixos primários representam vermelho (R), verde (G) e azul (B) normalizado de 0,0 para 1,0 (ou inteiros 0 para 255).
- A origem (0, 0, 0) representa preto.
- O vértice oposto (1, 1, 1) representa branco.
- As cores secundárias ficam nos vértices restantes: ciano (0, 1, 1), magenta (1, 0, 1) e amarelo (1, 1, 0).
- A linha diagonal reta que conecta o preto ao branco representa o Espectro em escala de cinza.

### O modelo HSI (Hue, Saturation, Intensity): desacoplando intensidade de cor
Embora o RGB seja ótimo para hardware de exibição, ele não é intuitivo para interpretação humana. Os humanos não descrevem um carro vermelho especificando porcentagens de luz vermelha, verde e azul; nós descrevemos sua tonalidade, saturação e brilho. O modelo de cores HSI resolve isso desacoplando a intensidade do conteúdo cromático:
- **Matiz (H)**: descreve o tom da cor pura (um ângulo representa vermelho, amarelo, verde, ciano, azul ou magenta).
- **Saturação (S)**: mede a pureza da cor ou a quantidade de luz branca que dilui o tom puro.
- **Intensidade (I)**: a medida acromática do brilho geral.

## Digitalização: Amostragem, quantização e representação de imagens
Para processa uma cena física e contínua em um computador, devemos converter sinais ópticos contínuos em uma matriz matemática discreta.

### Amostragem espacial e quantização de intensidade
A digitalização envolve duas operações fundamentais:

1. **Amostragem**: discretiza as coordenadas espaciais contínuas $(x, y)$ de uma cena em uma grade regular de pontos discretos chamados **pixels**. A amostragem determina a resolução espacial da imagem (por exemplo, 1920 x 1080 pixels).
2. **Quantização**: discretiza os valores contínuos de intensidade luminosa (amplitude) em cada pixel em um conjunto finito de níveis discretos. A quantização determina a resolução de intensidade ou profundidade de bits (por exemplo, a quantização de 8 bits produz $2^8 = 256$ níveis de cinza variando de 0 a 255).

### Artefatos de degradação
O que acontece quando a taxa de amostragem ou os níveis de quantização são reduzidos em excesso?
- **Resolução espacial reduzida**: causa serrilhamento espacial, bordas irregulares em forma de "dente de serra" em curvas/diagonais, perda de detalhes finos de textura e pixelização perceptível quando ampliada usando interpolação de vizinho mais próximo.
- **Quantização de intensidade reduzida**: causa contornos falsos, limites abruptos e não naturais em gradientes suaves (como céus ou tons de pele) porque o sistema não possui níveis de intensidade intermediários suficientes para renderizar transições contínuas.

### Representação matemática em código
Matematicamente , uma imagem acromática 2D (monocromática/em tons de cinza) é representada como uma função matricial escalar. $f(x, y)$, onde $x$ representa o índice da linha e $y$ representa o índice da coluna, retornando um único valor de nível de cinza.

Uma imagem colorida (RGB) é representada matematicamente como uma função vetorial:
$$f(x, y) = \big(f_r(x, y), \, f_g(x, y), \, f_b(x, y)\big)$$

Em código (como Python com NumPy e OpenCV), isso corresponde a uma matriz tensorial 3D de formato $(height, width, 3)$.

### Barros de pixel
Ao aplicar filtros espaciais (como suavização, nitidez ou detecção de bordas), os algoritmos operam em um pixel $p$ nas coordenadas $(x, y)$ utilizando seus vizinhos espaciais circundantes:

- **4-vizinhos ($N_4(p)$)**: os 4 pixels ortogonais que compartilham uma aresta:$(x+1, y)$, $(x-1, y)$, $(x, y+1)$, e $(x, y-1)$.
- **Bairro diagonal ($N_d(p)$)**: os 4 pixels dos cantos se tocam diagonalmente:$(x+1, y+1)$, $(x+1, y-1)$, $(x-1, y+1)$, and $(x-1, y-1)$.
- **8-vizinhos ($N_8(p)$)**: a união de $N_4(p)$ e $N_d(p)$, formando o completo $3x3$, totalizando 8 pixels circundantes.

# Guia de revisão – Processamento de Imagens (Aulas 1–6)

Oct 12, 2026 · @Mariana

## Como priorizar

Foque em entender, para cada técnica, o objetivo, como funciona, quando usar e como interpretar o resultado. Os Capítulos 3 e 6 do Gonzalez & Woods cobrem as Aulas 2 a 6 (transformações de intensidade, histogramas, filtragem espacial) e a parte de cor (Aula 1 e tonalidade de imagens coloridas); os slides e o cronograma são o guia, o livro é complemento.

| Nível | O que estudar | Onde |
| --- | --- | --- |
| Prioridade alta | Transformações pontuais (negativo, brilho, log, exponencial, gamma, lineares por partes), limiarização, histograma e equalização, reamostragem (vizinho mais próximo e bilinear), filtragem espacial linear (correlação, convolução, padding, separabilidade), box, gaussiano, mediana, laplaciano, gradiente/Sobel | Aulas 2 a 6 |
| Entender sem decorar | Modelo HSI: o que são H, S e I, por que separar intensidade de cor e quando usar. **Não** memorizar as contas RGB ↔ HSI | Aulas 1 e 2 |
| Menor aprofundamento | Especificação de histogramas | Aula 3 |
| Contexto | Sistema visual humano, espectro, sensores, bibliotecas (NumPy, Matplotlib, OpenCV) | Aula 1 |

Revise também os exemplos passo a passo feitos em aula: a tabela de equalização (imagem 64×64 com 3 bits), a filtragem com o kernel de Sobel, correlação × convolução (285 × 165), o kernel separável e a mediana de 3×3. O que cai é o procedimento e a interpretação do resultado.

## Aula 1 – Fundamentos, visão, cor e representação de imagens

Aula de contexto: o que importa para a prova é o vocabulário e a base conceitual que as aulas seguintes usam.

- **Processamento de imagens × visão computacional.** Processamento recebe imagem e devolve imagem (contraste, ruído, bordas). Visão computacional interpreta o conteúdo (detectar faces, classificar gato/cachorro).
- **Sistema visual humano.** Cones (6–7 milhões, cor, visão fotópica, concentrados na fóvea) × bastonetes (75–150 milhões, sem cor, visão periférica e escotópica). O brilho percebido é uma função logarítmica da intensidade, o que justifica as transformações log e gamma.
- **Ilusões que importam para realce.** Bandas de Mach (o olho exagera diferenças perto de bordas) e contraste simultâneo (o brilho percebido depende do fundo). Por isso a avaliação de realce é subjetiva.
- **Cor.** A cor percebida vem da ativação relativa de 3 tipos de cone (L \~65%, M \~33%, S \~2%), daí R, G, B como primárias.
- **RGB.** Cubo com preto em (0,0,0), branco em (1,1,1) e cinzas na diagonal; ciano, magenta e amarelo são as secundárias.
- **HSI (entender, não decorar a conta).** Matiz (cor pura), saturação (quão diluída em branco) e intensidade (brilho). Sua utilidade: **desacopla intensidade de cor**, então dá para mexer em brilho e contraste sem alterar as cores.
- **Amostragem × quantização.** Amostragem discretiza o espaço (resolução espacial, grade de pixels); quantização discretiza a intensidade (profundidade de bits). Reduzir a primeira pixeliza; reduzir a segunda causa falsos contornos.
- **Vizinhança de pixel:** 4, d (diagonais) e 8. Reaparece em toda filtragem.
- **Ferramentas:** NumPy (arrays), Matplotlib (exibir), OpenCV (ler e processar). Imagem como matriz f(x, y).

## Aula 2 – Transformações pontuais (intensidade e brilho)

**Ideia central:** no domínio espacial, cada pixel de saída depende só do pixel de entrada na mesma posição. Para imagens em cinza:

```latex
g(x,y) = T[f(x,y)] \quad\Longleftrightarrow\quad s = T(r)
```

O objetivo aqui é **melhoramento para visualização**, e o julgamento do resultado é subjetivo e depende da aplicação (realce de estruturas em imagem médica, correção de fotos escuras ou claras).

| Técnica | Como funciona | Quando usar / como interpretar |
| --- | --- | --- |
| Negativo | s = 255 − r (0↔255). Em RGB, aplicado em cada canal: vermelho vira ciano, azul vira amarelo, verde vira magenta | Realçar detalhes escondidos em regiões escuras |
| Brilho (soma/subtração) | s = r ± c | Mantém a diferença absoluta entre pixels; em RGB pode alterar a cor |
| Brilho (multiplicação) | s = c · r | Amplia a diferença proporcional; em RGB preserva a cor |
| Brilho em HSI | Altera só o canal I | Preserva matiz e saturação, por isso a cor não muda |
| Logarítmica | s = c · log(1 + r) | Expande tons escuros e comprime claros: clareia imagem **escura** |
| Exponencial | s = c · r^γ com expoente que comprime escuros | Efeito oposto: escurece, recupera detalhe em imagem **clara** |
| Gamma | s = c · r^γ | γ < 1 clareia (como log); γ > 1 escurece (como exponencial) |
| Linear por partes | Combina trechos lineares (pontos (r1,s1), (r2,s2)…) | Expande contraste só numa faixa e comprime o resto; forma arbitrária |

Caminho de leitura do gráfico T(r): curva **acima** da diagonal clareia, **abaixo** escurece, trecho **mais inclinado que 1** aumenta o contraste naquela faixa, **mais plano** reduz.

**Exercício da aula (alargamento de contraste).** Mapear \[r\_min, r\_max\] para \[0, 255\] com uma reta:

```latex
s = \frac{255}{r_{max}-r_{min}}\,(r - r_{min})
```

Se a imagem usava só parte da faixa, o resultado usa os 256 níveis e o contraste sobe.

**Modelo HSI nesta aula:** a conversão RGB↔HSI aparece, mas você só precisa saber que existe, que separa I de H e S, e por que isso permite ajustar brilho sem distorcer a cor.

## Aula 3 – Cor, limiarização e histogramas

**Correções tonais em imagem colorida.** Antes de mexer em saturação, equilibre brilho e contraste preservando a matiz. Em RGB aplica-se a **mesma transformação nos três canais**; em HSI modifica-se **só o canal I**. Padrões típicos: imagem opaca (baixo contraste) → curva sigmoide; imagem clara → exponencial/gamma > 1; imagem escura → logarítmica/gamma < 1.

**Limiarização de intensidade.** Destaca uma faixa de interesse. Método 1: faixa vira um tom (branco), o resto outro (preto), gerando imagem **binária**. Método 2: clareia ou escurece a faixa e **mantém** os demais níveis. Exemplo da aula: angiografia, para isolar vasos sanguíneos.

**Limiarização de cor.** Define-se uma região do espaço RGB com centro e tamanho: um **cubo** (largura W) ou uma **esfera** (raio R). Pixels dentro da região são mantidos; fora, descartados. A esfera usa distância euclidiana até a cor de referência.

**Histograma.** Contagem de pixels por nível de intensidade; normalizado vira a PMF p(r\_k) = n\_k / (M·N). Em RGB, um histograma por canal.

- Imagem escura: histograma deslocado para a esquerda. Clara: para a direita. Baixo contraste: estreito. **Alto contraste: espalhado e aproximadamente uniforme.**
- Cuidado: imagens muito diferentes podem ter o mesmo histograma (o histograma não diz onde estão os pixels).

**Equalização de histograma (alta prioridade).** Objetivo: automatizar o ganho de contraste levando a distribuição para algo próximo do uniforme. A transformação é a CDF escalada:

```latex
s_k = T(r_k) = (L-1)\sum_{j=0}^{k} p_r(r_j)
```

Procedimento (o que a prova cobra):

1. Calcule p(r\_k) = n\_k / total.
2. Some cumulativamente (CDF).
3. Multiplique por L−1 (aqui L = 8, então × 7).
4. Arredonde para o inteiro mais próximo.

Exemplo da aula (imagem 64×64, 3 bits, total 4096 pixels):

| r | n\_r | p(r) | CDF | 7·CDF | Arredondado |
| --- | --- | --- | --- | --- | --- |
| 0 | 790 | 0,19 | 0,19 | 1,33 | 1 |
| 1 | 1023 | 0,25 | 0,44 | 3,08 | 3 |
| 2 | 850 | 0,21 | 0,65 | 4,55 | 5 |
| 3 | 656 | 0,16 | 0,81 | 5,67 | 6 |
| 4 | 329 | 0,08 | 0,89 | 6,23 | 6 |
| 5 | 245 | 0,06 | 0,95 | 6,65 | 7 |
| 6 | 122 | 0,03 | 0,98 | 6,86 | 7 |
| 7 | 81 | 0,02 | 1,00 | 7,00 | 7 |

Interpretação: níveis diferentes podem se fundir (4 e 3 viram 6; 5, 6 e 7 viram 7), então o histograma resultante **não fica perfeitamente plano** em imagens discretas, mas fica mais espalhado. Os níveis mais frequentes ganham mais espaço.

**Equalização em imagem colorida.** Não equalize R, G e B separadamente (as cores ficam erradas). Converta para HSI, equalize só **I** e mantenha H e S.

**Equalização localizada.** A global usa todos os pixels e pode amplificar ruído ou não revelar detalhes pequenos. A local equaliza em uma vizinhança (5×5, 11×11…) percorrendo pixel a pixel; nas bordas precisa de padding.

**Especificação de histograma (baixa prioridade).** Em vez de uma distribuição uniforme, força a imagem a ter o histograma de uma referência (ou de uma curva desejada). Saiba a ideia: equalizar a imagem, equalizar a referência e compor as duas transformações. Não precisa de mais profundidade que isso.

## Aula 4 – Reamostragem e fundamentos da filtragem espacial

### Reamostragem (interpolação)

Reamostrar é aumentar ou diminuir o número de pixels (zoom, redução, correções geométricas). Como não há dado nas posições novas, **interpola-se** a partir dos vizinhos conhecidos. Procedimento geral: para cada pixel (r\_o, c\_o) da imagem de saída, mapeie para a posição correspondente (r\_m, c\_m) na imagem de entrada (proporcional ao fator de escala) e estime o valor ali.

| Método | Como estima | Prós e contras |
| --- | --- | --- |
| Vizinho mais próximo | Copia o valor do pixel mais próximo de (r\_m, c\_m) | Rápido e simples; causa serrilhado (aliasing), pixelização em ampliação e perda de detalhe em redução |
| Bilinear | Média ponderada dos 4 vizinhos (r0,c0), (r0,c0+1), (r0+1,c0), (r0+1,c0+1) | Transições suaves, melhor qualidade visual; leve borramento |
| Bicúbico | Usa 16 vizinhos | Citado como o terceiro método principal (OpenCV: INTER\_CUBIC) |

**Como a bilinear funciona (passo a passo):** interpole linearmente na direção x entre os pares superiores e inferiores, depois interpole linearmente na direção y entre esses dois resultados. Com a = r\_m − r0 e b = c\_m − c0 (frações entre 0 e 1):

```latex
J = (1-a)(1-b)\,I_{00} + (1-a)\,b\,I_{01} + a\,(1-b)\,I_{10} + a\,b\,I_{11}
```

Interpretação: quanto mais perto o ponto está de um vizinho, maior o peso dele; no centro exato o resultado é a média dos quatro. Os valores novos ficam sobre as retas que ligam os originais.

**OpenCV:** `cv2.resize(src, dsize, fx=, fy=, interpolation=)`, com flags `INTER_NEAREST`, `INTER_LINEAR` (bilinear), `INTER_AREA` (boa para reduzir) e `INTER_CUBIC`. Pode-se passar `dsize` (tamanho final) ou fatores `fx`, `fy`.

### Filtragem espacial (início)

- **Transformação pontual × filtragem:** a pontual usa 1 pixel; a filtragem é uma operação **local**, que substitui o pixel por uma função dele e de seus vizinhos.
- **Passa-baixas:** suavizam (borram); atenuam altas frequências (bordas, ruído, detalhe fino). **Passa-altas:** realçam bordas, detalhes e ruído.
- **Linear × não linear:** box, gaussiano, Sobel e laplaciano são lineares; a mediana é não linear (estatística de ordem).
- **Mecânica da filtragem linear:** posicione o kernel (matriz de pesos) sobre a vizinhança, multiplique elemento a elemento e some. Exemplo da aula, kernel de Sobel horizontal no canto superior esquerdo:

```latex
(-1)(10)+0+(1)(22)+(-2)(20)+0+(2)(21)+(-1)(50)+0+(1)(8) = -28
```

- **Padding** (pixels extras em volta): preserva o tamanho da saída e trata as bordas. Tipos: zero, replicate (repete a borda), reflect (espelha) e constant (valor definido). A escolha afeta principalmente as bordas.

## Aula 5 – Correlação, convolução, separabilidade e filtro box

**Para que filtrar:** reduzir ruído sem destruir estruturas, atenuar detalhes irrelevantes e preparar a imagem para etapas como detecção de bordas ou segmentação.

**Correlação × convolução.**

- O procedimento da aula passada (deslizar o kernel e somar produtos) é a **correlação**.
- A **convolução** é igual, mas com o kernel **rotacionado 180°**.
- Com kernel **simétrico** os dois dão o mesmo resultado (box, gaussiano).
- Exemplo da aula: região e kernel com valores 1 a 9. Correlação = 1·1 + 2·2 + … + 9·9 = **285**. Convolução = 1·9 + 2·8 + … + 9·1 = **165**.
- Resposta ao impulso: a convolução deixa uma cópia **idêntica** do kernel na posição do impulso; a correlação deixa uma cópia **rotacionada**.
- Propriedades: a convolução é comutativa, associativa e distributiva; a correlação não é comutativa, então a ordem importa nela.

**Kernels separáveis.** Um kernel W (m×n) é separável se for o produto de um vetor coluna por um vetor linha: W = v₁ v₂ᵀ. Filtrar com ele equivale a **duas convoluções 1D** (uma por linhas, outra por colunas), o que reduz o custo de m·n multiplicações por pixel para m + n.

Exercício resolvido: o kernel \[1 0 3; 2 0 6; 3 0 9\] é separável porque as linhas são múltiplas da primeira. Decomposição: v₁ = \[1 2 3\]ᵀ e v₂ᵀ = \[1 0 3\]. A decomposição não é única, basta o produto reconstruir W. Teste prático: todas as linhas são múltiplas de uma só (posto 1).

**OpenCV:** `cv2.filter2D(src, ddepth, kernel, borderType)` e `cv2.sepFilter2D(src, ddepth, kernelX, kernelY, borderType)`. Com `ddepth = -1` a saída mantém a precisão da entrada.

**Filtros de suavização em geral.** Atenuam transições bruscas (ruído, detalhe, falsos contornos). São passa-baixas: regiões uniformes = baixa frequência; bordas, ruído e detalhe fino = alta frequência.

**Filtro box.** Kernel com todos os pesos iguais, normalizado para somar 1 (por exemplo 3×3 com 1/9 em cada posição): cada pixel vira a **média** da vizinhança.

- Quanto maior o kernel (3×3, 11×11, 21×21), maior o borramento e maior a perda de detalhe.
- Em imagem colorida, aplica-se **independentemente em cada canal**.
- O padding altera as bordas: zero padding escurece as bordas da imagem filtrada; espelhado (reflect) evita isso.
- Limitação: a resposta em frequência favorece as direções horizontal e vertical e modela mal imagens desfocadas por lente. Isso motiva o gaussiano.

## Aula 6 – Gaussiano, mediana e filtros passa-altas

### Passa-baixas (continuação)

**Gaussiano.** Média ponderada em que pixels mais próximos do centro pesam mais:

```latex
G(x,y) = K\,e^{-\frac{x^2+y^2}{2\sigma^2}}
```

- σ controla a distribuição dos pesos (σ pequeno concentra no centro, σ grande suaviza mais); o tamanho do kernel só define a janela. Exemplo: σ = 0,8 dá peso 0,64 ao centro; σ = 2,0 dá 0,16.
- Regra prática: kernel de ⌈ 6σ ⌉ × ⌈ 6σ ⌉, pois além de 3σ os valores são desprezíveis.
- É **separável** e **circularmente simétrico**; a convolução de duas gaussianas é outra gaussiana.
- Versus box: não favorece horizontal/vertical, modela melhor o desfoque de lente e suaviza de forma mais natural. Por isso é o mais usado.

**Mediana (não linear).** Troca o pixel pela mediana da vizinhança ordenada. Exemplo da aula: janela 3×3 com o valor 255 isolado; ordenando 9, 10, 10, 10, 11, 11, 12, 12, 255, a mediana é **11**, o extremo não domina. Ótimo contra **ruído impulsivo** (sal e pimenta), preservando bordas melhor que box e gaussiano. OpenCV: `cv2.medianBlur(src, ksize)` (padding por replicação).

### Passa-altas: derivadas

Passa-baixas são integrativos (médias); passa-altas são **derivativos**, enfatizam variações rápidas (bordas, detalhes, ruído).

|  | 1ª derivada | 2ª derivada |
| --- | --- | --- |
| Discreta (h = 1) | f(x+1) − f(x) (progressiva), f(x) − f(x−1) (regressiva) ou centrada | f(x+1) + f(x−1) − 2f(x) |
| Região constante | zero | zero |
| Rampa | continua diferente de zero ao longo da rampa | zero ao longo da rampa; responde no início e no fim |
| Efeito | bordas mais largas | detalhes finos; mais sensível a ruído |

### Laplaciano (2ª ordem, aguçamento)

Soma das segundas derivadas em x e y. Kernel clássico de vizinhança 4, e variantes com diagonais ou com sinais trocados:

```latex
\nabla^2 f = f(x{+}1,y)+f(x{-}1,y)+f(x,y{+}1)+f(x,y{-}1)-4f(x,y)
```

Para **aguçar**, soma-se o Laplaciano à original: g = f + c·∇²f, com c = −1 se o centro do kernel é negativo e c = +1 se positivo. A imagem laplaciana sozinha mostra só bordas e detalhes (a maior parte é zero ou próxima disso); somada à original, realça os detalhes sem perder o fundo.

**Máscara de nitidez (alternativa com suavização).** Máscara = original − suavizada; aguçada = original + k · máscara. Ideia: o que o borramento removeu são os detalhes, e somar de volta os realça.

### Gradiente (1ª ordem, bordas)

- O vetor gradiente (derivadas parciais gx e gy) aponta para a **direção de maior aumento** de intensidade; sua **magnitude** mede a força da variação:

```latex
M(x,y) = \sqrt{g_x^2 + g_y^2} \approx |g_x| + |g_y|
```

- **Sobel:** aproxima gx e gy com diferenças centradas e suavização perpendicular (kernel \[−1 0 1; −2 0 2; −1 0 1\] e sua versão girada). É separável.
- Em imagem colorida há um gradiente por canal.

**Laplaciano × gradiente para aguçar.** Em geral o Laplaciano é preferido; o gradiente reforça bordas espessas, que às vezes desagradam visualmente. O gradiente brilha em **inspeção industrial** (realce de defeitos e remoção de fundo que varia devagar), tipicamente como pré-processamento.

**Combinação de métodos (exemplo do raio-x).** Imagem com faixa dinâmica estreita e muito ruído: usa-se Laplaciano para detalhes finos, gradiente para bordas proeminentes e depois transformação de intensidade para o contraste final. A lição: nenhuma técnica resolve sozinha, e cada uma cuida de um aspecto.