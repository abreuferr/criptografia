# gerador_senha/gerador_senha.tex

Estudo de combinatória sobre espaço de busca de senhas. Único documento do
repositório que usa `amsmath` para fórmulas. Não tem exemplos de comando
OpenSSL nem trata de algoritmos de cifra — é matemática pura sobre
contagem de senhas possíveis.

## Conteúdo e resultados

- Política fixa: senha de 12 caracteres, 3 de cada classe (maiúscula,
  minúscula, número, especial), calculada como permutação de multiconjunto
  vezes combinações por grupo. Resultado:
  `5,84577386545152 × 10^19` senhas possíveis. Confirmado, não recalcular.
- Política variada: senha de 12 caracteres livres entre 70 símbolos
  (`70^12`). Resultado: `1,3841287201 × 10^22` senhas possíveis.
  Confirmado, não recalcular.

## Pendências

O estudo é só contagem bruta de combinações; falta o que tornaria a
análise útil na prática:

- Entropia em bits (`log2` do total de senhas), não só o número absoluto.
- Modelo de ataque: força bruta online (limitada por rate limit) vs.
  offline contra hash vazado (limitada pelo custo do KDF) — resultados
  combinatórios sem isso não dizem quanto tempo um ataque real levaria.
- Custo por tentativa (ligado aos parâmetros de KDF de
  `../algoritmo/algoritmo.tex`: 600.000 iterações de PBKDF2).
- Comparação com passphrases (ex.: método Diceware) e recomendação de uso
  de gerenciador de senhas.
