# 🛠 Ferramentas para Análise de Malware

<p align="center">
  <img src="../../../assets/yara.png" alt="Capa Ferramentas" width="600"/>
</p>

Esta pasta lista e descreve as ferramentas utilizadas nos laboratórios de análise de malware e investigação forense, separadas por categoria.

---

## 🔹 Análise Estática de Binários

- **YARA**: criação de regras para identificar malwares por padrões de código ou strings.  
- **PEBear**: análise detalhada de arquivos PE (Portable Executable).  
- **DIE (Detect It Easy)**: identificação de packers, compressão e criptografia em binários.  
- **FLOSS**: extração e análise de strings de executáveis, mesmo quando ofuscados.

---

## 🔹 Análise de Tráfego e Rede

- **Wireshark**: captura e análise detalhada de tráfego de rede (PCAPs), útil para identificar comunicação suspeita.  
- **tcpdump**: captura de pacotes de rede via linha de comando, complementar ao Wireshark.

---

##  📂 Estrutura da Pasta

| Pasta                            | Descrição                                                                                       |
|----------------------------------|-------------------------------------------------------------------------------------------------|
| [YARA, PEBear, DIE e FLOSS](Yara-PEBear-Die-Floss/README.md) | Guias de instalação, configuração e uso dessas ferramentas de análise de binários.             |
| [Wireshark + Tcpdump](Wireshark-Tcpdump/README.md)           | Guias de instalação, configuração e uso para captura e análise de tráfego de rede.             |

---

## ⚡ Observações

- Todas as ferramentas devem ser utilizadas **em ambientes isolados e seguros**.  
- Os laboratórios de análise real de malware são documentados nas pastas correspondentes de cada lab.  
- Este README serve apenas como referência rápida das ferramentas disponíveis.

---
