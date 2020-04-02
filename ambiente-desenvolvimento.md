# Configurando o ambiente de desenvolvimento typescript

Para iniciar o setup do ambiente de desenvolvimento `TypeScript`, serão adicionados alguns pacotes, você pode usar o gerenciador de pacotes de sua preferência, estarei usando o `yarn`.

No diretório do seu projeto inicie com:

    yarn init -y

Copie e cole em seu terminal para adicionar os pacotes necessários para linting e transpilação do `TypeScript`:

    yarn add -D typescript ts-node-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin prettier eslint-config-prettier eslint-plugin-prettier

A instalação desses pacotes fornecerá toda a estrutura para que o projeto possa ser transpilado para Javascript e para que seja realizado todo o linter, que fará a varredura a procura de erros de sintaxe ou de códigos que não seguem o padrão adotado _(styleguide)_. Depois de instalados os pacotes, prossiga para a configuração.

> É necessário instalar tambem as respectivas extensões no vscode, acesse o menu de 'Extensions' e procure na loja por: eslint, prettier

Execute o seguinte comando em seu terminal para iniciar a configuração do TypeScript:

    yarn tsc --init

Será criado um arquivo `tsconfig.json` em seu diretório, é nele que é configurado todo tipo de regra ou parâmetro, você pode se aprodundar mais sobre o que cada um deles faz, mas agora, o interessante é somente o `outDir`, que dirá para o transpilador, onde colocar o código Javascript, por padrão, gosto de deixar dentro de um diretório `dist`, como no exemplo (não se esqueça de descomentar):

    "outDir": "./dist"

Feito isso agora iniciaremos a configuração do `eslint`, cole em seu terminal:

    yarn eslint --init

Neste momento, gosto de adotar a seguinte config:

1. **How would you like to use ESLint?** _To check syntax, find problems, and enforce code style_

2. **What type of modules does your project use?** _JavaScript modules (import/export)_

3)  **Which framework does your project use?** _None of these (se você está usando algum desses, vá em frente e selecione)_

4)  **Does your project use TypeScript?** _Yes_

5)  **Where does your code run?** _Node (varia de acordo com o seu projeto)_

6)  **How would you like to define a style for your project?** _Use a popular style guide_

7)  **Which style guide do you want to follow?** _Standard: https://github.com/standard/standard (escolha pessoal)_

8)  **What format do you want your config file to be in?** _JavaScript_

Aguarde o término e se caso esteja usando `yarn` como seu gerenciador de pacotes: execute o comando na pasta para que o seu `yarn.lock` seja atualizado

    yarn

Agora, no arquivo `.eslint.js` gerado adicione as seguintes configs dentro do array de extends:

```javascript
extends: [
  'plugin:@typescript-eslint/recommended',
  'prettier/@typescript-eslint',
  'standard'
]
```

O próximo passo é configurar o `Visual Studio Code` para que ele possa formatar nosso código automáticamente ao salvar um arquivo, seguindo as regras adotadas.

Pressione `cmd + shift + p` _(mac os)_ / `ctrl + shift + p` _(windows)_ e digite:

> Preferences: Open settings (JSON)

Parametrize de acordo com abaixo:

```JSON
"eslint.workingDirectories": [
  {
    "mode": "auto"
  }
],
"eslint.packageManager": "yarn",
"editor.formatOnSave": false,
"[javascript]": {
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
  }
},
"[javascriptreact]": {
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
  }
},
"[typescript]": {
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
  }
},
"[typescriptreact]": {
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
  }
}
```

**Neste ponto é importante que o vscode seja reiniciado, após realizado, abra algum arquivo de extensão .ts e os plugins deverão ser carregados, caso haja algum erro, monitore a aba de Output > ESLint, para verificar por problemas na inicialização**

Por final, no seu `package.json` adicione os scripts que irão fazer a transpilação, para quando você estiver desenvolvendo, ou for liberar para algum servidor:

```JSON
"scripts": {
  "dev:server": "ts-node-dev --respawn --transpileOnly src/index.ts"
}
```

> Para executar em ambiente de desenvolvimento: `yarn dev:server`

> Para fazer o build: `yarn tsc` 

Se você usa o jest na sua aplicação, adicione no arquivo `.eslint`

```json
{
  "env": {
    "jest": true
  }
}
```
