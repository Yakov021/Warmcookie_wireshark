# Análise de Tráfego: Malware WarmCookie 🍪

Repositório dedicado à análise forense de rede do trojan WarmCookie, focado em detecção de evasão, beaconing e movimentação lateral.

## 📌 Resumo Executivo (TL;DR)
- **PACIENTE_ZERO** IP ( 10.8.15.133)
- **Status_Code200** para arquivo connecttest.txt
- **Arquivo_malicioso** Invoice 876597035_003.zip (assinatura PK)
- **Host_Suspeito** quote.checkfedexexp.com (typosquatting) trafego total de  2.769 KB
- **Técnica de Evasão:** Mascaramento de ZIP como TXT (Magic Bytes check).
- **C&C:** Identificado via SYN anômalo e beaconing UDP.
- **ICMP_port_unreachable** tipo 3 , tentativa de comunicacao falha, ou exfiltracao
- **Filtros_UDP_DNS** foi notado tentativas de conexao com dominios legitimos como adobe e microsoft
- **erro_noSuch_name** consultas para wpad.lafontainebleau.org que retornaram erro. 
- **Ambiente:** Captura realizada em ambiente controlado (Sandbox).
-

## 🛠️ Ferramentas Utilizadas
- Wireshark
## PCAPS.ALERTS 
![ALERTAS_PCAPSUTILIZADOS](Documents/Warmcookie_wireshark/

## 🕵️ Fluxo da Investigação

### Fase 1: Inicio da infeccao e reconhecimento
- foi iniciado o filtro do ip hostil que foi identificado atraves dos alertas !
- ip.addr
 ![print](/home/kali/Documents/Warmcookie_wireshark/docs/imgs/ip_addr.png)
- e possivel notar uma requisicao para connecttest.txt com status code 200 ok!
  ![print](/home/kali/Documents/Warmcookie_wireshark/docs/imgs/connect_test.png)
- seguido de uma tentativa de conexao com um dominio da miscrosoft
- HTPP_export_objects
 ![print](/home/kali/Documents/Warmcookie_wireshark/docs/imgs/http_object_export.png)
- momento da infeccao ao analisar tcp.stream eq 112
 ![print](/home/kali/Documents/Warmcookie_wireshark/docs/imgs/momento_infeccao.png)
- arquivo identificado disfarcado de fatura  ( Invoice 876597035_003.zip)
## fluxo dns e udp 
- apos o download do arquivo foi feito um filtro dns do qual vimos algumas tentativas de conexao a dominios legitimos 
![print](/home/kali/Documents/Warmcookie_wireshark/docs/imgs/udp_stream_acesso_dominios_legitimos.png)
- e erro no such name para wpad.lafontainebleau.org 
![print](/home/kali/Documents/Warmcookie_wireshark/docs/imgs/no_such_name.png)

## Falha no ataque  e SYN FLOOD para ip 72.54.43.29 
- foi identificado atraves do filtro tcp.flags.syn == 1 && tcp.flags.ack == 0
- erro de port unreacheable ICMP (tentativa de conexao interna)
- SYN FLOOD sem resposta ACK
- Comportamento de Botnet
![print](/home/kali/Documents/Warmcookie_wireshark/docs/imgs/tcp_flags.png)
![print](/home/kali/Documents/Warmcookie_wireshark/docs/imgs/ICMP.png)

## Conclusao

- A análise revelou um ciclo completo de comprometimento: desde o download de um artefato disfarçado de fatura até a tentativa de comunicação com infraestrutura de comando e controle externa.
-  O bloqueio de serviços internos e as falhas de conexão (ICMP Unreachable) sugerem que mecanismos de defesa da rede limitaram o impacto da movimentação lateral."
