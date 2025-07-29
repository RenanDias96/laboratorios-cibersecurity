#  Réplica à Resposta da Equipe do ACS

Esta réplica traz pontos complementares à resposta oficial, com base nos próprios trechos apresentados e no cenário técnico observado durante os testes.

> O objetivo não é culpar o software em si, mas mostrar os riscos da configuração atual do ambiente, especialmente com acesso exposto à internet.

---

## Pontos em Destaque

### 1. Dispositivos “fantasma” realmente entram na plataforma

> “Como não existe autenticação na comunicação CPE -> ACS, é sim possível que dispositivos ‘fantasma’ sejam adicionados à plataforma.”

- Aqui está a confirmação de um dos principais riscos apontados: qualquer host pode se passar por uma CPE legítima e se registrar no ACS.
- Mesmo que esse “fantasma” seja excluído depois, o fato de ser aceito já abre margem para abuso. Desde coleta de informações até exploração futura.

---

### 2. Falta de autenticação é tratada como algo esperado

> “Esse ataque não é capaz de expor dados sensíveis nem permite a configuração de outros dispositivos.”

- Mesmo que o ataque não vá direto a dados sensíveis, a ausência total de autenticação e validação de origem permite:  
  - fingerprinting da aplicação
  - coleta de dados sobre a estrutura interna
  - entrada forçada de dispositivos falsos
  - manipulação indireta do banco

Essas são ações comprovadamente exploradas em campanhas automatizadas, e não podem ser tratadas como inofensivas.

---

### 3. HTTP aberto justificado por “compatibilidade”

> “Esse é o padrão por conta de modelos que não suportam conexão ao ACS com SSL/TLS.”

A justificativa é comum em ambientes legados, mas não segura.
Expor CWMP em HTTP, especialmente em portas acessíveis via internet, é um risco conhecido e documentado, sendo alvo direto de botnets.

---

### 4. Responsabilidade do provedor é clara

> “A responsabilidade pela exposição [...] pode ser feita na instalação do ACS ou posteriormente.”

Aqui fica claro que o problema não é o software ACS em si, mas a configuração feita pelo provedor.
Deixar a porta aberta para a internet, sem criptografia nem autenticação, é uma falha grave de segurança na infraestrutura do ISP.

---

### 5. Resposta 204 confirma parsing sem autenticação

> “A resposta HTTP 204 recebida foi o servidor interpretando a requisição como proveniente de uma CPE.”

> “Mesmo quando o método não é válido, responde com código 2xx para evitar quebra de comunicação.”

Isso mostra que o ACS aceita e interpreta comandos SOAP sem validar quem está mandando.
Esse comportamento é igual ao que os CVEs descritos nos documentos anteriores.

---

### 6. CVEs com comportamento parecido

Mesmo que não haja execução de código, o parsing dessas requisições SOAP abre caminho para ataques conhecidos:

| CVE               |                Vetor                                | Similaridade com Ambiente  |
|-------------------|-----------------------------------------------------|----------------------------|
| CVE-2017-17215    | RCE via `SetParameterValues` sem autenticação       | ✅ Alta                    |
| CVE-2014-9222     | Execução via CWMP sem validação de origem           | ✅ Moderada                |
| CVE-2018-10562    | Interface CWMP exposta publicamente sem autenticação| ✅ Alta                    |

Ou seja, só o fato de aceitar essas requisições já torna o ACS um alvo legítimo para ataques automatizados.

---

## Considerações finais

A equipe confirma:

- Não existe autenticação entre CPE e ACS
- Comandos SOAP arbitrários são processados
- A porta HTTP continua aberta por “compatibilidade”
- A segurança depende 100% da configuração feita pelo provedor, que nesse caso falhou

---
