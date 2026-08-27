# pki/pki.tex

PKI e Autoridade Certificadora: conceitos (Confidencialidade, Integridade,
Autenticidade), hierarquia de CAs (Root → Intermediária → Usuário Final) e
um exercício prático completo em OpenSSL (estrutura de diretórios,
`openssl.cnf` de CA raiz e intermediária, emissão de CSR, assinatura e
verificação de cadeia).

## Pendências confirmadas no texto

- Seção "Princípios" anuncia "quatro pilares importantes" mas o parágrafo
  e o `itemize` seguinte listam só três: Confidencialidade, Integridade,
  Autenticidade. Corrigir para "três pilares" (ou incluir o quarto, se a
  intenção original era manter Conhecimento Zero como em
  `../conceitos/criptografia.tex` — decidir e alinhar com o outro
  documento).
- `openssl.cnf` da CA intermediária, seção `[usr_cert]`, usa
  `nsCertType = client, email` — extensão legada do Netscape, hoje sem
  efeito na maioria dos validadores modernos. O `extendedKeyUsage` logo
  abaixo já cobre `clientAuth, emailProtection`; considerar remover
  `nsCertType`/`nsComment`.
- Bloco de copyright final não tem "Versão" nem "Autor", mesmo padrão
  faltante de `../kek/kek.tex` (ver `../CLAUDE.md`).

## Já verificado e descartado (não é um problema real)

- Revisão anterior apontava comandos multilinha com barra invertida
  seguida de espaço (continuação de shell inválida). Checado via
  `grep -P '\\\\[ \t]+$' pki.tex`: não há nenhuma ocorrência no arquivo
  atual. Não reintroduzir essa observação sem reconferir.
