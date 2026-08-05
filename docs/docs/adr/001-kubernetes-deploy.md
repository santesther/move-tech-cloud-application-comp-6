# ADR 001 — Uso de K3s em VM na Magalu Cloud para Deploy

**Status:** Aceito  
**Data:** 2026-08-04

## Contexto
A aplicação precisa ser executada em ambiente containerizado com orquestração de containers, capacidade de autorrecuperação (self-healing) e facilidade para atualização sem parada (rolling update). O orçamento é limitado para a fase atual do projeto.

## Alternativas Consideradas
- **K3s em VM BV2-2-40 (Ubuntu 24.04)** — Distribuição Kubernetes leve, de rápido provisionamento e baixo custo operacional. Requer gestão manual do nó da VM.
- **MKS (Magalu Kubernetes Service)** — Cluster Kubernetes totalmente gerenciado. Possui alta disponibilidade no control plane, porém com custo mais elevado e tempo de provisionamento maior para o escopo atual.
- **VM com Docker Compose** — Solução simples de containerização, mas carece de recursos nativos de orquestração como autorrecuperação de pods, ServiceLB e escalabilidade simplificada via manifests standard de K8s.

## Decisão
Utilizar o **K3s em uma VM única (BV2-2-40)** na Magalu Cloud. O critério decisivo foi o equilíbrio entre o custo reduzido e a compatibilidade total com os manifests padrão do Kubernetes (Deployments, Services, Secrets), garantindo um caminho direto de migração para o MKS no futuro caso a aplicação necessite de HA no cluster.

## Consequências
**Positivas:**
- Baixo custo mensal de infraestrutura.
- Provisionamento do ambiente em menos de 2 minutos.
- Uso de manifests universais de Kubernetes.

**Negativas:**
- Ponto único de falha (SPOF) no nível do nó/VM.
- A manutenção do nó (S.O., runtime K3s) é de responsabilidade interna.