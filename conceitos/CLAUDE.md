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

## Fontes .drawio (img/)

8 arquivos-fonte (editáveis no draw.io). Seis têm `.png` correspondente
referenciado no texto; dois são órfãos.

- `algoritmo_simetrico.drawio` — cifra simétrica: T cifrado com K vira M
  (`C(T,K)=M`); Bob decifra com K; Eva intercepta M. Usado em
  "Confidencialidade" > "Algoritmo Simétrico". Idêntico ao arquivo em
  `../kek/img/`.
- `algoritmo_assimetrico.drawio` — cifra assimétrica: Alice cifra T com
  KPuBob (`C(T,KPuBob)=M`); Bob decifra com KPrBob; Eva intercepta. Usado
  em "Confidencialidade" > "Algoritmo Assimétrico". Idêntico ao arquivo em
  `../pki/img/`.
- `assinatura.drawio` — assinatura digital: Alice assina com KPrAlice
  (`C(T,KPrAlice)=M`); Bob "verifica" com `D(M,KPuAlice)=T`. O diagrama
  reproduz a mesma imprecisão já registrada como pendência no texto
  (verificação descrita como descriptografia) — se o texto for corrigido,
  reexportar este diagrama também. Idêntico ao arquivo em `../pki/img/`.
- `hash.drawio` — Bob calcula `H(T)=N1`, envia T; Alice recalcula
  `H(T)=N2`. Não há indicação visual de comparação N1×N2 nem aviso de
  atacante ativo — mesmo ponto já registrado como pendência no texto.
  Idêntico ao arquivo em `../pki/img/`.
- `kdf.drawio` — entradas Senha Mestra + Salt + Nível de Dificuldade +
  Tamanho da Chave → KDF → Chave Mestra (`MK=KDF(SM)`). Consistente com o
  texto de KDF.
- `kek.drawio` — fluxo completo de KEK: Senha Mestra → (KDF) → MKBob;
  MKBob protege DEKBob (M1); DEKBob protege T (M2). Idêntico ao arquivo em
  `../kek/img/` — coerente com o reaproveitamento de texto entre os dois
  documentos.
- `ca.drawio` — hierarquia CA Root → CA Intermediária → Usuário Final.
  **Órfão**: não referenciado em `criptografia.tex`. É idêntico ao
  `ca.drawio`/`ca.png` de `../pki/img/`, onde faz sentido (pki.tex trata
  de hierarquia de CA). Reforça a suspeita de cópia por engano — decidir
  remover ou integrar.
- `confidencialidade_autenticidade.drawio` — fluxo combinado de
  confidencialidade+autenticidade: Bob cifra T com DEKBob (M1), cifra
  DEKBob com KPuAlice (M2); Alice decifra M2 com KPrAlice e depois M1 com
  DEKBob. **Órfão**: não referenciado em `criptografia.tex`, que usa
  `confidencialidade.png` (versão mais simples, só confidencialidade).
  Pode ser diagrama preparado para ampliar a seção, ainda não integrado.
