Exercícios de prática de MongoDB.

-----

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
```
db.pets.insert({name: "Hilbert", species: "Peixe"})
db.pets.insert({name: "Frodo", species: "Hamster"})
```

2. Faça uma contagem dos pets na coleção
```
db.pets.count()
```

3. Retorne apenas um elemento o método prático possível
```
db.pets.findOne()
```

4. Identifique o ID para o Gato Kilha.
```
db.pets.findOne({name: "Kilha"})._id
```

5. Faça uma busca pelo ID e traga o Hamster Mike
```
db.pets.findOne({_id: ObjectId("5ea5d556c15ff73237ac3b7b")})
```

6. Use o find para trazer todos os Hamsters
```
db.pets.find({species: "Hamster"})
```

7. Use o find para listar todos os pets com nome Mike
```
db.pets.find({name: "Mike"})
```

8. Liste apenas o documento que é um Cachorro chamado Mike
```
db.pets.find({name: "Mike", species: "Cachorro"})
```

-----

Exercício 2 – Mama mia!\
Importe o arquivo dos italian-people.js do seguinte endereço: Downloads NoSQL FURB. Em seguida, importe o mesmo com o seguinte comando:

```
mongo italian-people.js
```

Analise um pouco a estrutura dos dados e em seguida responda:

1. Liste/Conte todas as pessoas que tem exatamente 99 anos. Você pode usar um count para indicar a quantidade.
```
db.italians.find({age: 99}).count()
```

2. Identifique quantas pessoas são elegíveis atendimento prioritário (pessoas com mais de 65 anos)
```
db.italians.find({age: {$gt: 65}}).count()
```

3. Identifique todos os jovens (pessoas entre 12 a 18 anos).
```
db.italians.find({age: {$gte: 12, $lte: 18}})
```

4. Identifique quantas pessoas tem gatos, quantas tem cachorro e quantas não tem nenhum dos dois

Pessoas que possuem gato.
```
db.italians.find({cat: {$exists: true}}).count()
```

Pessoas que possuem cão.
```
db.italians.find({dog: {$exists: true}}).count()
```

Pessoas que não possuem nem gato, nem cão.
```
db.italians.find({cat: {$exists: false}, dog: {$exists: false}}).count()
```

5. Liste/Conte todas as pessoas acima de 60 anos que tenham gato
```
db.italians.find({age: {$gt: 60}, cat: {$exists: true}})
```

6. Liste/Conte todos os jovens com cachorro
```
db.italians.find({age: {$gte: 12, $lte: 18}, dog: {$exists: true}})
```

7. Utilizando o $where, liste todas as pessoas que tem gato e cachorro
```
db.italians.find({$where:
    function() {
        return !(this["cat"]==null || this["dog"]==null);
    }
})
```

8. Liste todas as pessoas mais novas que seus respectivos gatos.
```
db.italians.find({$where:
    function() {
        return this["cat"]!=null && this["age"] < this["cat"]["age"];
    }
})
```

9. Liste as pessoas que tem o mesmo nome que seu bichano (gatou ou cachorro)
```
db.italians.find({$where:
    function() {
        let pet=this["cat"]?this["cat"]:this["dog"];

        if(pet==null)
            return false;

        return this.firstname==pet.name;
    }
})
```

10. Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo

Aqui, eu não tenho certeza se o ID também era desejado, então o omiti de propósito.
```
db.italians.find({bloodType:{$regex: /-$/}}, {_id:false, firstname:true, surname:true})
```

11. Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. Não mostre o identificado do mongo (ObjectId)
```
db.italians.find(
    {
        $or:
            [
                { cat: { $exists: true } },
                { dog: { $exists: true } }
            ]
    },
    {
        cat: true,
        dog: true,
        _id: false
    }
)
```

12. Quais são as 5 pessoas mais velhas com sobrenome Rossi?
```
db.italians.find({surname: "Rossi"}).sort({age: -1}).limit(5)
```

13. Crie um italiano que tenha um leão como animal de estimação. Associe um nome e idade ao bichano
```
db.italians.insert(
    {
        firstname: "Lorenzo",
        surname: "Coppola",
        username: "user10100",
        age: 42,
        email: "Lorenzo.Coppola@yahoo.com",
        bloodType: "O+",
        id_num: "684314636153",
        registerDate: new Date(),
        ticketNumber: 4656,
        jobs: ["Produção Sucroalcooleira"],
        favFruits: ["Mamão", "Laranja"],
        movies: [
            {
                title: "Forrest Gump: O Contador de Histórias (1994)",
                rating: 5.0
            }
        ],
        lion: {
            name: "Simba",
            age: 12
        }
    }
)
```

14. Infelizmente o Leão comeu o italiano. Remova essa pessoa usando o Id.
```
let lionOwner = db.italians.findOne({lion: {$exists: true}})
if (lionOwner) db.italians.remove({_id: lionOwner._id})
```

15. Passou um ano. Atualize a idade de todos os italianos e dos bichanos em 1.
```
db.italians.update({}, {$inc: {age: 1}}, {multi: true})
db.italians.update({cat: {$exists: true}}, {$inc: {"cat.age": 1}}, {multi: true})
db.italians.update({dog: {$exists: true}}, {$inc: {"dog.age": 1}}, {multi: true})
```

16. O Corona Vírus chegou na Itália e misteriosamente atingiu pessoas somente com gatos e de 66 anos. Remova esses italianos.
```
db.italians.remove({age: 66, cat: {$exists: true}})
```

17. Utilizando o framework agregate, liste apenas as pessoas com nomes iguais a sua respectiva mãe e que tenha gato ou cachorro.
```
db.italians.aggregate(
    {
        $match: {
            $expr: {
                $eq: [
                    "$firstname",
                    "$mother.firstname"
                ]
            }
        }
    }
)
```

18. Utilizando aggregate framework, faça uma lista de nomes única de nomes. Faça isso usando apenas o primeiro nome

19. Agora faça a mesma lista do item acima, considerando nome completo.

20. Procure pessoas que gosta de Banana ou Maçã, tenham cachorro ou gato, mais de 20 e menos de 60 anos.
```
db.italians.aggregate(
    {
        $match: {
            $expr: {
                $and: [
                    {
                        $or: [
                            { $in: ["Banana", "$favFruits"] },
                            { $in: ["Maçã", "$favFruits"] }
                        ]
                    },
                    {
                        $or: [
                            { $ne: ["$cat", null] },
                            { $ne: ["$dog", null] }
                        ]
                    },
                    {
                        $and: [
                            { $gt: ["$age", 20] },
                            { $lt: ["$age", 60] }
                        ]
                    }
                ]
            }
        }
    }
)
```
