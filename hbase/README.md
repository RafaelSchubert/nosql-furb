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
```
put 'italians', '11', 'personal-data:name', 'Dante Alighieri'
put 'italians', '11', 'personal-data:city', 'Florenca'
put 'italians', '11', 'personal-data:birth', '1256-05-11'
put 'italians', '11', 'professional-data:role', 'Poeta'
put 'italians', '11', 'professional-data:salary', '1000'
put 'italians', '11', 'professional-data:experience', '30'
put 'italians', '12', 'personal-data:name', 'Galileo Galilei'
put 'italians', '12', 'personal-data:city', 'Pisa'
put 'italians', '12', 'personal-data:birth', '1564-02-15'
put 'italians', '12', 'professional-data:role', 'Astronomo'
put 'italians', '12', 'professional-data:salary', '2000'
put 'italians', '12', 'professional-data:experience', '50'
```

1.2. Adicione o controle de 5 vers�es na tabela de dados pessoais.
```
alter 'italians', NAME => 'personal-data', VERSIONS => 5
```

1.3. Fa�a 5 altera��es em um dos italianos;
```
put 'italians', '12', 'personal-data:name', 'galileo Galilei'
put 'italians', '12', 'personal-data:name', 'Galileo galilei'
put 'italians', '12', 'personal-data:name', 'Ga-li-le-o Ga-li-lei'
put 'italians', '12', 'personal-data:name', 'GALILEO GALILEI'
put 'italians', '12', 'personal-data:name', 'galileo.galilei'
```

1.4. Com o operador get, verifique como o HBase armazenou o hist�rico.
```
get 'italians', '12', { COLUMN => 'personal-data:name', VERSIONS => 5 }
```

1.5. Utilize o scan para mostrar apenas o nome e profiss�o dos italianos.
```
scan 'italians', { COLUMNS => ['personal-data:name', 'professional-data:role'] }
```

1.6. Apague os italianos com row id �mpar

Essa certamente n�o � a forma mais apropriada, mas � o que d� para fazer sem usar APIs ou, provavelmente, MapReduce.
```
t = get_table 'italians'
for idx in (1..(t.count)).step(2) do
  t.deleteall String(idx)
end
```

1.7. Crie um contador de idade 55 para o italiano de row id 5
```
incr 'italians', '5', 'personal-data:age', 55
```

1.8. Incremente a idade do italiano em 1
```
incr 'italians', '5', 'personal-data:age', 1
```
Mas tamb�m se pode omitir o incremento, que � `1` por padr�o.
```
incr 'italians', '5', 'personal-data:age'
```
