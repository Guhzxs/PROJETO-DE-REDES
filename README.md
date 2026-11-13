# 🌐 Projeto de Infraestrutura de Rede: Interconexão de Redes

## 📖 Sobre o Projeto
Este repositório contém a implementação de uma topologia de rede corporativa simulada no **Cisco Packet Tracer**. O projeto integra cálculos de endereçamento (IPv4 Subnetting/VLSM), roteamento dinâmico entre filiais, serviços de rede centralizados (DNS/HTTP/DHCP) e políticas de segurança via listas de controle de acesso (ACL).

## 🏗️ Arquitetura da Topologia
A infraestrutura está dividida em 3 redes locais (LANs) principais interligadas via roteadores Cisco (com interfaces seriais HWIC-2T):

* **🟡 LAN1 (Rede Amarela - Servidores):** Núcleo da rede (120.20.30.0/24). Subdividida para isolar os serviços. Hospeda o **Server Farm** central com os Servidores HTTP e DNS.
* **🔵 LAN2 (Rede Azul - Clientes LAN):** Rede intermediária (192.168.0.0/16 fatiada em sub-redes `/18`). Conecta terminais cabeados com IPs estáticos (PC4 a PC7).
* **🟢 LAN3 (Rede Verde - Wi-Fi e DHCP):** Rede de acesso dinâmico (10.0.0.0/18). Conta com um servidor DHCP para distribuição de IPs e um Access Point configurado com WPA2-PSK para conectar laptops via rede sem fio.

## ⚙️ Principais Funcionalidades Implementadas
* **Subnetting (VLSM):** Planejamento e fatiamento de IPs para otimizar o endereçamento e respeitar os requisitos de *hosts* de cada setor.
* **Roteamento Dinâmico (RIPv2):** Configuração do RIP versão 2 nos roteadores com `no auto-summary`, garantindo a comunicação total de ida e volta entre as 3 LANs.
* **Serviços de Rede:**
  * **Web e DNS:** Resolução de nomes (`index.html`) apontando para o servidor web central.
  * **DHCP:** Alocação dinâmica de IPs, Gateway e DNS para os dispositivos da rede verde.
  * **Wireless:** Conectividade móvel segura utilizando o padrão WPA2-PSK.
* **Segurança e Filtros (ACL):** Implementação de uma *Extended Access Control List* (ACL 100) atuando na borda da LAN3. A regra **bloqueia** o tráfego da rede Verde para a Azul, mas **permite** o acesso total aos servidores da rede Amarela.

## 💻 Tecnologias e Ferramentas
* **Simulador:** Cisco Packet Tracer
* **Equipamentos:** Roteadores (Cisco 2901), Switches (2960), Access Point e Laptops (Módulos WPC300N).
* **Protocolos:** IPv4, RIPv2, ICMP, DNS, HTTP, DHCP, 802.11 (WPA2).

## 🚀 Como Executar
1. Instale o **Cisco Packet Tracer**.
2. Clone este repositório e abra o arquivo `.pkt` principal.
3. Para testar a conectividade: abra o *Web Browser* de um dos Laptops da LAN3 e acesse `index.html`, ou faça um `ping` do PC6 (LAN2) para os servidores da LAN1.

---
*Projeto prático desenvolvido para a disciplina de Redes de Computadores do Centro Universitário FEI.*
