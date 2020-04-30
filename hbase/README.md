Exerc�cios de pr�tica de HBase.

-----

#Exerc�cio 1 -- Aquecendo com alguns dados\
Importe o arquivo italians.txt do reposit�rio Downloads NoSQL FURB. Em seguida, carregue o mesmo no hbase. Para isso, voc� precisa enviar o arquivo para a imagem Docker primeiro.
```
docker cp italians.txt hbase-furb:/tmp
```
Agora, de dentro da imagem, fa�a os seguintes procedimentos:

1. Crie a tabela com 2 fam�lias de colunas:
  a. personal-data
  b. professional-data
2. Importe o arquivo via linha de comando

Agora execute as seguintes opera��es:

1.1. Adicione mais 2 italianos mantendo adicionando informa��es como data de nascimento nas informa��es pessoais e um atributo de anos de experi�ncia nas informa��es profissionais;

1.2. Adicione o controle de 5 vers�es na tabela de dados pessoais.

1.3. Fa�a 5 altera��es em um dos italianos;

1.4. Com o operador get, verifique como o HBase armazenou o hist�rico.

1.5. Utilize o scan para mostrar apenas o nome e profiss�o dos italianos.

1.6. Apague os italianos com row id �mpar

1.7. Crie um contador de idade 55 para o italiano de row id 5

1.8. Incremente a idade do italiano em 1
