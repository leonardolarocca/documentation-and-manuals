# Automatização de padrões de código e commits

## Realizando ações antes do commit 

Afim de previnir que todas as modificações que estão sendo comitadas então seguindo os padrões do projeto, vamos usar o husky, lint-staged e o cross-env

Essas ferramentas irão padronizar nosso código de acordo com o styleguide adotado, corrigindo assim possíveis erros e realizando os testes antes de qualquer arquivo ser comitado

Para isso adicione em seu projeto os seguintes pacotes:

    yarn add -D husky lint-staged cross-env

Todas as configurações serão realizadas dentro do `package.json`, observe:

```json
{
"husky": {
  "hooks": {
    "pre-commit": "lint-staged"
  }
},
"lint-staged": {
    "*.ts": [
        "eslint --fix",
        "git add",
        "cross-env CI=true yarn test --bail --findRelatedTests"
      ]
  }
}
```

Dessa forma, quando um hook parametrizado dentro do `husky`for lançado, serão executados todos os comandos definidos dentro do lint-staged, como por exemplo neste caso, toda vez antes de realizar o commit, para todos os arquivos de extensão .ts o eslint irá tentar fazer o fix de erros, se houver, se esses arquivos forem corrigidos, serao adicionados novamente para area de staged, e por fim, o cross-env garante que o comando para executar os testes funcionará independente de estar usando linux, windows ou mac

## Padronizando mensagens de commit

Para seguir uma padronização nos commits iremos utilizar o commitlint e commitzen

    yarn add -D @commitlint/config-conventional @commitlint/cli commitizen

Após instalado o pacote, adicione o arquivo `commitlint.config.js` na pasta raiz do seu projeto com o seguinte conteúdo:

```javascript
module.exports = {extends: ['@commitlint/config-conventional']}
```

Nas configs do `husky` agora você precisa adicionar o que vai ser feito toda vez que alguem comitar algo:

```json
"husky": {
  "hooks": {
    "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
  }
```

E na hora de commitar, siga os [exemplos](https://commitlint.js.org/#/concepts-commit-conventions)


Para configurar o commitizen, depois de instalados os pacotes, inicie com:

    yarn commitizen init cz-conventional-changelog --yarn --dev --exact

E se estiver usando npm 

    npm commitizen init cz-conventional-changelog --save-dev --save-exact


Depois desses comandos executados, ele irá adicionar as configurações necessárias dentro do `package.json`, você precisa adionar a seguinte linha dentro dos hooks do husky:

```json
{
  "husky": {
    "hooks": {
      "prepare-commit-msg": "exec < /dev/tty && git cz --hook || true",
    }
  }
}
```

Você pode usar o Adapter > cz-customizable, para personalizar as suas regras

