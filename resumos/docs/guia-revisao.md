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

## Autoteste e checklist

Tente responder sem olhar o guia; marque o que já domina.

- [ ] Qual a diferença entre transformação pontual e filtragem espacial?
- [ ] Por que o negativo ajuda a ver detalhes em regiões escuras? O que ele faz com as cores em RGB?
- [ ] Quando usar soma/subtração e quando multiplicação para ajustar brilho em RGB? E por que HSI evita mudar a cor?
- [ ] Qual transformação (log, exponencial, gamma) usar para imagem escura? E para imagem clara?
- [ ] Dada uma tabela de n\_r, calcule p(r), CDF, s e o arredondamento da equalização. O que muda no histograma?
- [ ] Por que não equalizar R, G e B separadamente? O que fazer em vez disso?
- [ ] Quando a equalização local é melhor que a global?
- [ ] Nearest neighbor × bilinear: como cada um calcula o valor, e quais artefatos aparecem?
- [ ] Calcule uma saída de filtragem 3×3 à mão (como o exemplo com Sobel). Como o padding muda as bordas?
- [ ] Correlação × convolução: quando dão o mesmo resultado? Calcule os dois para um kernel assimétrico.
- [ ] Um kernel é separável? Decomponha e diga o ganho de custo.
- [ ] Box × gaussiano: diferenças, efeito de aumentar o kernel e o papel de σ.
- [ ] Para que tipo de ruído a mediana é indicada e por quê? Calcule a mediana de uma janela.
- [ ] 1ª × 2ª derivada: o que cada uma faz em degrau e em rampa? Qual é mais sensível a ruído?
- [ ] Como aguçar com Laplaciano? Qual o sinal certo de c conforme o kernel? O que é máscara de nitidez?
- [ ] O que o gradiente e sua magnitude representam? Quando preferir gradiente a Laplaciano?
- [ ] Para uma imagem dada (ruído, pouco contraste, borrada), qual sequência de técnicas você escolheria e por quê?

Sugestão de ordem de estudo: Aula 2 e 3 (tabela de equalização), depois Aulas 4 a 6 refazendo à mão os exemplos numéricos e rodando os códigos de OpenCV, e por fim a leitura dos capítulos 3 e 6 do livro nos pontos em que os slides ficaram curtos.
