# 📄 Réplica Técnica à Resposta da Equipe do ACS

Esta réplica apresenta **considerações complementares** aos pontos abordados, com base técnica e nos próprios trechos do documento oficial.

> ⚠️ **Nota:** Esta réplica não busca atribuição de falhas ao software, mas sim reforçar os riscos observados na **configuração atual do ambiente do ISP**, conforme os testes autorizados.

---

## 🔍 Pontos Técnicos em Destaque

### 1. ✅ CPEs “fantasmas” podem ser adicionados sem restrições

> “Como não existe autenticação na comunicação CPE -> ACS, é sim possível que dispositivos ‘fantasma’ sejam adicionados à plataforma.”

Essa confirmação reforça o ponto central da análise: **qualquer host pode iniciar a comunicação CWMP com o ACS, mesmo vindo da internet pública**, simulando o comportamento de uma CPE legítima.  
Mesmo que esses dispositivos sejam excluídos por rotina, o **simples aceite e entrada no banco de dados já representa uma vulnerabilidade por design.**

---

### 2. ❗ Falta de autenticação e validação de origem são assumidas como normais

> “Esse ataque não é capaz de expor dados sensíveis nem permite a configuração de outros dispositivos.”

Mesmo com essas limitações, a ausência de autenticação e validação **permite fingerprinting, enumeração da estrutura interna do ACS e manipulação externa do banco de dados.**  
Esse comportamento representa **vetor real de ataque**, já documentado em diversas campanhas automatizadas.

---

### 3. 🔓 Exposição via HTTP justificada por “compatibilidade”

> “Esse é o padrão por conta de modelos que não suportam conexão ao ACS com SSL/TLS.”

Essa justificativa é compreensível do ponto de vista de compatibilidade, mas **frágil tecnicamente**:  
**Portas CWMP em HTTP, sem criptografia ou autenticação**, são o **padrão explorado** por botnets como **Mirai**.

> “A utilização de portas em HTTP é opcional.”

⚠️ Mesmo sendo opcional, manter a porta **57547** em **HTTP** **expõe o ambiente ao risco de interceptação e ataques automatizados.**

---

### 4. 🛠️ Responsabilidade do provedor é reconhecida

> “A responsabilidade pela exposição [...] pode ser feita na instalação do ACS ou posteriormente.”

Isso confirma o foco do alerta: **o problema não está no sistema ACS, mas sim na configuração insegura do provedor**, que expôs a porta à internet pública **sem criptografia e sem autenticação.**

---

### 5. 📡 Resposta 204 confirma parsing sem autenticação

> “A resposta HTTP 204 recebida foi o servidor interpretando a requisição como proveniente de uma CPE.”

> “Mesmo quando o método não é válido, responde com código 2xx para evitar quebra de comunicação.”

Isso reforça que o servidor **parseia e interpreta comandos SOAP mesmo sem autenticação**, comportamento **idêntico ao de ambientes explorados por CVEs como Mirai e Hajime.**

Essa política de resposta “failsafe” **favorece o sucesso de scanners automatizados**, que **identificam o ACS como válido e prosseguem com ataques.**

---

### 6. 📊 Tabela de CVEs comparadas (comportamento semelhante)

Mesmo sem execução real, o parsing de payloads SOAP com resposta 204 **replica o comportamento explorado por CVEs conhecidas**:

| CVE               |                Vetor                                | Similaridade com Ambiente  |
|-------------------|-----------------------------------------------------|----------------------------|
| CVE-2017-17215    | RCE via `SetParameterValues` sem autenticação       | ✅ Alta                    |
| CVE-2014-9222     | Execução via CWMP sem validação de origem           | ✅ Moderada                |
| CVE-2018-10562    | Interface CWMP exposta publicamente sem autenticação| ✅ Alta                    |

> 🔎 O parsing sem autenticação, mesmo que sem execução real, **marca o ACS como alvo viável** para campanhas automatizadas.

---

## ✅ Considerações Finais

A própria equipe responsável pelo ACS confirma que:

- ❌ Não há autenticação entre CPE e ACS;
- ✅ SOAPs arbitrários são parseados e respondidos;
- 🔓 Porta HTTP permanece ativa por decisão de compatibilidade;
- ⚠️ Segurança depende **inteiramente da configuração do provedor**, que **no ambiente testado, estava vulnerável**.

---

## 🔐 Recomendações Reforçadas

- 🔐 **Bloquear portas 7547 e 57547 na borda pública**, permitindo acesso apenas de IPs internos confiáveis.
- 🔒 **Substituir HTTP por HTTPS**, com fallback controlado ou tunelamento via VPN.
- 🧱 **Aplicar regras de firewall (iptables)** usando `connlimit`, `recent`, `psd`, etc.
- 🧪 **Implementar autenticação mínima**, mesmo opcional, para novos dispositivos CPE.
- 📈 **Habilitar logging e alertas em SIEM/Syslog** para SOAPs externos suspeitos.
- 🧼 **Remover automaticamente CPEs inativos ou anômalos.**
- 🕵️ **Revisar redes de origem autorizadas no ACS.**
- 📶 **Isolar o ACS em VLAN/sub-rede dedicada com controle de fluxo.**

---

## 🙋‍♂️ Conclusão

Coloco-me à disposição para esclarecimentos, apoio técnico ou colaboração na mitigação.

> ✅ Toda análise foi conduzida com **transparência**, visando exclusivamente a **proteção dos ativos do ISP e dos usuários finais**.
