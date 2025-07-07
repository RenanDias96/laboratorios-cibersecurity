# 🛰️ Investigação Técnica: Exposição ACS (TR-069) em Ambiente ISP Real

📅 **Início da análise:** 03/06/2025  
✍️ **Autor:** Renan D M  
🔎 **Foco:** Detecção, análise e documentação de uma falha de exposição CWMP (TR-069) em servidor ACS de um ISP real.

---

## 🧭 Introdução

Este conjunto de laboratórios foi originado a partir da **detecção real de um painel ACS (TR-069)** exposto à internet pública por um ISP. A análise foi conduzida com o objetivo de **avaliar riscos técnicos**, **simular comportamentos de dispositivos CPEs** e **documentar o impacto de requisições SOAP não autenticadas**.

Toda a investigação foi **realizada de forma ética**, sem qualquer tipo de ataque destrutivo e **com aprovação prévia do ISP**. As provas de conceito são controladas e voltadas exclusivamente à **detecção e documentação de riscos técnicos**, utilizando apenas ferramentas open source, sem exploração ofensiva.

---

## 🧱 Estrutura de Navegação

O material está dividido em **5 pastas principais**, seguindo uma **ordem cronológica de investigação**. Cada pasta contém documentação detalhada, testes práticos e prints ilustrativos.

|Etapa|                   Pasta                           |                           Descrição                                                         |
|----|----------------------------------------------------|---------------------------------------------------------------------------------------------|
| 1️⃣ | [01-Analise-Inicial-Exposicao](01-Analise-Inicial-Exposicao/README.md)                     | Primeira análise da superfície exposta (portas abertas, painel web, respostas básicas)      |
| 2️⃣ | [02-Analise-Tecnica-ACS-SOAP](02-Analise-Tecnica-ACS-SOAP/README.md)                      | Versão condensada e didática dos testes e descobertas mais relevantes                       |
| 3️⃣ | [03-Resumo-Tecnico-ACS](03-Resumo-Tecnico-ACS/README.md)                            | Resumo técnico com linguagem acessível para público leigo ou não técnico                    |
| 4️⃣ | [04-Resposta-ACS](04-Resposta-ACS/README.md)                                  | Resposta oficial da equipe técnica do ACS aos riscos apontados                              |
| 5️⃣ | [05-Replica-Tecnica](05-Replica-Tecnica/README.md)                               | Réplica técnica e fundamentada em cima da resposta do fornecedor                            |

> 📌 **Recomenda-se a leitura na ordem acima**, para compreender o avanço da investigação, os riscos reais identificados e as implicações de segurança.

---

## 💡 Por que isso importa?

- Exposições TR-069 já foram amplamente exploradas por **botnets como Mirai, Hajime e variantes**, causando **comprometimento massivo de CPEs** e **sequestro de infraestrutura ISP**.
- A falta de autenticação em requisições SOAP permite simular dispositivos e injetar comandos no ambiente ACS — sem validação de origem ou identidade.
- Detectar, documentar e reportar este tipo de falha é **essencial para reduzir a superfície de ataque em ambientes reais de telecomunicações.**

---

## 🔒 Ética e Anonimização

- Nenhum dado sensível foi publicado.
- Todas as menções ao nome do fornecedor foram removidas dos textos e prints.
- Prints que exibiam logotipos foram editados para preservar a identidade visual da ferramenta.
- Os testes foram **não intrusivos**, com foco apenas em simulação passiva de comportamentos.

---

## 📩 Contato

Caso tenha dúvidas ou deseje discutir o caso tecnicamente:

📧 **renandmm96@gmail.com**

---

> ⚠️ *Esta investigação representa uma situação real de exposição crítica, com documentação técnica precisa e ética. O objetivo é educacional, contribuindo para melhores práticas em ISPs e infraestrutura de rede.*
