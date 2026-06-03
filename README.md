# Pipeline de Processamento de Imagens para Scanner de Documentos (CamScanner Clone)

**Disciplina:** CP701TIN3 — Processamento de Imagens Digitais  
**Trabalho:** Projeto Final (entrega em slides PDF + vídeo demonstrativo)

---

## Objetivo

Documentar e reproduzir um **pipeline de processamento de imagens** (sem IA) que transforma uma foto inclinada de um documento em uma imagem “escaneada”: plana, retificada e com texto mais legível — no mesmo espírito de aplicativos como o **CamScanner**.

O foco da disciplina está nas **etapas clássicas de visão computacional** (pré-processamento, filtros, segmentação, transformação geométrica), não em classificadores ou redes neurais.

---

## Referências (“X” do trabalho)

| Referência | Tipo | Papel no trabalho |
|------------|------|-------------------|
| **[CamScanner](https://www.camscanner.com)** | Aplicação comercial | Onde e por que o pipeline é usado (digitalização de documentos, PDF, OCR posterior) |
| **[Python-Document-Scanner-OpenCV](https://github.com/ArashNasrEsfahani/Python-Document-Scanner-OpenCV)** | Programa open source (2025) | Base da implementação em notebook |
| **PyImageSearch — Mobile Document Scanner (2014)** | Tutorial | Referência clássica do mesmo fluxo (Canny → contornos → perspectiva) |

Nossa versão está em `projeto_final.ipynb`, adaptada às aulas da CP701TIN3, com comentários em português e figuras por etapa.

---

## Pipeline (resumo)

```
Foto (BGR)  →  cinza  →  blur Gaussiano  →  Canny  →  contorno (4 cantos)
    →  warpPerspective  →  realce (gamma, CLAHE, nitidez)  →  resultado_scan.jpg
```

| Fase | Operações principais | Aula CP701TIN3 relacionada |
|------|----------------------|-------------------------|
| Pré-processamento | `imread`, `cvtColor` BGR→GRAY | Aula 1 |
| Processamento | `GaussianBlur` (3×3), `Canny` (50, 150) | Aulas 7–8, 9* |
| Segmentação | `findContours`, `approxPolyDP` | Docs OpenCV / extensão Aula 9 |
| Formatação | `getPerspectiveTransform`, `warpPerspective`, `enhance` | Aulas 3–5, 6 |

\* Canny relacionado aos operadores de gradiente (Sobel, Prewitt) da Aula 9.

---

## Estrutura de pastas

```
Projeto_Final/
├── README.md                 ← este arquivo
├── projeto_final.ipynb       ← pipeline + plots para slides
├── requirements.txt
├── slides/
│   ├── SLIDES_CP701.md       ← roteiro completo dos slides (exportar para PDF)
│   └── COMO_EXPORTAR_PDF.md  ← instruções para gerar o PDF
├── img/                      ← fotos de entrada (você adiciona)
│   └── image.png             ← exemplo usado no grupo
├── resultado_final/          ← saída gerada pelo notebook
│   └── resultado_scan.jpg
└── .venv/                    ← ambiente Python (opcional, recomendado)
```

---

## Requisitos e instalação

**Python:** 3.11 ou 3.13 recomendado.

```bash
cd Projeto_Final
python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python -m ipykernel install --user --name=cp701-projeto-final --display-name="CP701TIN3 Projeto Final (.venv)"
```

**Dependências** (`requirements.txt`):

- `numpy`, `opencv-python-headless`, `matplotlib`, `ipykernel`, `notebook`

Usamos **`opencv-python-headless`** no macOS para evitar travamento do kernel Jupyter ao importar o OpenCV.

---

## Como executar o notebook

1. Coloque uma ou mais fotos em `img/` (`.jpg`, `.png`, etc.). O documento deve contrastar com o fundo (mesa escura, papel claro).

2. Abra `projeto_final.ipynb` no Cursor/VS Code.

3. Selecione o kernel **「CP701TIN3 Projeto Final (.venv)」** (ou o interpretador `Projeto_Final/.venv/bin/python`).

   > Evite o `.venv` antigo na pasta raiz do repositório “Processamento de Imagens”, se ele causar timeout no Jupyter.

4. Na primeira célula de código:

   ```python
   IMAGE_NAME = None   # usa a primeira imagem em img/
   # IMAGE_NAME = 'image.png'   # ou fixe o nome do arquivo
   ```

5. Execute todas as células (**Run All**).

6. Confira a saída em `resultado_final/resultado_scan.jpg` e use os gráficos do notebook nas ilustrações do **Slide 4**.

---

## Entrega acadêmica (checklist)

Conforme enunciado do professor:

| Item | Onde está |
|------|-----------|
| Slide 1 — Título + subtítulo CP701TIN3 | `slides/SLIDES_CP701.md` |
| Slide 2 — Integrantes (nome, RA, turma) | Preencher no mesmo arquivo |
| Slide 3 — Resumo do artigo/app (1–2 figuras) | Texto + usar `img/image.png` e screenshot do CamScanner ou README do GitHub |
| Slide 4 — Elementos de processamento enumerados | Tabela no notebook + `slides/SLIDES_CP701.md` |
| Vídeo ≤ 5 min (YouTube ou similar) | Link no último slide — gravar execução do notebook |
| Arquivo final | **PDF** dos slides (ver `slides/COMO_EXPORTAR_PDF.md`) |

---

## Créditos

Implementação adaptada de:

**[Python-Document-Scanner-OpenCV](https://github.com/ArashNasrEsfahani/Python-Document-Scanner-OpenCV)** — Arash Nasr Esfahani (2025).

Disciplina: **CP701TIN3 — Processamento de Imagens Digitais**.
