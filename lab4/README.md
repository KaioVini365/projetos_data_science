# Módulo 4 — Álgebra Linear: Vetores e Geometria

## Exercícios

**1. Similaridade entre usuários** — produto escalar bruto entre dois vetores de preferência. Limitação: é sensível à magnitude, não só à direção. Em produção isso se resolve com cosseno.

**2. Reajuste de preços em 15%** — multiplicação escalar vetorizada sobre um array. Mesma lógica de scaling de features em pré-processamento.

**3. Vetor unitário que maximiza o produto escalar com `v = [3, 4, 5]`** — resolvido analiticamente (`u = v/‖v‖`) e validado numericamente com `scipy.optimize.minimize`. As duas soluções batem.

**4. Vetor normal a um plano por 3 pontos** — resultado deu `[0, 0, 0]`: os pontos são colineares, não formam plano. O zero é o dado avisando que a premissa não se sustenta.
