---
name: divida-tecnica
description: Catalogar, priorizar e pagar divida tecnica de forma incremental, sem parar a entrega de valor.
---

# Divida tecnica e refatoracao

## Quando usar

Ao identificar divida, ao planejar capacidade e sempre que a mesma area quebra repetidamente.

## Catalogar

Divida nao catalogada volta como surpresa. Para cada item:

```
Item: <descricao>
Onde: <modulo/arquivo>
Tipo: estrutural | duplicacao | cobertura | dependencia | operacional
Dor atual: <o que custa hoje, com evidencia>
Risco: <o que acontece se nada for feito>
Custo de pagar: <estimativa relativa>
Proposta: <passo incremental>
```

Dor com evidencia, nao opiniao: "tres defeitos nos ultimos dois meses nesta area" vale mais que
"este codigo e feio".

## Priorizar

Pague primeiro a divida que:

1. Esta no caminho da proxima entrega planejada (paga de graca, junto).
2. Gera defeito recorrente (a correcao se paga).
3. Bloqueia alguem (ninguem mexe por medo).
4. Aumenta custo de operacao de forma mensuravel.

Deixe para depois a divida em modulo estavel que ninguem toca: codigo feio e parado nao custa nada.

## Pagar de forma incremental

- **Junto com a feature.** Melhorar o que se encosta e a forma mais barata.
- **Isolar antes de trocar.** Coloque uma interface na frente e troque a implementacao com o
  sistema funcionando.
- **Nunca reescrever tudo.** Reescrita total some com o conhecimento acumulado nas correcoes e
  recria defeitos ja resolvidos.
- **Teste primeiro.** Antes de refatorar, garanta teste que prove o comportamento atual — ele e a
  rede de seguranca.

## Duplicacao

Antes de extrair o terceiro duplicado, pergunte se as tres copias realmente mudam juntas.
Abstracao prematura e divida nova. Mas quando o mesmo padrao existe duplicado em mais de um lugar
(por exemplo, o mesmo anexo de recurso em dois pontos), a correcao de um deles precisa ser avaliada
nos outros — senao o defeito reaparece com outro sintoma.

Ao corrigir um defeito, procure o padrao: `grep` pelo trecho, nao apenas pelo arquivo relatado.

## Dependencias

- Atualize em passos pequenos e frequentes, nao em salto de versao maior acumulado.
- Dependencia com vulnerabilidade conhecida tem prioridade de defeito, nao de melhoria.
- Dependencia abandonada e risco: planeje a saida antes da urgencia.
- Biblioteca especifica de uma plataforma usada em servico que roda em outra quebra em tempo de
  execucao: ao migrar o primeiro uso, varra o repositorio pelo **nome do pacote/namespace**, nao
  pelos tipos usados — arquivos com apenas a importacao orfa tambem quebram a compilacao.

## Checklist

- [ ] Divida catalogada com dor baseada em evidencia
- [ ] Prioridade justificada por recorrencia, bloqueio ou custo mensuravel
- [ ] Pagamento incremental, preferencialmente junto a uma entrega
- [ ] Teste cobrindo o comportamento antes da refatoracao
- [ ] Padrao duplicado procurado em todo o repositorio
- [ ] Nenhuma reescrita total sem justificativa extraordinaria

## Pronto quando

O catalogo esta atualizado, os itens no caminho da proxima entrega foram pagos junto com ela, e a
area que quebrava repetidamente tem teste cobrindo o comportamento.
