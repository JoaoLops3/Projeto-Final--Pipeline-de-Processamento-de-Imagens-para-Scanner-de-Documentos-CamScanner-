# Roteiro dos slides — CP701TIN3 Projeto Final

> Copie cada bloco para um slide no PowerPoint, Google Slides ou Canva.  
> Exporte o conjunto como **PDF** para entrega.  
> **Imagens sugeridas** estão indicadas com caminhos relativos à pasta `Projeto_Final/`.

---

## Slide 1 — Capa

**Título:**  
Pipeline de Processamento de Imagens para Scanner de Documentos (CamScanner Clone)

**Subtítulo:**  
CP701TIN3 — Processamento de Imagens Digitais

**Opcional (rodapé):**  
Instituição · Ano/semestre 2026

---

## Slide 2 — Integrantes

*(Preencha um nome por linha — o professor exige RA e turma T1, T2 ou T3)*

| Nome completo | RA | Turma |
|---------------|-----|-------|
| Nome Integrante 1 | 0000000 | T_ |
| Nome Integrante 2 | 0000000 | T_ |
| … | … | … |

**Exemplo de formato no slide (texto simples):**

```
João Silva — RA 1234567 — Turma T1
Maria Souza — RA 7654321 — Turma T1
```

---

## Slide 3 — Resumo do artigo / trabalho / aplicação

### O que é o “X”?

Trabalhamos com **dois referenciais complementares**:

1. **CamScanner** — aplicativo mobile (INTSIG) para fotografar documentos, corrigir perspectiva, realçar contraste e gerar PDF. Usado em escolas, escritórios e arquivo pessoal de recibos/notas.

2. **Python-Document-Scanner-OpenCV** — programa open source em Python/OpenCV que **replica esse fluxo sem IA**: detecta o papel na foto, retifica e melhora a legibilidade.  
   - Autor: Arash Nasr Esfahani (2025)  
   - Repositório: https://github.com/ArashNasrEsfahani/Python-Document-Scanner-OpenCV  

**Referência clássica adicional:** tutorial *Mobile Document Scanner* (Adrian Rosebrock, PyImageSearch, 2014) — mesmo pipeline educacional.

### Onde é utilizado?

- Digitalização de provas, listas de exercícios e apostilas  
- Arquivo de notas fiscais e contratos em PDF  
- Pré-processamento antes de OCR (reconhecimento de caracteres)

### Ilustrações (incluir 1 ou 2 no slide)

| Figura | Arquivo / origem |
|--------|------------------|
| Entrada — foto inclinada do documento | `img/image.png` |
| Saída — documento “escaneado” | `resultado_final/resultado_scan.jpg` |

*Opcional:* screenshot da interface do CamScanner ou do README do GitHub com antes/depois.

---

## Slide 4 — Elementos de processamento de imagens

*(O professor pede **mais de uma** técnica por categoria e **detalhes de parâmetros** para nota máxima.)*

### Visão geral do pipeline

```
Original → Cinza → Blur → Canny → Contornos (4 pts) → Perspectiva → Realce → Scan final
```

**Figura sugerida:** captura de tela do notebook com os subplots (cinza, blur, Canny, contorno verde, resultado final) — execute o notebook e cole no slide.

---

### 1. Pré-processamento

| # | Operação | Como é aplicada | Parâmetros | Aula CP701TIN3 |
|---|----------|-----------------|------------|------------|
| 1.1 | Leitura da imagem | `cv.imread` — matriz BGR | arquivo em `img/` | Aula 1 |
| 1.2 | Conversão de cor | `cv.cvtColor(..., COLOR_BGR2GRAY)` | 3 canais → 1 canal de luminância | Aula 1 |

**Por quê?** Reduzir dados para filtros e bordas; o documento é identificado pelo contraste de intensidade, não pela cor.

---

### 2. Processamento (filtros)

| # | Operação | Como é aplicada | Parâmetros | Aula CP701TIN3 |
|---|----------|-----------------|------------|------------|
| 2.1 | Filtro passa-baixa | `cv.GaussianBlur` — convolução com kernel Gaussiano | kernel **(3, 3)**, σ = 0 | Aulas 7–8 |
| 2.2 | Detecção de bordas | `cv.Canny` — gradiente + histerese | **threshold1 = 50**, **threshold2 = 150** | Aula 9* |

**Por quê?** O blur remove ruído da câmera e textura do papel; o Canny marca o contorno forte entre papel e fundo.

\* Relacionado aos operadores Sobel/Prewitt (Aula 9); Canny é extensão com supressão de não-máximos e histerese.

---

### 3. Segmentação

| # | Operação | Como é aplicada | Parâmetros | Aula CP701TIN3 |
|---|----------|-----------------|------------|------------|
| 3.1 | Extração de contornos | `cv.findContours` no mapa de bordas | `RETR_EXTERNAL`, `CHAIN_APPROX_SIMPLE` | OpenCV docs |
| 3.2 | Seleção do documento | Maior contorno com 4 vértices | `approxPolyDP`: ε = **0,02 × perímetro** do contorno | — |
| 3.3 | Ordenação dos cantos | Função `reorder` (topo-esq → … → baixo-esq) | 4 pontos `float32` | — |

**Por quê?** O papel na mesa forma um quadrilátero; os 4 cantos definem a região a ser recortada e retificada.

**Ilustração:** imagem com retângulo verde sobre o documento (célula do notebook “Segmentação”).

---

### 4. Formatação (saída final)

| # | Operação | Como é aplicada | Parâmetros | Aula CP701TIN3 |
|---|----------|-----------------|------------|------------|
| 4.1 | Transformação de perspectiva | `getPerspectiveTransform` + `warpPerspective` | Destino fixo **600 × 850** px; 4 pts origem → retângulo | Aulas 3–5 |
| 4.2 | Correção gamma | `cv.LUT` com tabela gamma | **γ = 1,1** | Aula 6 (LUT/histograma) |
| 4.3 | Equalização local | `cv.createCLAHE` no canal L (LAB) | `clipLimit=2.0`, `tileGridSize=(12,12)` | Aula 6 (histograma) |
| 4.4 | Nitidez | `addWeighted` + `GaussianBlur` (unsharp mask) | pesos **1,5** e **-0,5** | Aulas 7–8 |

**Por quê?** A perspectiva simula o scanner “de cima”; o realce aumenta contraste do texto sem binarizar agressivamente (diferente de limiarização global da Aula 8).

**Ilustração:** lado a lado `img/image.png` e `resultado_final/resultado_scan.jpg`.

---

### O que **não** usamos (e por quê isso importa na nota)

- **Sem redes neurais / classificador** — atende ao tema “processamento de imagens”, não “só IA”.  
- OCR do CamScanner comercial **não** faz parte do nosso pipeline — só a preparação da imagem.

---

## Slide 5 — Demonstração em vídeo

**Título:** Demonstração do pipeline

**Conteúdo do vídeo (máx. 5 minutos):**

1. Mostrar foto original na pasta `img/`  
2. Executar o notebook `projeto_final.ipynb` (células principais ou Run All)  
3. Mostrar etapas: cinza → Canny → contorno → resultado em `resultado_final/`  
4. Comparar entrada vs. saída  

**Link do vídeo (preencher após upload):**

```
https://www.youtube.com/watch?v=SEU_VIDEO_ID
```

*Alternativas: Google Drive (link público), Vimeo, Streamable.*

---

## Slide 6 — Referências (opcional, recomendado)

1. Esfahani, A. N. — **Python-Document-Scanner-OpenCV** (2025). https://github.com/ArashNasrEsfahani/Python-Document-Scanner-OpenCV  
2. Rosebrock, A. — *How to Build a Kick-Ass Mobile Document Scanner in Just 5 Minutes* (2014). https://pyimagesearch.com/2014/09/01/build-kick-ass-mobile-document-scanner-just-5-minutes/  
3. OpenCV — *Geometric Transformations* / *Finding Contours*. https://docs.opencv.org/4.x/  
4. **CamScanner** — https://www.camscanner.com  

---

## Checklist antes de enviar o PDF

- [ ] Slide 1: título + subtítulo CP701TIN3  
- [ ] Slide 2: todos os integrantes com RA e turma  
- [ ] Slide 3: resumo + **pelo menos 1 figura** (entrada ou saída)  
- [ ] Slide 4: etapas numeradas + parâmetros + **ilustração** (plots ou antes/depois)  
- [ ] Link do vídeo válido e ≤ 5 min  
- [ ] Apenas **1 integrante** envia o PDF (conforme professor)  
- [ ] Arquivo exportado em **PDF**
