# Estudos sobre MongoDB
[Curso Completo MongoDB 2023 [NoSQL do Básico ao Avançado!]](https://www.udemy.com/course/mongodb-curso-completo/)

# Seção 2 - Instalação
## Aula 8. Instalando no windows
- [Link](https://www.youtube.com/watch?v=gB6WLkSrtJk)
- **NOTA 1:** Desmarcar *Install MongoDB as a Service* para que ele não inicie junto com windows.
- **NOTA 2:** Criamos um diretório chamado **data** em *C:\\* e dentro de *data* criamos outro diretório chamado *db* (*C:\data\db*).
- **NOTA 3:** Configurar variável *Path* em variáveis de ambiente do windows, para poder acessar tanto o servidor quanto o cliente do servidor por linha de comando para poder ligar e acessar o mongdb por linha de comando, em qualquer diretório.
- 
- **Para Iniciar o MongoDB** 
  - Abrir um terminal e digitar **mongod** p/ligar o BD.
  - Abrir outro terminal e digitar **mongosh** p/acessar o BD pelo cliente em linha de comando.

# Seção 3 - Básico

## Aula 11. Conhecendo o comando Mongo
- mongod -> o daemon do mongo, o mongo em si, o BD.
- mongosh -> o cliente.
- mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.1.0 -> linha de conexão padrão com o BD.

## Aula 14. Inserindo nosso primeiro documento
- **db.\<tecla tab>** apresenta os comandos disponíveis.
- **db** demonstra o banco de dados em que estamos conectados.
- **use \<nome_do_banco>** cria no **SHELL** um novo banco, ou seja, ainda não gravou em disco o novo BD.
- **show dbs** lista os bancos no **DISCO**. 
- **db.\<nome_da_collection>.insert({ "key": "value" })** -> funciona mas está depreciado. Ideal usar:
  - **insertOne**
  - **insertMany**
  - **bulkWrite**
- Dessa forma, o BD em shell será gravado em disco com o dado do objeto.
```json
{
  acknowledged: true,
  insertedIds: { '0': ObjectId('655ccb72ecc8dcd7608df3d5') }
}
```

## Aula 15. Resgatando dados inseridos
-  Com o BD selecionado:
-  **db.\<nome_da_collection>.find().pretty()** o qual retornará os objetos presentes no BD com uma formatação legível.
-  Retorna todos os objetos json com id gerado automaticamente pelo próprio MongoDB:
```json
[
  { _id: ObjectId('655ccb72ecc8dcd7608df3d5'), valor: 123 },
  { _id: ObjectId('655ccb81ecc8dcd7608df3d6'), valor: 123 }
] 
 ```

## Aula 16. Conceitos básicos: Database, Collection e Document
- **Database** - BD, a raiz a qual receberá as colletions.
- **Collection** - funcionam como diretórios. Um BD pode ter várias collections. São agrupados com algum sentido em comum. Uma collection precisa pertencer a um BD.
- **Document** - O objeto armazenado no BD. Entretanto um objeto é armazenado a uma collection.
- Então, um ou mais objetos pertencem a uma collection, e uma ou mais collection pertencem a um banco de dados.

## Aula 17. Criando nosso primeiro documento
## Aula 18. Entendendo os tipos de dados
## Aula 19. Inserindo o aluno na escola
## Aula 20. Destrinchando o ObjectID e entendendo seus benefícios
- BSON - Binary JSON -  tipo ao qual é armazenado no MongoDB.
- BSON tem outros tipos de dados, como Double, Binary Data, Date, etc..
- Ver neste [link](https://www.mongodb.com/docs/v7.0/reference/bson-types/?tck=docs_chatbot#bson-types).
```json
db.alunos.insert({
...     "nome": "Toninho",
...     "idade": 12,
...     "gostaDe": ["futebol", "carrinho", "video game"],
...     "naEscola": true,
...     "matérias": {
...             "Português": 8.9,
...             "Matemática": 9.5,
...             "História": 2.4
...     }
... })
```
- **Tipo de dado: ObjectId** - a inserção do json acima retornou:
```json
[
  {
    _id: ObjectId('655ce608ecc8dcd7608df3d7'),
    nome: 'Toninho',
    idade: 12,
    gostaDe: [ 'futebol', 'carrinho', 'video game' ],
    naEscola: true,
    'matérias': { 'Português': 8.9, 'Matemática': 9.5, 'História': 2.4 }
  }
]
```
- Note o campo *_id: ObjectId('655ce608ecc8dcd7608df3d7')* - é a chave primária do objeto criada automaticamente pelo MongoDB. 
- É possível usar uma outra chave padrão, como números sequênciais não repetidos.
- Para entender melhor o **[ObjectId](https://www.mongodb.com/docs/v7.0/reference/bson-types/?tck=docs_chatbot#objectid)**.

## Aula 21. Resgatando o Timestamp do ObjectID
- **db.alunos.findOne()._id** retorna somente o ObjectId.
- **db.alunos.findOne()._id.getTimestamp()** retorna a data de criação do objeto *ISODate('2023-11-21T17:16:56.000Z')*
- Seu timestamp é baseado no horário do cliente, não do servidor. 

## Aula 22. MongoDB como um processo independente
- **mongod --dbpath \<caminho-do-diretório-do-db> --fork** mais o comando **--logpath**(utiliza um arquivo próprio para armazenar os logs) ou **--syslog**(pega os logs do mongodb e coloca junto com os logs do SO [não funciona no windows]) 
- **--dbpath** não é obrigatório se houver criado o diretórios padrões em **C:\data\db**.
- **NOTA**: --fork somente funciona no windows se o mongodb houver sido iniciado como um [serviço do windows](https://www.mongodb.com/community/forums/t/how-to-use-fork-in-windows-os/12959/2).

## Aula 24. CRUD e sua aplicação com o MongoDB
[Link](https://www.mongodb.com/docs/manual/reference/method/js-collection/#collection-methods) para métodos do mongo no terminal.
[Link](https://www.mongodb.com/docs/v7.0/crud/#mongodb-crud-operations) para as operações de CRUD.
**CREATE**
  - insert
  - insertOne
  - insertMany
  - bulkWrite
**READ**
- find
- findOne
**UPDATE**
- update
- updateOne
- updateMany
**DELETE**
- deleteOne
- deleteMany

## Aula 25. C - Inserindo dados com insertOne e insertMany
insert aceita:
- db.crud.insert({a: 123})
- db.crud.insert([{a: 123}, {b: 456}])
[insertOne](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.insertOne/#syntax) aceita somente:
- db.crud.insertOne({a: 123})
- [insertMany](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.insertMany/#syntax) aceita:
- db.crud.insertMany([{a: 123}, {b: 456}, {c: 789}])
- db.crud.insertMany([{a: 123}, {b: 456}, {c: 789}, {d: 028}], {ordered: false}) - faz tratamento de erro. Por exemplo, se o **ordered** estiver **true**, ele adiciona cada objeto um por um em sequência, entretanto, se o objeto **b** houver algum erro (de validação, p.e.), os elementos **c** e **d** não serão inseridos. Já com **ordered** estiver **false**, ele continuará inserindo.

## Aula 26. Insert ordenado ou não-ordenado?
- Digamos que temos esses objetos no BD:
```json
[ 
  { _id: 1, a: 123 }, 
  { _id: 2, b: 456 }, 
  { _id: 3, c: 789 } 
]
```
- E tentamos inserir: **db.testInsertMany.insertMany([{_id:2}, {_id: 7, msg: 'teste'}, {_id:4, msg: 'teste2'}])**
- Retorna o erro **code:11000** e **insertedCount: 0**, ou seja, não insere dados depois do erro pois o **ordered: true** 
- Com ordered:false: **db.testInsertMany.insertMany([{_id:2}, {_id: 7, msg: 'teste'}, {_id:4, msg: 'teste2'}], {ordered: false})**
- Retorna o erro **code:11000** e **insertedCount: 2**, ou seja, insere os dados que não estejam errados

## Aula 27. R - Buscando dados com find e findOne
- Método [find](https://www.mongodb.com/docs/v7.0/crud/#read-operations) retorna um cursor ao invés de um objeto javascript. Ele aceita:
- db.crud.find() - retorna todos os dados.
- db.crud.find({b: 123}) - retorna todos os dados semelhantes a query/filter dentro dos parênteses.
- db.crud.find({ c: { $exists: true} }) - retorna todos os elementos 'c' se existirem.
- db.crud.find({ c: { $exists: false} }) - retorna todos os elementos que não sejam 'c'.
- Métod findOne já vem com *.pretty()* e aceita:
- db.crud.findOne() - retorna 1º elemento da collection.
- db.crud.findOne({ b: 456}) - retorna 1º elemento correspondente a query/filter dentro dos parênteses da collection.

**[PROJECTION](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.find/#syntax):** escolhe os campos do objeto buscado a serem visualizados.
- db.alunos.findOne({}, { nome: 1, matérias: 1 }) - 1ª chaves é a query/filter, 2ª chaves são os campos a serem retornados, 1 de cada no caso.
- db.alunos.findOne({}, { nome: 1, matérias: 1, _id: 0 }) - mesma coisa que acima mas não retorna o id.

## Aula 28. U - Atualizando dados com update, updateOne e replaceOne
- No método [update](https://www.mongodb.com/docs/v7.0/crud/#update-operations), geralmente fazemos uma busca do objeto que queremos alterar e depois sim o modificamos.
- um exemplo de [updateOne](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.updateOne/#syntax) com findOne:
```bash
meudb> db.crud.findOne({ b: 456 })
  
  { _id: ObjectId('655f426b2d98383a2796eaca'), b: 456 }

meudb> db.crud.updateOne({ b:456}, { $set: { d: 555 } })
  {
    acknowledged: true,
    insertedId: null,
    matchedCount: 1,
    modifiedCount: 1,
    upsertedCount: 0
  }

meudb> db.crud.findOne({ b: 456 })
  
  { _id: ObjectId('655f426b2d98383a2796eaca'), b: 456, d: 555 }
```
- db.crud.updateOne({ b:456}, { $set: { d: 555 } }) - 1ª chaves é query/filter do objeto buscado anteriormente, 2ª chaves é o que queremos alterar no objeto, no caso, estamos inserindo.
- Um exemplo de [updateMany](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.updateMany/#syntax):
- Com os dados abaixo executados com updateOne acima, note só foi alterado o 3º item:
```json
[
  { _id: ObjectId('655f42392d98383a2796eac8'), a: 123 },
  { _id: ObjectId('655f426b2d98383a2796eac9'), a: 123 },
  { _id: ObjectId('655f426b2d98383a2796eaca'), b: 456, d: 555 },
  { _id: ObjectId('655f42eb2d98383a2796eacb'), a: 123 },
  { _id: ObjectId('655f46f12d98383a2796eacc'), a: 123 },
  { _id: ObjectId('655f46f12d98383a2796eacd'), b: 456 },
  { _id: ObjectId('655f46f12d98383a2796eace'), c: 789 },
  { _id: ObjectId('655f48622d98383a2796eacf'), a: 123 },
  { _id: ObjectId('655f48622d98383a2796ead0'), b: 456 },
  { _id: ObjectId('655f48622d98383a2796ead1'), c: 789 },
  { _id: ObjectId('655f48622d98383a2796ead2'), d: 28 },
  { _id: ObjectId('655f72793239c0a177a5c435'), b: 123 }
]
```
- meudb> db.crud.updateMany({ a: 123}, { $set: { h: 20 } }) - altera todos os objetos 'a's:
```bash
meudb> db.crud.updateMany({ a: 123}, { $set: { h: 20 } })

{
  acknowledged: true,
  insertedId: null,
  matchedCount: 5,
  modifiedCount: 5,
  upsertedCount: 0
}

meudb> db.crud.find()

[
  { _id: ObjectId('655f42392d98383a2796eac8'), a: 123, h: 20 },
  { _id: ObjectId('655f426b2d98383a2796eac9'), a: 123, h: 20 },
  { _id: ObjectId('655f426b2d98383a2796eaca'), b: 456, d: 555 },
  { _id: ObjectId('655f42eb2d98383a2796eacb'), a: 123, h: 20 },
  { _id: ObjectId('655f46f12d98383a2796eacc'), a: 123, h: 20 },
  { _id: ObjectId('655f46f12d98383a2796eacd'), b: 456 },
  { _id: ObjectId('655f46f12d98383a2796eace'), c: 789 },
  { _id: ObjectId('655f48622d98383a2796eacf'), a: 123, h: 20 },
  { _id: ObjectId('655f48622d98383a2796ead0'), b: 456 },
  { _id: ObjectId('655f48622d98383a2796ead1'), c: 789 },
  { _id: ObjectId('655f48622d98383a2796ead2'), d: 28 },
  { _id: ObjectId('655f72793239c0a177a5c435'), b: 123 }
]
```
- Com [replaceOne](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.replaceOne/#syntax) troca o objeto, ao invés de inserir mais dados.
```bash
meudb> db.crud.find({c: 789})

  [
    { _id: ObjectId('655f46f12d98383a2796eace'), c: 789 },
    { _id: ObjectId('655f48622d98383a2796ead1'), c: 789 }
  ]

meudb> db.crud.replaceOne({c: 789}, {outro: "Documento"})

  {
    acknowledged: true,
    insertedId: null,
    matchedCount: 1,
    modifiedCount: 1,
    upsertedCount: 0
  }

meudb> db.crud.find({c: 789})
  
  [ { _id: ObjectId('655f48622d98383a2796ead1'), c: 789 } ]

meudb> db.crud.find()
[
  { _id: ObjectId('655f42392d98383a2796eac8'), a: 123, h: 20 },
  { _id: ObjectId('655f426b2d98383a2796eac9'), a: 123, h: 20 },
  { _id: ObjectId('655f426b2d98383a2796eaca'), b: 456, d: 555 },
  { _id: ObjectId('655f42eb2d98383a2796eacb'), a: 123, h: 20 },
  { _id: ObjectId('655f46f12d98383a2796eacc'), a: 123, h: 20 },
  { _id: ObjectId('655f46f12d98383a2796eacd'), b: 456 },
  { _id: ObjectId('655f46f12d98383a2796eace'), outro: 'Documento' },
  { _id: ObjectId('655f48622d98383a2796eacf'), a: 123, h: 20 },
  { _id: ObjectId('655f48622d98383a2796ead0'), b: 456 },
  { _id: ObjectId('655f48622d98383a2796ead1'), c: 789 },
  { _id: ObjectId('655f48622d98383a2796ead2'), d: 28 },
  { _id: ObjectId('655f72793239c0a177a5c435'), b: 123 }
]
```

## Aula 29. D - Nos livrando de dados com deleteOne e deleteMany
- Método [delete](https://www.mongodb.com/docs/v7.0/crud/#delete-operations) é semelhante ao update, é melhora buscar 1º e depois executar o delete.
- [deleteOne](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.deleteOne/#syntax) tem somente o query/filter e apaga:
- db.crud.deleteOne({a: 123}) - apaga o 1º objeto com esse query/filter.
- [deleteMany](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.deleteMany/#syntax):
- db.crud.deleteMany({}) - apaga todos os registros da base.
- db.crud.deleteMany({b: 456}) - apaga todos os registros que correspondam a essa query/filter.
- db.crud.deleteMany({h: {$exists: true}}) - apaga todos os registros com base no exists.

# Seção 5 - Modelagem e Relacionamentos
- MongoDB não é um BD relacional, mas há formas de contornar essa situação.

## Aula 33. One To One | Um Para Um
- Num relacionamento **One To One - Estudante to Carteirinha**.
- Podemos fazer referência, semelhante em um BD relacional.
- Podemos usar **Embedded Document**:
```json
{
  "_id": 1,
  "cpf": "123456798",
  "nome": "Joãozinho",
  "cidade": "São Paulo",
  "carteirinha": {
    "_id": 8,
      "turma": "220A",
      "ra": 1245
  }
}
```
- **carteirinha** é o Embedded document.

## Aula 35. One To Many | Um Para Muitos
- Num relacionamento **One To Many - Setor To Funcionário**.
- Podemos fazer referência, semelhante em um BD relacional. 
- Podemos usar **Embedded Document**:
```json
{
  "_id": 3,
  "meta": 4000,
  "nome": "Vendas",
  "funcionarios": [
      {
          "_id": 8,
          "salario": 1400,
          "turno": "tarde",
          "cargo": "vendedor"
      },
      {
          "_id": 12,
          "salario": 1600,
          "turno": "noite",
          "cargo": "vendedor"
      }
  ]
} 
```

## Aula 37. Many To Many | Muitos Para Muitos | Tabela auxiliar
## Aula 38. Many To Many | Muitos Para Muitos | Referência
- Num relacionamento **Many To Many - Fornecedor To Produto**.
- Podemos fazer referência, semelhante em um BD relacional.
- Podemos usar **Arrays**:
```json
// Collection Fornecedores
{
  "_id": "f04",
  "cnpj": "165486984",
  "nome": "Fornecedor Legal",
  "cep": "1098465",
  "produto_ids": ["p16", "p21"]
}

{
  "_id": "f07",
  "cnpj": "98498151",
  "nome": "Fornecedor Maneiro",
  "cep": "198498",
  "produto_ids": ["p21", "p47"]
}
// Collection Produtos
{
  "_id": "p16",
  "descricao": "Panela",
  "preco": 45.50,
  "fornecedor_ids": ["f04"]
}

{
  "_id": "p21",
  "descricao": "Prato",
  "preco": 14,
  "fornecedor_ids": ["f04", "f07"]
} 

{
  "_id": "p47",
  "descricao": "Faqueiro",
  "preco": 127.46,
  "fornecedor_ids": ["f07"]
}
```

# Aula 40. Many To Many | Muitos Para Muitos | Híbrido de Referência com Embedded
- Mesmo relacionamento que a aula passada, mas na seguinte situação, dois fornecedores vendem o mesmo produto mas com valores diferentes:
```json
// Collection Fornecedores
{
  "_id": "f04",
  "cnpj": "165486984",
  "nome": "Fornecedor Legal",
  "cep": "1098465",
  "produtos": [
      {
          "_id": "p16",
          "preco": 46.50
      },
      {
          "_id": "p21",
          "preco": 12
      }
  ]
}

{
  "_id": "f07",
  "cnpj": "98498151",
  "nome": "Fornecedor Maneiro",
  "cep": "198498",
  "produtos": [
      {
          "_id": "p21",
          "preco": 16
      },
      {
          "_id": "p47",
          "preco": 127.46
      }
  ]
}


// Collection Produtos
{
  "_id": "p16",
  "descricao": "Panela",
  "fornecedores": [
      {
          "_id": "f04",
          "preco": 46.50
      }
  ]
}

{
  "_id": "p21",
  "descricao": "Prato",
  "fornecedores": [
      {
          "_id": "f04",
          "preco": 12
      },
      {
          "_id": "f07",
          "preco": 16
      }
  ]
} 

{
  "_id": "p47",
  "descricao": "Faqueiro",
  "fornecedores": [
      {
          "_id": "f07",
          "preco": 127.46
      }
  ]
}
```

# Aula 42. Duplicar ou não duplicar? Segredos e vantagens desse modelo
- Se tem mais leituras que updates/alteração é ideal usar a duplicação de dados, como visto na aula anterior.
- Se houver mais alterações do que leituras, é melhor não usar.