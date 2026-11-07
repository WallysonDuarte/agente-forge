---
name: arquitetura-em-camadas
description: Estruturar o sistema em camadas com dependencia unidirecional, separacao de responsabilidade e testabilidade por injecao.
---

# Arquitetura em camadas

## Quando usar

Projeto novo, refatoracao estrutural, revisao de implementacao, duvida sobre onde colocar uma
regra.

## As camadas

```
Apresentacao     controladores, rotas, componentes de interface
      v
Aplicacao        casos de uso, orquestracao, DTOs, validacao de entrada
      v
Dominio          entidades, regras invariantes, interfaces de repositorio
      ^
Infraestrutura   repositorios, integracoes, armazenamento, mensageria
```

A infraestrutura aponta **para dentro**: ela implementa interfaces que o dominio declara. Essa
inversao e o que torna o dominio testavel sem banco.

## Onde cada coisa mora

| Decisao | Camada |
|---|---|
| Formato da resposta HTTP | Apresentacao |
| Ordem dos passos de um fluxo | Aplicacao |
| "Pedido pago nao pode ser cancelado" | Dominio |
| Como persistir pedido | Infraestrutura |
| Validacao de formato de entrada | Aplicacao (DTO) |
| Invariante que nunca pode ser violada | Dominio (na entidade) |

Teste rapido: se a regra continua verdadeira trocando o banco e a interface, ela e de dominio.

## Injecao de dependencia

Registre por tempo de vida:

- **Por requisicao** — repositorios, unidade de trabalho, contexto do usuario
- **Unico** — cache em memoria, configuracao, cliente HTTP reutilizavel
- **Transitorio** — fabricas, objetos sem estado

Armadilhas:

- Objeto de requisicao resolvido de um escopo criado a mao e **outra** instancia: o componente que
  deveria receber o dado preenchido recebe vazio. Resolva do escopo da requisicao em curso.
- Dependencia de tempo de vida curto dentro de uma instancia unica cria referencia presa a um
  escopo morto.
- Configuracao tipada sem o registro correspondente entrega objeto com valores padrao — sem erro
  nenhum. Em validacao de credencial isso recusa **qualquer** valor, inclusive o correto.

## Separacao de leitura e escrita

Quando leitura e escrita tem exigencias diferentes, separe os modelos: escrita normalizada e com
invariantes; leitura desnormalizada e otimizada para a consulta. Nao e obrigatorio — adote quando
a consulta de leitura comeca a distorcer o modelo de escrita.

## Revisao arquitetural

- [ ] Nenhuma dependencia circular
- [ ] Dominio sem referencia a infraestrutura
- [ ] Nenhuma regra de negocio em controlador
- [ ] Entidade nao exposta na API
- [ ] Nenhuma entrada/saida direta em servico de aplicacao (sempre via interface)
- [ ] Configuracao externalizada
- [ ] Servicos testaveis com substitutos
- [ ] Erro de dominio distinguido de erro de infraestrutura

## Antipadroes

- Servico que faz tudo (centenas de linhas, dezenas de dependencias).
- Entidade anemica: so propriedades, com toda a regra espalhada em servicos.
- Repositorio devolvendo tipo de biblioteca de acesso a dados para camadas superiores.
- Caminho rapido duplicando uma decisao que ja existe no caminho canonico: se um deles tem defeito,
  os dois precisam de correcao. Auxiliar compartilhado que apenas um dos caminhos chama nao resolve.

## Pronto quando

Uma pessoa nova encontra onde colocar uma regra nova sem perguntar, e o dominio tem teste que roda
sem banco.
