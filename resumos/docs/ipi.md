# Introdução ao Processamento de Imagens

> Este documento apresenta os conceitos fundamentais do processamento de imagens, incluindo técnicas de filtragem, segmentação e reconhecimento de padrões.

---

## Sumário
[Unidade 1: Fundamentos de Imagens Digitais](#unidade-1-fundamentos-de-imagens-digitais)

[Unidade 2: Transformações de Imagens](#unidade-2-transformacoes-de-imagens)

[Unidade 3: Filtragem Espacial](#unidade-3-filtragem-espacial)

[Unidade 4: Domínio da Frequência](#unidade-4-dominio-da-frequencia)

[Unidade 5: Morfologia e Segmentação](#unidade-5-morfologia)

[Unidade 6: Análise e Classificação](#unidade-6-analise-e-classificacao)

## Unidade 2: Transformações de Imagens

### Aula 4 - Reamostragem e Interpolação

-> O valor estimado é sempre igual ao da sua amostra mais próxima.

-> Dada a imagem original I, com dimensões Ri x Ci, e a imagem reamostrada J, com dimensões Ro x Co. Se J é maior que I, temos uma ampliação (zoom).
Caso contrário, temos uma redução (shrink). Procedimento:

    - Mapeamos pixels com coordenadas (ro, co) de J para posições (rm, cm) de I.
    - Atribuímos a cada pixel (ro, co) de J o valor do pixel mais próximo de I, ou seja, J(ro, co) = I(round(rm), round(cm)).

Matematicamente:

rm = ro * [(Ri / Ro)] escala
cm = co * [(Ci / Co)] escala

### Interpolação Bilinear

-> 