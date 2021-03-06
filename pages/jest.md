# JEST

Antes de falar sobre JEST que é uma ferramente que nos ajuda a realizar testes na nossa aplicação, precisamos entender primeiramente o que são testes e por quê é importante fazer em nosso ambiente.

Testes são funcionalidades executadas automaticamente para validar se o código está fazendo realmente aquilo que foi criado para fazer. Parece meio óbivio, pra mim também parecia, eu pensava: "como assim fazer um teste pra testar aquilo que eu to fazendo, se eu estou fazendo eu sei o que estou fazendo, não vou perder mais tempo criando mais código pra testar código" e é nesse momento que mora o perigo. Quando criamos a funcionalidade, no momento que recebemos a demanda, nos passam tudo que desejam que seja feita nessa funcionalidade (quase nunca passam isso completo...rs), aí você sabe exatamente o que fazer pois está fresco na sua memória, então você desenvolve cria a funcionalidade, até o momento tudo bem e você realmente fez o que tinha fazer, mas os dias não param, correto? Depois de 3 meses, as vezes até antes no meu caso, chega uma nova demanda e você começa a desenvolver novamente e de repente uma coisa que estava funcionando para de funcionar porquê você mexeu uma funcionalidade que também era usada em outro ponto e pior ainda, isso foi descoberto depois que a nova funcionalidade entrou em produção. Nesse momento é a hora que a gente tira o joio do trigo. Grande parte das vezes conseguimos arrumar, porém foram necessário muitas horas pra achar o problema, precisou de mais pessoas para atuar, deslocamentos de times etc. Tudo isso poderia ter sido evitado por uma simples rotina de teste automatizado, onde o sistema iria testar se com a nova implementação alguma outra rotina parou de funcionar.

Quando falamos de testes, não podemos deixar de falar sobre o TDD, se não ouviu ainda, pode ter certeza que chegará o momento e isso será mais breve do que pensa. Quando pensamos em desenvolvimento de código e principalmente quando temos uma equipe toda produzindo novas rotinas, novos códigos, pessoas diferentes, certamente você vai ouvir a seguinte frase: "Mas isso aqui estava funcionando", ou pior, "Isso funcionava antes", pois bem, o conceito de testes na nossa aplicação vem nos ajudar principalmente a diminuir os riscos de atualizações de código, pois quando atualizamos alguma coisa sabemos que existe uma validação se as funcionalidades do sistema ainda estão funcionando. TDD nos ajuda a ter códigos com menos riscos de falhas por alguma atualização.

TDD não é um tipo de testes, ele é uma metodológia de desenvolvimento de sistema que traz uma forma diferente se começar simplesmente saindo digitando código.

Como o próprio nome diz TDD - Test Driven Development, desenvolvimento dirigido por testes, ou seja, primeiramente criamos o teste para depois criar o código para passar nos testes.

Para mais informações sobre TDD, acesse [TDD um novo recomeço]().

# Tipos de Testes

Existem dois tipos de testes, testes unitários e testes integrados.

## Teste Unitário

São testes realizados em apenas um trecho do nosso sistema, uma única parte. Normalmente nos testes unitários testamos basicamente as regras de negócio, ou seja, testar uma funcionalidade.

## Teste Integrados

São testes realizado para testar o fluxo completo, desde a chamada no controller até o retorno do controller para a rota. Testas as chamadas das API externas como serviços de e-mail, conexões com banco de dados etc.


# Iniciando com JEST

Adiconar o pacote JEST como depedência de desenvolvimento

```shell
yarn add jest -D
```

Caso esteja usando typescript

```shell
yarn add @types/jest -D
```

Agora iniciamos o jest no projeto 

```shell
yarn jest --init 
```

Ele aparecer algumas perguntas para configurar o ambiente

> The following questions will help Jest to create a suitable configuration for your project

Would you like to use Jest when running "test" script in "package.json"? › (Y/n)
- Y

Would you like to use Typescript for the configuration file? › (y/N)
- Y

Choose the test environment that will be used for testing › - Use arrow-keys. Return to submit.
- <ins>**node**</ins>
- jsdom (browser-like)

Do you want Jest to add coverage reports? › (y/N)
- N

> Coverage reports é o relatório que mostra o quanto de testes foi realizado na aplicação, mostra também o percentual de cobertura dos testes

Which provider should be used to instrument code for coverage? › - Use arrow-keys. Return to submit.
- <ins>**v8**</ins>
- babel

Automatically clear mock calls and instances between every test? › (y/N)
- Y

Será gerado um arquivo de configuração do JEST na raiz do projeto com o nome **jest.config.ts**, com todas as informações o jest irá utilizar para realizar os testes nas aplicações.

Caso esteja trabalhando com typscript, instalar o novo pacote **ts-jest** como dependência de desenvolvimento.

```shell
yarn add ts-jest -D
```

Alterar as seguintes propriedades do arquivo **jest.config.ts**

- preset: alterar de undefined para ts-jest
- testMath: descomentar a propriedade
- bail: descomentar e atribuir o valor **true**

> Em cima de cada propriedade está informando o que cada um faz.


# Projeto Exemplo

Repositório [Helpers2-Jest](https://github.com/lucaslimas/helpers2-jest)


