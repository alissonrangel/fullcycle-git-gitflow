Git Flow
Configurações dos nossos branches
Pull Requests / Templates para PR
Code Review
Plugin do VSCode
codeowners
SemVer

# Iniciando com GitFlow

## Introdução ao Gitflow
 - Metodologia e extensão
 - main
 - develop, tudo que for fazendo vai no develop
 - feature, novas features nessa branch, qnd terminar joga no develop
 - release, qnd tiver terminando uma sprint, coloco do develop para o release, mergeio na main, gero uma tag e jogo no main
 - hotfix, encontrei um erro na produção, crio um branch de hotfix com a nova tag, corrigi, dou um merge no main e no develop 
 - cuidado, pois em alguns momentos vai tá commitando direto na main, qnd pullrequest
 
## Mão na massa
```
usage: git flow <subcommand>

Available subcommands are:
   init      Initialize a new git repo with support for the branching model.
   feature   Manage your feature branches.
   bugfix    Manage your bugfix branches.
   release   Manage your release branches.
   hotfix    Manage your hotfix branches.
   support   Manage your support branches.
   version   Shows version information.
   config    Manage your git-flow configuration.
   log       Show log deviating from base branch.
```
 - git flow init
  - opcoes
 - git flow feature start welcome
  - cria os arquivos e faz os trabalhos
  - após terminar a feature e está prontinha
 - git flow feature finish welcome
 ```
 Summary of actions:
  - The feature branch 'feature/welcome' was merged into 'develop'
  - Feature branch 'feature/welcome' has been locally deleted
  - You are now on branch 'develop'
 ```

 - git flow release start 0.1.0
 ```
 Switched to a new branch 'release/0.1.0'

  Summary of actions:
    - A new branch 'release/0.1.0' was created, based on 'develop'
    - You are now on branch 'release/0.1.0'

  Follow-up actions:
    - Bump the version number now!
    - Start committing last-minute fixes in preparing your release
    - When done, run:

     git flow release finish '0.1.0'
 ```
 - supondo que uma nova feature vai ser criada
 - git flow feature start contact
  - cria o contact.html
  - finaliza a feature
 - git add . git commit
 - git flow feature finish contact
 ```
 Deleted branch feature/contact (was b7d0b6d).
 
 Summary of actions:
  - The feature branch 'feature/contact' was merged into 'develop'
  - Feature branch 'feature/contact' has been locally deleted
  - You are now on branch 'develop'
 ```
 - git checkout release/0.1.0
  - faz alteração de última hora no index.html
  - git add . git commit -m 'Add !'
  - terimna o release e manda para a main
 - git flow release finish 0.1.0 
  - abriu o vim com mensagem de merge com a main
  - abriu o vim para criar uma tag para a release
  - abriu o vim para merge com o develop, pois foi alterada a release
  ``` 
  Summary of actions:
    - Release branch 'release/0.1.0' has been merged into 'main'
    - The release was tagged '0.1.0'
    - Release tag '0.1.0' has been back-merged into 'develop'
    - Release branch 'release/0.1.0' has been locally deleted
    - You are now on branch 'develop'
  ```
  - deu problema da produção
  - git flow hotfix start contact => (index) => colocar a nova tag ao inves de contact
  ```
  Switched to a new branch 'hotfix/contact'

  Summary of actions:
    - A new branch 'hotfix/contact' was created, based on 'main'
    - You are now on branch 'hotfix/contact'
  Follow-up actions:
    - Start committing your hot fixes
    - Bump the version number now!
    - When done, run:

     git flow hotfix finish 'contact'
  ```
  - corrige o erro no index.html, pois é o que tem na main
    - git add . git commit -m 'Add !' 
  - git flow hotfix finish contact
    - 1) msg merge no main
    - 2) msg nova tag
    - 3) msg merge no develope
  ```
  Summary of actions:
    - Hotfix branch 'hotfix/contact' has been merged into 'main'
    - The hotfix was tagged 'contact'
    - Hotfix tag 'contact' has been back-merged into 'develop'
    - Hotfix branch 'hotfix/contact' has been locally deleted
    - You are now on branch 'develop'
  ```

# Assinatura de commits

## Entendendo sobre assinaturas

- user.name e user.email podem ser facilmente trocados e uma outra pessoa passar por você ao fazer um pull request pode acontecer

- fazer assinatura digital do commit
- pull request com commits com assinaturas
- com chave pública e chave privada
- projeto open gpg
- instalação do GPG [https://gpgtools.org/](https://gpgtools.org/)

## Gerando chave gpg e assinando commits
- gpg --list-secret-key --keyid-form LONG => para listar as chaves existentes
- gpg --full-generate-key => para gerar uma nova chave
  - cria uma RSA, 4096, data de expiração, [user.name_do_git], [email], [comment], o (OK), [password]
  - chave criada na pasta: ~/.gnupg
- gpg --list-secret-key --keyid-form LONG => para listar as chaves existentes
- gpg --armor --export [id-da-chave]
  - copia a chave publica
  - no gitgub/configs/add gpg keys
- git config --global user.signingkey [id-da-chave]
- export GPG_TTY=$(tty)
- vim ~/.bash_profile
- git config --global commit.gpgsign true
- git config --global tag.gpgSign true
- git add . 
- git commit -S -m "" => (-S) assinar o commit caso não esteja global
- git log --show-signature -1

- vim ~/.gnupg/gpg.conf
  - use-agent :wq
- gpgconf --launch pgp-agent

## Adicionando outro email chave

- gpg --edit-key [key-id]
  - abre um terminal
    - adduid
      - nome, email, comment, o,
    - uid 2
    - trust
      - 5
      - y
    - save 

# PRs e Code Review

## Protegendo branchs

- git checkout -b develop
- git push origin develop

## Criando nossa primeira PR

- git checkout -b develop
- git push origin develop
  - git checkout -b feature/contact
    - Add files
    - git add, git commit  
  - git push origin feature/contact  
  - alterações no github
    - comments
    - merge com develop
    - delete branch feature/contact da origin
- git branch -d feature/contact

## Criando template para PRs
[https://github.com/embeddedartistry/templates](https://github.com/embeddedartistry/templates)

## Protegendo branch para code review

## Codeowners

## Extensão do Github para o VSCode
- Github Pull Requests and Issues
