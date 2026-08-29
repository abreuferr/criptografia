# Estudos de criptografia

Materiais didáticos em LaTeX (ver README.md para a lista de documentos e o
comando de compilação). Cada subdiretório tem um CLAUDE.md com as
particularidades daquele documento — leia o CLAUDE.md da pasta antes de
editar o `.tex` dela.

`password_manager/` — tópico planejado; diretório ainda não criado (por
isso não aparece no README).

PDFs de `algoritmo/`, `conceitos/` e `custodia/` estão versionados no git —
não recompilar por conta própria só para "atualizar" o PDF; isso gera diff
binário. Se recompilar para conferir algo, reverter o PDF depois
(`git checkout -- caminho/arquivo.pdf`) a menos que a mudança de conteúdo
do PDF seja intencional.

## Pontos transversais (afetam mais de um documento)

- **Hash sem chave não é integridade autenticada.** `conceitos.tex` e
  `pki.tex` mostram Bob enviando `T` e `H(T)`; Alice recalcula e compara.
  Isso detecta alteração acidental, não um atacante ativo — quem altera a
  mensagem recalcula o hash. Para integridade autenticada, usar HMAC ou
  assinatura. Adicionar a ressalva onde falta.
- **Assinatura RSA sem declarar padding.** Só `conceitos.tex` tem comando
  de assinatura de fato (`openssl dgst -sha256 -sign`, PKCS#1 v1.5 por
  padrão); `pki.tex` trata assinatura apenas conceitualmente, sem `-sign`.
  Se a intenção é RSA-PSS, declarar `-sigopt rsa_padding_mode:pss` e o
  hash usado.
- **Geração de chave RSA já padronizada com tamanho explícito.** Todos os
  `openssl genpkey` do repositório (`conceitos.tex`, `custodia.tex`,
  `pki.tex`) já passam `-pkeyopt rsa_keygen_bits` (2048 para chaves de
  usuário/certificado final, 4096 para CA root/intermediária em
  `pki.tex`). Não é mais um problema real no repo.
- **Parâmetros de KDF já estão alinhados** entre `algoritmo.tex`,
  `conceitos.tex`, `custodia.tex` e `kek.tex`: PBKDF2-HMAC-SHA256, salt de
  128 bits, 600.000 iterações, chave de 256 bits. Novo conteúdo deve seguir
  o mesmo padrão — não reintroduzir salt de 64 bits / 10.000 iterações
  (isso já foi corrigido, não é mais um problema real no repo).
- **Bloco de copyright final inconsistente.** `algoritmo.tex`,
  `conceitos.tex`, `custodia.tex` e `gerador_senha.tex` têm "Versão" e
  "Autor"; `kek.tex` e `pki.tex` não têm. Padronizar.

## Ordem sugerida de correção

1. ~~Corrigir o bloco de comandos duplicado/quebrado em `custodia.tex`~~ —
   feito (decrypt de M5, ver `custodia/CLAUDE.md`).
2. Adicionar ressalva de HMAC/assinatura nas seções de Hash de
   `conceitos.tex` e `pki.tex`.
3. Declarar padding (PSS ou PKCS#1 v1.5 explícito) nos exemplos de
   assinatura digital.
4. ~~Padronizar tamanho de chave RSA explícito em todos os `openssl
   genpkey`~~ — feito.
5. Completar `kek.tex` (seções "Arquitetura da Solução" e "Implementação e
   Melhores Práticas") e revisar alegações absolutas de segurança.
6. Corrigir "quatro pilares" em `pki.tex` (lista só três) e a classificação
   de "Conhecimento Zero" como pilar em `conceitos.tex`.
7. Enriquecer `gerador_senha.tex` com entropia em bits e modelo de ataque.
8. Padronizar bloco de copyright (Versão/Autor) em `kek.tex` e `pki.tex`.
9. Decidir política de versionamento de PDF gerado (hoje só
   `algoritmo/`, `conceitos/` e `custodia/` versionam o PDF).
