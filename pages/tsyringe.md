# TSYRINGER

TSyringe é ferramenta utilizada para se fazer injeção de dependências.

Para mais informações, acesse [npm tsyringe](https://www.npmjs.com/package/tsyringe).

## Configuração

Adicionar o pacote do **tsyringer** e  **reflect-metadata**;

```cmd
yarn add tsyringe reflect-metadata
```

Modifique o arquivo **tsconfig.json**

```json
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

Criar o arquivo onde serão guardados todas as injeções de dependências.
Todos os arquivos irão compartilhar as dependências, criar o arquivo **index.ts** na pasta **src/shared/container**

```ts
import { container } from 'tsyringe';

container.registerSingleton<ITERFACE>('NOME USADO PARA INJECAO', CLASSE_QUE_SERA_INSTANCIA);

```

Exemplo:

```ts
import { container } from 'tsyringe';

import { UserRepository } from '../../repositories/implementations/User.repository';
import { IUserRepository } from '../../repositories/IUserRepository';

container.registerSingleton<IUserRepository>('UserRepository', UserRepository);

```

Importar o arquivo de reflexão (Container) no arquivo de rotas


```ts
import './shared/container';
```

## Utilização

### UserCase 

Utilizando os decorators das injeções **@injectable** e **inject**

> O decorator **inject** recebe como parâmetro o nome da injeção declara no arquivo do container do tsyringe

```ts
import { User } from '@prisma/client';
import { injectable, inject } from 'tsyringe';

import {
  IUserCreateOrUpdateDTO,
  IUserRepository,
} from '../../../repositories/IUserRepository';

@injectable()
class CreateUserCase {
  constructor(
    @inject('UserRepository')
    private userRepository: IUserRepository
  ) {}
  async execute({
    name,
    email,
    password,
  }: IUserCreateOrUpdateDTO): Promise<User> {
    const newUser = await this.userRepository.create({
      name,
      password,
      email,
    });

    return newUser;
  }
}

export { CreateUserCase };
```

### Controller

É necessário iportar o pacote de reflexão **reflect-metada** sempre que usar o resolve do container;

```ts
import 'reflect-metadata';

import { Request, Response } from 'express';
import { container } from 'tsyringe';

import { CreateUserCase } from './CreateUserCase';

class CreateUserController {
  async handler(req: Request, res: Response) {
    const { name, email, password, confirmPassword } = req.body;

    if (password !== confirmPassword) {
      return res
        .status(400)
        .send('Senha e confirmação de senha não são iguais');
    }

    const createUserCase = container.resolve(CreateUserCase);

    const newUser = await createUserCase.execute({
      name,
      email,
      password,
    });

    return res.json(newUser);
  }
}

const createUserController = new CreateUserController();

export { createUserController };

```

> O método container.resolver é encarregado de injetar todas as depedências e retornar instanciado o classe.

