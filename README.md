# Estudos sobre MongoDB
[Curso Completo MongoDB 2023 [NoSQL do Básico ao Avançado!]](https://www.udemy.com/course/mongodb-curso-completo/)
- Link para o repositório oficial do [curso](https://github.com/renanpallin/udemy-mongodb).

# Seção 2 - Instalação
### Aula 8. Instalando no windows
- [Link](https://www.youtube.com/watch?v=gB6WLkSrtJk)
- **NOTA 1:** Desmarcar *Install MongoDB as a Service* para que ele não inicie junto com windows.
- **NOTA 2:** Criamos um diretório chamado **data** em *C:\\* e dentro de *data* criamos outro diretório chamado *db* (*C:\data\db*).
- **NOTA 3:** Configurar variável *Path* em variáveis de ambiente do windows, para poder acessar tanto o servidor quanto o cliente do servidor por linha de comando para poder ligar e acessar o mongdb por linha de comando, em qualquer diretório.
- 
- **Para Iniciar o MongoDB** 
  - Abrir um terminal e digitar **mongod** p/ligar o BD.
  - Abrir outro terminal e digitar **mongosh** p/acessar o BD pelo cliente em linha de comando.

# Seção 3 - Básico

### Aula 11. Conhecendo o comando Mongo
- mongod -> o daemon do mongo, o mongo em si, o BD.
- mongosh -> o cliente.
- mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.1.0 -> linha de conexão padrão com o BD.

### Aula 14. Inserindo nosso primeiro documento
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

### Aula 15. Resgatando dados inseridos
-  Com o BD selecionado:
-  **db.\<nome_da_collection>.find().pretty()** o qual retornará os objetos presentes no BD com uma formatação legível.
-  Retorna todos os objetos json com id gerado automaticamente pelo próprio MongoDB:
```json
[
  { _id: ObjectId('655ccb72ecc8dcd7608df3d5'), valor: 123 },
  { _id: ObjectId('655ccb81ecc8dcd7608df3d6'), valor: 123 }
] 
 ```

### Aula 16. Conceitos básicos: Database, Collection e Document
- **Database** - BD, a raiz a qual receberá as colletions.
- **Collection** - funcionam como diretórios. Um BD pode ter várias collections. São agrupados com algum sentido em comum. Uma collection precisa pertencer a um BD.
- **Document** - O objeto armazenado no BD. Entretanto um objeto é armazenado a uma collection.
- Então, um ou mais objetos pertencem a uma collection, e uma ou mais collection pertencem a um banco de dados.

### Aula 17. Criando nosso primeiro documento
### Aula 18. Entendendo os tipos de dados
### Aula 19. Inserindo o aluno na escola
### Aula 20. Destrinchando o ObjectID e entendendo seus benefícios
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

### Aula 21. Resgatando o Timestamp do ObjectID
- **db.alunos.findOne()._id** retorna somente o ObjectId.
- **db.alunos.findOne()._id.getTimestamp()** retorna a data de criação do objeto *ISODate('2023-11-21T17:16:56.000Z')*
- Seu timestamp é baseado no horário do cliente, não do servidor. 

# Seção 4 - CRUD

### Aula 22. MongoDB como um processo independente
- **mongod --dbpath \<caminho-do-diretório-do-db> --fork** mais o comando **--logpath**(utiliza um arquivo próprio para armazenar os logs) ou **--syslog**(pega os logs do mongodb e coloca junto com os logs do SO [não funciona no windows]) 
- **--dbpath** não é obrigatório se houver criado o diretórios padrões em **C:\data\db**.
- **NOTA**: --fork somente funciona no windows se o mongodb houver sido iniciado como um [serviço do windows](https://www.mongodb.com/community/forums/t/how-to-use-fork-in-windows-os/12959/2).

### Aula 24. CRUD e sua aplicação com o MongoDB
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

### Aula 25. C - Inserindo dados com insertOne e insertMany
insert aceita:
- db.crud.insert({a: 123})
- db.crud.insert([{a: 123}, {b: 456}])
[insertOne](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.insertOne/#syntax) aceita somente:
- db.crud.insertOne({a: 123})
- [insertMany](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.insertMany/#syntax) aceita:
- db.crud.insertMany([{a: 123}, {b: 456}, {c: 789}])
- db.crud.insertMany([{a: 123}, {b: 456}, {c: 789}, {d: 028}], {ordered: false}) - faz tratamento de erro. Por exemplo, se o **ordered** estiver **true**, ele adiciona cada objeto um por um em sequência, entretanto, se o objeto **b** houver algum erro (de validação, p.e.), os elementos **c** e **d** não serão inseridos. Já com **ordered** estiver **false**, ele continuará inserindo.

### Aula 26. Insert ordenado ou não-ordenado?
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

### Aula 27. R - Buscando dados com find e findOne
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

### Aula 28. U - Atualizando dados com update, updateOne e replaceOne
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

### Aula 29. D - Nos livrando de dados com deleteOne e deleteMany
- Método [delete](https://www.mongodb.com/docs/v7.0/crud/#delete-operations) é semelhante ao update, é melhora buscar 1º e depois executar o delete.
- [deleteOne](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.deleteOne/#syntax) tem somente o query/filter e apaga:
- db.crud.deleteOne({a: 123}) - apaga o 1º objeto com esse query/filter.
- [deleteMany](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.deleteMany/#syntax):
- db.crud.deleteMany({}) - apaga todos os registros da base.
- db.crud.deleteMany({b: 456}) - apaga todos os registros que correspondam a essa query/filter.
- db.crud.deleteMany({h: {$exists: true}}) - apaga todos os registros com base no exists.

# Seção 5 - Modelagem e Relacionamentos
- MongoDB não é um BD relacional, mas há formas de contornar essa situação.

### Aula 33. One To One | Um Para Um
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

### Aula 35. One To Many | Um Para Muitos
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

### Aula 37. Many To Many | Muitos Para Muitos | Tabela auxiliar
### Aula 38. Many To Many | Muitos Para Muitos | Referência
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

## Aula 40. Many To Many | Muitos Para Muitos | Híbrido de Referência com Embedded
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

## Aula 42. Duplicar ou não duplicar? Segredos e vantagens desse modelo
- Se tem mais leituras que updates/alteração é ideal usar a duplicação de dados, como visto na aula anterior.
- Se houver mais alterações do que leituras, é melhor não usar.
- Exemplo de dados duplicados na colllection de Fornecedores e Produtos, com relação ao produto:
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
            "descricao": "Panela",
            "preco": 46.50
        },
        {
            "_id": "p21",
            "descricao": "Prato",
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
            "descricao": "Prato",
            "preco": 16
        },
        {
            "_id": "p47",
            "descricao": "Faqueiro",
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
            "preco": 46.50,
            "cep": "1098465"
        }
    ],
    "peso": 4,
    "unidade_peso": "kg",
    "altura": 14,
    "largura": 15,
    "profundidade": 20
}
 
{
    "_id": "p21",
    "descricao": "Prato",
    "fornecedores": [
        {
            "_id": "f04",
            "preco": 12,
            "cep": "1098465"
        },
        {
            "_id": "f07",
            "preco": 16,
            "cep": "198498",
        }
    ]
} 
 
{
    "_id": "p47",
    "descricao": "Faqueiro",
    "fornecedores": [
        {
            "_id": "f07",
            "preco": 127.46,
            "cep": "198498",
        }
    ]
}
```

# Seção 6 - Schema e Validation

## Aula 44. Introdução ao createCollection
- Link de documentação do [createCollection](https://www.mongodb.com/docs/manual/reference/method/db.createCollection/).
- db.createCollection(name, [options](https://www.mongodb.com/docs/manual/reference/method/db.createCollection/#syntax))

## Aula 45. O problema da falta de validação
- Se acaso houver objetos no BD em que o tipo de dado de um sejam diferentes entre si, por evento de uma inserção pelo backend sem validação, por exemplo, em uma collection *cars*, repare no campo year:
```json
{
  "model": "Fusca",
  "madeBy": "Volkswagen",
  "year": "2012"
},
{
  "model": "Onix",
  "madeBy": "Chevrolet",
  "year": 2020
}
```
- O MongoDB aceita essa inserção, porém se ocorrer uma busca pelo ano (*db.cars.find({year: 2012})*) o Fusca não será encontrado a não ser que façamos uma busca pelo ano com string ao invés de inteiro.

## Aula 46. Como fazemos uma collection com validation
- Sobre [Schema Validation](https://www.mongodb.com/docs/manual/core/schema-validation/).
- Sobre [JSON Schema Validation](https://www.mongodb.com/docs/manual/core/schema-validation/specify-json-schema/#steps).
- Basicamente, criamos a collection com nome e no campo **options** passamos o **validator**, o qual tem em sua estrutura o **$jsonSchema**, o qual contém as propriedades para validações.

## Aula 47. Criando nossa collection carros
## Aula 48. Finalizando a validação da nossa collection
- bsonType: "object" - pq os documentos são objetos.
- required: ["model", "year"] - só aceita o documento se todas propriedades descritas neste vetor estiverem presentes no documento.
- properties: { campo1:{}, campo2:{} } - é o local onde configuramos os campos do documento para validação.
- [Palavras chaves](https://www.mongodb.com/docs/manual/reference/operator/query/jsonSchema/#available-keywords) que podem ser usadas com no **$jsonSchema**.
- Exemplo de um $jsonSchema:
```json
db.createCollection("cars", {
	validator: {
		$jsonSchema: {
			bsonType: "object",
			required: ["model", "year"],
			properties: {
				model: {
					bsonType: "string",
					description: "O modelo é necessário e deve ser do tipo string."
				},
				madeBy: {
					bsonType: "string",
					minLength: 3
				},
				year: {
					bsonType: "int",
					minimum: 1980,
					maximum: 2025
				}
			}
		}
	}
})
```

# Aula 49. Testando as validações que criamos
- db.cars.drop() - apaga a collection.
- Como o bsonType do campo year é int (ver exemplo aula anterior), a inserção não pode ser **year: 2022**, pois resultará em erro, pois no js o campo **year** é do tipo **number** (e number aceita ponto flutuante), não **int**. Logo, precisamos usar uma função para converter:
```json
db.cars.insertOne({ "model": "Fiesta",  "madeBy": "Ford",  "year": NumberInt(2015) })
db.cars.insertOne({ "model": "Fusca", "madeBy": "Volkswagen", "year": NumberInt(2012) })
db.cars.insertOne({ "model": "Onix",  "madeBy": "Chevrolet",  "year": NumberInt(2020) })
```

## Aula 51. Capped Collections | Funciona como uma fila!
[capped](https://www.mongodb.com/docs/manual/reference/method/db.createCollection/#syntax)

- No campo **options** podemos passar um **capped**, o qual faz a collection trabalhar como uma fila. 
- Digamos que estamos inserindo documentos diferentes por um longo tempo e por regra, os 1ºs documentos podem ser apagados, como por exemplo registros de logs.
- size: 2048 - indica o tamanho (em bytes) de armazenamento que queremos em disco. Size é obrigatório.
- max: 1000 - indica o nº máximo de documentos que a fila comporta, se entrar algum novo com a fila cheia, os mais antigos são retirados e inseridos os mais novos. Max não é obrigatório.
- A precedência é com base no size e depois o max, logo, com 3 docs com 1MB cada, alocamos o 1º e o 2º, ao inserir o 3º o 1º é apagado.
- Exemplo de capped:
```json
db.createCollection("logs", {
  capped: true,
  size: 2048,
  max: 5
})
```
- Inserções até estourar: **db.logs.insert({n: 1})** até **db.logs.insert({n: 6})**

# Seção 7 - Algumas preparações para as queries

## Aula 55. Conhecendo o repositório oficial do curso
- Link para o repositório oficial do [curso](https://github.com/renanpallin/udemy-mongodb).

## Aula 64. Importando dados com o mongoimport
- Dentro do diretório em que está o arquivo .csv ou .json com os dados a serem importados usar o comando:
- **mongoimport --db \<nome-do-banco-de-dados> --collection \<nome-da-collection> --type json --file \<nome-do-arquivo>.json --jsonArray**
- para apagar um BD, ao conectar o BD com use, usar o comando db.dropDatabase().

# Seção 8 - Queries! Vamos buscar nossos dados!

## Aula 65. Rodando nossa primeira query - encontrando os pokémons legendários + count
- Listar todos os pokémons legendários:
  - terminal: **db.pokemon.find({ legendary: true })**
  - compass: **{ legendary: true }**
- Retornar quantidade total de pokémons legendários:
  - terminal: **db.pokemon.find({ legendary: true }).count()**
  - terminal: **db.pokemon.countDocuments({ legendary: true })** - 
  - terminal: **db.pokemon.count({ legendary: true })** - DEPRECIADO

## Aula 66. Entendendo o it no cursor e utilizando o distinct para encontrar as geracoes pos
- Sobre **it**, ao executarmos uma busca, se a base de dados retornar uma quantidade muito grande, a quantidade retornada para visualizar é limitada tanto pelo compass quanto pelo terminal. Em ambos podemos alterar isso.
- No terminal, ao final da busca aparecerá a palavra 'it' para digitar e receber a próxima página.
- Sobre **distinct**, funciona exatamente como a clausúla de mesmo nome em BD relacionais, ele evitará de retornar dados não repetidos com base em um campo, p.e.: **db.pokemon.distinct("generation")**.

## Aula 67. Buscando por nome e utilizando regex
- Procurando por pokémon com as letras 'pik' em qualquer lugar do nome completo (semelhante a cláusula **LIKE** do SQL):
  - db.pokemon.find({ name: /Pik/ })
- Procurando por pokémon ao qual o nome começa com as letras 'pik':
  - db.pokemon.find({ name: /^Pik/ })
- Procurando por pokémon ao qual o nome termina com a letra 'a':
  - db.pokemon.find({ name: /a$/ })

## Aula 68. Entendendo o projection na pratica
- Executando uma busca e retornando somente o nome:
  - db.pokemon.find({ name: /^Pik/ }, { name: true })
- Para outros campos do documento, pode-se alocar true ou false (1 ou 0) para serem apresentados na resposta da busca.

## Aula 69. Aprendendo e utilizando flags no regex para case insensitive
- Regex é case sensitive
- Mas com o uso do 'i' ignoramos o case sensitive e tratamos as letras maiúsculas e minúsculas como iguais:
  - db.pokemon.find({ name: /^pik/i }, { name: true })

## Aula 70. Encontrando pokemons mais fortes com operadores de comparacao
- Buscando pokémon com ataque em determinado valor:
  - db.pokemon.find({ attack: 65 }, { name: 1, attack: 1 })
- Buscando pokémon com ataque maior ou igual a um determinado valor:
  - db.pokemon.find({ attack: { $gte: 85 } }, { name: 1, attack: 1 })

## Aula 71. Entendendo os operadores de comparacao disponiveis para nos
- Ver link para operadores de comparação do [MongoDB](https://www.mongodb.com/docs/manual/reference/operator/query-comparison/#comparison-query-operators).

## Aula 72. Buscando strings dentro de arrays
- Busca pokémons com "Fire" no campo types, sendo este um campo do tipo array:
  - db.pokemon.find({ types: "Fire" }, { name: 1, types: 1, _id: 0 })

## Aula 73. Selecionando uma faixa de valores para os arrays
- Com operador **$in** buscamos, em um campo do tipo array, documentos que coincidam com um OU outro tipo:
  - db.pokemon.find({ types: { $in: ["Fire", "Rock"]} }, { name: 1, types: 1, _id: 0 })
- Com operador **$nin** buscamos, em um campo do tipo array, documentos que NÃO coincidam com um OU outro tipo, ou seja, é a negação do anterior:
  - db.pokemon.find({ types: { $nin: ["Fire", "Rock"]} }, { name: 1, types: 1, _id: 0 })

## Aula 74. $in e $nin nao sao apenas para arrays
- O campo nome não é do tipo array, mas podemos usar **\$in** e **\$nin** na busca:
  - db.pokemon.find({ name: { $in: ["Pichu", "Pikachu"]} })
  - db.pokemon.find({ name: { $in: [/^Pi/, /^Pu/]} })

## Aula 75. Nao caia nessa pegadinha com o $in
- O operador **$in** não retorna uma faixa de valores quando executamos uma busca com dois parâmetros, pelo contrário, ele retorna todos os valores daqueles dois parâmetros, por exemplo, a busca abaixo não trará uma faixa de valores, e sim todos valores que correspondam a 80 e 90:
  - db.pokemon.find({ defense: { $in: [80, 90] }}, { _id:0, name:1, defense:1 })

# Seção 9: Queries - Combinando operadores e se aprofundando

## Aula 76. Buscando por faixa de valores combinando operadores
- Retornando uma faixa de valores de um campo:
  - db.pokemon.find({ defense: { $gt: 60, $lte: 72 } }, { _id: 0, name: 1, defense: 1 })

## Aula 77. Mesclando queries com operadores logicos $or
- Link para operadores lógicos do [MongoDB](https://www.mongodb.com/docs/manual/reference/operator/query-logical/#logical-query-operators).
- Buscando numa faixa de valores OU um outro valor específico, no caso, valores de defense entre 60 e 72 OU defense igual a 100:
  - db.pokemon.find({ $or: [ { defense: { $gte: 60, $lte: 72 } }, { defense: 100 } ] }, { _id: 0, name: 1, defense: 1 })

## Aula 78. Filtrando mais de um campo com o $or
- Buscando pokémons que tenham valores de **attack** maior que 80 OU **defense** com valor maior que 80:
  - db.pokemon.find({ $or: [ { defense: { $gte: 80 } }, { attack: { $gte: 80 } } ] }, { _id: 0, name: 1, defense: 1, attack: 1 })

## Aula 79. Encontrando pokemons fortes OU com muita defesa
- Buscando pokémons que tenham valores de **hp** E **attack** maiores que 80 OU **defense** E **speed** com valores maiores que 80:
  - db.pokemon.find({ $or: [ { defense: { $gte: 80 }, hp: { $gte: 80 } }, { attack: { $gte: 80 }, speed: { $gte: 80 } } ] })

## Aula 80. Aprendendo a ordenar os dados com o sort
- Para ordenar em ordem crescente, passamos o método [sort](https://www.mongodb.com/docs/manual/reference/method/cursor.sort/#cursor.sort--) com o dado a ser usado para ordenação com o valor 1, para decrescente, passamos com o valor -1: 
  - db.pokemon.find({ $or: [ { defense: { $gte: 80 }, hp: { $gte: 80 } }, { attack: { $gte: 80 }, speed: { $gte: 80 } } ] }, { _id: 0 }).sort({ hp: 1 })
- Também podemos passar mais de um parâmetro para executar a busca ordenada:
  - db.pokemon.find({ $or: [ { defense: { $gte: 80 }, hp: { $gte: 80 } }, { attack: { $gte: 80 }, speed: { $gte: 80 } } ] }, { _id: 0 }).sort({ hp: 1, defense: -1, attack: 1 })
- A ordem dos fields no sort importa.

## Aula 82. Buscando pelo tamanho com o $size e por valores com o $all
-  Com base no campo **types**, o qual é um vetor, podemos executar um busca com base no tamanho deste vetor, ou seja, o nº de elementos dentro dele:
  - db.pokemon.find({ types: { $size: 1 } }, { _id: 0, name: 1, types: 1 })
- Executando uma busca em o tipo retornado seja de um tipo E de outro tipo:

## Aula 83. Mais de uma query com o mesmo resultado
- Usando **$and** e recebendo mesmo resultado que $all:
  - db.pokemon.find( { $and: [ { types: "Flying" }, { types: "Bug" } ]}, { _id: 0, name: 1, types: 1 })
- As duas querys abaixo (com e sem **$and** são equivalentes):
  - db.pokemon.find( { $and: [ { types: "Flying" }, { legendary: true } ]}, { _id: 0, name: 1, types: 1, legendary: 1 })
  - db.pokemon.find( { types: "Flying", legendary: true }, { _id: 0, name: 1, types: 1, legendary: 1 })

## Aula 85. Criando paginacao com skip e limit
- Retornando uma lisat com 5 pokemons do tipo Fire mais fortes, no caso estamos buscando a página 2:
  - db.pokemon.find( {  types: "Fire" }, { _id: 0, name: 1, attack: 1 } ).sort( { attack: -1 } ).skip(5).limit(5)
- A paginação varia de acordom com o valor do limit, se limite for 5, então skip(0) é referente a pg 1, skip(5) pg 2, skip(10) pg 3, e assim sucessivamente.

## Aula 86. Preparando pokemon com embeded document
- Buscamos um pokemon, por exemplo:
  - db.pokemon.find({name: "Charmander"})
- Alteramos criando o embedded document **battle_points** via compass, pokemons com _id: 1 e _id: 5:
```json
{
  "_id": 5,
  "types": [
    "Fire"
  ],
  "name": "Charmander",
  "legendary": false,
  "battle_points": {
    "hp": 39,
    "attack": 52,
    "defense": 43,
    "speed": 65
  },
  "generation": 1
}
```

## Aula 87. Filtrando em embeded documents com o dot notation
- Podemos buscar dados com base se um campo existe ou não:
  - db.pokemon.find( { battle_points: { $exists: true } } )
- Vamos fazer uma query que buscará informações dentro do embedded document, no caso, dentro do campo **battle_points**, necessário usar aspas duplas e o ponto para acessar o campo do documento:
  - db.pokemon.find( { "battle_points.hp": { $lte: 40 } } )

# Seção 10 - Updates

## Aula 89. Adicionando campos com o $set
- Por padrão e para evitar falhas, geralmente começamos buscando o dado a ser alterado:
  - db.pokemon.find({ name: /^O/ }).pretty()
- Agora vamos adicionar um novo campo ao 1º documento que começa com 'O':
  - db.pokemon.updateOne({ name: /^O/ }, { $set: {startsWithO: true} })
- Receberemos uma resposta desse tipo:
```json
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}
```
- O campo **matchedCount** retorna a quantidade de documentos que corresponderam com a query (*name: /^O/*).
- O campor **modifiedCount** retorna a quantidade de documentos modificados com o update.
- Podemos também executar um *updateMany* para todos os documentos que começam com 'O':
  - db.pokemon.updateMany({ name: /^O/ }, { $set: {startsWithO: true} })
- E seu retorno são 6 documentos correspondetes e 5 documentos alterados:
```json
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 6,
  modifiedCount: 5,
  upsertedCount: 0
}
```
- O comando **$set** também funciona, não só como uma inserção de campos, mas também funciona como uma alteração de campos:
  - db.pokemon.updateMany({ name: /^O/ }, { $set: { name: "Alterando o nome original" } })

## Aula 90. Removendo campos com o $unset e entendendo ainda mais o $set
- Link para [Update Operators](https://www.mongodb.com/docs/manual/reference/operator/update/#update-operators-1).
- Agora iremos remover o campo que insermos na aula anterior de todos os documentos:
  - db.pokemon.updateMany({ name: /^O/ }, { $unset: {startsWithO:""} })
- Podemos passar o campo ao qual vamos excluir com uma string vazia ou qualquer outro valor, só não podemos passavar sem um valor (*$unset: {startsWithO}*) pois não será aceito.
- Também podemos excluir vários campos, somente separando por vírgula (*$unset: {campo1:valor, campo2:valor, campo3:valor}*).

## Aula 91. Incrementando valores com $inc e $mul
- Digamos que todos os pokémons do com o campo *types* correspondente a *Fire* ganharão +10 pontos no campo *attack*:
  - db.pokemon.updateMany({ types: "Fire" }, { $inc: { attack: 10 } })
- Com o comando **$inc** incrementamos, ou decrementamos o valor do campo informado.
  - db.pokemon.updateMany({ types: "Fire" }, { $inc: { attack: -10 } })
- Também podemos multiplicar ou dividir o valor de um campo com o comando **$mul**:
  - db.pokemon.updateMany({ types: "Fire" }, { $mul: { attack: 3 } })
  - db.pokemon.updateMany({ types: "Fire" }, { $mul: { attack: 1/3 } })

## Aula 92. Impondo limites aos documentos com $min e $max
- Comando **$min** atualiza um campo somente se o valor especificado para atualização for menor do que o valor que já existe no campo:
  - db.pokemon.updateMany({ types: "Fire" }, { $min: { attack: 150 } })
- Ou seja, se houver um valor acima do especificado pelo comando **$min**, ao executar o comando, o valor acima se tornará exatamente o valor do especificado. Outros abaixo não são alterados. Ou seja, $min é um limite superior.
- Com isso, vamos alocar um limite inferior com **$max** em toda a base:
  - db.pokemon.updateMany({}, { $max: { attack: 75 } })

## Aula 93. Dica sobre registrar updates nos documentos utilizando o $currentDate
- Para alterações de registros, usar:
  - CreatedAt - registra a data de criação do documento
  - UpdatedAt - registra a data de alteração do documento
- Vamos alterar o campo de um documento (nome) e adicionar um novo campo (currentDate):
  - db.pokemon.updateMany({ types: "Bug" }, { $set: { name: "É do tipo Bug" }, $currentDate: { updatedAt: true } })
  - db.pokemon.updateMany({ types: "Bug" }, { $set: { name: "É do tipo Bug" }, $currentDate: { updatedAt: { $type: "timestamp" } } })
- Há dois tipos de datas no mongoDB, uma é a **date**, a qual é a padrão e usada na 1ª opção, e o **timestamp**, a da 2ª opção.
- Timestamp é a contagem em milissegundo desde 1970.

## Aula 94. O grande segredo do upsert
## Aula 95. Regra para insert no upsert com o $setOnInsert
- Digamos que precisamos necessariamente de um certo documento na base, precisamos garantir que haja esse documento. Então, ao fazer uma busca, se houver o documento ok, do contrário, precisamos inserir.
- Para fazer isso, ao invés de usar dois comandos, um para buscar e outro para inserir, podemos usar um comando somente no update, no caso, upsert true:
  - db.pokemon.updateOne({ name: "Charmander" }, { $set: { attack: 150 } }, { upsert: true })
- Bom, com o caso acima o que temos é, se houver o documento, atualize o campo, do contrário, crie o documento. Mas veja que estamos passando somente um campo para a criação e o documento pode conter vários outros campos, logo precisamos de algo:
  - Se houver o documento, altere o campo.
  - Do contrário, crie um documento com todos os campos.
- Abaixo conseguimos isso, com o comando **$setOnInsert**:
  - db.pokemon.updateOne({ name: "Charmander Braquial" }, { $set: { attack: 150 }, $setOnInsert: { defense: 35, hp: 800, speed: 85 } }, { upsert: true })

## Aula 96. Podemos alterar o _id?
- Não é possível pois é um campo imutável.

# Seção 11 - Updates de Arrays

## Aula 97. Atualizando elementos dentro de arrays
- Com o comando **$set** conseguimos alterar o campo **types**, entretanto esse comando altera todo o vetor. O que buscamos agora é alterar o que está dentro do campo do vetor, um único dado. P.e., alterar o '**Poison'** para qualquer outro valor.
```json
{
  _id: 1,
  types: [ 'Grass', 'Poison' ],
  name: 'Bulbasaur',
  legendary: false,
  battle_points: { hp: 45, attack: 49, defense: 49, speed: 45 },
  generation: 1,
  attack: 75
}
```
- Na query precisamos apontar qual o campo e valor a ser alterado e em **$set** usamoas a *dotnotation* para alterar:
  - db.pokemon.updateOne({ _id:1, types: "Poison" }, { $set: { "types.$": "Lasanha" } })
- De outra forma, não passamos o campo e valor na query, mas passamos **'[]'** na *dotnotation*, o qual irá trocar todos os valores do vetor para o valor específicado:
  - db.pokemon.updateOne({ _id:1 }, { $set: { "types.$[]": "Pizza" } })

## Aula 98. Adicionando um elemento em um array com o $push
- Agora, ao invés de alterar os campos do vetor, vamos adicionar mais um campo/valor ao vetor. Neste caso, o comando **$push** adiciona somente um elemento ao vetor.
  - db.pokemon.updateOne({ _id: 1 }, { $push: { types: "Lasanha" } })

## Aula 99. Colocando mais de um elemento no array com o $each
- Para adicionar mais de um elemento ao vetor, no $push, em types, ao invés de passarmos um valor, passamos um objeto contendo o comando $each e um vetor de valores.
  - db.pokemon.updateOne({ _id: 1 }, { $push: { types: { $each:  ["Hotdog", "Hamburguer", "Sorvete"] } } })

## Aula 100. Controlando as inserções no array com o $position
- Por padrão, no mongoDB as inserções em vetores ocorrem no final do vetor, ou seja, um novo elemento ao ser adicionado ocupa a última posição do vetor.
- Podemos mudar esse padrão da seguinte forma:
  - db.pokemon.updateOne({ _id: 1 }, { $push: { types: { $each:  ["Esfiha"], $position: 2 } } })
  - db.pokemon.updateOne({ _id: 1 }, { $push: { types: { $each:  ["Tomilho", "Alface", "Beterraba"], $position: 1 } } })
- Ou seja, podemos usar a forma acima tanto para inserir um único elemento no vetor em qualquer posição especificada por **$position** quanto para um conjunto de elementos, iniciando na posição especificada por $position.
- Não conseguimos usar um $position sem usar um $each.

## Aula 101. Prevenindo insercoes duplicadas em arrays com o $addToSet
- Com os comandos informados anteriormente, inserções em vetores aceitam valores iguais.
- Para inserimos algo no vetor e verificar se aquele valor já está presente, e se estiver, não inserir, do contrário inserir, como um upsert, podemos fazer o seguinte:
  - db.pokemon.updateOne({ _id: 1 }, { $addToSet: { types: "Hamburger" } })
- O mesmo pode ser usado com $each:
  - db.pokemon.updateOne({ _id: 1 }, { $addToSet: { types: { $each: ["Hamburger", "Bolo", "Esfiha", "Mousse"] } } })

## Aula 102. Ordenando elementos dos arrays com o $sort
- Lembrando que $sort pode, por enquanto, assumir o valor de 1 (crescente/alfabética) ou -1 (decrescente):
- Neste caso, o comando $sort não funciona sem o operador $each:
  - db.pokemon.updateOne({ _id: 1 }, { $push: { types: { $each: ["Batata Frita"], $sort: 1 } } })
- Podemos passar também um vetor vazio de $each, se não quisermos adicionar nada.
  - db.pokemon.updateOne({ _id: 1 }, { $push: { types: { $each: [], $sort: -1 } } })

## Aula 103. Removendo parte do array com o $slice
- Com o comando **$slice** podemos delimitar uma faixa de elementos do vetor [0,N], e o que for superior a N será apagado, por exemplo, $slice: 7 manterá as 7 1ºs elementos do vetor, de 0 até 6.
  - db.pokemon.updateOne({ _id: 1 }, { $push: { types: { $each: [], $slice: 7 } } })
- Também podemos manter os 3 últimos elementos do vetor, passando um número negativo, o qual inverte a ordem de contagem dos elementos:
  - db.pokemon.updateOne({ _id: 1 }, { $push: { types: { $each: [], $slice: -3 } } })

## Aula 104. Tratando arrays como filas e listas com o $pop
- Removendo UM ÚNICO elemento do vetor com o comando **$pop**, com o valor 1 removemos o último elemento, com -1 removemos o 1º elemento. Esse comando não remove mais que um único elemento, como numa fila/pilha.
  - db.pokemon.updateOne({ _id: 1 }, { $pop: { types: -1 } })

## Aula 105. Removendo elementos de um arrays através de uma query com o $pull
- O comando **$pull** remove o que for definido através de uma query.
- Removendo um único elemento:
  - db.pokemon.updateOne({ _id: 1 }, { $pull: { types: "Esfiha" } })
- Removendo elementos que correspondam a query presente no campo **$in**. Se houver mais de um elemento com esse valor, eles serão removidos:
  - db.pokemon.updateOne({ _id: 1 }, { $pull: { types: { $in: ["Esfiha", "Hamburger"] } } })

## Aula 106. Removendo elementos de um array através de uma lista com o $pullAll
- O comando **$pullAll** remove todos de uma lista. Ele não trabalha com query.
- Passamos a lista, e se algum elemento da lista corresponder com os elementos do BD, o elemento do BD será removido.
  - db.pokemon.updateOne({ _id: 1 }, { $pullAll: { types: ["Esfiha", "Hamburger"] } })
- Funciona como o pull da aula passsada, onde usamos o $in, entretanto, com $in, podemos fazer coisas mais complexas, como usar operadores, por exemplo operadores de comparação. 
- Se for algo mais simples, sem necessitar de operadores, podemos usar o pullAll.

## Aula 107. Caprichando no types agora sera um array de documentos
- Alterando o campo **types** para algo mais intrincado:
  - db.pokemon.updateOne({ _id: 1 }, { $set: { types: [ { name: "Fire", bonus_points: 45, weakness: "Water" }, { name: "Rock", bonus_points: 12, weakness: "Paper" }, { name: "Bug", bonus_points: 14, weakness: "Chinelão" } ] } })

## Aula 109. Como ficariam os comandos num array de documentos
- O documento vai ter a seguinte estrutura após a alteração da aula passada, usando a técnica do **embedded document**:
```json
{
  _id: 1,
  types: [
    { name: 'Fire', bonus_points: 45, weakness: 'Water' },
    { name: 'Rock', bonus_points: 12, weakness: 'Paper' },
    { name: 'Bug', bonus_points: 14, weakness: 'Chinelão' }
  ],
  name: 'Bulbasaur',
  legendary: false,
  battle_points: { hp: 45, attack: 49, defense: 49, speed: 45 },
  generation: 1,
  attack: 75
}
```
- Logo, se fossemos usar um operação de **slice** sobre esse novo campo **types**, onde queremos somente os 2 1ºs, não faz diferença o tipo que temos no vetor, funciona da mesma maneira. Inclusive, operações de **push** também seriam da mesma forma, mas ao invés de uma lista, usariamos o objeto:
  - db.pokemon.updateOne({ _id: 1 }, { $push: { types: { $each: [{ name: 'value', bonus_points: value, weakness: 'value' }], $position: 2 } } })
- Agora, para atualizar um valor que está dentro do objeto:
  - db.pokemon.updateOne({ _id: 1, "types.name": "Fire" }, { $set: { "types.$.bonus_points": 18 } })
  - No caso acima, o que ocorre é, busque um pokémon, de *id=1*, e dentro do seu campo *types*, procure um objeto que tenha o campo *name-Fire*. Ao encontrar o documento que procuramos, podemos alterar os campos necessários, conforme acima, usando *dotnotation*.
- E esse sistema também funciona se quisermos adicionar um novo campo ao(s) objeto(s) dentro do campo **types**, conforme aprendemos em aulas anteriores.P.e.:
- Ou remover esse NOVO_CAMPO
  - db.pokemon.updateOne({ _id: 1, "types.name": "Fire" }, { $unset: { "types.$.NOVO_CAMPO": true } })

## Aula 110. Ordenando o array de documentos
- Se quisermos ordernar os objetos dentro do campo **types**, p.e., pelo bonus_points:
  - db.pokemon.updateOne( { _id: 1 }, { $push: { types: { $each: [], $sort: { bonus_points: 1 } } } } )
  - E podemos usar outros campos para melhor ordenação, como visto anteriormente.

## Aula 111. Podemos fazer exatamente o que quisermos com nested arrays?
- MongoDB não suporta updates de vetores acima do 1º nível, logo não conseguiremos usar o placeholder ($) para acessar o campo de um vetor de 2º nível. O update abaixo:
  - db.pokemon.updateOne( { _id: 1, "types.name": "Fire" }, { $set: { "types.$.strong_against": ["Ice", "Bug", "Poison"] } } )
- Gerará o seguinte documento:
```json
{
  _id: 1,
  types: [
    { name: 'Rock', bonus_points: 12, weakness: 'Paper' },
    { name: 'Bug', bonus_points: 14, weakness: 'Chinelão' },
    {
      name: 'Fire',
      bonus_points: 18,
      weakness: 'Water',
      strong_against: [ 'Ice', 'Bug', 'Poison' ]
    }
  ],
  name: 'Bulbasaur',
  legendary: false,
  battle_points: { hp: 45, attack: 49, defense: 49, speed: 45 },
  generation: 1,
  attack: 75
}
```
- E se quisermos adicionar/remover mais um elemento ao campo **strong_against** não conseguiremos, pois ele é um vetor dentro de outro vetor, logo, ele é de 2º nível.
- Para adicionar/remover um elemento desse vetor de 2º nvl, poderíamos:
  - db.pokemon.updateOne( { _id: 1, "types.name": "Fire" }, { $set: { "types.$.strong_against": ["Ice", "Bug", "Poison", "Rock"] } } )
  - OU:
  - db.pokemon.updateOne( { _id: 1, "types.name": "Fire" }, { $set: { "types.$.strong_against": ["Ice", "Poison"] } } )

## Aula 112. Conhecendo dois metodos underground findOneAndUpdate e o findAndModify
- **findOneAndUpdate** trabalha como o updateOne, mas o retorno é diferente. 
  - db.pokemon.findOneAndUpdate( { _id: 256 }, { $set: { using_find_one_and_update: true } } )
- Ele retorna o documento mas sem o campo recém inserido. Ou seja, ele retorna o documento antigo. 
- Se quiser que retorne o documento atualizado precisamos a flag **returnNewDocument**:
  - db.pokemon.findOneAndUpdate( { _id: 258 }, { $set: { using_find_one_and_update: true } }, { returnNewDocument: true } )
- Com **findAndModify** podemos usar ele para executar uma alteração ou para remover um elemento.
- Fazemos o update de uma forma mais verbosa:
  - db.pokemon.findAndModify({ query: { _id: 546 }, update: { $set: { using_find_one_and_update: true } } })
- O retorno é igual ao findOneAndUpdate, para retornar o novo documento:
  - db.pokemon.findAndModify({ query: { _id: 548 }, update: { $set: { using_find_one_and_update: true } }, new: true })
- Também podemos usar um remoção com:
  - db.pokemon.findAndModify({ query: { _id: 548 }, remove: true })

# Seção 12 - Indexes

## Aulas 113 até 11
- Quando executamos uma busca restrita com **find** na base de dados, por exemplo *db.pokemon.find({ name: "Charmander" })*, se houver 1 milhão de documentos e houver somente 1 com o nome buscado, ele verificará todos os documentos. Mas se houver mais de 1, ele retornará todos os documentos com aquele nome. E se não houver nenhum, ele fará a procura em toda a base de dados da mesma forma, ou seja, ele fará uma busca sequencial até o último dado da base.
- Neste caso, o ideal é, se estamos buscando algo bem específico e único, usamos **findOne**, pois assim que ele encontrar o que buscamos, ele interromperá a leitura dos dados, ou seja, ele não fará a busca por toda a base de dados, dessa forma evitando processamento desnecessário. Ou seja, efetua uma busca sequencial e quando encontrar o dado, interrompe a busca.
- Uma forma de melhorar a busca é ordenar física os dados com base em um parâmetro, entretanto não é a melhor solução, pois essa ordenação implica na inserção ordenada, pois se quisermos inserir um dado que ficará nos entremeios da base de dados, será necessário mover todos os dados que vem depois da posição dessa inserção uma posição à frente. 
- **Aula 117** - Idexação é a ordenação em camada lógica, onde um campo aponta para todos os documentos que possuem este campo.
- **Aula 118** - Explicação de busca binária - precisa de documentos ordenados.

## Aula 120. Confirmando nossas hipóteses com o explain 
- Comando **explain** permite visualizar parâmetros que ocorreram na execução da busca. 
  - pokemon_center> db.pokemon.find({attack:{$gte:85}}).explain(). 
- Dentre toda a estrutura retornada, temos dois campos interessantes, **winningPlan** e **rejectedPlans**.
- **winningPlan** - é a melhor execução definida pelo mongoDB para efetuar a busca.
  - Em *winningPlan* também temos um campo importante chamado **stage: 'COLLSCAN'**, o qual, no caso, informa que houve um busca sequencial, passando linha por linha. Se fosse uma busca por id, teríamos **stage: 'IDHACK'**, o qual vai direto no id.
- **rejectedPlans** - é a lista de execuções rejeitadas pelo mongDB para efetuar a busca. 

## Aula 121. Entendendo bem como o explain funciona e pode nos ajudar
- Com **explain** podemos passar um parâmetro chamado **executionStats** que traz vários outros parâmetros relacionados a execução, como sucesso, nº de documentos retornados(*nReturned*), tempo de execução, total de ids examinados, total de documentos examinados (*totalDocsExamined*), etc. 
- O ideal é que o valor em **totalDocsExamined** seja igual ao valor em **nReturned**, pensando no menor valor possível.

## 122. Criando nosso primeiro index
- O mongoDB cria um index padrão com base nos **_id** dos documentos.
- Com base na seguinte busca, a qual faz um busca sequencial em todos os documentos (veja *totalDocsExamined* do retorno da busca):
  - db.pokemon.find({ name: "Umbreon" }).explain('executionStats')
- Vamos criar um index para melhorar essa busca:
  - db.pokemon.createIndex({ name: 1 })
  - index pode ser crescente ou decrescente
- Após isso, podemos verificar os indexes presentes com:
  - db.pokemon.getIndexes()
- Que retorna:
```json
[
  { v: 2, key: { _id: 1 }, name: '_id_' },
  { v: 2, key: { name: 1 }, name: 'name_1' }
]
```
- Podemos mudar o nome do index com:
  - pokemon_center> db.pokemon.createIndex({ name: 1 }, { name: "meu_index"  })

## Aula 123. Como ficou nossa query agora com indexes
- Após a criação do index, ao executarmos:
  - db.pokemon.find({ name: "Umbreon" }).explain('executionStats')
- O retorno é mais elaborado por conta do index, inclusive:
  - Em **inputStage** -> **stage: 'ixseek'**
- Alterou-se para a busca por index.
- Um outro ponto é em **executionStats**, onde **totalKeysExamined** agora é 1, no caso, a chave criada para o index:
```json
executionStats: {
    executionSuccess: true,
    nReturned: 1,
    executionTimeMillis: 29,
    totalKeysExamined: 1,
    totalDocsExamined: 1,
  ...
```

## Aula 124. Indexando o campo de attack e verificando as alteracoes
- Agora, a seguinte busca e seu retorno:
  -  db.pokemon.find({attack:{$gte:85}}).explain("executionStats")
```json
...
  executionStats: {
    executionSuccess: true,
    nReturned: 294,
    executionTimeMillis: 2,
    totalKeysExamined: 0,
    totalDocsExamined: 801,
    executionStages: {
      stage: 'filter',
      planNodeId: 1,
      nReturned: 294,
      executionTimeMillisEstimate: 0,
...
```
- Criando um index para o campo *attack*:
  - db.pokemon.createIndex({ attack: 1 }, { name: "index_attack_1" })
  - db.pokemon.getIndexes()
```json
[
  { v: 2, key: { _id: 1 }, name: '_id_' },
  { v: 2, key: { name: 1 }, name: 'name_1' },
  { v: 2, key: { attack: 1 }, name: 'index_attack_1' }
]
```
  - db.pokemon.find({attack:{$gte:85}}).explain("executionStats")
```json
...
    winningPlan: {
      queryPlan: {
        stage: 'FETCH',
        planNodeId: 2,
        inputStage: {
          stage: 'IXSCAN',
          planNodeId: 1,
          keyPattern: { attack: 1 },
          indexName: 'index_attack_1',
          isMultiKey: false,
          multiKeyPaths: { attack: [] },
          isUnique: false,
          isSparse: false,
          isPartial: false,
          indexVersion: 2,
          direction: 'forward',
          indexBounds: { attack: [ '[85, inf.0]' ] }
        }
...
  executionStats: {
    executionSuccess: true,
    nReturned: 294,
    executionTimeMillis: 2,
    totalKeysExamined: 294,
    totalDocsExamined: 294,
...
  command: {
    find: 'pokemon',
    filter: { attack: { '$gte': 85 } },
    '$db': 'pokemon_center'
  }
...
```

## Aula 125. Entendendo como e executada uma query um pouco mais complexa
- E como funcionaria com regex:
  - db.pokemon.find({ name: /^R/}).explain("executionStats")
```json
...
executionStats: {
  executionSuccess: true,
    nReturned: 25,
    executionTimeMillis: 3,
    totalKeysExamined: 26,
    totalDocsExamined: 25
...
```
  - db.pokemon.find({ name: /^R/, attack: { $gte: 70 } }).explain("executionStats")
```json
 ...
 rejectedPlans: [
      {
        queryPlan: {
          stage: 'FETCH',
          planNodeId: 2,
          filter: { name: { '$regex': '^R' } },
          inputStage: {
            stage: 'IXSCAN',
            planNodeId: 1,
            keyPattern: { attack: 1 },
            indexName: 'index_attack_1',
            isMultiKey: false,
            multiKeyPaths: { attack: [] },
            isUnique: false,
            isSparse: false,
            isPartial: false,
            indexVersion: 2,
            direction: 'forward',
            indexBounds: { attack: [ '[70, inf.0]' ] }
          }
...
```

## Aula 126. Compound indexes - indexes com mais de um field
- Queremos criar um index por dois campos (name e attack) para criar uma busca mais complexa, por exemplo, esta busca:
  - db.pokemon.find({ name: /^R/, attack: { $gte: 100 } }).explain("executionStats")
- Assim como no *sort*, a ordem dos indícies fazem sentido.
- Ele pega os nomes, ordena de maneira ascendente, e se houver empate, desempatará com base no attack, de maneira descendente. O 2º comando cria um index com a ordem dos indícies ao contrário:
  - db.pokemon.createIndex({ name: 1, attack: -1 })
  - db.pokemon.createIndex({ attack: 1, name: 1 })
- E temos os seguintes indícies:
```json
[
  { v: 2, key: { _id: 1 }, name: '_id_' },
  { v: 2, key: { name: 1 }, name: 'name_1' },
  { v: 2, key: { attack: 1 }, name: 'index_attack_1' },
  { v: 2, key: { name: 1, attack: -1 }, name: 'name_1_attack_-1' },
  { v: 2, key: { attack: 1, name: 1 }, name: 'attack_1_name_1' }
]
```
- Então, pelo campo **winningPlan** temos o index utilizado em **indexName: 'name_1_attack_-1'**, o qual foi usado na busca dentre os indícies disponíveis.
- Pelo campo **rejectedPlans** temos **indexName: 'name_1'**, **indexName: 'index_attack_1'**, **indexName: 'attack_1_name_1'**, os quais são os indícies que foram avaliados a serem usados, mas rejeitados para a busca.
- E no campo **executionStats**:
```json
...
 executionStats: {
    executionSuccess: true,
    nReturned: 6,
    executionTimeMillis: 7,
    totalKeysExamined: 26,
    totalDocsExamined: 6,
...
```

## Aula 127. Sugerindo o index para o mongo db utilizar
- Como o mongoDB escolhe por si próprio qual index utilizar, com o comando **hint** podemos indicar para ele testar um index. E neste caso, ele usará o index sugerido além de testar e mostrar um log de como ele foi utilizado.
- Precisa necessariamente ser antes do explain e usamos as chaves do index na ordem que está nele:
  - db.pokemon.find({ name: /^R/, attack: { $gte: 100 } }) .hint({ attack: 1, name: 1 }).explain("executionStats")
- Em seu retorno não há o campo *rejectedPlans*.
- Seu *executionStats* temos 30 chaves examinadas a mais do que com o index escolhido por ele mesmo na aula anterior:
```json
...
executionStats: {
  executionSuccess: true,
  nReturned: 6,
  executionTimeMillis: 1,
  totalKeysExamined: 56,
  totalDocsExamined: 6
...
```

## Aula 128. Excluindo indexes desnecessarios
- Apagamos indícies com o comando abaixo. Aceita somente um nome por vez.
  - db.pokemon.dropIndex("NOME-DO-INDEX")

## Aula 129. Compondo indexes com elegancia
- Digamos que tenhamos uma busca muito usada com base em um ataque elevado e uma defesa baixa.:
  - db.pokemon.find({ attack: { $gte: 85 }, defense: { $lte: 50 } }).explain("executionStats")
  - No caso, usando um *count()*, temos 25 documentos.
- Porém, ao usarmos o *explain("executionStats")* temos **nReturned: 25**. Mas em contrapartida temos **totalDocsExamined: 294** e **totalKeysExamined: 294**.
- Isso ocorre porque, temos 294 documentos com o campo attack=50 (*db.pokemon.find({ attack: { $gte: 85 } }).count()*) entretanto, o campo **defense** não está sendo coberto por nenhum index, por isso a discrepância entre docs retornados e examinados. 
- E neste caso, é necessário examinar a *defense* uma a uma.
- Este é um forte motivo para criar um **compounding index**, como feito anteriormente.
- Relembrando, queremos ataque alto e defesa baixa, logo:
  - db.pokemon.createIndex({ attack: 1, defense: -1 }, { name: "ataque_alto_defesa_baixa"})
- E executando a mesma busca de antes, temos os seguintes dados no campo *executionStats*: 
```json
executionStats: {
  executionSuccess: true,
  nReturned: 25,
  executionTimeMillis: 3,
  totalKeysExamined: 74,
  totalDocsExamined: 25,
```

## Aula 130. Nos livrando de indexes redundantes