---
name: registro-de-decisao
description: Documentar decisoes com contexto, alternativas, consequencias e reversibilidade, em formato curto e durave.
---

# Registro de decisao arquitetural

## Quando usar

Escolha de tecnologia, mudanca estrutural, padrao adotado para o projeto, limite entre modulos,
estrategia de dado — qualquer decisao caro de reverter.

## Formato

Um arquivo curto por decisao, numerado e **imutavel**: decisao superada nao se apaga, ganha estado
"substituida por" e aponta para a nova.

```markdown
# ADR-0007 — Fila em processo para tarefas assincronas

Estado: aceita | proposta | substituida por ADR-00XX | descartada
Data: <data>

## Contexto
<qual problema exigia decidir; restricoes reais: volume, equipe, prazo, custo>

## Decisao
<o que foi decidido, em uma frase>

## Alternativas consideradas
| Opcao | Vantagem | Custo | Por que nao |
|---|---|---|---|

## Consequencias
Facilita: <o que fica mais simples>
Dificulta: <o que fica mais dificil>
Assume: <premissas que, se mudarem, invalidam a decisao>

## Reversibilidade
facil | custosa | praticamente irreversivel
<como seria reverter>
```

## Como decidir bem

1. **Enuncie a restricao, nao a preferencia.** "Precisamos processar 500 itens por minuto com
   perda zero" e restricao; "queremos usar mensageria" e preferencia.
2. **Liste no minimo duas alternativas reais.** Decisao com uma opcao e justificativa, nao decisao.
3. **Inclua a opcao de nao fazer nada.** Com frequencia ela ganha.
4. **Escolha pelo custo total**: operacao, aprendizado da equipe e reversao, nao apenas
   implementacao.
5. **Declare as premissas.** Quando uma delas muda, a decisao volta para a mesa sem drama.

## Decisao irreversivel

Decisao praticamente irreversivel (formato de dado persistido, modelo de identidade, divisao de
modulo exposta em contrato publico) merece analise mais longa e confirmacao explicita de quem vai
conviver com ela.

Prefira, quando possivel, o caminho que **preserva a opcao**: comecar com o componente mais simples
que satisfaz a restricao, isolado atras de uma interface, de modo que a troca seja local.

## Diagramas

Quatro niveis, do mais amplo ao mais detalhado:

1. **Contexto** — o sistema e os atores e sistemas externos.
2. **Container** — as aplicacoes e bancos que compoem o sistema.
3. **Componente** — as partes internas de um container.
4. **Codigo** — raramente vale desenhar; o codigo e a fonte.

Prefira diagrama como texto versionado junto ao registro, para evoluir com o codigo.

## Checklist

- [ ] Contexto com restricao mensuravel
- [ ] Minimo duas alternativas, incluindo nao fazer nada
- [ ] Consequencias boas **e** ruins declaradas
- [ ] Premissas explicitas
- [ ] Reversibilidade classificada
- [ ] Numerado, imutavel, versionado junto ao codigo
- [ ] Decisao substituida aponta para a nova

## Pronto quando

Alguem que chegou depois le o registro e entende por que a escolha foi aquela, sem precisar
perguntar a quem decidiu.
