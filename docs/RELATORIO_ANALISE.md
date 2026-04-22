# Análise de Tráfego: Malware WarmCookie 🍪

## 🎯 Objetivo
Este projeto documenta a análise forense de rede de uma infecção real pelo backdoor **WarmCookie**. O foco é identificar o vetor de ataque, os indicadores de comprometimento (IoCs) e o comportamento do malware pós-infecção.

## 🛡️ Metodologia
A análise foi realizada em ambiente controlado (Kali Linux) utilizando:
- **Wireshark**: Para inspeção profunda de pacotes.
- **Suricata/Snort Rules**: Para correlação de alertas de segurança.

## 🔍 Descobertas Principais

### 1. Vetor de Infecção (Delivery)
Identificado o download de um artefato malicioso via protocolo HTTP.
- **Domínio:** `quote.checkfedexexp.com`
- **Arquivo:** `Invoice 876597035_003.zip`
- **Assinatura Técnica:** Cabeçalho de arquivo `PK` (Zip) confirmado via TCP Stream.

### 2. Comportamento de C2 e Persistência
O malware realiza consultas DNS para domínios legítimos (`adobe.com`, `office.com`) para camuflar sua atividade e testar a conectividade da máquina infectada.

### 3. Tentativa de Movimentação Lateral
Identificadas tentativas de conexão para o host interno `10.8.15.4`. A presença de pacotes **ICMP Destination Unreachable** e uma sequência massiva de **TCP SYN Retransmissions** comprova que o malware tentou se espalhar pela rede mas foi contido por políticas de firewall ou ausência do serviço alvo.

## 🚀 Conclusão
A análise demonstra o ciclo de vida completo de um ataque de backdoor inicial, ressaltando a importância do monitoramento de tráfego de saída e da segmentação de rede para impedir a movimentação lateral do atacante.
