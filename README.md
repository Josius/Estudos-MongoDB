# Estudos sobre MongoDB
[Link do Curso](https://www.udemy.com/course/mongodb-curso-completo/)

# Aula 8. Instalando no windows
- [Link](https://www.youtube.com/watch?v=gB6WLkSrtJk)
- **NOTA 1:** Desmarcar *Install MongoDB as a Service* para que ele não inicie junto com windows.
- **NOTA 2:** Criamos um diretório chamado **data** em *C:\\* e dentro de *data* criamos outro diretório chamado *db* (*C:\data\db*).
- **NOTA 3:** Configurar variável *Path* em variáveis de ambiente do windows, para poder acessar tanto o servidor quanto o cliente do servidor por linha de comando para poder ligar e acessar o mongdb por linha de comando, em qualquer diretório.
- 
- **Para Iniciar o MongoDB** 
  - Abrir um terminal e digitar **mongod** p/ligar o BD.
  - Abrir outro terminal e digitar **mongosh** p/acessar o BD pelo cliente em linha de comando.

# Aula 11. Conhecendo o comando Mongo
- mongod -> o daemon do mongo, o mongo em si, o BD.
- mongosh -> o cliente.
- mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+2.1.0 -> linha de conexão padrão com o BD.

# Aula 14. Inserindo nosso primeiro documento
- **db.\<tecla tab>** apresenta os comandos disponíveis.
- **db** escolhe o banco de dados em que estamos conectados.
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

# Aula 15. Resgatando dados inseridos
-  Com o BD selecionado:
-  **db.\<nome_da_collection>.find().pretty()** o qual retornará os objetos presentes no BD com uma formatação legível.
-  Retorna todos os objetos json com id gerado automaticamente pelo próprio MongoDB:
```json
[
  { _id: ObjectId('655ccb72ecc8dcd7608df3d5'), valor: 123 },
  { _id: ObjectId('655ccb81ecc8dcd7608df3d6'), valor: 123 }
] 
 ```

# Aula 16. Conceitos básicos: Database, Collection e Document
- **Database** - BD, a raiz a qual receberá as colletions.
- **Collection** - funcionam como diretórios. Um BD pode ter várias collections. São agrupados com algum sentido em comum. Uma collection precisa pertencer a um BD.
- **Document** - O objeto armazenado no BD. Entretanto um objeto é armazenado a uma collection.
- Então, um ou mais objetos pertencem a uma collection, e uma ou mais collection pertencem a um banco de dados.

# Aula 17. Criando nosso primeiro documento
# Aula 18. Entendendo os tipos de dados
# Aula 19. Inserindo o aluno na escola
# Aula 20. Destrinchando o ObjectID e entendendo seus benefícios
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

# Aula 21. Resgatando o Timestamp do ObjectID
- **db.alunos.findOne()._id** retorna somente o ObjectId.
- **db.alunos.findOne()._id.getTimestamp()** retorna a data de criação do objeto *ISODate('2023-11-21T17:16:56.000Z')*
- Seu timestamp é baseado no horário do cliente, não do servidor. 

# Aula 22. MongoDB como um processo independente
- **mongod --dbpath \<caminho-do-diretório-do-db> --fork** mais o comando **--logpath**(utiliza um arquivo próprio para armazenar os logs) ou **--syslog**(pega os logs do mongodb e coloca junto com os logs do SO [não funciona no windows]) 
- **--dbpath** não é obrigatório se houver criado o diretórios padrões em **C:\data\db**.
- **NOTA**: --fork somente funciona no windows se o mongodb houver sido iniciado como um [serviço do windows](https://www.mongodb.com/community/forums/t/how-to-use-fork-in-windows-os/12959/2).

# Aula 24. CRUD e sua aplicação com o MongoDB
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

# Aula 25. C - Inserindo dados com insertOne e insertMany
insert aceita:
- db.crud.insert({a: 123})
- db.crud.insert([{a: 123}, {b: 456}])
[insertOne](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.insertOne/#syntax) aceita somente:
- db.crud.insertOne({a: 123})
- [insertMany](https://www.mongodb.com/docs/v7.0/reference/method/db.collection.insertMany/#syntax) aceita:
- db.crud.insertMany([{a: 123}, {b: 456}, {c: 789}])
- db.crud.insertMany([{a: 123}, {b: 456}, {c: 789}, {d: 028}], {ordered: false}) - faz tratamento de erro. Por exemplo, se o **ordered** estiver **true**, ele adiciona cada objeto um por um em sequência, entretanto, se o objeto **b** houver algum erro (de validação, p.e.), os elementos **c** e **d** não serão inseridos. Já com **ordered** estiver **false**, ele continuará inserindo.