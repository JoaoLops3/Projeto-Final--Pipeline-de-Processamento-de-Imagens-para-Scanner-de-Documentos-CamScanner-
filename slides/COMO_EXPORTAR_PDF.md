# Como gerar o PDF dos slides

O professor pede entrega em **PDF**. O arquivo `SLIDES_CP701.md` é o roteiro completo; escolha uma opção abaixo.

---

## Opção 1 — Google Slides (mais simples)

1. Acesse [Google Slides](https://slides.google.com) → apresentação em branco.  
2. Abra `SLIDES_CP701.md` lado a lado.  
3. Crie **um slide por seção** (Slide 1, Slide 2, …).  
4. Insira imagens: **Upload** de `../img/image.png` e `../resultado_final/resultado_scan.jpg`.  
5. **Arquivo → Fazer download → PDF (.pdf)**.

---

## Opção 2 — PowerPoint / Keynote

Mesmo processo: copiar textos do roteiro, colar figuras, exportar como PDF.

---

## Opção 3 — Marp (Markdown → PDF)

Se tiver [Marp CLI](https://github.com/marp-team/marp-cli) instalado:

```bash
cd Projeto_Final/slides
marp SLIDES_CP701.md --pdf -o APRESENTACAO_CP701.pdf
```

Pode ser necessário ajustar quebras de slide (`---` entre slides) se usar Marp diretamente no `.md` longo; para isso, duplique o conteúdo em um arquivo `marp-deck.md` só com slides curtos.

---

## Figuras prontas no projeto

| Uso no slide | Caminho |
|--------------|---------|
| Antes (entrada) | `img/image.png` |
| Depois (scan) | `resultado_final/resultado_scan.jpg` |
| Etapas intermediárias | Screenshots das células do `projeto_final.ipynb` |

Dica: no notebook, execute todas as células e use **Print Screen** ou exporte figuras do Matplotlib com botão direito → salvar imagem.
