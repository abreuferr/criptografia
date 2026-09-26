# algoritmo/algoritmo.tex

Tabela de referência: algoritmos e parâmetros recomendados. É a fonte de
verdade que os outros documentos devem seguir (especialmente os
parâmetros de KDF). Ver também pontos transversais em `../CLAUDE.md`.

## Conteúdo atual

- Chave simétrica: AES-256-GCM recomendado (FIPS PUB 197 e NIST SP 800-38D), nonce preferencialmente de 96 bits nunca repetido com a mesma chave e tag de 128 bits. AES-CBC é legado e requer IV imprevisível mais MAC com chave independente.
- Chave assimétrica: RSA, mínimo 2048 bits (NIST SP 800-57 Part 1).
- Hash: SHA-2 (SHA-256/384/512).
- HMAC: SHA-2 (SHA-256/384/512).
- KDF: PBKDF2, 600.000 iterações, salt ≥ 128 bits, chave de 256 bits
  (NIST SP 800-132). Esses valores já são os usados em `conceitos.tex`,
  `custodia.tex` e `kek.tex` — não alterar sem atualizar os três.

Compila limpo (verificado com `pdflatex`, 6 páginas, sem erro).

## Pendências

- Nas seções Hash e HMAC, o rótulo é "Tamanho da chave" — hash não tem
  chave. Trocar por "Tamanho da saída do hash" na seção Hash; na seção
  HMAC, diferenciar "tamanho da chave" de "tamanho da tag".
