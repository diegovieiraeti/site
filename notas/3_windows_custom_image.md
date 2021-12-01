---
title: Criando imagem customizada do Windows
layout: simple_page 
tags: [docker, vscode]
date: 2021-11-30
---

A criação da imagem customizada é realizada utilizando uma instalação do Windows feita no Virtualbox e posteriormente gerada utilizando o comando DISM.

1. Crie no Virtualbox um *disco rígido virtual* do tipo **VHD *(virtual Hard Disk)***.

## No Virtualbox

2. Após a instalação do Windows, antes do **OOBE *Out-of-box experience*** (Configuração antes do primeiro acesso ao sistema operacional), pressione *CTRL+SHIFT+F3* para entrar no modo de auditoria *Sysprep Audit Mode* e efetue a instalação dos softwares.

3. Ao finalizar a instalação dos Softwares e limpe os arquivos temporários, execute o **Syprep**, disponível em: *C:\Windows\System32\Sysprep*, com as opções:
    - Entrar na Configuração Inicial pelo Usuário do Sistema (OOBE);
    - Marcar: Generalizar;
    - Desligar.

## No computador hospedeiro

4. Use a pesquisa do Windows e procurar por *Criar e formatar partições do disco rígido* ou utilizar o *menu de usuário avançado do Windows* (**Win+X**) e clique em *Gerenciamento de disco*.

5. No menu superior, selecione *Ação* >> *Anexar VHD*.

6. Localize o *disco rígido virtual* utilizado no virtualbox e anexa-a, após localize a unidade que contém a instalação do Windows (vamos considerar unidade **E:** como exemplo).

7. Execute no *Terminal do Windows (Administrador)*, os seguintes comandos:

```bash
# Mude para a unidade VHD que contém a instalação do Windows:
E:
# otimize a imagem
dism /image:E:\ /optimize-image /boot
# Mude para o local destino
C:
# Capture a imagem 
dism /capture-image /imagefile:WindowsCustom.wim /capturedir:E:\ /name:"WindowsCustom" /compress:max
```

8. Extraia um arquivo *ISO* de instalação do Windows e remova o arquivo *install.wim* ou *install.esd* da pasta **\sources**.

9. Copie o arquivo *WindowsCustom.wim* para a pastas com os arquivos da ISO **\sources** e renomeie para *install.wim*.

10. Execute o **Dism++** e abra a opção *Criar ISO* em *Utilitários >> Ferramentas*.
    - Download em [https://github.com/Chuyu-Team/Dism-Multi-language/releases](https://github.com/Chuyu-Team/Dism-Multi-language/releases)

11. Copie o arquivo *ISO* para o pendrive preparado com o **Ventoy**.
    - Download em [https://www.ventoy.net/en/doc_start.html](https://www.ventoy.net/en/doc_start.html)

