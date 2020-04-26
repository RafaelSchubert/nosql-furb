Exerc�cios de pr�tica de MongoDB.

-----

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
> db.pets.insert({name: "Hilbert", species: "Peixe"})\
db.pets.insert({name: "Frodo", species: "Hamster"})

2. Fa�a uma contagem dos pets na cole��o
> db.pets.count()

3. Retorne apenas um elemento o m�todo pr�tico poss�vel
> db.pets.findOne()

4. Identifique o ID para o Gato Kilha.
> db.pets.findOne({name: "Kilha"})._id

5. Fa�a uma busca pelo ID e traga o Hamster Mike
> db.pets.findOne({_id: ObjectId("5ea5d556c15ff73237ac3b7b")})

6. Use o find para trazer todos os Hamsters
> db.pets.find({species: "Hamster"})

7. Use o find para listar todos os pets com nome Mike
> db.pets.find({name: "Mike"})

8. Liste apenas o documento que � um Cachorro chamado Mike
> db.pets.find({name: "Mike", species: "Cachorro"})

-----

Exerc�cio 2 � Mama mia!\
Importe o arquivo dos italian-people.js do seguinte endere�o: Downloads NoSQL FURB. Em seguida, importe o mesmo com o seguinte comando:

```
mongo italian-people.js
```

Analise um pouco a estrutura dos dados e em seguida responda:

1. Liste/Conte todas as pessoas que tem exatamente 99 anos. Voc� pode usar um count para indicar a quantidade.

2. Identifique quantas pessoas s�o eleg�veis atendimento priorit�rio (pessoas com mais de 65 anos)

3. Identifique todos os jovens (pessoas entre 12 a 18 anos).

4. Identifique quantas pessoas tem gatos, quantas tem cachorro e quantas n�o tem nenhum dos dois

5. Liste/Conte todas as pessoas acima de 60 anos que tenham gato

6. Liste/Conte todos os jovens com cachorro

7. Utilizando o $where, liste todas as pessoas que tem gato e cachorro

8. Liste todas as pessoas mais novas que seus respectivos gatos.

9. Liste as pessoas que tem o mesmo nome que seu bichano (gatou ou cachorro)

10. Projete apenas o nome e sobrenome das pessoas com tipo de sangue de fator RH negativo

11. Projete apenas os animais dos italianos. Devem ser listados os animais com nome e idade. N�o mostre o identificado do mongo (ObjectId)

12. Quais s�o as 5 pessoas mais velhas com sobrenome Rossi?

13. Crie um italiano que tenha um le�o como animal de estima��o. Associe um nome e idade ao bichano

14. Infelizmente o Le�o comeu o italiano. Remova essa pessoa usando o Id.

15. Passou um ano. Atualize a idade de todos os italianos e dos bichanos em 1.

16. O Corona V�rus chegou na It�lia e misteriosamente atingiu pessoas somente com gatos e de 66 anos. Remova esses italianos.

17. Utilizando o framework agregate, liste apenas as pessoas com nomes iguais a sua respectiva m�e e que tenha gato ou cachorro.

18. Utilizando aggregate framework, fa�a uma lista de nomes �nica de nomes. Fa�a isso usando apenas o primeiro nome

19. Agora fa�a a mesma lista do item acima, considerando nome completo.

20. Procure pessoas que gosta de Banana ou Ma��, tenham cachorro ou gato, mais de 20 e menos de 60 anos.
