# Wii / Nintendont — PAL-M 240p experimental

Este repositório mantém uma modificação **experimental** do [FIX94/Nintendont](https://github.com/FIX94/Nintendont) para Wii real: saída **PAL-M em 240p** selecionável diretamente no menu, sem trocar o padrão de cores para NTSC.

## Opção no menu

No Nintendont modificado, selecione:
- **Video:** Force
- **Videomode:** MPAL
- **MPAL 240p (EXP):** On
- **Force Progressive:** Off (480p desativado)
- **Skip IPL:** On recomendado
- **Patch PAL50:** On se o título europeu depender do patch

A opção experimental fica desabilitada para Wii U/vWii e só entra em ação com Force + MPAL. Quando desligada, o comportamento original 480i do Nintendont é preservado.

## Como funciona

- O loader usa o modo de vídeo nativo `TVMpal240Ds` da libogc (**VI_TVMODE_MPAL_DS**, valor **0x09**), mantendo a codificação PAL-M.
- O kernel converte estruturas `GXRModeObj` de jogos reconhecidas para MPAL 240p, solicita XFB de 240 linhas e mantém a altura EFB original para permitir redução vertical do quadro completo quando o jogo respeita a estrutura.
- Este modo é **240p não entrelaçado (15 kHz)**, **não 480p**.
- Nenhum arquivo de BIOS/ROM está incluído.

## Limitações

**Experimental e não validado no Wii físico:** alguns jogos podem ignorar os patches, cortar a imagem, deixar texto ilegível ou apresentar tela preta. O modo padrão 480i permanece disponível: basta desligar `MPAL 240p (EXP)`. O modo é desativado em tempo de execução ao carregar uma IPL/BIOS. A opção não corrige outros problemas de Force MPAL já existentes.

O repositório contém arquivos-fonte modificados em `overrides/`. Um workflow baixa o upstream, sobrepõe esses arquivos e tenta gerar `loader.dol`. **Uma compilação concluída não significa compatibilidade com todos os jogos.**

## Compilação

O upstream recomenda devkitARM r53, devkitPPC r35 e libOGC 1.8.23. Em **Actions**, o workflow `Build PAL-M 240p` tenta compilar e publica `boot.dol` se funcionar. O workflow depende de um arquivo histórico de toolchains; valide a procedência do arquivo antes de executar binários distribuídos por terceiros.

Para montar manualmente:

```sh
git clone https://github.com/FIX94/Nintendont.git upstream
cp -a overrides/. upstream/
# configure os toolchains compatíveis conforme o README do Nintendont
cd upstream && make forced
# resultado: loader/loader.dol
```

Coloque o `loader.dol` como `SD:/apps/Nintendont/boot.dol` para testar, mantendo cópia do `boot.dol` anterior.

**Créditos/licença:** alterações baseadas no código GPL-2.0 do Nintendont de seus respectivos autores.
