# Atualizações — Ponto Eletrônico Grupo Educacional Rocha

Este repositório serve **apenas** o canal de atualização automática do
aplicativo desktop. O código-fonte fica em um repositório privado.

## O que tem aqui

- **`update.json`** — manifesto lido pelo aplicativo toda vez que ele abre.
  Informa a versão mais recente, a URL do `.exe` e o SHA-256 para verificação.
- **Releases** — cada release traz o `PontoRocha.exe`, o executável completo
  (build `--onefile`).

## Como o cliente atualiza

Na abertura, o aplicativo mostra um loading ("Procurando atualizações...") e
lê o `update.json` deste repositório. Se a versão publicada for maior que a
instalada, ele baixa o `.exe` do release correspondente, confere o SHA-256,
substitui o executável e reabre sozinho.

Sem internet, ou se qualquer etapa falhar, o aplicativo abre normalmente na
versão que já está instalada. A atualização nunca impede o uso (a menos que
`mandatory: true`).

## Formato do manifesto

```json
{
  "version": "1.0.1",
  "download_url": "https://github.com/4ugust0/ponto-rocha-updates/releases/download/v1.0.1/PontoRocha.exe",
  "sha256": "<hash do .exe>",
  "notes": "Resumo do que mudou nesta versão.",
  "mandatory": false
}
```

`mandatory: true` faz o aplicativo recusar-se a abrir caso a atualização não
possa ser instalada — use quando a versão anterior parar de funcionar (ex.:
mudança incompatível no painel web).

## Publicando uma versão nova

1. Gere o novo `PontoRocha.exe` (PyInstaller `--onefile`, ver repositório
   principal).
2. Calcule o SHA-256: `sha256sum PontoRocha.exe` (ou `Get-FileHash` no
   PowerShell).
3. Suba o release `vX.Y.Z` com o `.exe` como anexo — **antes** de atualizar o
   `update.json`. Se o manifesto anunciar uma versão cujo `.exe` ainda não
   existe, o download falha e os clientes seguem na versão anterior até a
   próxima abertura.
4. Atualize `version` e `sha256` no `update.json` e suba (`git push`).
