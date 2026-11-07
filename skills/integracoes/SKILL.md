---
name: integracoes
description: Integrar sistemas com nova tentativa, interrupcao de circuito, idempotencia, verificacao de assinatura e fila de descarte.
---

# Integracoes e mensageria

## Quando usar

Integracao com servico externo, comunicacao entre modulos, recebimento de notificacao automatica,
processamento assincrono.

## Chamada a servico externo

Toda chamada que sai do processo precisa de:

1. **Tempo limite explicito.** Padrao de biblioteca costuma ser longo demais: com dependencia
   atras de tunel instavel, cada requisicao prende por dezenas de segundos.
2. **Nova tentativa com espera crescente** e variacao aleatoria, apenas em falha transitoria.
3. **Interrupcao de circuito:** apos N falhas, pare de tentar por um periodo.
4. **Distincao entre falha transitoria e definitiva.** Excesso de requisicoes, indisponibilidade e
   tempo esgotado sao transitorios; nao encontrado, nao autenticado e sem permissao sao definitivos.

Resolucao de configuracao remota na partida: em falha transitoria, **insista** em rodadas com
espera, sem limite rigido — subir devagar e melhor que subir quebrado. Em falha definitiva, siga,
senao uma chave removida impede a partida para sempre. Nunca siga com valor vazio: troca um erro
claro por uma falha obscura mais adiante.

Quando muitas instancias iniciam no mesmo minuto, acrescente espera aleatoria curta antes da
primeira consulta.

## Idempotencia

Toda operacao com efeito precisa de chave de idempotencia verificada **antes** de gravar:

```
chave = tipo + identificador_de_origem + parcela
se existe lancamento com essa chave -> nao faz nada
senao -> grava
```

Protecao na borda da API nao basta: ela vale enquanto a chamada esta viva. Se a conexao cai, o
cliente tenta de novo e o executor remoto continua processando. A garantia precisa viver **no
executor**: trava por cliente, nova verificacao imediatamente antes de inserir, e remocao de
duplicata dentro do mesmo lote.

## Notificacao automatica recebida (webhook)

- **Verifique a assinatura** com o corpo **cru**, capturado antes de qualquer interpretacao de
  JSON.
- O segredo de assinatura e por destino: trocar o destino exige trocar o segredo.
- Use o identificador do evento do provedor como chave de idempotencia.
- Em conta compartilhada entre varios produtos, filtre por metadado proprio em **todo** tratador.
- Responda rapido e processe depois: o provedor reenvia quando demora.
- Nunca dependa so da notificacao para creditar algo: ofereca um caminho de conciliacao sob
  demanda.

## Mensageria

- Encaminhamento por chave hierarquica, com uma fila por responsabilidade.
- Confirmacao apos processar, nunca ao receber.
- Fila de descarte para o que falha repetidamente, com contador de tentativas.
- Consumidor idempotente: entrega pode ser repetida por desenho.
- Mensagem versionada; consumidor tolera campo novo que nao conhece.

## Comando para executor remoto

Canal de comando filtrado por vinculo (identificador em outro sistema) descarta silenciosamente
tudo quando o vinculo esta nulo: recupere o vinculo pelo nome no sinal de vida e **prove a entrega
com ida e volta** (comando enviado, resultado recebido) — inserir o comando no banco nao e prova.

Endpoint que muda estado e **tambem** precisa sinalizar o executor: mudar apenas um indicador no
banco impede a redistribuicao, mas nao interrompe o que ja esta rodando.

## Checklist

- [ ] Tempo limite explicito em toda chamada externa
- [ ] Nova tentativa apenas em falha transitoria, com espera crescente
- [ ] Interrupcao de circuito configurada
- [ ] Idempotencia garantida no executor, nao apenas na borda
- [ ] Assinatura de notificacao verificada com corpo cru
- [ ] Fila de descarte com contador de tentativas
- [ ] Caminho de conciliacao independente da notificacao
- [ ] Entrega de comando comprovada por ida e volta

## Pronto quando

A integracao sobrevive a reinicio e a entrega duplicada sem gerar efeito duplo, e a falha do
servico externo degrada o sistema em vez de derruba-lo.
