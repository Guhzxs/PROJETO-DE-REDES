# Projeto Prático de Redes: IPv4 Subnetting e Troubleshooting

## 📝 Descrição do Projeto
Este repositório documenta a resolução de um laboratório de Redes de Computadores, dividido em duas frentes: a consolidação teórica do cálculo avançado de sub-redes (IPv4) e a aplicação prática de *troubleshooting* de conectividade (Camada 3) numa topologia simulada no Cisco Packet Tracer.

## 🧠 Parte 1: Teoria e Cálculo de Sub-redes
Estruturação e resolução de exercícios de fatiamento de redes IPv4, englobando a definição de novas máscaras de sub-rede para atender a requisitos específicos de *hosts* ou quantidade de sub-redes.

**Metodologia Aplicada:**
* Identificação da **Rede Base (Estaca Zero)** a partir de IPs de hosts genéricos.
* Cálculo da **Nova Máscara de Sub-rede** (empréstimo de bits).
* Determinação do **Tamanho do Bloco (Saltos)** nos octetos alvo.
* Mapeamento de intervalos de IPs válidos, endereços de rede e de *broadcast*.
* **Cenários resolvidos:** Máscaras originais `/11`, `/18`, `/24` e `/27`.

## 🛠️ Parte 2: Troubleshooting no Cisco Packet Tracer
Diagnóstico e resolução de um problema de conectividade onde os terminais PC6 e PC7 (pertencentes à LAN3 / "Rede Azul") não conseguiam aceder ao Servidor HTTP localizado na LAN1 (IP `120.20.30.132`).

**Passos de Isolamento de Falhas:**
1. **Camada de Aplicação / DNS:** Validação das configurações de IP, Gateway e Servidor DNS via `ipconfig` nos terminais.
2. **Camada de Rede (ICMP):** Testes de `ping` direto ao IP numérico do Servidor HTTP, resultando em *Request timed out* com 100% de perda de pacotes.
3. **Validação de Segurança (ACL):** Desativação temporária da ACL 100 no Router2 (`no ip access-group 100 in`) para descartar bloqueios indevidos por regras de firewall.
4. **Análise de Roteamento (RIPv2):** Verificação da tabela de roteamento no roteador da LAN1 através do comando `show ip route`. 

**A Causa Raiz:**
A tabela de roteamento do Roteador da LAN1 não continha a rota para a rede de origem dos pacotes (`192.168.64.0/18`). O servidor recebia o ping, mas o roteador descartava o pacote de resposta (Pong) por desconhecer o caminho de volta.

**A Solução Implementada:**
Configuração do anúncio da rede no protocolo RIPv2 dentro do Roteador da Rede Azul, forçando a versão 2 e desativando a sumarização automática:
```bash
Router> enable
Router# configure terminal
Router(config)# router rip
Router(config-router)# version 2
Router(config-router)# no auto-summary
Router(config-router)# network 192.168.64.0
