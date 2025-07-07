# 📊 Splunk + Suricata — Integração IDS + SIEM para Monitoramento de Rede

<p align="center">
  <img src="../../../assets/soc.png" alt="Capa do Laboratório de Cibersegurança" width="800"/>
</p>

Esta pasta documenta a integração entre duas ferramentas robustas: **Suricata**, um sistema de detecção de intrusão (IDS), e **Splunk**, uma poderosa plataforma de análise e correlação de logs.

A união dessas ferramentas forma um ambiente funcional de **Network Monitoring System (NMS)** e **Security Operations Center (SOC)**, voltado para:

- 🚨 **Detecção de ameaças em tempo real**
- 🔍 **Análise detalhada de tráfego**
- 📈 **Visualização e correlação de eventos**
- 🛡️ **Resposta ativa a incidentes**

---

## 🛠️ Por que Splunk + Suricata?

🔹 **Suricata** permite a inspeção profunda de pacotes (DPI), com suporte a regras personalizadas e decodificação de protocolos.  
🔹 **Splunk** atua como SIEM, centralizando e visualizando os alertas em dashboards analíticos e acionáveis.  

A integração permite capturar eventos gerados pelo Suricata e exibi-los em tempo real no Splunk, viabilizando um fluxo eficiente de **detecção → análise → resposta.**

---

## 🧱 Estrutura da Pasta

| Pasta                        |                     Finalidade                                                     |
|------------------------------|------------------------------------------------------------------------------------|
|  [Instalacao](Instalacao/README.md)                | Instalação do ambiente com Suricata + Splunk, incluindo forwarders e dependências. |
|  [Regras-e-Alertas](Regras-e-Alertas/README.md)          | Regras personalizadas Suricata e exemplos de eventos detectados (portscan, etc).   |
|  [Mitigacoes-e-Respostas](Mitigacoes-e-Respostas/README.md)    | Estratégias de resposta após detecção: bloqueios, automação, firewall, etc.        |

> 📌 As detecções práticas (ex: varredura de portas) são explicadas em seus próprios `README.md` nas subpastas.

---

## 🚧 Em Desenvolvimento

Este ambiente será expandido com novos cenários de ataque e defesa:

- 📦 Novas regras de detecção para ataques específicos
- 📡 Integração com dashboards visuais em Splunk
- 🤖 Scripts automatizados de mitigação
- 🔗 Correlação com outras ferramentas (ex: Zeek, Wazuh, Elastic)

---

## 🧪 Objetivo do Projeto

> Criar uma base sólida e prática para **investigação, resposta e mitigação de ameaças de rede**, especialmente em ambientes simulados e redes locais.

---

## ⚠️ Aviso

Todos os testes foram conduzidos em laboratório controlado. As ferramentas são open source e a finalidade é exclusivamente **educacional e técnica.**

---

