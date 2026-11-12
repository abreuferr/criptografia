# Criptografia

Materiais de estudo sobre conceitos e fluxos de criptografia.

- `conceitos/` — conceitos fundamentais, KDF, KEK, hash, assinatura e
  criptografia híbrida.
- `pki/` — infraestrutura de chaves públicas e autoridade certificadora.
- `custodia/` — custódia e recuperação de chaves.
- `kek/` — introdução a Key-Encryption-Key.
- `algoritmo/` — algoritmos e parâmetros de referência.
- `gerador_senha/` — estudo do espaço de busca de senhas.

Cada documento segue o modelo `documentoPadrao`:

- `main.tex` — documento raiz (capa, resumo, sumário e `\input` dos capítulos).
- `config/preambulo.tex` — pacotes, estilos e configurações.
- `capitulos/` — um `.tex` por `\section` (`00-resumo.tex`, `01-...`).
- `img/` — imagens e diagramas.
- `latexmkrc` — saída em `build/` (fora do git).

Compile a partir do diretório do documento:

```sh
cd conceitos && latexmk main.tex
```

O PDF sai em `build/main.pdf`.
