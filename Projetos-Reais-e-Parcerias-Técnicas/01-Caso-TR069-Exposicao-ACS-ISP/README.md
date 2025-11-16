# 🛰️ Investigação Técnica: Exposição ACS (TR-069) em Ambiente ISP Real

**Início da análise:** 03/06/2025  
**Autor:** Renan Dias Mends  
**Foco:** Detecção, análise e documentação de uma falha de exposição CWMP (TR-069) em servidor ACS de um ISP real.

---

## 📝 Introdução

Identifiquei um servidor ACS exposto à internet e iniciei uma investigação com foco em:

- Entender os riscos associados à exposição TR-069

- Simular requisições legítimas de CPEs

- Documentar impactos e boas práticas de mitigação

Todo o processo foi **ético e seguro**: sem exploração ofensiva, com autorização prévia, uso de ferramentas open-source e foco exclusivo em documentação técnica.
---

## 📂 Estrutura do Material

O material está organizado em 5 pastas principais, seguindo a ordem cronológica da investigação:

|Etapa|                   Pasta                           |                           Descrição                                                         |
|----|----------------------------------------------------|---------------------------------------------------------------------------------------------|
| 1️⃣ | [Exposição Inicial](01-Analise-Inicial-Exposicao/README.md)                     | Análise inicial da exposição (portas, painel, respostas básicas) |
| 2️⃣ | [Vulnerabilidades em Requisições SOAP](02-Analise-Tecnica-ACS-SOAP/README.md)                      | Testes com requisições SOAP reais e avaliação de riscos |
| 3️⃣ | [Resumo](03-Resumo-Tecnico-ACS/README.md)                            | Versão resumida para leitura rápida ou público não técnico |
| 4️⃣ | [Resposta-ACS](04-Resposta-ACS/README.md)                                  | Resposta da equipe técnica responsável pelo ACS |
| 5️⃣ | [Replica](05-Replica-Tecnica/README.md)                               | Réplica com argumentos e sugestões de mitigação |

> ⚡ Seguir a ordem facilita compreender a evolução da investigação, do descobrimento inicial às respostas do fornecedor.

---
s
## 🔍 Por que isso importa

- Ataques TR-069 não são novidade: exemplos como o Mirai exploraram exposições similares.

- A ausência de autenticação nas requisições SOAP permite simular CPEs e enviar comandos ao ACS, aumentando significativamente a superfície de ataque.

- Documentar essas falhas ajuda a reforçar boas práticas e segurança em ISPs.

- E sim… eu sou cliente do ISP em questão 😅
  
---

## ⚖️ Ética e responsabilidade

- Nenhum dado sensível foi divulgado.
- Nomes e logotipos foram anonimizados
- Testes foram passivos, focando apenas em simular comportamentos legítimos de CPE

---

> ⚠️ *Esta investigação documenta uma exposição crítica real com rigor técnico e ética. Objetivo: educacional, contribuindo para melhores práticas em ISPs e segurança de redes.*
