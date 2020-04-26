Exerc�cios de pr�tica de MongoDB.

Exerc�cio 1- Aquecendo com os pets\
Insira os seguintes registros no MongoDB e em seguida responda as quest�es abaixo:\

```
use petshop
db.pets.insert({name: "Mike", species: "Hamster"})
db.pets.insert({name: "Dolly", species: "Peixe"})
db.pets.insert({name: "Kilha", species: "Gato"})
db.pets.insert({name: "Mike", species: "Cachorro"})
db.pets.insert({name: "Sally", species: "Cachorro"})
db.pets.insert({name: "Chuck", species: "Gato"})
```

1. Adicione outro Peixe e um Hamster com nome Frodo

2. Fa�a uma contagem dos pets na cole��o

3. Retorne apenas um elemento o m�todo pr�tico poss�vel

4. Identifique o ID para o Gato Kilha.

5. Fa�a uma busca pelo ID e traga o Hamster Mike

6. Use o find para trazer todos os Hamsters

7. Use o find para listar todos os pets com nome Mike

8. Liste apenas o documento que � um Cachorro chamado Mike
