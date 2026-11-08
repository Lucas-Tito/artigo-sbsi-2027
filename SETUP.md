# Editar este artigo no VS Code, no Ubuntu

Passo a passo para quem quer escrever o artigo com pré-visualização do PDF na
própria máquina. Leva uns 10 minutos, quase tudo é download.

Quem não quiser instalar nada pode editar pelo navegador: abra o repositório no
GitHub e aperte `.`, que o VS Code web abre no lugar. O PDF sai na aba
**Actions** a cada push, como artifact.

## 1. LaTeX

```bash
sudo apt update
sudo apt install -y \
  texlive-latex-base \
  texlive-latex-recommended \
  texlive-latex-extra \
  texlive-fonts-recommended \
  texlive-pictures \
  texlive-bibtex-extra \
  latexmk
```

São cerca de 1,5 GB. Não precisa do `texlive-full`, que passa de 5 GB.

Cada pacote está aí por um motivo: `texlive-pictures` traz o TikZ, que desenha a
Figura 1; `texlive-fonts-recommended` traz a Times, que o estilo da SBC exige; e
`texlive-latex-extra` traz o `titlesec`, usado pelo `sbc-template.sty`.

Confira com:

```bash
pdflatex --version && latexmk --version
```

## 2. VS Code e a extensão

Se ainda não tiver o VS Code:

```bash
sudo snap install code --classic
```

A extensão é a **LaTeX Workshop** (`James-Yu.latex-workshop`):

```bash
code --install-extension James-Yu.latex-workshop
```

## 3. Clonar e abrir

```bash
git clone https://github.com/Lucas-Tito/artigo-sbsi-2027.git
code artigo-sbsi-2027
```

## 4. Escrever

Abra o `artigo.tex`. A extensão compila sozinha ao salvar e o PDF aparece ao
lado.

| Atalho | O que faz |
| --- | --- |
| `Ctrl+Alt+B` | Compilar agora |
| `Ctrl+Alt+V` | Abrir o PDF ao lado |
| `Ctrl+Alt+J` | Pular, no PDF, para o trecho onde o cursor está |

Clicar no PDF segurando `Ctrl` leva de volta à linha correspondente no `.tex`.

Se o painel de PDF não abrir sozinho, use a aba **TeX** na barra lateral, em
Build LaTeX project → Recipe: latexmk.

## 5. Compilar na mão, se precisar

```bash
latexmk -pdf -bibtex artigo.tex
```

Para limpar os arquivos intermediários:

```bash
latexmk -C
```

Eles já estão no `.gitignore`, então não vão parar no repositório.

## O que não fazer

Não edite `sbc-template.sty`, `sbc.bst` nem `caption2.sty`. São o estilo oficial
da SBC e vieram do pacote que a chamada manda usar.

## Se der erro

A mensagem útil quase nunca é a última linha. Procure a primeira linha que
começa com `./artigo.tex:` seguida de um número, que é onde o problema está de
fato.

Dois erros comuns neste documento:

- **Linha em branco dentro do `\address`** quebra o `\maketitle` com uma mensagem
  confusa sobre `\begin{document}` faltando. Mantenha o bloco de endereços sem
  linhas vazias no meio.
- **`Citation ... undefined`** na primeira compilação é normal: o BibTeX precisa
  de duas passadas. O `latexmk` já faz isso sozinho.
