# kek/kek.tex

Documento incompleto: aplica o conceito de KEK ao produto "Segura Cofre"
(PAM). Boa parte do texto (Confidencialidade, Algoritmo Simétrico,
Key-Encryption-Key) é reaproveitada quase literalmente de
`../conceitos/criptografia.tex` — ao editar um dos dois, checar se o outro
precisa do mesmo ajuste.

## Estado atual — pendências confirmadas no texto

- Seção "Arquitetura da Solução" está duplicada: o título aparece duas
  vezes seguidas (`\subsection{Arquitetura da Solução}` repetido) e ambas
  vazias, sem conteúdo. Remover a duplicata e escrever o conteúdo real.
- Seção "Implementação e Melhores Práticas" existe só como título
  (`\section`), sem nenhum parágrafo depois. Precisa ser escrita.
- Linha "estabelecemos camadas de segurança que tornam virtualmente
  impossível o acesso não autorizado aos dados mais críticos" é uma
  alegação de segurança absoluta. Proteção depende de algoritmo,
  implementação, gestão de chaves e modelo de ameaça — nenhuma camada
  torna acesso não autorizado "virtualmente impossível". Reformular.
- Bloco de copyright final não tem "Versão" nem "Autor", diferente do
  padrão dos outros documentos do repositório (ver `../CLAUDE.md`).

Como este é o documento menos maduro do repositório, qualquer trabalho
aqui provavelmente envolve completar as duas seções vazias antes de
revisar detalhes menores.

## Fontes .drawio (img/)

2 arquivos, ambos idênticos aos de `../conceitos/img/` (mesmo diagrama
reaproveitado, não uma variação):

- `algoritmo_simetrico.drawio` — cifra simétrica (`C(T,K)=M`; Eva
  intercepta). Igual a `../conceitos/img/algoritmo_simetrico.drawio`.
- `kek.drawio` — fluxo completo de KEK (Senha Mestra → KDF → MKBob →
  protege DEKBob (M1) → protege T (M2)). Igual a
  `../conceitos/img/kek.drawio`.

Confirma, pelo lado das figuras, a nota já registrada acima de que
`kek.tex` reaproveita conteúdo de `conceitos/criptografia.tex` quase
literalmente — aqui isso se estende às imagens. Ao editar
`kek.drawio`/`algoritmo_simetrico.drawio` num dos dois diretórios,
replicar a mudança no outro.
