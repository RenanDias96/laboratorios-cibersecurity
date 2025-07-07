# 🛠️ Instalação e Configuração Geral do Ambiente

- 💡 Guia completo para configuração de ambiente base em Ubuntu 22.04 Desktop utilizado nos laboratórios.

## 📥 1. Baixando e Instalando o Ubuntu 22.04  
📅 Data: 25/05/2025

- Acesse: [https://releases.ubuntu.com/jammy](https://releases.ubuntu.com/jammy)

![Download ISO](prints/1.png)

- Crie uma nova VM no VirtualBox com nome, hostname e senha definidos:

![Criando VM](prints/2.png)
![Configurações iniciais](prints/3.png)

- Inicie a instalação:

![Instalação 1](prints/4.png) 
![Instalação 2](prints/4.png) 

## 🔐 2. Corrigindo Permissões de Sudo

- Teste o comando:

```bash
sudo -i
```

![Teste sudo](prints/6.png)

Se falhar, reinicie a VM e entre em modo recovery:

Pressione ESC várias vezes durante o boot

Vá em Advanced Options > Recovery Mode

Selecione Root - Drop to root shell prompt

![Recovery Menu 1](prints/7.png)
![Recovery Menu 2](prints/8.png)
![Recovery Options](prints/9.png)

Após pressionar Enter na opção será solicitado a senha que você definiu na configuração do lab, depois disso execute os comandos:

```bash
mount -o remount,rw /
usermod -aG sudo vboxuser
reboot
```

![Root Prompt](prints/10.png)
![Sudo OK](prints/11.png)


Reinicie a VM normalmente e teste o comando sudo -i

⌨️ 3. Configurações de Teclado e Clipboard

Vá até Settings > Keyboard e altere o idioma:

![Keyboard](prints/12.png)

![Teclado PT-BR](prints/13.png)

🗌 Habilitando Copiar e Colar (shared clipboard)

No VirtualBox, com a VM desligada, vá em:

Configurações > Geral > Avançado > Área de transferência: Bidirecional

![Clipboard](prints/14.png)

📀 4. Instalação dos Adicionais para Convidado

Inicie a VM e no menu superior clique em:

Dispositivos > Inserir imagem de CD dos Adicionais para Convidado

Irá aparecer um icone de CD na barra lateral do ubuntu.

Abra o terminal e execute os comandos:

```bash
sudo apt install build-essential dkms linux-headers-$(uname -r)
cd /media/$USER/VBox_GAs_*/
sudo ./VBoxLinuxAdditions.run
```

![Instalando headers](prints/15.png)
![Finalizando a instalação](prints/16.png)


Reinicie a VM após a instalação.

📦 5. Instalação Curl

```bash
sudo apt install curl -y
```

## 🧱 Estrutura Final do Ambiente
- ✅ Ubuntu 22.04 instalado
- ✅ Permissões sudo funcionando
- ✅ Teclado em português configurado
- ✅ Copiar/colar entre host e VM ativo
- ✅ Curl instalado
- ✅ Adicionais para Convidado aplicados

