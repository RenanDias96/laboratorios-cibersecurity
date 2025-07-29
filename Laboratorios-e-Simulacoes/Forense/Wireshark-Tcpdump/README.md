#  Wireshark & Tcpdump — Análise de Tráfego Malicioso

<p align="center">
  <img src="../../../assets/wireshark.png" alt="Capa do wireshark" width="800"/>
</p>

Essa parte do lab é focada em tráfego. Aqui, busco analisar capturas reais (simuladas com segurança) e usar ferramentas como Wireshark e tcpdump pra entender como ataques acontecem. Desde um simples scan até C2, exfiltração e movimentação lateral.

Tudo foi feito em ambiente controlado, com cenários montados pra simular incidentes que podem acontecer em um SOC ou numa investigação real.

---

| Pasta                            | Descrição                                                                                                                                          |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| [Instalacao](Instalacao/README.md)                     | Passo a passo da instalação do Wireshark e tcpdump, configuração básica e dicas iniciais.                                                          |
| [Analise-de-Malware](Analise-de-Malware/README.md)             | Casos de análise de tráfego malicioso a partir de PCAPS, com foco em padrões de infecção, persistência, C2 e extração de IOCs. |


---

## Em andamento / Próximos passos

- Tráfego malicioso escondido dentro de protocolos legítimos (DNS, HTTPS, etc)
- Exfiltração de arquivos via HTTP/FTP
- Comunicação contínua com C2 remoto
- Simulação de campanhas phishing com captura completa da cadeia

---

> ⚠️ *Todas as amostras foram processadas em ambiente isolado e os dados utilizados foram anonimizados. Foco exclusivo em aprendizado técnico e ético.*
