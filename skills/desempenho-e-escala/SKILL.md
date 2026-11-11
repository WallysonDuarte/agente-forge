---
name: desempenho-e-escala
description: Medir antes de otimizar, encontrar o gargalo real e aplicar cache, paginacao, indice e compressao onde a medicao aponta.
---

# Desempenho e escala

## Quando usar

Relato de lentidao, crescimento de volume, custo de infraestrutura subindo, preparacao para pico.

## Medir primeiro — sempre

Otimizacao sem medicao e desperdicio com risco. Antes de qualquer mudanca:

1. **Reproduza** com volume realista, nao com dez registros.
2. **Meca por camada:** tempo no proxy, tempo no servico, tempo no banco, tempo no navegador.
3. **Use percentis**, nao media. Media de 200 ms com percentil 95 de 8 s e um sistema lento.
4. **Registre a linha de base** antes de mexer.

Sem tempo por requisicao no registro do proxy reverso, um pico transitorio **nao** pode ser
atribuido a ninguem — instrumente antes de diagnosticar.

## Onde o tempo costuma estar

| Sintoma | Causa provavel |
|---|---|
| Lento sempre, igual para todos | consulta sem indice, N+1, pagina em memoria |
| Lento so no primeiro acesso | partida fria, cache vazio, resolucao de segredo externo |
| "As vezes segundos, as vezes minutos" | volume baixado pelo cliente + invalidacao de cache, nao carga do servidor |
| Lento sob concorrencia | contencao de bloqueio, esgotamento de pool de conexoes |
| Degrada com o tempo | vazamento de memoria, cache sem limite, fila crescendo |

Quando o servidor responde rapido e o usuario reclama, conte as **requisicoes** do primeiro
carregamento e meca o peso baixado. Mecanismo que pre-busca todos os pedacos do pacote baixa
centenas de arquivos num unico acesso — em rede ruim, minutos.

## Banco

- Indice composto na ordem das condicoes da consulta; confira o plano de execucao.
- Elimine consulta em laco; busque por lista de identificadores.
- Pagine com cursor em listagem grande; deslocamento alto degrada.
- Projete apenas as colunas usadas: documento grande trafegado inteiro e custo invisivel.
- Pool por base de dados: varredura em muitas bases esgota o limite de conexoes do servidor. Libere
  o pool ao terminar cada base ou desative o agrupamento nessa varredura.

## Cache

Cada nivel resolve um problema diferente:

| Nivel | Para que | Cuidado |
|---|---|---|
| Rede de distribuicao | recurso estatico com nome versionado | nunca a pagina principal |
| Navegador | recurso com resumo no nome, imutavel | pagina principal sempre sem cache |
| Memoria do processo | dado quente pequeno | limite de tamanho e expiracao obrigatorios |
| Cache distribuido | dado compartilhado entre instancias | invalidar na escrita |

Regra: a pagina principal de uma aplicacao de pagina unica **sempre** sem cache; apenas arquivos
com resumo no nome podem ser imutaveis. Sem isso o usuario continua na versao antiga.

Cache compartilhado precisa de protecao contra avalanche: use trava de uma unica permissao antes de
reconsultar, senao toda requisicao concorrente vai ao banco no momento da expiracao.

## Cliente

Carregamento tardio por rota, divisao de pacote, remocao de codigo nao usado, imagem em formato
moderno com conjunto de tamanhos, fonte com exibicao imediata, compressao no servidor.

## Limite de uso

Limite por origem em servico consumido por muitos clientes atras de um mesmo endereco de saida faz
os clientes bloquearem uns aos outros. Limite por **identidade do chamador**.

Quando aparecer excesso de requisicoes, identifique a camada (limite do proxy, limite da aplicacao,
plano do usuario) pelo registro de cada uma antes de afirmar a causa. Limite do proxy costuma
aparecer com codigo diferente do limite da aplicacao.

## Checklist

- [ ] Linha de base medida antes da mudanca
- [ ] Percentis, nao media
- [ ] Gargalo identificado por medicao
- [ ] Plano de execucao conferido nas consultas quentes
- [ ] Paginacao e projecao no banco
- [ ] Cache com expiracao, limite e invalidacao na escrita
- [ ] Pagina principal sem cache; recursos versionados imutaveis
- [ ] Medicao repetida depois, comprovando o ganho

## Pronto quando

A medicao depois mostra ganho no percentil que importava, e a linha de base nova esta registrada.
