# 🧬 Forense — Análise de Ameaças e Artefatos Maliciosos

<p align="center">
  <img src="../../assets/forense.png" alt="Capa do Laboratório de Cibersegurança" width="800"/>
</p>

Esta seção contém laboratórios voltados à **análise forense de malware e tráfego malicioso**, com foco em metodologias práticas e técnicas utilizadas por analistas de resposta a incidentes (IR) e investigadores de ameaças (Threat Hunters).

Aqui são aplicadas ferramentas como **Wireshark, tcpdump, YARA, PE-Bear, DIE e FLOSS**, com estudos baseados em **exemplos reais de malware** e capturas de tráfego suspeito.

---

## 📦 Estrutura Atual

| Pasta                                  | Descrição                                                                                                      |
|----------------------------------------|----------------------------------------------------------------------------------------------------------------|
| [Wireshark-Tcpdump](Wireshark-Tcpdump/README.md)                    | Análise de tráfego capturado (`.pcap`) com foco em botnets, C2 e exfiltração de dados.                         |
| [Yara-PEBear-Die-Floss](Yara-PEBear-Die-Floss/README.md)                | Análise estática de binários maliciosos (.exe) com ferramentas de engenharia reversa e criação de regras YARA. |

---

## 🧩 Metodologia

1. 🕵️ **Identificação de artefatos suspeitos**
2. 🔬 **Coleta e isolamento do material** (PCAPs, executáveis, amostras)
3. 📖 **Análise comportamental e estrutural**
4. 📊 **Extração de IOCs** (hashes, domínios, strings, signatures)
5. ⚙️ **Criação de assinaturas (YARA, Snort, etc.)**

---

## 🚧 Próximos Estudos (Planejados)

- 🧠 **Análise dinâmica com Cuckoo Sandbox**
- 🔒 **Extração de credenciais e keyloggers**
- 📁 **Recuperação de arquivos e strings ofuscadas**
- 🧪 **Comparações entre famílias de malware**

---

## 🎯 Objetivo

> Fornecer exemplos práticos e bem documentados de análise forense aplicável a ambientes reais de resposta a incidentes, SOC e threat intelligence.

---

> ⚠️ *Todas as análises foram realizadas em ambiente seguro e isolado, com amostras obtidas legalmente para fins educacionais.*
