# nosql-exercicio-neo4j
Exercícios de Neo4j da disciplina de Bancos de Dados Não-Relacionais do curso de Especialização em Data Science, turma 2, pela FURB.

-----

- Exercise 1.1: Retrieve all nodes from the database.
> MATCH (vertice) RETURN vertice;

- Exercise 1.2: Examine the data model for the graph.
> CALL db.schema.visualization();

Nota: o material de consulta menciona como "CALL db.schema;". No entanto, isso mudou.

- Exercise 1.3: Retrieve all Person nodes.
> MATCH (vertice:Person) RETURN vertice;

- Exercise 1.4: Retrieve all Movie nodes.
> MATCH (vertice:Movie) RETURN vertice;

-----

- Exercise 2.1: Retrieve all movies that were released in a specific year.
> MATCH (filme:Movie {released: 2000}) RETURN filme;

- Exercise 2.2: View the retrieved results as a table.


- Exercise 2.3: Query the database for all property keys.
> CALL db.propertyKeys;

- Exercise 2.4: Retrieve all Movies released in a specific year, returning their titles.
> MATCH (filme:Movie {released: 1998}) RETURN filme.title AS Titulo;

- Exercise 2.5: Display title, released, and tagline values for every Movie node in the graph.
> MATCH (filme:Movie) RETURN filme.title, filme.released, filme.tagline;

- Exercise 2.6: Display more user-friendly headers in the table
> MATCH (filme:Movie) RETURN filme.title AS Titulo, filme.released AS Lancamento, filme.tagline AS Slogan;

-----

- Exercise 3.1: Display the schema of the database.

Idem ao Exercício 1.2:
> CALL db.schema.visualization();

- Exercise 3.2: Retrieve all people who wrote the movie Speed Racer.
> MATCH (autor:Person)-[:WROTE]->(:Movie {title: "Speed Racer"}) RETURN autor.name;

- Exercise 3.3: Retrieve all movies that are connected to the person, Tom Hanks.
> MATCH (:Person {name: "Tom Hanks"})--(filme:Movie) RETURN filme;

- Exercise 3.4: Retrieve information about the relationships Tom Hanks had with the set of movies retrieved earlier.
> MATCH (:Person {name: "Tom Hanks"})-[relacao]-(filme:Movie) RETURN filme.title AS Titulo, type(relacao) AS Relacao;

- Exercise 3.5: Retrieve information about the roles that Tom Hanks acted in.
> MATCH (:Person {name: "Tom Hanks"})-[atuacao:ACTED_IN]-(filme:Movie) RETURN filme.title AS Filme, atuacao.roles AS Papeis;

-----

- Exercise 4.1: Retrieve all movies that Tom Cruise acted in.
> MATCH (filme)\
  WHERE (filme:Movie)<-[:ACTED_IN]-(:Person {name: "Tom Cruise"})\
  RETURN filme.title AS Filme;

- Exercise 4.2: Retrieve all people that were born in the 70's.
> MATCH (pessoa:Person)\
  WHERE 1970 <= pessoa.born AND pessoa.born < 1980\
  RETURN pessoa.name AS Nome, pessoa.born AS Nascimento;

- Exercise 4.3: Retrieve the actors who acted in the movie The Matrix who were born after 1960.
> MATCH (ator)\
  WHERE (ator:Person)-[:ACTED_IN]->(:Movie {title: "The Matrix"})\
        AND 1960 < ator.born\
  RETURN ator.name AS Ator, ator.born AS Nascimento;

- Exercise 4.4: Retrieve all movies by testing the node label and a property.
> MATCH (filme)\
  WHERE filme:Movie AND exists(filme.title)\
  RETURN filme;

- Exercise 4.5: Retrieve all people that wrote movies by testing the relationship between two nodes.
> MATCH (autor)\
  WHERE (autor:Person)-[:WROTE]->(:Movie)\
  RETURN autor.name AS Autor;

- Exercise 4.6: Retrieve all people in the graph that do not have a property.
> MATCH (pessoa:Person)\
  WHERE NOT exists(pessoa.rating)\
  RETURN pessoa.name AS Nome;

- Exercise 4.7: Retrieve all people related to movies where the relationship has a property.
> MATCH (pessoa:Person)-[relacao]->(filme:Movie)\
  WHERE exists(relacao.roles)\
  RETURN pessoa.name AS Ator, filme.title AS Filme, relacao.roles AS Papeis;

- Exercise 4.8: Retrieve all actors whose name begins with James.
> MATCH (ator:Person)\
  WHERE ator.name STARTS WITH "James"\
  RETURN ator.name AS Ator;

- Exercise 4.9: Retrieve all all REVIEW relationships from the graph with filtered results.
> MATCH (revisor:Person)-[revisao:REVIEWED]->(filme:Movie)\
  WHERE 70 <= revisao.rating\
  RETURN revisor.name AS Revisor, filme.title AS Filme, revisao.rating AS Nota;

- Exercise 4.10: Retrieve all people who have produced a movie, but have not directed a movie.
> MATCH (produtor:Person)\
  WHERE (produtor)-[:PRODUCED]->(:Movie)\
        AND NOT (produtor)-[:DIRECTED]->(:Movie)\
  RETURN produtor.name AS Produtor;

- Exercise 4.11: Retrieve the movies and their actors where one of the actors also directed the movie.
> MATCH (ator:Person)-[:ACTED_IN]->(filme:Movie)\
  WHERE exists((filme)<-[:DIRECTED]-(:Person)-[:ACTED_IN]->(filme))\
  RETURN filme.title AS Filme, ator.name AS Ator;

- Exercise 4.12: Retrieve all movies that were released in a set of years.
> MATCH (filme:Movie)\
  WHERE filme.released IN [1960, 1970, 1980, 1990, 2000]\
  RETURN filme.title AS Filme, filme.released AS Lancamento;

- Exercise 4.13: Retrieve the movies that have an actor's role that is the name of the movie.
> MATCH (filme:Movie)<-[atuacao:ACTED_IN]->(:Person)\
  WHERE filme.title IN atuacao.roles\
  RETURN filme;

-----

- Exercise 5.1: Retrieve data using multiple MATCH patterns.
> MATCH (:Person {name: "Laurence Fishburne"})-[:ACTED_IN]->(filme:Movie),\
        (ator:Person)-[:ACTED_IN]->(filme:Movie)\
  RETURN filme, ator;

- Exercise 5.2: Retrieve particular nodes that have a relationship.
> MATCH (a:Person)--(b:Person) RETURN a, b;

- Exercise 5.3: Modify the query to retrieve nodes that are exactly three hops away.
> MATCH (a:Person)-[*3]-(b:Person) RETURN a, b;

- Exercise 5.4: Modify the query to retrieve nodes that are one and two hops away.
> MATCH (a:Person)-[*1..2]-(b:Person) RETURN a, b;

- Exercise 5.5: Modify the query to retrieve particular nodes that are connected no matter how many hops are required.
> MATCH (a:Person)-[*]-(b:Person) RETURN a, b;

- Exercise 5.6: Specify optional data to be retrieved during the query.
> MATCH (ator:Person)\
  WHERE (ator:Person)-[:ACTED_IN]->(:Movie)\
        AND ator.born < 1980\
  OPTIONAL MATCH (ator:Person)-[:ACTED_IN]->(filme:Movie)<-[:ACTED_IN]-(:Person {name: "Al Pacino"})\
  RETURN ator.name, ator.born, filme.title, filme.released;

- Exercise 5.7: Retrieve nodes by collecting a list.
> MATCH (diretor:Person)-[:DIRECTED]->(:Movie)\
  RETURN collect(diretor.name) AS Diretores;

- Exercise 5.9: Retrieve nodes as lists and return data associated with the corresponding lists.
> MATCH (filme:Movie)\
  WHERE 1980 <= filme.released AND filme.released < 1990\
  RETURN count(*) AS \`Filmes da Década de 80\`, collect(filme.title) AS Filmes;

- Exercise 5.10: Retrieve nodes and their relationships as lists.
> MATCH (vertice)-[relacao]-(outro)\
  RETURN vertice AS Vértice, collect(type(relacao)) AS Relações, collect(outro) AS Com;

- Exercise 5.11: Retrieve the actors who have acted in exactly five movies.
> MATCH (ator:Person)-[:ACTED_IN]->(filme:Movie)\
  WITH ator, count(ator) AS qtdFilmes, collect(filme.title) AS filmesAtuados\
  WHERE qtdFilmes = 5\
  RETURN ator.name AS Ator, filmesAtuados AS Filmes;

- Exercise 5.12: Retrieve the movies that have at least 2 directors with other optional data.
> MATCH (filme:Movie)<-[:DIRECTED]-(diretor:Person)\
  WITH filme, count(filme) AS qtdDiretores, collect(diretor.name) AS diretores\
  WHERE qtdDiretores >= 2\
  OPTIONAL MATCH (filme)<-[:REVIEWED]-(revisor:Person)\
  RETURN filme.title AS Filme, diretores AS Diretores, collect(revisor.name) AS Revisores;

-----

- Exercise 6.1: Execute a query that returns duplicate records.
> MATCH (p:Person)-[:DIRECTED|:PRODUCED]->(f:Movie)\
  RETURN p.name, f.title;

- Exercise 6.2: Modify the query to eliminate duplication.
> MATCH (p:Person)-[:DIRECTED|:PRODUCED]->(f:Movie)\
  RETURN DISTINCT p.name, f.title;

- Exercise 6.3: Modify the query to eliminate more duplication.

Este exercício por acaso esperava que eu tivesse feito uma query **muito** específica no exercício 6.1?

- Exercise 6.4: Sort results returned.
> MATCH (p:Person)-[:DIRECTED|:PRODUCED]->(f:Movie)\
  RETURN DISTINCT p.name, f.title\
  ORDER BY p.name, f.title;

- Exercise 6.5: Retrieve the top 5 ratings and their associated movies.
> MATCH (filme:Movie)<-[revisao:REVIEWED]-(revisor:Person)\
  RETURN filme.title AS Filme, revisor.name AS Revisor, revisao.rating AS Nota\
  ORDER BY Nota DESC\
  LIMIT 5;

- Exercise 6.6: Retrieve all actors that have not appeared in more than 3 movies.
> MATCH (ator:Person)-[:ACTED_IN]->(filme:Movie)\
  WITH ator, count(ator) AS qtdFilmes, collect(filme.title) AS filmes\
  WHERE qtdFilmes <= 3\
  RETURN ator.name AS Ator, filmes AS Filmes\
  ORDER BY Ator;

-----

- Exercise 7.1: Collect and use lists.
> MATCH (ator:Person)-[:ACTED_IN]->(filme:Movie)<-[:ACTED_IN]-(atorMatrix:Person)\
  WHERE NOT (ator)-[:ACTED_IN]->(:Movie {title: "The Matrix"})\
        AND (atorMatrix)-[:ACTED_IN]->(:Movie {title: "The Matrix"})\
  WITH filme, collect(DISTINCT atorMatrix.name) AS elencoMatrix, collect(DISTINCT ator.name) AS elenco\
  RETURN filme.title AS Filme, elencoMatrix AS \`Elenco Matrix\`, elenco AS Elenco\
  ORDER BY Filme;

- Exercise 7.2: Collect a list.
> MATCH (filme:Movie)\
  WITH filme.released AS ano, collect(filme.title) AS filmes\
  RETURN ano AS Ano, filmes AS Lançamentos\
  ORDER BY Ano DESC;

- Exercise 7.3: Unwind a list.
> MATCH (filme:Movie)\
  WITH filme.released AS ano, collect(filme.title) AS filmes\
  ORDER BY ano DESC\
  UNWIND filmes AS filme\
  RETURN ano AS Ano, filme AS Filme;

- Exercise 7.4: Perform a calculation with the date type.
> MATCH (diretor:Person)-[:DIRECTED]->(filme:Movie)\
  WHERE exists(diretor.born)\
  WITH diretor, date().year - diretor.born AS idade, collect(filme.title) AS filmes\
  ORDER BY size(filmes) DESC, idade DESC, diretor.name\
  RETURN diretor.name AS Diretor, idade AS Idade, filmes AS Dirigiu;

-----

- Exercise 8.1: Create a Movie node.
> CREATE (:Movie {title: "Avatar", tagline: "Enter the world", released: 2009});

- Exercise 8.2: Retrieve the newly-created node.
> MATCH (filme:Movie {title: "Avatar"}) RETURN filme;

- Exercise 8.3: Create a Person node.
> CREATE (:Person {name: "James Cameron", born: 1954});

- Exercise 8.4: Retrieve the newly-created node.
> MATCH (diretor:Person {name: "James Cameron"}) RETURN diretor;

- Exercise 8.5: Add a label to a node.
> MATCH (cameron:Person {name: "James Cameron"})\
  SET cameron:Director\
  RETURN labels(cameron);

- Exercise 8.6: Retrieve the node using the new label.
> MATCH (diretor:Director) RETURN diretor;

- Exercise 8.7: Add the Female label to selected nodes.
> MATCH (atriz:Person)-[:ACTED_IN]->(:Movie)\
  WHERE atriz.name IN [\
    "Bonnie Hunt",\
    "Carrie Fisher",\
    "Charlize Theron",\
    "Demi Moore",\
    "Halle Berry"\
  ]\
  SET atriz:Female\
  RETURN count(DISTINCT atriz);

- Exercise 8.8: Retrieve all Female nodes.
> MATCH (mulher:Female) RETURN mulher;

- Exercise 8.9: Remove the Female label from the nodes that have this label.
> MATCH (mulher:Female)\
  REMOVE mulher:Female\
  RETURN count(mulher);

- Exercise 8.10: View the current schema of the graph.
> CALL db.schema.visualization;

- Exercise 8.11: Add properties to a movie.
> MATCH (filme:Movie {title: "Avatar"})\
  SET filme += {runningTime: 60 * 2 + 42, genres: ["Sci fi", "Action"]}\
  RETURN filme;

- Exercise 8.12: Retrieve an OlderMovie node to confirm the label and properties.

A label "OlderMovie" não foi incluída durante os exercícios, mas bastaria especificá-la no padrão do nó ("MATCH (node:OlderMovie)...").\
Por isso, eu vou apenas retornar o nó atualizado no exercício 8.11.
> MATCH (filme:Movie {title: "Avatar"}) RETURN filme;

- Exercise 8.13: Add properties to the person, Robin Wright.

Mais uma vez, uma operação sobre um nó que não existe. Mas a query seria algo assim:
> MATCH (atriz:Person {name: "Robin Wright"})\
  SET atriz += {gender: "female"}\
  RETURN atriz;

- Exercise 8.14: Retrieve an updated Person node.
> MATCH (pessoa:Person)\
  WHERE exists(pessoa.gender)\
  RETURN pessoa;

- Exercise 8.15: Remove a property from a Movie node.
> MATCH (filme:Movie {title: "Avatar"})\
  SET filme += {runningTime: null, genres: null}\
  RETURN count(DISTINCT filme);

- Exercise 8.16: Retrieve the node to confirm that the property has been removed.
> MATCH (filme:Movie {title: "Avatar"}) RETURN filme;

- Exercise 8.17: Remove a property from a Person node.
> MATCH (clint:Person {name: "Clint Eastwood"})\
  REMOVE clint.born\
  RETURN count(clint);

- Exercise 8.18: Retrieve the node to confirm that the property has been removed.
> MATCH (clint:Person {name: "Clint Eastwood"}) RETURN clint;

-----

- Exercise 9.1: Create ACTED_IN relationships.
> MATCH (filme:Movie {title: "Avatar"})\
  CREATE (sam:Person {name: "Sam Worthington", born: 1976})\
  CREATE (zoe:Person {name: "Zöe Saldaña", born: 1978})\
  CREATE (sam)-[:ACTED_IN]->(filme),\
         (zoe)-[:ACTED_IN]->(filme);

- Exercise 9.2: Create DIRECTED relationships.
> MATCH (diretor:Person {name: "James Cameron"})\
  WITH diretor\
  MATCH (filme:Movie {title: "Avatar"})\
  CREATE (diretor)-[:DIRECTED]->(filme);

- Exercise 9.3: Create a HELPED relationship.
> CREATE (horner:Person {name: "James Horner", born: 1953})\
  CREATE (fiore:Person {name: "Mauro Fiore", born: 1964})\
  WITH horner, fiore\
  MATCH (filme:Movie {title: "Avatar"})\
  CREATE (horner)-[:HELPED]->(filme)\
  CREATE (fiore)-[:HELPED]->(filme);

- Exercise 9.4: Query nodes and new relationships.
> MATCH (alguem:Person)-[relacao:ACTED_IN|:DIRECTED|:HELPED]->(:Movie {title: "Avatar"})\
  RETURN alguem.name AS Pessoa, type(relacao);

- Exercise 9.5: Add properties to relationships.
> MATCH (ator:Person {name: "Sam Worthington"})-[atuacao:ACTED_IN]-(:Movie {title: "Avatar"})\
  SET atuacao.roles = ["Jake Sulivan"];\
> MATCH (ator:Person {name: "Zöe Saldaña"})-[atuacao:ACTED_IN]-(:Movie {title: "Avatar"})\
  SET atuacao.roles = ["Neytiri"];

- Exercise 9.6: Add a property to the HELPED relationship.
> MATCH (ajudante:Person {name: "James Horner"})-[relacao:HELPED]->(:Movie {title: "Avatar"})\
  SET relacao.function = "composer";\
> MATCH (ajudante:Person {name: "Mauro Fiore"})-[relacao:HELPED]->(:Movie {title: "Avatar"})\
  SET relacao.function = "cinematography";

- Exercise 9.7: View the current list of property keys in the graph.
> CALL db.propertyKeys;

- Exercise 9.8: View the current schema of the graph.
> CALL db.schema.visualization;

- Exercise 9.9: Retrieve the names and roles for actors.
> MATCH (ator:Person)-[atuacao:ACTED_IN]->(filme:Movie)\
  UNWIND atuacao.roles AS papel\
  RETURN ator.name AS Ator, papel AS Papel, filme.title AS Filme\
  ORDER BY Ator, Filme, Papel;

- Exercise 9.10: Retrieve information about any specific relationships.
> MATCH (revisor:Person)-[revisao:REVIEWED]->(filme:Movie)\
  RETURN filme.title AS Filme, revisao.rating AS Nota, revisor.name AS Revisor, revisao.summary AS Resumo\
  ORDER BY Filme, Nota;

- Exercise 9.11: Modify a property of a relationship.
> MATCH (:Person {name: "Jessica Thompson"})-[revisao:REVIEWED]->(:Movie)\
  WHERE exists(revisao.rating) AND revisao.rating < 50\
  SET revisao.rating = 50;

- Exercise 9.12: Remove a property from a relationship.
> MATCH (:Person)-[revisao:REVIEWED]-(:Movie)\
  WHERE revisao.rating = 100\
  REMOVE revisao.rating;

- Exercise 9.13: Confirm that your modifications were made to the graph.
> MATCH (:Person {name: "Jessica Thompson"})-[revisao:REVIEWED]->(:Movie)\
  RETURN revisao;\
> MATCH (:Person)-[revisao:REVIEWED]-(:Movie)\
  WHERE NOT exists(revisao.rating)\
  RETURN revisao;

-----

- Exercise 10.1: Delete a relationship.
> MATCH (:Person {name: "Tom Hanks"})-[direcao:DIRECTED]->(:Movie) DELETE direcao;

- Exercise 10.2: Confirm that the relationship has been deleted.
> MATCH (:Person {name: "Tom Hanks"})-[direcao:DIRECTED]->(:Movie) RETURN direcao;

- Exercise 10.3: Retrieve a movie and all of its relationships.
> MATCH (a)-[r]-(filme:Movie {title: "Avatar"})\
  RETURN filme.title AS Filme, type(r) AS Relação, a AS \`Com Quem\`;

- Exercise 10.4: Try deleting a node without detaching its relationships.
> MATCH (f:Movie {title: "Avatar"}) DELETE f;

- Exercise 10.5: Delete a Movie node, along with its relationships.
> MATCH (f:Movie {title: "Avatar"})-[r]-() DELETE r, f;

- Exercise 10.6: Confirm that the Movie node has been deleted.
> MATCH (a)-[r]-(filme:Movie {title: "Avatar"})\
  RETURN filme.title AS Filme, type(r) AS Relação, a AS \`Com Quem\`;

-----
