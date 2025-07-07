# 🛡️ Mitigações e Respostas — Ações Defensivas no Suricata

<p align="center">
  <img src="../../../../assets/construcao.png" alt="Em construção" width="800"/>
</p>

Esta pasta é dedicada à implementação de **respostas práticas e automáticas a eventos detectados**, com foco em ambientes que utilizam **Suricata como IDS/IPS** e **integração com ferramentas como Splunk ou firewall locais**.

O objetivo é transformar alertas em ações reais de contenção e mitigação — desde o simples **drop de tráfego malicioso**, até **respostas condicionais a PoCs mais sofisticadas**.

---

## ⚙️ Objetivos da Seção

- 🧱 Criar políticas reativas com **Suricata em modo IPS (inline)**  
- 🔥 Integrar com **iptables/firewalld** para bloqueios dinâmicos
- 🤖 Simular **respostas automatizadas** a ataques (portscan, beaconing, etc.)
- 🧩 Estudar integração com **scripts externos**, **SOAR** e pipelines de resposta

---

## 🚧 Em Desenvolvimento

| Tópico                              | Descrição                                                                         |
|-------------------------------------|-----------------------------------------------------------------------------------|
| 🧨 **Drop automático de Portscan**  | Bloqueio imediato de IPs que realizam varreduras de porta detectadas via Suricata |
| 🪓 **Resposta a Exploração HTTP**   | Detecção de payloads malformados e envio de resets TCP                            |
| 📈 **Reação baseada em volume**     | Resposta condicional baseada em taxa de eventos (ex: SYN flood, burst)            |

---

## 📌 Considerações

> A proposta desta seção é avançar para um modelo **reativo**, saindo da simples detecção passiva. O foco é explorar ao máximo os recursos do Suricata em modo ativo, mas sempre com **cuidado para evitar falsos positivos e impactos na operação legítima da rede**.

---

## 🧪 Ambiente Sugerido

- Suricata operando com `NFQUEUE` ou modo IPS
- Firewall Linux com `iptables` configurado
- Scripts auxiliares (ex: bloqueio automático com `fail2ban`, integração Python/Bash)
- Simulações controladas via `nmap`, `hping3` e tráfego realístico

---

> ⚠️ *Todos os testes realizados nesta pasta seguem em ambiente controlado. O objetivo é educacional e voltado à melhoria de práticas defensivas em redes reais.*
