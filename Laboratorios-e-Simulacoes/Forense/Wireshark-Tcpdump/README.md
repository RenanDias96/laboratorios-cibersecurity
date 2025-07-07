# 📡 Wireshark & Tcpdump — Análise de Tráfego Malicioso

<p align="center">
  <img src="../../../assets/wireshark.png" alt="Capa do wireshark" width="800"/>
</p>

Esta pasta contém estudos práticos com **Wireshark** e **tcpdump** voltados à **análise forense de tráfego de rede**, com foco em identificação de ameaças, comunicação com C2 (Command and Control), exfiltração de dados e movimentação lateral.

Todos os casos foram reproduzidos em ambiente seguro, com tráfego capturado a partir de simulações controladas.

---

## 📦 Estrutura Atual

| Pasta                            | Descrição                                                                                                                                          |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| [Instalacao](Instalacao/README.md)                     | Passo a passo da instalação do Wireshark e tcpdump, configuração básica e dicas iniciais.                                                          |
| [Analise-de-Malware](Analise-de-Malware/README.md)             | Casos práticos de análise de tráfego malicioso a partir de arquivos `.pcap`, com foco em padrões de infecção, persistência, C2 e extração de IOCs. |


---

## 🔍 Abordagens Utilizadas

- Filtragem e reconstrução de pacotes TCP/UDP
- Identificação de padrões anômalos no tráfego
- Marcação de sessões e análise por fluxo
- Extração de IOCs diretamente do tráfego
- Uso de perfis personalizados no Wireshark

---

## 🎯 Objetivo

> Ensinar a detectar e interpretar tráfego malicioso em ambientes reais, partindo de capturas simples até análises avançadas com correlação de eventos.

---

## 🚧 Em Expansão

As próximas análises incluirão:

- 🧠 Tráfego malicioso disfarçado de serviços legítimos (DNS, HTTPS)
- 📤 Exfiltração de arquivos via FTP e HTTP
- 🔀 Comunicação persistente com C2s remotos
- 🎣 Capturas reais de campanhas phishing (emulada)

---

> ⚠️ *Todas as amostras foram processadas em ambiente isolado e os dados utilizados foram anonimizados. Foco exclusivo em aprendizado técnico e ético.*
