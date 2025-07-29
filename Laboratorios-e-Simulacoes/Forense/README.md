# Forense — Análise de Ameaças e Malwares

<p align="center">
  <img src="../../assets/forense.png" alt="Capa do Laboratório de Cibersegurança" width="800"/>
</p>

Essa parte do repositório é focada em análise forense de malware e tráfego suspeito, com labs voltados para resposta a incidentes e threat hunting. 

Ferramentas usadas: Wireshark, tcpdump, YARA, PE-bear, DIE, FLOSS — entre outras que ajudam a entender o comportamento e criar detecções sob medida.

---

| Pasta                                  | Descrição                                                                                                      |
|----------------------------------------|----------------------------------------------------------------------------------------------------------------|
| [Wireshark-Tcpdump](Wireshark-Tcpdump/README.md)                    | Análises de tráfego (PCAP)                 |
| [Yara-PEBear-Die-Floss](Yara-PEBear-Die-Floss/README.md)                | Engenharia reversa em executáveis (.exe), explorando funções suspeitas, strings e criação de regras YARA personalizadas. |

---

## Metodologia

- Identificação de artefatos suspeitos**
- Coleta e isolamento do material (PCAPs, executáveis, amostras)
- Análise comportamental e estrutural
- Extração de IOCs (hashes, domínios, strings, signatures)
- Criação de assinaturas (YARA, Snort, Suricata etc)

---

> ⚠️ *Todas as análises foram realizadas em ambiente seguro e isolado, com amostras obtidas legalmente para fins educacionais.*
