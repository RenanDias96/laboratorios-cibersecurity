# 📊 Splunk + Suricata — Integração IDS + SIEM para Monitoramento de Rede

<p align="center">
  <img src="../../../assets/soc.png" alt="Capa do Laboratório de Cibersegurança" width="800"/>
</p>

Este projeto documenta a integração entre o Suricata (IDS) e o Splunk (SIEM) para construção de um ambiente funcional de monitoramento e resposta a incidentes em redes locais.

A proposta é simular, detectar e reagir a ameaças, usando ferramentas amplamente utilizadas em ambientes corporativos.

---
## O que você vai encontrar aqui:

- Detecção em tempo real de comportamentos suspeitos
- Análise de tráfego e alertas gerados pelo IDS
- Dashboards de visualização no Splunk
- Estratégias de resposta e mitigação com firewall e scripts

---

## Por que essa integração?

- Suricata: faz inspeção profunda de pacotes, suporta regras customizadas, detecta desde portscan até payloads suspeitos.
- Splunk: organiza e exibe os alertas em tempo real, com filtros, painéis e buscas para facilitar a análise.
Juntos, formam um pipeline completo: detecção → visualização → resposta.

---

## Estrutura da Pasta

| Pasta                        |                     Finalidade                                                     |
|------------------------------|------------------------------------------------------------------------------------|
|  [Instalacao](Instalacao/README.md)                | Instalação do ambiente com Suricata + Splunk e dependências. |
|  [Regras-e-Alertas](Regras-e-Alertas/README.md)          | Regras personalizadas Suricata e exemplos de eventos detectados (portscan, etc).   |
|  [Mitigacoes-e-Respostas](Mitigacoes-e-Respostas/README.md)    | Resposta após detecção: bloqueios, automação, firewall, etc.        |

> Cada subpasta tem seu próprio README explicando o contexto, o cenário simulado e os resultados obtidos.

---

## Em Desenvolvimento

- Novas regras de detecção (ex: C2, brute force, exfiltração)
- Integrações com dashboards mais visuais
- Scripts de resposta automática
- Conexões futuras com Zeek, Wazuh e Elastic Stack

---

## ⚠️ Aviso

> Todos os testes foram feitos em ambiente de laboratório, isolado da internet. O conteúdo é técnico, prático e com finalidade exclusivamente educacional.
