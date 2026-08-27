# algoritmo/algoritmo.tex

Tabela de referência: algoritmos e parâmetros recomendados. É a fonte de
verdade que os outros documentos devem seguir (especialmente os
parâmetros de KDF). Ver também pontos transversais em `../CLAUDE.md`.

## Conteúdo atual

- Chave simétrica: AES-CBC, 256 bits (FIPS PUB 197).
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
- AES-CBC é recomendado sem menção a autenticação (MAC/AEAD). Alinhar com
  a ressalva já usada em `conceitos.tex`/`custodia.tex` sobre CBC puro não
  detectar adulteração.
- `img/` tem 9 imagens; só `logo.png` é referenciada no `.tex`. As outras
  7 (`chave_alice.png`, `chave_bob.png`, `cliente_chave_mestra.png`,
  `custodiando.png`, `legenda_assimetrico.png`, `legenda_simetrico.png`,
  `recuperacao_chaves_alice.png`, `recuperacao_chaves_bob.png`) não
  aparecem no texto. Os nomes batem com o fluxo de `custodia/img/` —
  parecem ter sido copiadas por engano ou preparadas para uma seção que
  nunca foi escrita. Decidir: integrar ao texto ou remover do diretório.
