# Criando CLI Customizada

CLI são programas que rodam na linha de comando no terminal que executam tarefas em background e podem interagir com o usuário através do prompt de comando.

Um exemplo de CLI é o sequelize, atraves da linha de comando é possível criar uma estrutura para a criação de migrations

```shell
npx sequelize migration:create --name=minha-migration
```

Em muitos casos, diversas operações são realizadas no desenvolvimento de um sistema e muitas delas acabam sendo repetidas, por exemplo, criar um controller, um model, um arquivo de rotas ou até mesmo em reactjs, criar um página. 

O CLI permite que sejam criados um ou mais arquivos através de uma única linha código, dessa forma iniciar uma criação de um componente reactjs, não necessite o usuário criar a pasta do componente, o index dentro da pasta e o arquivo de estilos dentro da pasta. E a criação vai além de simplesmente criar um arquivo, ele pode ser montado com o código dentro dele variável, imagima com uma linha de código montar o model do seu projeto node? 

Criar um CLI aumenta a performace do desenvolvimento de qualquer aplicação.

# Criação de CLI

Utilizaremos uma biblioteca para auxiliar a criação de CLI chamdo **gluegun**

```shell
yarn global add gluegun
```

Acessa a local onde deseja criar a pasta do projeto e execute o comando

```shell
gluegun new NOME-CLI
```
> NOME-CLI é o nome que utilizado para a sua CLI

Informe o modelo do projeto do projeto como javascript

Nesse tutorial usaremos o nome **inno* para a CLI.

```shell
gluegun new inno
```

Which language would you like to use?

- TypeScript - Gives you a build pipeline out of the box (default)
- <ins>**Modern JavaScript - Node 8.2+ and ES2016+ (https://node.green/)**</ins>

Após a instalação abra a pasta do projeto criado com o vscode e no terminal digite

```shell
yarn link
```

Esse comando irá disponibilizar na linha de comando do vscode o novo CLI

Para testar o CLI, digite o nome da cli criada no terminal e pressione ENTER.

```shell
inno
```

Deverá aparecer a mensagem de boas vindas!

Por padrão a CLI criada já vem com alguns comando pré-definidos, para visualizar digite

```shell
inno -h
```

Irá aparecer a lista de comandos que pode ser executado, por exemplo, podemos ver a versão da CLI digitando

```shell
inno -v
```

## Estrutura dos arquivos

O gluegun por padrão, cria uma estrutura simples de arquivos dentro do projeto, o principal ponto que vamos trabalhar é na pasta **commands**, onde cada arquivo criado dentro dessa pasta significa um comando disponível na CLI, ou seja, a CLI nesse momento possuí dois comandos, generate e inno pois dentro da pasta commands só existem dois arquivos.

## Criando um comando personalizado

Inclua dentro da pasta commands um arquivo chamado **test.js**

```js
const command = {
  name: 'test',
  run: async toolbox => {
    const { print } = toolbox

    print.info('Olá eu sou um teste')
  }
}

module.exports = command
```

A estrura do comando é bem simples, você diz o nome do comando (normalmente erá o mesmo nome do arquivo, para ser fácil a manutenção da CLI) e qual o comando que deseja executar quando o comando for solicitado.

Para testar o comando digite

```shell
inno test
```

Deverá ser exibido no console a mensagem **Olá eu sou um teste**

## Adicionando Atalhos aos Comandos

Uma outra forma de executar os comandos é utilizando atalhos, ou seja, sem a necessidade digitar o nome do comando completo. Para isso vamos atribuir a propriedade **alias** no arquivo **test.js**

```js
const command = {
  name: 'test',
  alias: ['t'],
  run: async toolbox => {
    const { print } = toolbox

    print.info('Olá eu sou um teste')
  }
}

module.exports = command
```

Execute o comando

```shell
inno t
```

Deverá ser exibido no console a mensagem **Olá eu sou um teste**

Você também pode visualizar o novo comando criado digitando

```shell
inno -h
```

## Função RUN

Todo comando possuí uma função run dentro do arquivo, e esse método recebe a propriedade **toolbox**. Ela é responsável por interagir com o usuário e dentro dela existem diversas outras propriedades e métodos. Para ver os métodos disponíveis, acesse o [site da gluegun](https://infinitered.github.io/gluegun/#/toolbox-api).

## Parametros e Argumentos

Em geral comando podem receber parâmetros e argumentos no mesmo momento da sua execução, a diferença entre argumento e parâmetro está na forma de se passar valores para o comando. Paramtros são passados em uma sequência e não possuem nome (variável) para guardar o seu valor, já argumentos podemos atribuir um nome ao valor e não tem a necessidade de ter uma ordem de entrada.

Por exemplo, utilizando o mesmo comando de teste, vamos passar dois parâmetros e um argumento

```shell
inno test 2 4 --op=soma
```

Os números 2 e 4 são parâmetros, enquanto o --op é o argumento cujo nome do argumento é op e o valor dele é soma. Para ver como isso chega na CLI, vamos adicionar um console.log obtendo o valor de **parameters** de dentro do **toolbox**

```js
const command = {
  name: 'test',
  alias: ['t'],
  run: async toolbox => {
    const { print, parameters } = toolbox

    console.log(parameters);

    print.info('Olá eu sou um teste')
  }
}

module.exports = command
```

Execute o comando

```shell
inno t 2 4 --op=soma
```

No terminal aparecerá 

```js
{
  plugin: 'inno',
  command: 'test',
  array: [ 2, 4 ],
  options: { op: 'soma' },
  raw: [
    '/opt/homebrew/Cellar/node@12/12.22.3/bin/node',
    '/Users/lucaslima/.yarn/bin/inno',
    't',
    '2',
    '4',
    '--op=soma'
  ],
  argv: [
    '/opt/homebrew/Cellar/node@12/12.22.3/bin/node',
    '/Users/lucaslima/.yarn/bin/inno',
    't',
    '2',
    '4',
    '--op=soma'
  ],
  first: 2,
  second: 4,
  third: undefined,
  string: '2 4'
}
```

Os Parâmetros informados estão dentro da propriedade **array** e também dentro das propriedades **first** e **second**. Por padrão os 3 primeiros parâmetros, serão atribuídos também as propriedades first, second e third, a partir do terceiro o acesso aos valores serão apenas na propriedade **array**.

Os Argumentos estão na propriedade **options**, onde cada nome do argumento informado será uma propriedade de options.

## Criando Calculadora

Sabendo o que são parâmetros e argumentos, podemos criar um comando chamado **calculate** para realizar algumas operações matemáticas.

Criar o arquivo **calculate.js** dentro da pasta **commands**

```js
const command = {
  name: 'calculate',
  alias: ['calc'],
  run: async toolbox => {
    const { print, parameters } = toolbox

    const { first, second, options } = parameters

    const { op } = options

    let result = null

    switch (op) {
      case 'soma':
        result = first + second
        print.info(`${first} + ${second} = ${result}`)
        break

      case 'subt':
        result = first - second
        print.info(`${first} - ${second} = ${result}`)
        break

      case 'mult':
        result = first * second
        print.info(`${first} x ${second} = ${result}`)
        break

      case 'div':
        result = first / second
        print.info(`${first} / ${second} = ${result}`)
        break

      default:
        print.error('Operador não informado ou não encontrado')
        break
    }
  }
}

module.exports = command
```

Execute os comandos para testar a calculadora

- soma

```shell
inno calc 4 2 --op=soma
```

- subtração

```shell
inno calc 4 2 --op=subt
```

- divisão

```shell
inno calc 4 2 --op=div
```

- multiplicação

```shell
inno calc 4 2 --op=mult
```

Caso informe uma operação não definida, o CLI apresentará uma mensagem de erro

```shell
inno calc 4 2 --op=raiz
```

# Templates


# Publicando

Para publicar a CLI é necessário ter uma conta no NPM para subir o repositório.  

```shell
$ npm login
$ npm whoami
$ npm lint
$ npm test
(if typescript, run `npm run build` here)
$ npm publish
```




Referência [Rocketseat](https://www.youtube.com/watch?v=Rt-xG_VzD6M)