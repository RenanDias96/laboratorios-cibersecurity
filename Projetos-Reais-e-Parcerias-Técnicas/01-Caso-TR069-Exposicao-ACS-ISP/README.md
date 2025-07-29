# Investigação Técnica: Exposição ACS (TR-069) em Ambiente ISP Real

**Início da análise:** 03/06/2025  
**Autor:** Renan D M  
**Foco:** Detecção, análise e documentação de uma falha de exposição CWMP (TR-069) em servidor ACS de um ISP real.

---

## Introdução

Esta investigação começou a partir da identificação de um servidor ACS (TR-069) exposto à internet por um ISP. O objetivo foi entender os riscos envolvidos, simular requisições legítimas feitas por CPEs e documentar as implicações de segurança.

Todo o processo foi feito com responsabilidade: sem exploração ofensiva, com autorização prévia, uso de ferramentas open source e foco exclusivo em documentação técnica.

---

## Estrutura

O material está dividido em **5 pastas principais**, seguindo uma **ordem cronológica de investigação**. Cada pasta contém documentação detalhada, testes práticos e prints ilustrativos.

|Etapa|                   Pasta                           |                           Descrição                                                         |
|----|----------------------------------------------------|---------------------------------------------------------------------------------------------|
| 1️⃣ | [Exposição Inicial](01-Analise-Inicial-Exposicao/README.md)                     | Análise inicial da exposição (portas, painel, respostas básicas) |
| 2️⃣ | [Vulnerabilidades em Requisições SOAP](02-Analise-Tecnica-ACS-SOAP/README.md)                      | Testes com exemplos reais de requisições e respostas |
| 3️⃣ | [Resumo](03-Resumo-Tecnico-ACS/README.md)                            | Versão resumida para leitura rápida ou público não técnico |
| 4️⃣ | [Resposta-ACS](04-Resposta-ACS/README.md)                                  | Resposta da equipe técnica responsável pelo ACS |
| 5️⃣ | [Replica](05-Replica-Tecnica/README.md)                               | Réplica com argumentos e sugestões de mitigação |

> A leitura na ordem sugerida ajuda a entender a progressão da investigação, desde a descoberta inicial até os desdobramentos com o fornecedor.

---

## Por que isso importa?

- Ataques baseados em TR-069 não são novidade. Casos como Mirai exploraram exatamente esse tipo de exposição para comprometer milhões de dispositivos. A ausência de autenticação nas requisições SOAP permite simular um CPE e injetar comandos no ACS, o que amplia drasticamente a superfície de ataque de um ISP.
- Documentar esse tipo de falha é essencial para incentivar boas práticas e reforçar a segurança em ambientes reais de telecomunicações.
- E eu sou cliente do ISP em questão rsrs
  
---

## Ética e responsabilidade

- Nenhum dado sensível foi divulgado.
- Todos os nomes e logotipos foram removidos ou anonimizados.
- Os testes foram passivos, focados apenas em simular comportamentos legítimos de CPE.

---

> ⚠️ *Esta investigação representa uma situação real de exposição crítica, com documentação técnica precisa e ética. O objetivo é educacional, contribuindo para melhores práticas em ISPs e infraestrutura de rede.*
