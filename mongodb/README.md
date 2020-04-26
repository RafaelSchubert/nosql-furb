Exercícios de prática de MongoDB.

Exercício 1- Aquecendo com os pets\
Insira os seguintes registros no MongoDB e em seguida responda as questões abaixo:\

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
> db.pets.insert({name: "Hilbert", species: "Peixe"})
> db.pets.insert({name: "Frodo", species: "Hamster"})

2. Faça uma contagem dos pets na coleção
> db.pets.count()

3. Retorne apenas um elemento o método prático possível
> db.pets.findOne()

4. Identifique o ID para o Gato Kilha.
> db.pets.findOne({name: "Kilha"})._id

5. Faça uma busca pelo ID e traga o Hamster Mike
> db.pets.findOne({_id: ObjectId("5ea5d556c15ff73237ac3b7b")})

6. Use o find para trazer todos os Hamsters
> db.pets.find({species: "Hamster"})

7. Use o find para listar todos os pets com nome Mike
> db.pets.find({name: "Mike"})

8. Liste apenas o documento que é um Cachorro chamado Mike
> db.pets.find({name: "Mike", species: "Cachorro"})
