[Voltar](/Readme.md)

# Typescript

Typescript não é uma linguagem de programação, ele é um conjunto de funcionalidades que roda sobre o javascript.

# Node.js

Node.js é uma plataforma construída sobre o motor JavaScript do Google Chrome para facilmente construir aplicações de rede rápidas e escaláveis. Node.js usa um modelo de I/O direcionada a evento não bloqueante que o torna leve e eficiente, ideal para aplicações em tempo real com troca intensa de dados através de dispositivos distribuídos.

[Referência](http://nodebr.com/o-que-e-node-js/)

# Hello Word

Criar a pasta para o projeto e acessa-lá através da linha de comando. Executar o seguinte comando para iniciar o projeto node.js

```
yarn init -y
```

> O -y indica para criar os arquivos com os valores default
> Será criado o arquivo **package.json** com as informações do projeto.

Adicionar o pacote **express**

```
yarn add express
```

Na raiz do projeto criar a pasta **src** e dentro da pasta criar o arquivo **server.ts**.
> Arquivo server deverá estar com a extensão .ts

```ts
import express from 'express';

const server = express();

server.get('/', (req, res) => {
  res.send('Hello World!');
});

server.listen(3333);
```

Ao digitar o código acima, linha por linha, perceba que ao digitar a linha **server.get** no momento que digitou o ponto, ele não vai sugerir os métodos do express, MAS COMO ASSIM? NÃO TEM TIPAGEM? correto, não existe tipagem, isso porquê não foi instalado a tipagem do express, repare no código dentro do vscode, na linha da importação do express, após o **from** no inicio do nome **express** existem 3 pontinhos debaixo dele, colocando o mouse sobre eles, vai indicar que precisa instalar a tipagem do pacote

```
Could not find a declaration file for module 'express'. '/Users/lucaslima/Documents/codes/Estudos/starter/node_modules/express/index.js' implicitly has an 'any' type.
  Try `npm i --save-dev @types/express` if it exists or add a new declaration (.d.ts) file containing `declare module 'express';`ts(7016)
```

Na própria mensagem de aviso, ele exibe o possível comando para instalação da biblioteca de tipagem, porém está usando o npm. Repare que a instalar é feito como dependência de desenvolvimento.

```
yarn add @types/express -D
```

> Nem todos os pacotes precisam instalar a dependência de tipos, alguns possuem no próprio pacote. Não existe um certo e errado, porém os pacotes mais reconhecidos costumam fazer a parte

Com a instalação do pacote de tipagem, o vscode começa a interpretar tudo que existe dentro do express, assim após digitar o **ponto** depois do server, irá aparecer as sugestões de preenchimento "Tipagem".

## Rodando a aplicação

Para rodar a aplicação execute o comando tradicional do node

```cmd
node src/server.ts
```

Infelizmente o node **não reconhece o import e export**, é necessário converter o nosso arquivo typescript para um formato que ele entenda, seria basicamente converter o typescript para javascript. Para isso vamos instalar o pacote **typescript** como dependência de desenvolvimento.

```
yarn add typescript -D
```
Com o typescript instalado no projeto, é necessário iniciar o typescript, executando o seguinte comando:

```
yarn tsc --init
```

>tsc é um pacote que foi instalado junto do typescript, dentro da pasta node_modules -> bin

Na raiz do projeto será criado o arquivo **tsconfig.json** com todos as configurações que será usando pelo conversor de código de typescript para javascript.

Com os pacotes devidamente instalados podemos converter nosso código typescript utilizando o comando 

```
yarn tsc
```
Ao executar o comando, para cada arquivo **.ts** serão criados arquivos **.js** no msm diretório do arquivo typescript. 
Nesse momento podemos executar novamente o comando do node para executar o arquivo, porém apontando para o arquivo javascript
```
node src/server.js
```
Se a linha de comando ficar parada como se estive travado, significa que o servidor está rodando. Vamos alterar o arquivo para mostrar no terminal que ele está rodando, inserindo um console.log.

Altere o arquivo **server.ts** incluindo

```
server.listen(3333, () => {
  console.log('server is running...')
});
```

O arquivo final deverá ser algo como:

```ts
import express from 'express';

const server = express();

server.get('/', (req, res) => {
  res.send('Hello World!');
});

server.listen(3333, () => {
  console.log('server is running...')
});
```

Após alterar o arquivo typescript execute novamente o comando do node para executar o arquivo
```
node src/server.js
```
O terminal continuará da mesma forma que estava antes, não exibira o console log, isso porquê a alteração foi feita apenas no arquivo typescript, é necessário recompilar o arquivo para gerar o novo javascript com as atualizações.

```
yarn tsc
```

Nesse momento podemos executar o comando do node 

```
 node src/server.js
```

PRONTO!!!!!

Agora imagine sempre q alterar algum arquivo devemos converter o arquivo para javascript puro, é inviavél, nesse momento acabamos pensando no nodemon, pois assim que salvamos um arquivo, ele iria recompilar o arquivo e iniciar novamente, porém com typescript usando outra ferramenta para o monitoramento de atualizações, o pacote é o **ts-node-dev** e deverá ser usado apenas em desenvolvimento.

```
yarn add ts-node-dev -D
```

Criar o script no **package.json** para agilizar a execução do projeto
```json
{
  "scripts": {
    "dev": "ts-node-dev src/server.ts"
  }
}
```

> Referência [ts-node-dev npm](https://www.npmjs.com/package/ts-node-dev).

Execute o comando para iniciar a aplicação
```
yarn dev
```

A partir de agora qualquer alteração irá fazer o reload da aplicação.
O **ts-node-dev** irá converter todos os arquivos, e também irá fazer uma série de checagem nos arquivos, isso não apenas no seu arquivo, mas também no node_modules, nesse primeiro momento ele executará bem rápido pois estamos com poucos pacotes e arquivos no nosso projeto, mas isso pode ficar inviável com o tempo. Para agilizar o processo de "compilação", podemos passar diversos parametros para o ts-node-dev, como apenas fazer a compilação e não checagem já que o vscode realiza essa tarefa na própria interface. Para não fazer a checagem no arquivo usando o parametro **--transpile-only** e diversos outros:

```
ts-node-dev --inspect --transpile-only --ignore-watch node_modules --respawn  src/server.ts 
```

> Dúvidas sobre os parametros consulte a referência [ts-node-dev npm](https://www.npmjs.com/package/ts-node-dev).

## Alterando o output dos arquivos compilados

No momento que precisamos gerar uma distribuição, não queremos que os arquivos compilados fiquem no mesmo lugar dos arquivos typescript, pois a entrega da aplicação deverá ser apenas os arquivos javascript.
Para alterar o local dos arquivos gerados, deverá acessar o arquivo **tsconfig.json** na raiz do projeto, procurar por **outDir** e descomentar a linha. Porém dessa forma todos os arquivos serão gerados na raiz do projeto e se misturando com todo o resto. Para os arquivos não ficarem soltos na raiz do projeto alteramos o **outDir** indicando um pasta, por exemplo a pasta **dist**

```
"outDir":"./dist"
```
Assim ao executar o comando **yarn tsc** os arquivos serão criados na pasta **dist** na raiz do seu projeto.

Lembrando que para executar os arquivos compilados, deverá executar o comando do node e não mais do ts-node-dev

```
node dist/server.js
```


# Próximos Passos

- [Instalando ESLint](/pages/eslint_prettier.md)
- [Organizando os imports](/pages/import_organizer.md)
- [Controllers](/pages/controllers.md)