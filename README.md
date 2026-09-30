# Desenvolvimento de Relatórios Mensais de Desenvolvimento

[![CI Status](https://github.com/FelipeMadson/desenvolvimento-de-relatorios-mensais-de-desenvolvimento/actions/workflows/ci.yml/badge.svg)](https://github.com/FelipeMadson/desenvolvimento-de-relatorios-mensais-de-desenvolvimento/actions)
[![Latest Release](https://img.shields.io/github/v/release/FelipeMadson/desenvolvimento-de-relatorios-mensais-de-desenvolvimento?color=145e4d&logo=github)](https://github.com/FelipeMadson/desenvolvimento-de-relatorios-mensais-de-desenvolvimento/releases)
[![Conventional Commits](https://img.shields.io/badge/Conventional%20Commits-1.0.0-yellow.svg)](https://conventionalcommits.org)
[![SemVer 2.0.0](https://img.shields.io/badge/semver-2.0.0-blue.svg)](https://semver.org)
[![Node.js Version](https://img.shields.io/badge/Node.js-22%20%7C%2024%20LTS-brightgreen.svg)](https://nodejs.org)
[![Architecture](https://img.shields.io/badge/Architecture-Clean%20Hexagonal%20Multi--Tenant-blue.svg)](docs)
[![Test Suite](https://img.shields.io/badge/Tests-100%25%20Passing%20(18%20tests%20node%3Atest)-success.svg)](tests)
[![Security](https://img.shields.io/badge/Security-TimingSafeEqual%20%7C%20Zero--Leak-success.svg)](src/security)
[![Zero Runtime Dependencies](https://img.shields.io/badge/dependencies-0%20runtime-brightgreen.svg)](package.json)
[![License](https://img.shields.io/badge/License-MIT-lightgrey.svg)](LICENSE)

> **Desenvolvedores precisam gerenciar e compartilhar suas jornadas de desenvolvimento, mas atualmente não há uma ferramenta eficiente para isso.**

---

## 🖥️ Demonstração em Terminal Vetorial (Execução & Benchmarks)

<p align="center">
  <img src="docs/assets/terminal-demo.svg" alt="Terminal Demo - Desenvolvimento de Relatórios Mensais de Desenvolvimento" width="840" />
</p>

---

## 🏛️ Visão Geral & Arquitetura de Software

O **Desenvolvimento de Relatórios Mensais de Desenvolvimento** foi construído seguindo princípios de **Engenharia de Software de Alto Rigor Corporativo**, estruturado em camadas desacopladas (Clean / Hexagonal DDD) e operando com **100% de APIs nativas do Node.js** (zero dependências externas de runtime).

```mermaid
flowchart TD
    CLI["CLI / Consumer Input"] --> Domain["Domain Layer (Value Objects & Invariants)"]
    Domain --> Engine["Core Multi-Tenant Engine"]
    
    subgraph Layers["Subsistemas de Alta Resiliência"]
        Engine --> Security["Security Vault (Timing-Safe HMAC / Zero-Leak)"]
        Engine --> WAL["Storage WAL (Write-Ahead Log + Crash Recovery)"]
        Engine --> Outbox["Outbox Dispatcher (Idempotência)"]
        Engine --> RateLimiter["Token-Bucket Rate Limiter"]
        Engine --> CircuitBreaker["Circuit Breaker (CLOSED / OPEN / HALF_OPEN)"]
        Engine --> EventBus["Event Bus (Domain Events Pub/Sub)"]
        Engine --> Telemetry["Prometheus Exporter (P50/P95/P99)"]
    end
    
    Security --> Output["Audit Verified Record"]
```

---

## 🚀 Diferenciais Técnicos de Nível Staff/Senior

* **Zero Runtime Dependencies:** Total independência de ecossistemas externos, garantindo menor superfície de ataque e inicialização instantânea (< 30ms).
* **Multi-Tenancy Criptograficamente Segregado:** Cada registro é isolado por TenantIdentifier e protegido contra vazamento entre contas.
* **Proteção Criptográfica Contra Side-Channel:** Comparação de hashes via `crypto.timingSafeEqual` prevenindo ataques de canal lateral baseados em tempo de resposta.
* **Higienização Recursiva Automática (Zero Credential Leak):** Algoritmo profundo que mascara chaves de API, senhas e tokens antes de persistir no log.
* **Write-Ahead Logging (WAL) com Recuperação de Desastres:** Reconstrução determinística de estado através de `replay()` validado por somas de verificação SHA-256.
* **Resiliência Integrada:** Algoritmo Token-Bucket para controle de taxa e Circuit Breaker para tolerância a falhas.
* **Telemetria Prometheus Nativa:** Cálculo de percentis de latência (P50, P95, P99) e métricas expostas no formato oficial.

---

## 📦 Polyglot Client SDKs (TypeScript & Python)

Este projeto acompanha SDKs tipados de alta resiliência prontos para uso em microsserviços e integrações:

### Exemplo em TypeScript / Node.js
```typescript
import { DesenvolvimentodeRelatriosMensaisdeDesenvolvimentoClient } from "./sdk/ts/client.ts";

const client = new DesenvolvimentodeRelatriosMensaisdeDesenvolvimentoClient({
  baseUrl: "http://127.0.0.1:3000",
  tenantId: "acme-corp"
});

const health = await client.checkHealth();
console.log("Status do Motor:", health.status);

const item = await client.processItem("order-key-101", { value: 1500 });
console.log("Item verificado com hash:", item.hash);
```

### Exemplo em Python 3 (Zero Dependências)
```python
from sdk.python.client import DesenvolvimentodeRelatriosMensaisdeDesenvolvimentoClient

client = DesenvolvimentodeRelatriosMensaisdeDesenvolvimentoClient(base_url="http://127.0.0.1:3000", tenant_id="acme-corp")
health = client.check_health()
print(f"Status do Serviço: {health}")

res = client.process_item("sensor-01", {"temperature": 24.2})
print(f"Processado: {res.get('id')}")
```

---

## 🛠️ Instalação, Execução & Verificação

```bash
# Executa a suíte corporativa de 18 testes automatizados
npm test

# Executa o benchmark de performance integrado (> 5.000 ops/seg)
npm run cli benchmark

# Inspeciona as estatísticas e saúde do motor
npm run cli status

# Diagnóstico de integridade do ambiente
npm run cli doctor

# Verificação criptográfica do pacote de release
sha256sum -c dist/releases/SHA256SUMS
```

---

## 👤 Autor & Licença

* **Autor:** Felipe Madison ([@FelipeMadson](https://github.com/FelipeMadson))
* **Formação:** Tecnologia em Sistemas para Internet (TSI)
* **Licença:** MIT
