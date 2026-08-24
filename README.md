# 🛡️ Configuração de ACL no Cisco ASA Firewall 5506-X

[![Baixar Laboratório](https://img.shields.io/badge/Download-Clique%20Aqui-green?style=for-the-badge&logo=cisco)](https://raw.githubusercontent.com/unknown-code7/configure-wireless-router-and-client/main/Configure%20a%20Wireless%20Router%20and%20Client.pka)
[![Assistir Tutorial](https://img.shields.io/badge/Assistir_Tutorial-Clique_Aqui-blue?style=for-the-badge&logo=facebook)](https://www.facebook.com/share/v/18rSKRXpVg/)

---

## 📌 Sobre o Laboratório

Neste laboratório prático da **Série: Laboratórios Cisco NetAcad powered by Jordão Paulo (Unknown Code)**, demonstra a implementação de **Listas de Controle de Acesso (ACL)** em um Firewall corporativo **Cisco ASA 5506-X**. 

O objetivo principal deste cenário é isolar a rede interna (Intranet) e garantir que clientes externos (Rede 192.168.1.0/24) possuam acesso restrito e exclusivo ao servidor **Web (HTTP)** hospedado na **DMZ**, impedindo qualquer comunicação direta com a infraestrutura interna ou servidores críticos como o DHCP.

---

## 🎯 Principais Implementações & Recursos

* **DHCPv4 Server:** Configuração e distribuição dinâmica de endereços IP para a LAN.
* **Segmentação em DMZ:** Isolamento do servidor HTTP (`Server-PT web_server`) em uma zona desmilitarizada dedicada.
* **Políticas de ACL no ASA Firewall:** Regras rígidas permitindo que dispositivos da sub-rede `192.168.1.0/24` acessem unicamente o serviço HTTP na DMZ.
* **Diagnóstico ICMPv4:** Testes de conectividade (ping) e validação dos bloqueios aplicados pelo firewall.
* **Segurança de Borda:** Proteção dos ativos da LAN interna (`192.168.3.0/24`) contra acessos não autorizados vindo de redes externas.

---

## 🏗️ Arquitetura da Rede (Mermaid Diagram)

```mermaid
graph TD
    %% Nós da Rede Externa
    subgraph NETWORK_01 ["NETWORK 01 (192.168.1.0/24)"]
        Laptop["💻 Client Laptop<br/>192.168.1.x"]
    end

    %% Roteador de Borda
    Router["🌐 Router 2911<br/>Gig0/0: 192.168.1.1<br/>Gig0/1: 192.168.2.1"]

    %% Link intermediário
    subgraph NETWORK_02 ["NETWORK 02 (192.168.2.0/24) - Level 0"]
        ASA["🛡️ ASA 5506-X Firewall<br/>Gig1/1: 192.168.2.2<br/>Gig1/2: 192.168.3.1"]
    end

    %% Nós da Rede Interna (DMZ + LAN)
    subgraph NETWORK_03 ["NETWORK 03 (192.168.3.0/24) - Intranet Level 100"]
        Switch["🔌 Switch 2960"]
        
        subgraph DMZ ["Zone: DMZ (Acesso Restrito)"]
            WebServer["🌐 Web Server (HTTP)<br/>192.168.3.2"]
        end

        subgraph LAN ["Zone: LAN Interna (Protegida)"]
            DHCP["🖥️ Server DHCP"]
            SysAdmin["💻 Sysadmin PC"]
            DevPC["💻 Dev PC"]
            RHPC["💻 RH PC"]
            Printer["🖨️ Printer"]
        end
    end

    %% Conexões
    Laptop -->|Fa0| Router
    Router -->|Gig0/1| ASA
    ASA -->|Gig1/2| Switch

    Switch --> WebServer
    Switch --> DHCP
    Switch --> SysAdmin
    Switch --> DevPC
    Switch --> RHPC
    Switch --> Printer

    %% Estilização do Diagrama
    style ASA fill:#003366,stroke:#0066cc,stroke-width:2px,color:#fff
    style WebServer fill:#006622,stroke:#009933,stroke-width:2px,color:#fff
    style Router fill:#444,stroke:#888,stroke-width:1px,color:#fff
    style Switch fill:#444,stroke:#888,stroke-width:1px,color:#fff
    style DMZ fill:#1a3300,stroke:#336600,stroke-width:1px
    style LAN fill:#1a1a1a,stroke:#444,stroke-width:1px
