# Automatizando a ordenação de imports 

Assumindo que o seu projeto já contenha o `ESLint`, siga os seguintes passos, caso contrário, vá para [Configurando o ambiente de desenvolvimento](ambiente-desenvolvimento.md), para a sessão `yarn eslint --init`.

Para iniciar, adicione o pacote ao seu projeto:

    yarn add -D eslint-plugin-import-helpers

Dentro do arquivo `.eslint.js/json`, adicione as linhas: 

```javascript
"plugins": {
  "import-helpers"
},
"rules": { 
  'import-helpers/order-imports': [
      'warn',
      {
          newlinesBetween: 'always', // new line between groups
          groups: [
              'module',
              '/^@shared/',
              // '/ˆreact/', exemplo
              ['parent', 'sibling', 'index'],
          ],
          alphabetize: { order: 'asc', ignoreCase: true },
      },
    ],
}
```

Você pode criar a sua própria ordenação através do uso de `regex` dentro do `groups: []`