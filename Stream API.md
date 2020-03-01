# Stream API

A Stream API é uma funcionalidade do Java 8 para manipular coleções de uma maneira mais simples e eficiente.

Streams são sequências de elementos que podem ser obtidos de uma collection. Maps tem uma estrutura diferente, com um mapeamento de chaves para valores, sem sequências. Isso não significa que não podemos converter um Map em diferentes sequências com a Streams API.

```java
//Temos um HashMap com chave do tipo Long e valor do tipo Player
private Map<Long, Player> playerList = new HashMap<>();

//pega os valores da lista e coloca em um stream
List<Long> list = playerList.values().stream()
							//filtra de forma que cada valor (player) deve ter o idTime iguais (devem ser do 									mesmo time)
                .filter(player -> idTime.equals(player.getIdTime()))
  						//mapeia esses dados pegando somente o ID desses players
                .map(Player::getId)
  						//ordena de forma natural
                .sorted()
  						//coleta esses IDs e adiciona em uma lista
                .collect(Collectors.toList());
```



O exemplo abaixo busca somente o melhor jogador do time, portanto, além de pegar todos os jogadores que pertencem ao time indicado, fará uma comparação pelo nível de habilidade na ordem descendente (maior para menor, ou seja, reversa), e depois comparando pelo ID. O `min` indica o número mínimo possível de resultado, portanto, neste caso, é até equivalente ao uso de `findFirst()`, apesar de ter a chance de dar NPE.

```java
public Long buscarMelhorJogadorDoTime(Long idTime) {
        if (!teamList.containsKey(idTime)) throw new TimeNaoEncontradoException();
        return playerList.values().stream()
				//o min é como o findFirst()
          .filter(player -> idTime.equals(player.getIdTime())).min(Comparator.comparing(Player::getNivelHabilidade)
                        .reversed()
                        .thenComparingLong(Player::getId))
                .map(Player::getId)
                .get();
    }

public Long buscarJogadorMaiorSalario(Long idTime) {
        if (!teamList.containsKey(idTime)) throw new TimeNaoEncontradoException();
        return playerList.values().stream()
                .filter(player -> idTime.equals(player.getIdTime())).min(Comparator.comparing(Player::getSalario)
                        .reversed()
                        .thenComparingLong(Player::getId))
                .map(Player::getId)
                .get();
    }
```

O próximo exemplo traz alguns métodos novos, como o `limit`, que limita o resultado ao valor passado como parâmetro

```java
public List<Long> buscarTopJogadores(Integer top) {
        return playerList.values().stream()
                .sorted(Comparator.comparingInt(Player::getNivelHabilidade)
                        .reversed()
                        .thenComparingLong(Player::getId))
                .limit(top)
                .map(Player::getId)
                .collect(Collectors.toList());
    }
```

Uma conversão direta de um HashMap para uma List é algo assim:

```java
public List<Long> buscarTimes() {
        return teamList.keySet().stream()
                .sorted()
                .collect(Collectors.toList());
    }
```



***

## Links

[Documentação da Oracle](https://www.oracle.com/technetwork/pt/articles/java/streams-api-java-8-3410098-ptb.html)

https://www.baeldung.com/java-maps-streams

**Vídeos:**

- https://www.youtube.com/watch?v=rkI7gkTkw4c
- [Java 8 Streams Tutorial](https://www.youtube.com/watch?v=t1-YZ6bF-g0)
- [Playlist mais completa, em inglês](https://www.youtube.com/watch?v=vHwToYEYvsU&list=PLTyWtrsGknYdqY_7lwcbJ1z4bvc5yEEZl)
- [Streams: Beyond the Basics](https://www.youtube.com/watch?v=TCJdc9SYwlQ)