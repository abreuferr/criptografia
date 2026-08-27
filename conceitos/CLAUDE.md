# conceitos/criptografia.tex

Documento-base do repositório: fundamentos, Conhecimento Zero,
Confidencialidade (simétrica, assimétrica, KEK, KDF), Integridade (Hash),
Autenticidade (Assinatura Digital), Confidencialidade com Criptografia
Híbrida. É o texto mais completo e o melhor candidato para servir de
referência aos demais (`kek.tex` já reaproveita trechos dele).

Personagens fixos: Alice, Bob, Eva (atacante passiva/interceptadora).

## Já corrigido (não reabrir como pendência)

- O exemplo de cifra simétrica e o de criptografia híbrida já trazem, no
  próprio texto, a ressalva de que só demonstram confidencialidade e que
  produção exige cifra autenticada ou MAC.
- O exemplo RSA assimétrico já declara OAEP/SHA-256 explicitamente
  (`-pkeyopt rsa_padding_mode:oaep -pkeyopt rsa_oaep_md:sha256`) e avisa o
  limite de ~190 bytes de entrada para RSA-2048.
- Os parâmetros de KDF (PBKDF2, salt 128 bits, 600.000 iterações, chave de
  256 bits) já estão alinhados com `algoritmo/algoritmo.tex`.
- Geração de chave RSA já especifica `-pkeyopt rsa_keygen_bits:2048` em
  todos os `openssl genpkey` deste documento.

## Pendências reais

- Seção "Princípios" lista quatro pilares: Conhecimento Zero,
  Confidencialidade, Integridade, Autenticidade. "Conhecimento Zero" aqui
  descreve dados cifrados localmente (arquitetura zero-knowledge/client-side
  encryption), não uma prova de conhecimento zero — termo tecnicamente
  impreciso e de categoria diferente da tríade CIA. Ver nota em
  `../CLAUDE.md`.
- Seção "Integridade" > "Hash": Bob envia `T` e `H(T)`, Alice recalcula e
  compara. Não há ressalva de que isso não protege contra atacante ativo
  (falta o aviso de usar HMAC/assinatura que outras seções deste mesmo
  documento já incluem).
- Seção "Autenticidade" > "Assinatura Digital": a verificação é descrita
  como "Alice utiliza a chave pública de Bob para descriptografar a
  assinatura digital". Verificação de assinatura é uma operação
  criptográfica própria, não o inverso da cifragem. Falta também declarar
  que a confiança na chave pública de Bob depende de ela estar associada a
  ele de forma verificável (certificado).
- O comando de assinatura (`openssl dgst -sha256 -sign KPrBob.pem ...`)
  usa padding PKCS#1 v1.5 por padrão, sem declarar isso ou usar PSS. Ver
  ponto transversal em `../CLAUDE.md`.
