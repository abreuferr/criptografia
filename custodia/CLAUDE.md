# custodia/custodia.tex

Ciclo de custódia e recuperação de chaves entre Alice (custodiante) e Bob
(custodiado). É o documento com fluxo mais complexo do repositório —
qualquer edição deve manter texto, comandos, numeração de artefatos (M1,
M2, ...) e as figuras em `img/` coerentes entre si.

## Convenções deste documento

- `MK*` = Chave Mestra (derivada da senha mestra via PBKDF2); `DEK*` =
  Chave de Dados; `KPu*`/`KPr*` = par RSA.
- `M1`..`M9` são textos cifrados numerados sequencialmente ao longo de
  todo o documento (não reinicia por seção). A numeração continua entre
  `enumerate` com `\setcounter{enumi}{N}` — ao inserir um passo novo,
  ajustar todos os `\setcounter` posteriores.
- Já tem, no início do documento, a nota padrão de que os exemplos AES-CBC
  não são autenticados — não duplicar essa ressalva.

## Fluxo atual (já revisado, correto)

1. Bob recupera M3/M4 do servidor e decifra com sua chave mestra
   (`MKBob.hex`) para obter `KPrBob.pem` e `DEKBob.bin`.
2. Bob protege `KPrBob.pem` para Alice via envelope CMS
   (`openssl cms -encrypt`, usando o certificado `Alice.cert.pem`),
   gerando M5 — isso é criptografia híbrida corretamente modelada.
3. Bob protege `DEKBob.bin` para Alice via RSA-OAEP direto
   (`openssl pkeyutl -encrypt` com `rsa_padding_mode:oaep`), gerando M6.
4. Na recuperação, Bob deriva uma Nova Chave Mestra (`NMKBob.hex`) e a
   protege para Alice via RSA-OAEP, gerando M7.
5. Alice decifra M1 (sua própria chave privada), depois usa
   `KPrAlice.pem` para decifrar M5, M6 e M7, obtendo `KPrBob.pem`,
   `DEKBob.bin` e `NMKBob.hex`.
6. Alice re-protege `KPrBob.pem` e `DEKBob.bin` com `NMKBob.hex`, gerando
   M8 e M9.

Este fluxo já corresponde ao que as figuras (`custodiando.png`,
`nova-custodia_01.png`, `nova-custodia_02.png`) mostram. Não é mais
necessário realinhar texto/figura neste ponto.

## Já corrigido (não reabrir como pendência)

- Bug de texto na seção "Nova Custódia - Alice" (bloco de decifrar M5/M6
  com linha `-inkey KPrAlice.pem -out KPrBob.pem` solta e duplicada) foi
  corrigido — o comando `openssl cms -decrypt` agora aparece uma única vez,
  limpo.
- `openssl genpkey -algorithm RSA` para `KPrAlice.pem`/`KPrBob.pem` já
  especifica `-pkeyopt rsa_keygen_bits:2048`, igual a `conceitos.tex`.

## Pendências reais

- Falta, antes dos comandos, um modelo de ameaça: quem pode recuperar cada
  chave, quais artefatos o servidor de fato armazena, quando uma chave é
  destruída/rotacionada e como a recuperação é auditada. Isso ainda não
  foi escrito.

## Fontes .drawio (img/)

5 arquivos-fonte, todos com `.png` correspondente já referenciado no
texto — nenhum órfão.

- `chave_alice.drawio` — Alice gera par RSA (KPuAlice/KPrAlice) e
  DEKAlice; MKAlice protege KPrAlice (M1) e DEKAlice (M2). Corresponde à
  criação das chaves de Alice.
- `chave_bob.drawio` — equivalente para Bob: MKBob protege KPrBob (M3) e
  DEKBob (M4).
- `custodiando.drawio` — ciclo "Protegendo"/"Recuperando" KPrBob e DEKBob
  via MKBob (M3/M4) e depois via KPuAlice (M5/M6). Coerente com o fluxo de
  custódia descrito acima.
- `nova-custodia_01.drawio` — Bob deriva NMKBob e protege via KPuAlice,
  gerando M7.
- `nova-custodia_02.drawio` — sequência completa de recuperação por
  Alice: decifra M1 (chave própria), depois M5/M6/M7 com KPrAlice para
  obter KPrBob/DEKBob/NMKBob, e re-protege com NMKBob gerando M8 e M9.
  Corresponde exatamente aos passos 5 e 6 do "Fluxo atual" já documentado
  acima.

Os cinco diagramas batem com a numeração M1–M9 e com o fluxo já revisado
— nenhuma inconsistência entre diagrama e texto encontrada.
