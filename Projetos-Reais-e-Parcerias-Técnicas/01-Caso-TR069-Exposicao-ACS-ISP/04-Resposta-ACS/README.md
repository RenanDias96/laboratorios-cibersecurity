Resposta Técnica da Equipe Responsável pelo ACS

# Réplica Técnica: Resposta da Equipe Responsável pelo ACS

⚠️ **Nota de Privacidade**

Para fins de documentação técnica e respeito à confidencialidade, **o nome da empresa fornecedora do sistema ACS, bem como o nome do técnico responsável pela resposta, foram intencionalmente omitidos** neste registro. O conteúdo a seguir foi mantido na íntegra, respeitando fielmente os argumentos apresentados pela equipe responsável.

---

# Resposta às preocupações de segurança

Gostaria inicialmente de agradecer a disponibilidade e preocupação com a segurança do sistema. Neste documento abordaremos as preocupações de segurança relacionadas ao sistema ACS e à exposição de portas sem autenticação/criptografia na plataforma, com os pontos que forem pertinentes sendo comentados.

---

## Primeiro documento [Análise Inicial de Exposição TR-069 em Ambiente ISP Real](../01-Analise-Inicial-Exposicao/README.md)

**1.1** O painel TR-069 (ACS) do ISP está publicamente acessível, operando em HTTP sem nenhuma criptografia.

- O ACS possui uma funcionalidade de redirect de requisições. Quando o painel de login é acessado via HTTP, ele forçadamente se redireciona para HTTPS. Além disso, existe um sistema de permissionamento de redes de origem de login. Todas as redes fora da configuração são barradas.
- O ACS também suporta a ativação de MFA, através de aplicativos de geração de token, porém essa funcionalidade não é obrigatória.
- Quanto à porta 7547 exposta em HTTP, esse é o padrão por conta de modelos de dispositivos que não suportam conexão ao servidor ACS com SSL/TLS. Porém, apesar de padrão, a utilização de portas em HTTP é opcional, havendo a possibilidade de configuração em cada base para a recepção das requisições via HTTPS. Isso é feito no momento da instalação do sistema, mas pode ser solicitado ao suporte a qualquer momento.

---

## Segundo documento [Resumo Técnico da Análise de Exposição CWMP (TR-069) em Ambiente ISP Real](../03-Resumo-Tecnico-ACS/README.md)

**2.1** Um atacante externo consegue simular um roteador da rede e enviar comandos ao servidor ACS sem qualquer autenticação, mesmo via internet pública. Isso representa um risco imediato de comprometimento da infraestrutura e dos clientes.

Sobre essa afirmação, acho pertinente explorarmos inicialmente a arquitetura da aplicação e do protocolo CWMP.

- O protocolo CWMP funciona com a arquitetura cliente -> servidor, onde a CPE é o cliente. A CPE é quem inicia a comunicação, mas ela não tem poder algum de executar modificações no servidor. Quando uma requisição é recebida nas portas que expõem o protocolo CWMP, nenhuma modificação é feita no servidor: Apenas na entidade desse dispositivo específico no banco de dados. Pelo contrário, é a CPE que é controlada através dessa requisição.
- Quanto ao ACS especificamente, o protocolo CWMP é exposto por uma porta (normalmente 7547), e a API REST que permite fazer alterações em dispositivos por outra (443). A API, que realmente tem poder de configuração, está atrás de HTTPS e autenticação (por sessão quando via app, por Oauth quando via API), além de um sistema de permissionamento e do controle de origem citado anteriormente.

**2.2** Essa exposição é uma vulnerabilidade crítica, já explorada em incidentes reais

- A CVE-2017-17215 é uma vulnerabilidade encontrada em CPEs que possuem TR-064, um protocolo obsoleto e que foi descontinuado por questões de segurança. A vulnerabilidade acontece quando uma CPE recebe um comando não autorizado, essa CVE não afeta os dispositivos conectados ao ACS e nem o seu servidor.

**2.3** Comandos administrativos (como alteração de configurações) foram aceitos e processados com resposta HTTP 204, o que confirma a interpretação real dos comandos.

- Como citado acima, comandos administrativos não são recebidos via CWMP. Todos os comandos administrativos são recebidos por API REST, com controles de autenticação e acesso robustos. A resposta HTTP 204 recebida foi o servidor ACS interpretando a requisição como proveniente de um CPE normal, e como dita a especificação técnica do protocolo, a resposta à CPE deve ser um HTTP 200/204.

**2.4** Não há senha, token ou verificação de origem — qualquer host consegue enviar instruções diretamente ao ACS.

- Como não existe autenticação na comunicação CPE -> ACS, é sim possível que dispositivos “fantasma” sejam adicionados à plataforma. Isso é um risco potencialmente mitigado pela exposição da porta apenas na rede interna do provedor, procedimento esse que pode ser feito na instalação do ACS ou posteriormente.
- Apesar disso, esse ataque não é capaz de expor dados sensíveis nem permite a configuração de outros dispositivos na rede. A implementação de senhas de autenticação nesta comunicação não é suportada por todas as CPEs, como é o caso do HTTPS, por isso essa implementação ainda não foi feita, visto que tal alteração limitaria em muito os modelos suportados pelo ACS.

---

## Terceiro documento[Análise Técnica da Comunicação SOAP e Riscos no ACS (TR-069) - Ambiente ISP Real](../02-Analise-Tecnica-ACS-SOAP/README.md)

**3.1** Portas abertas sem aparente filtragem, Presença de portas incomuns pode indicar exposição indevida

- Como citado anteriormente, as portas têm sim filtragem por firewall, permitindo apenas conexões a partir da rede interna do provedor. A função de cada porta está descrita abaixo:

| Porta | Descrição                                                                                                                        |
|-------|--------------------------------------------------------------------------------------------------------------------------------- |
| 80    | Porta HTTP para recebimento de requisições para API. Uso não recomendado. Também funciona como fallback da porta 7547 para CPEs. |
| 443   | Porta HTTPS para recebimento de requisições via app ou API. Expõe API REST (possui autenticação).                                |
| 3000  | Servidor de Websocket para a aplicação front-end (possui autenticação).                                                          |
| 4007  | Servidor GraphQL de logs (possui autenticação).                                                                                  |
| 7547  | Porta HTTP para recebimento de requisições CWMP (configurável).                                                                  |
| 7547  | Porta HTTPS para recebimento de requisições CWMP (configurável).                                                                 |

- Nenhum dos serviços expostos é estranho ao ACS.

**3.2** Etapas 3 e 4 - Envio de um SOAP com simulação de Inform do dispositivo

- Esse é um exemplo da vulnerabilidade citada no ponto 2.4. Dispositivos “fantasma” podem ser adicionados à plataforma. Porém, esses dispositivos são facilmente identificáveis por não possuírem comportamentos essenciais a um dispositivo real. Além disso, isso não permite nenhum tipo de exposição de dados de outros dispositivos, nem seu controle ou da plataforma. Através de configurações de rotina do ACS, estes dispositivos podem ser automaticamente excluídos da plataforma a partir do momento que ficarem offline, mitigando o problema de acúmulo de dispositivos fantasma (problema este que pode ser evitado com a adoção de políticas no firewall já incluso na instalação do ACS).

**3.4** Mesmo sem execução, a aceitação do conteúdo com código 204 demonstra parsing sem autenticação ou validação — vetor semelhante a múltiplas CVEs de RCE via TR-069.

- O protocolo CWMP expõe RPCs específicos de cliente e de servidor. A RPC “SetParameterValues” é de uso exclusivo do servidor, e não tem efeito prático quando utilizada através da API para fazer modificações em dispositivos.
- O fato do servidor retornar 204 é um failsafe implementado para evitar envio de respostas com códigos 4xx à CPE. Abro um parênteses sobre esse comportamento:
    - O ACS não rejeita SOAPs sintaticamente corretos (por mais que semanticamente não estejam) por conta de limitações de alguns dispositivos. Existem CPEs que cessam completamente as requisições quando são respondidas com códigos de erro. Para evitar isso, o ACS decodifica e identifica corretamente que o método chamado não é válido, porém mesmo assim responde com um código 2xx.

**3.5** A superfície exposta pelo ACS replica, em todos os aspectos essenciais, os padrões de falha amplamente explorados por botnets como Mirai, Hajime e variantes.

- Novamente, essa CVE e este vetor de ataque são exclusivamente encontrados em CPEs. Não são vulnerabilidades às quais um servidor é suscetível.

---

Tentei ser o mais breve possível, e acredito que todos os pontos críticos da análise foram esclarecidos. Caso ainda reste alguma dúvida ou seja necessária uma explicação mais aprofundada sobre algum ponto, estou à disposição.

Mais uma vez, agradecemos o empenho e interesse em colaborar com a segurança da plataforma ACS. Nossa equipe de segurança da informação também colabora ativamente para a proteção dos ativos de rede manipulados pela ferramenta. Ainda assim, a exposição de alguns serviços desconhecidos ao usuário pode trazer dúvidas ao cliente, e é sempre pertinente que tais dúvidas sejam esclarecidas ou que venham a se tornar uma melhoria para a plataforma.

---

_Técnico responsável._  
_Equipe de desenvolvimento do ACS_
