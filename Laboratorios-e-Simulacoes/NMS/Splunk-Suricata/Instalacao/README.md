# 🧰 Instalação e Configuração do Ambiente — Splunk + Suricata

📅 **Data do ambiente:** 10/06/2025  
🔎 Este documento apresenta o passo a passo completo para configurar o Suricata com uma regra personalizada e integrar os logs ao Splunk Enterprise para visualização em tempo real.

---

## 📥 1. Instalação do Ambiente Geral

Antes de iniciar, siga o guia de instalação base:

🔗 [Instalação do Ambiente Geral](../../../Instalacao-do-Ambiente-Geral/README.md)

---

## 📦 2. Instalação do Suricata

```bash
sudo add-apt-repository ppa:oisf/suricata-stable -y
sudo apt update -y
```

![Instalação](prints/1.png)

## 📦 2.1 Instalação de Dependências

```bash
sudo apt install suricata jq tcpdump -y
suricata --build-info | grep 'Suricata'
```
![Dependencias](prints/2.png)

## 📁 3. Criação da pasta rules e do arquivo local.rules

```bash
sudo mkdir -p /etc/suricata/rules
cd /etc/suricata/rules
sudo nano local.rules

#Cole a seguinte regra para teste:
alert tcp any any -> any any (msg:"[TESTE] Alerta garantido"; sid:999999; rev:1;)
```
![Local.rules](prints/3.png)

## 🔧 4. Alteração no arquivo suricata.yaml

```bash
cd /etc/suricata
sudo nano suricata.yaml
```

Localize as linhas e edite:

default-rule-path: /etc/suricata/rules

rule-files:
  - local.rules

![Yaml](prints/4.png)

## 📒 5. Baixando um arquivo .pcap
- 🔗 Download do .pcap — [Malware Traffic Analysis](https://malware-traffic-analysis.net/2025/03/10/index.html)
- Senha: infected_20250310

![PCAP](prints/5.png)

## 🧪 6. Testando o Suricata

```bash
sudo suricata -r sample.pcap -l /var/log/suricata/
jq 'select(.event_type=="alert" and .alert.signature_id==999999)' /var/log/suricata/eve.json
```
![Teste Suricata](prints/6.png)

## 📦 7. Instalando o Splunk Enterprise
- 🔗 Site oficial do Splunk: [Splunk](https://www.splunk.com/en_us/download/splunk-enterprise.html)

Registre-se, acesse o e-mail e baixe a versão .deb

![Site Oficial](prints/7.png)
![Download](prints/8.png)

Utilize o comando wget:

```bash
wget -O splunk-9.4.3-237ebbd22314-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/9.4.3/linux/splunk-9.4.3-237ebbd22314-linux-amd64.deb"
sudo dpkg -i splunk-9.4.3-237ebbd22314-linux-amd64.deb
```
![wget](prints/9.png)
![dpkg](prints/10.png)

## 🔧 8. Primeira Configuração

```bash
sudo /opt/splunk/bin/splunk start --accept-license
```

Crie usuário e senha

![Licensa](prints/11.png)
![Http](prints/12.png)

Acesse via navegador: 

![Localhost](prints/13.png)
![Painel](prints/14.png)

## 🔁 9. Integração Suricata + Splunk
Verifique a pasta /var/log/suricata/ e localize o arquivo eve.json

```bash
cd /var/log/suricata/
```
![eve.json](prints/15.png)

Adicionando ao Splunk:
- Settings > Add Data > Monitor > Files & Directories
![Add Data](prints/16.png)
![Monitor](prints/17.png)
![Files & Directories](prints/18.png)
- Caminho: /var/log/suricata/eve.json
![Alertas Importados](prints/19.png)
- Hostname: Suricata | Index: main
![Hostname](prints/20.png)
![Confirmar](prints/21.png)
![Conclusão](prints/22.png)


## 📃 10. Leitura dos Logs no Splunk
- Acesse: Search & Reporting
![Search](prints/23.png)

```bash
index=* source="/var/log/suricata/eve.json" event_type=alert
```

![Alertas](prints/24.png)

## 🧱 11. Estrutura Final do Ambiente
- ✅ Suricata instalado e funcional
- ✅ Regras customizadas operacionais
- ✅ Leitura e análise de .pcap com alertas
- ✅ Splunk Enterprise instalado
- ✅ Integração e leitura de alertas do Suricata no Splunk