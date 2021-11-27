# ESLint e Prettier

São responsavéis pela **padronização do código** dentro projeto, isso porquê o javascript permite que seja utilizado no código diversas formas de escritas, como por exemplo, string podem ser aspas duplas ou aspas simples, final de limha pode ter ou não ponto e vírgula, o salto de linha, etc. Com Eslint podemos automatizar e facilitar o padrão de código do projeto, assim todos os desenvolvedores irão escrever códigos com as mesmas configurações de escrita.

Referência [Rocketseat](https://www.notion.so/ESLint-e-Prettier-Trilha-Node-js-d3f3ef576e7f45dfbbde5c25fa662779#eaf6e8bdcabc4d809cdae302e29750da)

- ESLint: Responsável por identificar padrões de código em desacordo com as regras pré-estabelecidas.
- Prettier: Responsável por formatar o código de acordo com essas regras.

## Instalando extensões
Antes de configurar o eslint no projeto, é necessário informar ao vscode para que ele utilize as configurações do eslint. Para isso vamos instalar a extensão **eslint** no vscode. 
[Clique aqui](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) para fazer a instalação no vscode.

## Configurando VSCode

Outra configuração importe a ser feita, é fazer com que o vscode sempre formate o código no momento que salvar um arquivo. 
Acesse as configurações do vscode, no mac command + shift + p, pesquise por **Open Settings (json)** e abre o arquivo. Inserir as linhas a seguir dentro do arquivo:

```json
"editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
```

# Inicio

Para executar os procedimentos a seguir utilizaremos o repositório utilizado anteriormente na documentação [Iniciando projeto node com typescript](https://github.com/lucaslimas/helpers2/blob/main/pages/start.md) .

O código base para utilização das tarefas a seguir foi utilizado o repositório [helpers2-starter](https://github.com/lucaslimas/helpers2-starter);

## ESLint

Para usar o eslint no projeto, devemos instalar o pacote do eslint em modo de desenvolvimento

```shell
yarn add eslint -D
```

Com o pacote instalado, podemos iniciar o eslint

```shell
yarn eslint --init
```

Quando iniciar o eslint, no console do vscode irá aparecer uma série de perguntas, cada pergunta significa algum tipo de configuração que o eslint irá utilizar no código do projeto.

Abaixo as respostas utilizadas nesse tutorial:

1 - How would you like to use ESLint? … 

  - To check syntax only
  - To check syntax and find problems**
  - <ins>**To check syntax, find problems, and enforce code style**</ins>

2 - What type of modules does your project use?

- <ins>**JavaScript modules (import/export)**</ins>
- CommonJS (require/exports)
- None of these

3 - Which framework does your project use?

- React
- Vue.js
- <ins>**None of these**</ins>

4 - Does your project use TypeScript? 

- No
- <ins>**Yes**</ins>

5 - Where does your code run? …  (Press space to select or unselect options)

- Browser
- <ins>**Node**</ins>

> Marque apenas o node

6 - How would you like to define a style for your project?

- Inspect your JavaScript file(s)
- <ins>**Use a popular style guide**</ins>
- Answer questions about your style

7 - Which style guide do you want to follow?

- <ins>**Airbnb: https://github.com/airbnb/javascript**</ins>
- Standard: https://github.com/standard/standard
- Google: https://github.com/google/eslint-config-google
- XO: https://github.com/xojs/eslint-config-xo

8 - What format do you want your config file to be in?

- JavaScript
- YAML
- <ins>**JSON**</ins>

9 - Would you like to install them now with npm? 

- No 
- <ins>**Yes**</ins>

> Mesmo se estamos utilizando yarn como gerenciador de pacote, no momento vamos deixar ele instalar usando o npm

Após a instalação dos pacotes, vamos deletar o arquivo **package-lock.json** que foi gerado na raiz do projeto pela instalação usando npm e refazer a instalação usando o **yarn**

```shell
yarn 
```

Instalar o plugin **import helpers** que irá auxiliar e organizar a ordem dos imports dentro dos arquivos e o plugin **import resolver** que permitirá importar arquivos typescript sem passar a extensão do arquivo. 

```shell
yarn add -D eslint-plugin-import-helpers eslint-import-resolver-typescript
```

Com as dependências devidamente instaladas, adicione o arquivo **.eslintignore** na raiz do projeto

```
/*.js
node_modules
dist
```

Acesse o arquivo **.eslintrc.json** e altere as seguintes propriedades:

- dentro a propridade **parseOptions**, altere a propriedade **ecmaVersion** para uma versão mais nova, nesse tutorial usaremos a versão **2021**
```json
"ecmaVersion": 2021,
``` 

- incluir na propriedade **env**

```json
"jest": true
```

- incluir na propriedade **extends**

```json
"plugin:@typescript-eslint/recommended"
```

- incluir na propriedade **plugins**

```json
"eslint-plugin-import-helpers"
```

- incluir na propriedade **rules**

```json
"camelcase": "off",
"import/no-unresolved": "error",
"@typescript-eslint/naming-convention": [
  "error",
  {
    "selector": "interface",
    "format": ["PascalCase"],
    "custom": {
      "regex": "^I[A-Z]",
      "match": true
    }
  }
],
"class-methods-use-this": "off",
"import/prefer-default-export": "off",
"no-shadow": "off",
"no-console": "off",
"no-useless-constructor": "off",
"no-empty-function": "off",
"lines-between-class-members": "off",
"import/extensions": [
  "error",
  "ignorePackages",
  {
    "ts": "never"
  }
],
"import-helpers/order-imports": [
  "warn",
  {
    "newlinesBetween": "always",
    "groups": ["module", "/^@shared/", ["parent", "sibling", "index"]],
    "alphabetize": { "order": "asc", "ignoreCase": true }
  }
],
"import/no-extraneous-dependencies": [
  "error",
  { "devDependencies": ["**/*.spec.js"] }
]
```

- Adicione no final do arquivo

```json
"settings": {
    "import/resolver": {
      "typescript": {}
    }
  }
```

### Arquivo final 

```json
{
    "env": {
        "es2021": true,
        "node": true,
        "jest": true
    },
    "extends": [
        "airbnb-base",
        "plugin:@typescript-eslint/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaVersion": 2021,
        "sourceType": "module"
    },
    "plugins": [
        "@typescript-eslint",
        "eslint-plugin-import-helpers"
    ],
    "rules": {
        "camelcase": "off",
        "import/no-unresolved": "error",
        "@typescript-eslint/naming-convention": [
            "error",
            {
                "selector": "interface",
                "format": [
                    "PascalCase"
                ],
                "custom": {
                    "regex": "^I[A-Z]",
                    "match": true
                }
            }
        ],
        "class-methods-use-this": "off",
        "import/prefer-default-export": "off",
        "no-shadow": "off",
        "no-console": "off",
        "no-useless-constructor": "off",
        "no-empty-function": "off",
        "lines-between-class-members": "off",
        "import/extensions": [
            "error",
            "ignorePackages",
            {
                "ts": "never"
            }
        ],
        "import-helpers/order-imports": [
            "warn",
            {
                "newlinesBetween": "always",
                "groups": [
                    "module",
                    "/^@shared/",
                    [
                        "parent",
                        "sibling",
                        "index"
                    ]
                ],
                "alphabetize": {
                    "order": "asc",
                    "ignoreCase": true
                }
            }
        ],
        "import/no-extraneous-dependencies": [
            "error",
            {
                "devDependencies": [
                    "**/*.spec.js"
                ]
            }
        ]
    },
    "settings": {
        "import/resolver": {
            "typescript": {}
        }
    }
}
```

## Prettier

Antes de começar a configuração é importante que você se certifique de remover a extensão Prettier - Code Formatter do seu VS Code, ela pode gerar incompatibilidades com as configurações que vamos fazer.

```shel
yarn add prettier eslint-config-prettier eslint-plugin-prettier -D
```

Com a instalação feita vamos modificar o arquivo `.eslintrc.json` adicionando no `"extends"` as seguintes regras:

```json
"prettier",
"plugin:prettier/recommended"
```

Nos `"plugins"` vamos adicionar apenas uma linha com:

```json
"prettier"
```

E nas `"rules"` vamos adicionar uma linha indicado para o **ESLint** mostrar todos os erros onde as regras do **Prettier** não estiverem sendo seguidas, como abaixo:

```json
"prettier/prettier": "error"
```

## Arquivo Final

```json
{
    "env": {
        "es2021": true,
        "node": true,
        "jest": true
    },
    "extends": [
        "airbnb-base",
        "plugin:@typescript-eslint/recommended",
        "prettier",
        "plugin:prettier/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaVersion": 2021,
        "sourceType": "module"
    },
    "plugins": [
        "@typescript-eslint",
        "prettier",
        "eslint-plugin-import-helpers"
    ],
    "rules": {
        "prettier/prettier": "error",
        "camelcase": "off",
        "import/no-unresolved": "error",
        "@typescript-eslint/naming-convention": [
            "error",
            {
                "selector": "interface",
                "format": [
                    "PascalCase"
                ],
                "custom": {
                    "regex": "^I[A-Z]",
                    "match": true
                }
            }
        ],
        "class-methods-use-this": "off",
        "import/prefer-default-export": "off",
        "no-shadow": "off",
        "no-console": "off",
        "no-useless-constructor": "off",
        "no-empty-function": "off",
        "lines-between-class-members": "off",
        "import/extensions": [
            "error",
            "ignorePackages",
            {
                "ts": "never"
            }
        ],
        "import-helpers/order-imports": [
            "warn",
            {
                "newlinesBetween": "always",
                "groups": [
                    "module",
                    "/^@shared/",
                    [
                        "parent",
                        "sibling",
                        "index"
                    ]
                ],
                "alphabetize": {
                    "order": "asc",
                    "ignoreCase": true
                }
            }
        ],
        "import/no-extraneous-dependencies": [
            "error",
            {
                "devDependencies": [
                    "**/*.spec.js"
                ]
            }
        ]
    },
    "settings": {
        "import/resolver": {
            "typescript": {}
        }
    }
}
```

# Padronizando Windows e Mac

Por conta das configurações, ao ter na equipe membros com mac e windows, o salto de linha entre utilizados por cada sistema operacional é diferente, por isso devemos instalar a extensão [EditorConfig](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig) para garantir o mesmo caracter em ambos sistemas operacionais.

Clique com o botão direito do mouse na raiz do projeto, e clique em **Generate .editorconfig**, isso irá criar o arquivo editorconfig na raiz do projeto

Com o arquivo criado você já está pronto para continuar. Todas as quebras de linha estarão no formato esperado pelo ESLint.

# Visualizando os Erros de formatação

Nesse momento ao visualizar o arquivo server.ts da pasta src, irá aparecer diversos erros no arquivo, basta salvar o arquivo novamente que as mensagens de erros irão desaparecer. Porém a primeira linha da importação do pacote express ainda aparecerá um erro de aspas duplas, pois o eslint airbnb utiliza de aspas simples, devemos então dizer para o prettier que utilizaremos aspas simples.

Crie o arquivo **.prettier.json** e informe

```json
{
  "singleQuote": true
}
```

Por fim, feche e abra o vscode novamente ou aperte command + shift + p e digite **Reload Window**.

Abra o arquivo server.ts e salve novamente!

# Código Fonte

Para acessar o código fonte do tutorial, acesse o repositório [helpers2-eslint-prettier](https://github.com/lucaslimas/helpers2-eslint-prettier)