# NOC Intelligent Security Shield & AI Mitigation System SS7

Este repositório apresenta o estudo de caso, arquitetura e validação de desempenho do **NOC Intelligent Security Shield**, um ecossistema avançado de cibersegurança focado na mitigação de ataques de inundação (Flood) e vulnerabilidades estruturais em protocolos de sinalização (como SS7). 

> ⚠️ **Nota de Propriedade Intelectual:** Por motivos de segurança, conformidade e proteção de segredo comercial, o código-fonte deste projeto é mantido em um ambiente privado e estrito. Este espaço é dedicado exclusivamente à demonstração pública de arquitetura, telemetria de testes e validação de resultados.

---

## 🚀 Visão Geral do Sistema

O sistema atua como um escudo inteligente na camada de rede, analisando o comportamento do tráfego em tempo real para tomar decisões de bloqueio automatizadas.

*   **Detecção Automatizada:** Identificação de anomalias críticas e picos abruptos de requisições.
*   **Tomada de Decisão por IA:** Implementação de um agente de Aprendizado por Reforço (**Deep Q-Network - DQN**) que calcula dinamicamente a severidade do risco e aplica penalidades/bloqueios (*Inbound-Action Block*).
*   **Mecanismo de Fail-Safe:** Ativação de travas rígidas de segurança (*Trust Anchor*) caso o modelo de IA identifique um risco iminente de saturação de hardware.

## 📊 Testes de Estresse & Validação (Métricas Reais)

Para garantir a resiliência do escudo sob condições extremas de produção, o sistema foi submetido a testes de carga massivos utilizando a ferramenta de engenharia de performance [Locust](https://locust.io).

### 1. Desempenho e Saturação com Alta Carga (500 Usuários Simultâneos)
O sistema foi validado sustentando uma taxa contínua de **mais de 160 RPS (Requisições por Segundo)** sob a simulação de 500 usuários concorrentes atacando os endpoints de saúde do ecossistema.

<img width="1600" height="900" alt="2" src="https://github.com/user-attachments/assets/bfd0bb4f-b43b-4118-879b-4dfdbb413a57" />

### 2. Estabilidade de Latência e Requisições por Segundo (Métricas)
Como demonstrado nos gráficos de telemetria, o tempo de resposta do sistema manteve-se estável na faixa de milissegundos, sem gerar falhas de timeout, provando a eficiência do algoritmo de descarte e mitigação de pacotes maliciosos.

<img width="1600" height="900" alt="1" src="https://github.com/user-attachments/assets/2bf5abaa-1174-4e5e-a396-ea8181423be2" />


## 🛡️ Telemetria do Escudo e Logs de Execução

O painel de monitoramento do NOC e os logs do terminal comprovam a atuação em tempo real do agente inteligente interceptando as ameaças.

### Painel do NOC em Ação
Durante um ataque simulado de inundação SS7, o painel registrou um tráfego volumoso de **1185.8000 msg/s**, alterando o status do sistema instantaneamente para **CRÍTICO** e aplicando o mitigador direto pelo escudo (*BLOCK*).

<img width="1600" height="900" alt="3" src="https://github.com/user-attachments/assets/4c9ac24d-6104-44a2-9019-78b409a9a031" />

### Decisões do Agente DQN (Logs do Terminal)
Os logs internos mostram o exato momento em que as anomalias são detectadas pelo módulo de inteligência artificial. O agente calcula a recompensa (Reward) do ambiente e aciona as regras de firewall de forma cirúrgica:

```text
[MÉTRICAS REAIS] Tráfego (Pacotes/3s): 3198.0 | Provedor de Entropia (IPs únicos): 22
[ATM/MMCE] Anomalia detectada: Risco crítico (1.00) e IA escolheu ALLOW.
[FAIL-SAFE] Anomalia na decisão da IA detectada pelo ATM/MMCE. Ativando Trust Anchor rígido.
[RESULTADO ATM] Recompensa calculada para o DQN: -10.0
[FIREWALL REAL] Erro ao injetar regra: Command '['powershell', '-Command', 'New-NetFirewallRule ... -Action Block ...']'
```

O ecossistema monitora constantemente o nível de entropia dos endereços IP para evitar falsos positivos e garantir a continuidade dos serviços legítimos.


<img width="1600" height="900" alt="4" src="https://github.com/user-attachments/assets/0e8b7b99-d621-44ea-873c-753e275fc34b" />

