##### 14/07/2024

Curso de Next.js: construa suas aplicações com Postgres e Prisma

```
docker compose up -d
docker compose down
docker ps
npm install -D typescript ts-node @types/node
npm i prisma
npx prisma migrate dev --name init
```


@01-Next fullstack

@@01
Apresentação

Estou muito animado que você esteja aqui para iniciar nosso novo curso de Next. Agora, vamos trabalhar com um Next Full Stack. O que isso significa?
No nosso curso anterior, que recomendamos fortemente que você faça, se ainda não o fez, desenvolvemos uma aplicação chamada CodeConnect. Nessa aplicação, que é um blog sobre tecnologias, integramos com uma API REST já existente.

Durante este curso, vamos remover completamente a API antiga e passaremos a nos comunicar diretamente com o banco de dados. A própria aplicação Next saberá como recuperar esses dados para exibir para a pessoa usuária.

Como vamos fazer isso?

Preparação do Ambiente de Desenvolvimento
O primeiro passo é preparar nosso ambiente de desenvolvimento. Pensando nisso, disponibilizamos no https://gist.github.com/viniciosneves/b4628877edbc78e890ab61eb23eaad49 um docker-compose.yaml que representa nossos serviços locais. Isso significa que ele vai configurar todas as dependências do nosso projeto para que possamos executá-lo sem problemas localmente.

Um dos principais pontos é garantir que, não importa se estamos no Mac, no Windows ou no Linux, haja um mínimo de compatibilidade entre nós, pessoas desenvolvedoras.

O que vamos fazer?

Copiamos todo o conteúdo que está no gist.github.com, teclamos "Ctrl + C" de todas as instruções do docker-compose.

docker-compose.yaml
services:
  # 'services' define um ou mais serviços, cada um representando um contêiner.
  
  postgres:
    # 'postgres' é o nome dado a este serviço. Você pode nomear o serviço como desejar.

    image: postgres:15-alpine
    # 'image' especifica a imagem Docker a ser usada para criar o contêiner.
    # 'postgres:15-alpine' indica que estamos usando a versão 15 da imagem do PostgreSQL, baseada na distribuição Alpine Linux, que é uma versão mais leve.

    ports:
      - 5432:5432
    # 'ports' permite mapear portas entre o contêiner e o host (sua máquina).
    # '5432:5432' significa que a porta 5432 do contêiner será mapeada para a porta 5432 do seu host. Isso permite que você acesse o serviço PostgreSQL pela porta 5432 na sua máquina.

    environment:
      POSTGRES_DB: codeconnect_dev
      # 'environment' define variáveis de ambiente dentro do contêiner.
      # 'POSTGRES_DB' define o nome do banco de dados a ser criado automaticamente quando o contêiner for iniciado pela primeira vez. Aqui, o banco de dados será chamado de 'codeconnect_dev'.

      POSTGRES_HOST_AUTH_METHOD: trust
      # 'POSTGRES_HOST_AUTH_METHOD' define o método de autenticação.
      # 'trust' significa que não é necessária senha para conectar ao banco de dados. Isso não é recomendado para ambientes de produção por razões de segurança.
COPIAR CÓDIGO
Agora, no VS Code, criamos um novo arquivo chamado docker-compose.yaml e colar o que copiamos do nosso gist.

Deixamos como pré-requisito para este curso um curso específico de docker. Se isso é novidade para você, vale a pena dar uma estudada lá primeiro. Mas para resumir este arquivo, além de termos deixado comentado o que cada uma dessas instruções faz, este docker-compose vai levantar um banco de dados postgres. Uma alternativa a isso é instalá-lo manualmente.

É uma alternativa, mas para garantir e maximizar nossa compatibilidade, recomendamos fortemente que você use o docker-compose.yaml, pois isso vai garantir que tenhamos o mesmo ambiente. Assim, tudo que fizermos aqui e você repetir do seu lado, estará conectado e homogêneo.

Execução do Docker Compose
Agora que já trouxemos o docker-compose.yaml, é hora de usar o terminal. Abrimos o terminal dentro do VS Code e, na raiz do projeto, rodamos o comando docker compose up. Isso vai levantar nosso ambiente de desenvolvimento. No entanto, vamos passar uma flag no final "-d".

docker compose up -d
COPIAR CÓDIGO
Isso fará com que o terminal não fique preso nesse comando. O próprio docker vai cuidar de manter esse Postgres rodando.

Quando pressionarmos a tecla "Enter", ele começará a fazer o download, preparar e baixar tudo que precisa para executar nosso Postgres. Quando terminar, ele exibirá uma mensagem informando que tudo está correto.

Conclusão e Próximos Passos
Se abrirmos nosso docker-desktop, veremos que, no containers/app, dentro do docker-desktop, ele exibirá e informará que nosso Postgres está rodando. Com tudo isso feito, nosso ambiente está preparado para darmos o próximo passo e começarmos a entender como o Next vai se conectar com esse Postgres.

Nos vemos no próximo vídeo!

https://gist.github.com/viniciosneves/b4628877edbc78e890ab61eb23eaad49

@@02
Preparando o ambiente

Olá!
É muito bom receber você neste curso.

Espero que seja uma experiência de aprendizado incrível e que possamos vencer todos os desafios. Como o curso é focado no Next.js 14, é super importante que você já possua uma boa base sobre o React e seu ecossistema.

Nós vamos evoluir o projeto desenvolvido no curso Next.js: crie estratégias de componentes Server-Side e para maximizar o seu aprendizado, se você ainda não passou pelo curso anterior, eu super recomendo você começar por lá!"

Para este curso, é preciso que você tenha o NodeJS e para garantir uma boa experiência de estudo, recomendo que baixe e utilize a versão 20.x. Assim você fica com a mesma versão que eu e isso vai melhorar a sua jornada de aprendizado.

E como vamos desenvolver uma aplicação fullstack, é preciso termos alguma bagagem com backend. As duas principais tecnologias que você precisa conhecer são:

Postgres
Docker
A gente também vai precisar de um SGBD (Data Base Management System ou Sistema de Gerenciamento de Banco de Dados) e exibir os dados disponíveis. Durante o curso eu uso o DBeaver mas você pode usar o que mais gostar ;)

Uma das tecnologias que vamos explorar é o Prisma, e pra que ele funcione direitinho na sua máquina precisamos garantir que você tenha os pré requisitos.

O Prisma é suportado no MacOS, no Windows e na maioria das distribuições Linux.

Se você usa Windows, confira se você possui o Microsoft Visual C++ Redistributable 2015.

Além disso, é bacana termos essa extensão do vscode para o Prisma instalada.

No primeiro vídeo nós vamos configurar o projeto inicial, que você pode baixar direto do GitHub ou clonar usando o seguinte comando:

git clone git@github.com:viniciosneves/3498-next-14-ssr-codeconnect-parte-2.git
COPIAR CÓDIGO
Com tudo instalado, podemos agora baixar os arquivos estáticos que vamos utilizar no decorrer do curso:

posts.json
404
500
O docker compose que vamos usar na aula 1:

docker-compose.yaml
E os dados que vamos usar para popular o nosso banco de dados na aula 2:

seed.js
E por último, o Figma. Sinta-se livre para passear um pouco por ele e explorar a aplicação que vamos iniciar.

Tudo pronto? Bora codar :)

https://cursos.alura.com.br/course/next-js-estrategias-componentes-server-side

https://nodejs.org/dist/v20.11.0/

https://cursos.alura.com.br/course/introducao-postgresql-primeiros-passos

https://cursos.alura.com.br/course/docker-criando-gerenciando-containers

https://dbeaver.io/download/

https://www.prisma.io/docs/orm/reference/system-requirements

https://www.prisma.io/docs/orm/reference/system-requirements#windows-runtime-dependencies

https://marketplace.visualstudio.com/items?itemName=Quernest.prisma

https://github.com/alura-cursos/3498-next-14-ssr-codeconnect-parte-2

https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts.json

https://github.com/alura-cursos/3498-next-14-ssr-codeconnect-parte-2/blob/aula-4/src/app/error/404.png

https://github.com/alura-cursos/3498-next-14-ssr-codeconnect-parte-2/blob/aula-2/src/app/error/500.png

https://gist.github.com/viniciosneves/b4628877edbc78e890ab61eb23eaad49#file-docker-compose-yaml

https://gist.github.com/viniciosneves/4c687fd38f6783f7d5394af584bf516e#file-3498-seed-js

https://www.figma.com/file/k2uVg4mzbUmXMwHFQKzfLr/CodeConnect--%7C-Formação-Next?node-id=622%3A6254&mode=dev

@@03
Postgres via docker-compose

Estou muito animado que você esteja aqui para iniciar nosso novo curso de Next. Agora, vamos trabalhar com um Next Full Stack. O que isso significa?
No nosso curso anterior, que recomendamos fortemente que você faça, se ainda não o fez, desenvolvemos uma aplicação chamada CodeConnect. Nessa aplicação, que é um blog sobre tecnologias, integramos com uma API REST já existente.

Durante este curso, vamos remover completamente a API antiga e passaremos a nos comunicar diretamente com o banco de dados. A própria aplicação Next saberá como recuperar esses dados para exibir para a pessoa usuária.

Como vamos fazer isso?

Preparação do Ambiente de Desenvolvimento
O primeiro passo é preparar nosso ambiente de desenvolvimento. Pensando nisso, disponibilizamos no https://gist.github.com/viniciosneves/b4628877edbc78e890ab61eb23eaad49 um docker-compose.yaml que representa nossos serviços locais. Isso significa que ele vai configurar todas as dependências do nosso projeto para que possamos executá-lo sem problemas localmente.

Um dos principais pontos é garantir que, não importa se estamos no Mac, no Windows ou no Linux, haja um mínimo de compatibilidade entre nós, pessoas desenvolvedoras.

O que vamos fazer?

Copiamos todo o conteúdo que está no gist.github.com, teclamos "Ctrl + C" de todas as instruções do docker-compose.

docker-compose.yaml
services:
  # 'services' define um ou mais serviços, cada um representando um contêiner.
  
  postgres:
    # 'postgres' é o nome dado a este serviço. Você pode nomear o serviço como desejar.

    image: postgres:15-alpine
    # 'image' especifica a imagem Docker a ser usada para criar o contêiner.
    # 'postgres:15-alpine' indica que estamos usando a versão 15 da imagem do PostgreSQL, baseada na distribuição Alpine Linux, que é uma versão mais leve.

    ports:
      - 5432:5432
    # 'ports' permite mapear portas entre o contêiner e o host (sua máquina).
    # '5432:5432' significa que a porta 5432 do contêiner será mapeada para a porta 5432 do seu host. Isso permite que você acesse o serviço PostgreSQL pela porta 5432 na sua máquina.

    environment:
      POSTGRES_DB: codeconnect_dev
      # 'environment' define variáveis de ambiente dentro do contêiner.
      # 'POSTGRES_DB' define o nome do banco de dados a ser criado automaticamente quando o contêiner for iniciado pela primeira vez. Aqui, o banco de dados será chamado de 'codeconnect_dev'.

      POSTGRES_HOST_AUTH_METHOD: trust
      # 'POSTGRES_HOST_AUTH_METHOD' define o método de autenticação.
      # 'trust' significa que não é necessária senha para conectar ao banco de dados. Isso não é recomendado para ambientes de produção por razões de segurança.
COPIAR CÓDIGO
Agora, no VS Code, criamos um novo arquivo chamado docker-compose.yaml e colar o que copiamos do nosso gist.

Deixamos como pré-requisito para este curso um curso específico de docker. Se isso é novidade para você, vale a pena dar uma estudada lá primeiro. Mas para resumir este arquivo, além de termos deixado comentado o que cada uma dessas instruções faz, este docker-compose vai levantar um banco de dados postgres. Uma alternativa a isso é instalá-lo manualmente.

É uma alternativa, mas para garantir e maximizar nossa compatibilidade, recomendamos fortemente que você use o docker-compose.yaml, pois isso vai garantir que tenhamos o mesmo ambiente. Assim, tudo que fizermos aqui e você repetir do seu lado, estará conectado e homogêneo.

Execução do Docker Compose
Agora que já trouxemos o docker-compose.yaml, é hora de usar o terminal. Abrimos o terminal dentro do VS Code e, na raiz do projeto, rodamos o comando docker compose up. Isso vai levantar nosso ambiente de desenvolvimento. No entanto, vamos passar uma flag no final "-d".

docker compose up -d
COPIAR CÓDIGO
Isso fará com que o terminal não fique preso nesse comando. O próprio docker vai cuidar de manter esse Postgres rodando.

Quando pressionarmos a tecla "Enter", ele começará a fazer o download, preparar e baixar tudo que precisa para executar nosso Postgres. Quando terminar, ele exibirá uma mensagem informando que tudo está correto.

Conclusão e Próximos Passos
Se abrirmos nosso docker-desktop, veremos que, no containers/app, dentro do docker-desktop, ele exibirá e informará que nosso Postgres está rodando. Com tudo isso feito, nosso ambiente está preparado para darmos o próximo passo e começarmos a entender como o Next vai se conectar com esse Postgres.

Nos vemos no próximo vídeo!

@@04
Instalando o Prisma

Agora que temos nossa infraestrutura para o ambiente de desenvolvimento funcionando, com o Docker e o container sendo executados, podemos evoluir nosso projeto. A primeira coisa que faremos é configurar o Prisma.
Configurando o Prisma
O Prisma é um ORM, ou seja, vamos escrever um pouco de código JavaScript e um pouco de código na própria linguagem do Prisma. O Prisma vai entender a sintaxe dele e esse ORM vai fazer a comunicação com o banco de dados. Isso vai evitar que precisemos escrever SQL manualmente.

Se o universo de um ORM ainda é novo para você, deixamos uma atividade nesta aula, um "Para Saber Mais", que explica de onde nasceu esse conceito de ORM e como podemos explorar e entender um pouco mais esse universo, onde abstraímos a escrita do nosso SQL para escrever algo mais específico da linguagem que estamos executando, no nosso caso, JavaScript. Escreveremos consultas que pegam dados do banco, mas usando JavaScript.

Para começar, temos três pequenos passos.

O primeiro: no VS Code, limpamos o terminal, porque ele ainda estava exibindo o resultado do docker compose up -d. Agora, o que faremos? Pedimos para o npx executar um script do Prisma e o script vai receber esse parâmetro init, que vai iniciar a nossa preparação do nosso ambiente Prisma.

npx prisma init
COPIAR CÓDIGO
O comando foi executado, ele retornou um resultado informando que está tudo certo, que já tem um arquivo .gitignore, e que o esquema da nossa aplicação foi criado dentro da pasta prisma em um arquivo chamado schema.prisma.

Your Prisma schema was created at prisma/schema.prisma
You can now open it in your favorite editor.

Analisaremos esse arquivo agora. Podemos clicar no ícone de lixeira no canto superior direito do terminal. Do lado esquerdo, clicamos na pasta prisma e depois em schema.prisma.

schema.prisma
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
    provider = "prisma-client-js"
}

datasource db {
    provider = "postgresql"
    url = env("DATABASE_URL")
}
COPIAR CÓDIGO
Repare, foi criado um generator, que é uma forma do Prisma criar o nosso client, e mais para frente vamos começar a fazer isso, e ele inseriu também um datasource, que é o nosso banco de dados.

Repare, já foi indicado na linha 9 que o provider é um Postgres, ou seja, por padrão, o Prisma já vem preparado para o Postgres. E a URL desse banco de dados, ele está vindo de uma variável de ambiente chamada DATABASE_URL.

Então, você provavelmente está se questionando sobre essa variável de ambiente, já que ainda não fizemos nada. Bem, quando executamos o comando prisma init, ele automaticamente criou um arquivo .env para nós.

Esse arquivo será responsável por armazenar todas as variáveis de ambiente personalizadas, além dos padrões do Node. E o Prisma já incluiu uma URL de banco de dados (DATABASE_URL) para nós.

Aqui, muito cuidado e atenção. No nosso cenário, nesse momento, essa variável de ambiente tem uma URL para uma conexão com o banco de dados local. Então, como aqui tem coisa local, o exemplo que ele gerou aqui até um exemplo que não funciona, que é um johndoe com random password.

Trecho de código indicado pelo instrutor:
DATABASE_URL="postgresql://johndoe: randompassword@localhost:5432/mydb?schema=public"
COPIAR CÓDIGO
Não tem problema comitarmos, porque é um ambiente seguro. É o nosso ambiente de desenvolvimento e não tem problema nenhum de isso estar público lá no GitHub.

Porém, quando mandarmos isso para a produção, esse .env não pode, de maneira nenhuma, conter os dados de acesso ao nosso banco. Senão, qualquer pessoa vai entrar no GitHub, pegar essa conexão e poderá manipular e fazer o que quiser com o banco de dados.

Normalmente, os arquivos .env, eles não são comitados. No nosso cenário, não tem problema, porque é só o ambiente de dev.

Desenvolvendo o schema.prisma
Agora que organizamos isso, avançamos para criar e desenvolver nosso schema.prisma. Vamos fechar tudo que está aberto e criar nossos modelos. Considerando nosso contexto, onde temos posts e os autores desses posts, o autor será um model. E já que estamos codificando em inglês, vamos chamar esse modelo de User.

schema.prisma
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
    provider = "prisma-client-js"
}

datasource db {
    provider = "postgresql"
    url = env("DATABASE_URL")
}

model User {

}
COPIAR CÓDIGO
Aqui se inicia o encanto de como conseguimos detalhar uma tabela para o Prisma.

Por exemplo, podemos iniciar solicitando um id, e afirmaremos que este id é um número inteiro (Int). Adicionalmente, faremos uma observação indicando que ele é a chave primária desta tabela usando @id. Também podemos atribuir um valor @default() (padrão) para este campo, especificando que o valor padrão é o autoincrement.

schema.prisma
// código omitido

model User {
 id Int @id @default(autoincrement())
}
COPIAR CÓDIGO
Criamos uma model User, ou seja, o Prisma vai criar no banco de dados uma tabela user, e adicionamos o primeiro campo id, que é um inteiro, de autoincrement.

Observação importante: No meu VS Code, o destaque de sintaxe (highlight) já está funcionando perfeitamente. As cores estão sendo modificadas conforme necessário. Isso ocorre porque tenho uma extensão instalada, especificamente a extensão do Prisma. Do lado esquerdo, clicamos no ícone de extensões e no campo de busca na parte superior digitamos "prisma".

Essa extensão do Prisma, presente no meu VS Code, realiza exatamente essa função: realçar o código escrito e também fornecer sugestões de preenchimento automático (autocomplete). Portanto, ela me sugeriu o @id, o autoincrement, e outras funcionalidades semelhantes.

Vamos seguindo. Além disso, pegamos um name, que vai ser uma string. Teremos o username da nossa pessoa usuária, que também vai ser um string.

schema.prisma
// código omitido

model User {
  id Int @id @default(autoincrement())
  name String
  username String
}
COPIAR CÓDIGO
Neste ponto, temos um detalhe.

Se estamos falando de um username, não podemos ter mais de um usuário com o mesmo nome. E conseguimos proteger isso quando estamos falando de banco de dados. Para indicar para o Prisma que esse campo é o único e não pode ter valores repetidos. Então, vamos colocar a anotação @unique.

schema.prisma
// código omitido

model User {
  id Int @id @default(autoincrement())
  name String
  username String @unique
}
COPIAR CÓDIGO
Temos o id, o name, o username, e também temos o avatar, que vai ser também uma string.

schema.prisma
// código omitido

model User {
  id Int @id @default(autoincrement())
  name String
  username String @unique
  avatar String
}
COPIAR CÓDIGO
Você pode estar se perguntando de onde vêm esses id, name, username e avatar. No curso anterior, nós utilizamos uma API que fornecia esses dados. Agora, estou com o repositório aberto, então se quiser mais detalhes, fique à vontade para explorá-lo. Mas se preferir não se aprofundar, confie em mim, vou revisitar o projeto antigo e garantir que tudo funcione corretamente.

Criamos a nossa model de user. Vamos seguir para a nossa model, que representa agora um post.

Criando a model Post
Digitamos na sequência "model Post".

schema.prisma
// código omitido

model User {
  id Int @id @default(autoincrement())
  name String
  username String @unique
  avatar String
}

model Post {

}
COPIAR CÓDIGO
Temos o posts.json, onde você pode visualizar de onde vêm esses dados iniciais, mas isso é para podermos pegar esses campos que vamos precisar. Deixei aberto no fundo o posts.json, vou trazer o VSCode para frente, porque vamos precisar analisar os dois. E vamos começar a escrever o nosso código.

O model Post vai ter também um id inteiro, que é um @id, e por padrão tem um autoincrement. Além disso, ele vai ter uma coluna chamada cover, que é onde está a capa, o endereço para a nossa imagem, que vai ser string. Vai ter um title, que também é string. Também vai ter um slug, que é um string. A ideia do slug é a mesma do username.

Queremos que o slug seja o único, que é um texto que conseguimos ler, mas que identifica de forma única o post dentro da nossa tabela. Então, id, cover, que é a capa, o title, que é o título, o slug.

schema.prisma
// código omitido

model User {
  id Int @id @default(autoincrement())
  name String
  username String @unique
  avatar String
}

model Post {
  id Int @id @default(autoincrement())
  cover String
  title String
  slug String @unique
}
COPIAR CÓDIGO
O que mais? Temos o body, que é o conteúdo em si do nosso post, que também será uma string. Temos o id, cover, title, slug, body, o markdown, que também será uma string.

schema.prisma
// código omitido

model User {
  id Int @id @default(autoincrement())
  name String
  username String @unique
  avatar String
}

model Post {
  id Int @id @default(autoincrement())
  cover String
  title String
  slug String @unique
  body String
  markdown String
}
COPIAR CÓDIGO
Agora, o que desejamos fazer? Desejamos mapear.

Desejamos informar para o Prisma que esse usuário vai possuir um ou mais posts. É um relacionamento que vamos criar. Então, como vamos fazer isso? Vamos indicar no model User que temos Post, o nosso P, com P maiúsculo mesmo, porque queremos relacionar as tabelas. Vamos dizer que ele é do tipo Post[], só que ele é um array. Um usuário pode ter múltiplos posts.

schema.prisma
// código omitido

model User {
  id Int @id @default(autoincrement())
  name String
  username String @unique
  avatar String
  Post Post[]
}

model Post {
  id Int @id @default(autoincrement())
  cover String
  title String
  slug String @unique
  body String
  markdown String
}
COPIAR CÓDIGO
Observe que temos um sublinhado na cor vermelha abaixo. Ao passarmos o mouse por cima, temos indicado pela próprio VSCode que o campo Post dentro de user está sendo relacionado com posts, mas o relacionamento inverso ainda não existe.

Mensagem em inglês exibida no VSCode:
Error validating field 'Post' in model 'User': The relation field Post on model 'User' is missing an opposite relation field on the model 'Post'. Either run 'prisma format' or add it manually.
Então, ou você pede para o Prisma formatar isso para você, ou adicionar manualmente. Adicionaremos manualmente para entendermos como o Prisma lida com isso.

Na nossa model Post, informamos que temos um autor, author. Esse author, ele é do tipo User. E informamos agora como que mapeamos a nossa relation, que é a nossa relação. Para isso, usamos @relation(). Como que post se relaciona com user.

Chamamos o @relation() para iniciar a montagem, começando com os fields. Nosso primeiro campo dentro da tabela post, representando um usuário, será o authorId. Este campo vai referenciar o id do usuário. Em outras palavras, na tabela Post, teremos a coluna authorId, que apontará para o id da pessoa usuária.

schema.prisma
// código omitido

model Post {
  id Int @id @default(autoincrement())
  cover String
  title String
  slug String @unique
  body String
  markdown String
  author User @relation(fields: [authorId], references: [id])
}
COPIAR CÓDIGO
Há um sublinhado na cor vermelha abaixo de authorId porque ainda não criamos esse campo. Então criamos ele acima do nosso relacionamento. Então, authorId vai ser um inteiro.

schema.prisma
// código omitido

model Post {
  id Int @id @default(autoincrement())
  cover String
  title String
  slug String @unique
  body String
  markdown String
  authorId Int
  author User @relation(fields: [authorId], references: [id])
}
COPIAR CÓDIGO
Agora, sim. O nosso authorId, ele aponta para o nosso usuário.

Conclusão e Próximos Passos
Já mapeamos a estrutura da nossa tabela, o nosso author, que é um usuário, e o nosso post.

Agora estamos prontos para dar o próximo passo: vamos conectar isso. Esse prisma tem que criar essa tabela, essa estrutura lá no Postgres. Vamos fazer isso no próximo vídeo.

@@05
Migrações, tabelas e DBeaver

Começamos a mapear nossos modelos, nossas entidades. Temos o nosso usuário e temos o nosso post. Agora, antes de conectarmos esses dois, percebemos que faltam duas coisas. Como estamos falando de posts, faz sentido guardarmos, por exemplo, a data de criação e a data de atualização dessa informação. Então, podemos criar esses campos.
Vamos ao arquivo schema.prisma no model Post.

Podemos criar, por exemplo, um createdAt, e esse campo será um dateTime e podemos inserir um campo padrão com @default() passando now(). Ou seja, criamos uma coluna chamada createdAt, que é um dateTime e quando o post é inserido, já fica com o timestamp daquela hora que ele foi criado.

Também podemos fazer o mesmo para updatedAt, que também terá o mesmo formato, que é um dateTime, a diferença é que ele não terá esse valor padrão. Porém, como isso é uma técnica muito comum, o Prisma também tem uma anotação para isso, que será o @updatedAt. Ele saberá que quando fizermos uma operação de update dentro dessa tabela, ele tem que preencher esse valor.

schema.prisma
// código omitido

model Post {
  id Int @id @default(autoincrement())
  cover String
  title String
  slug String @unique
  body String
  markdown String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  authorId Int
  author User @relation(fields: [authorId], references: [id])
}

// código omitido
COPIAR CÓDIGO
Agora sim, estamos com os nossos modelos bem definidos. Precisamos transformar isso agora em tabelas do nosso banco de dados.

Transformando em tabelas
Como vamos fazer isso? Precisamos conectar no nosso DATABASE_URL, dentro do nosso .env, esse valor tem que apontar para o nosso Docker que está rodando localmente. Vamos fazer isso? Abrimos o arquivo .env. E perceba que agora temos que configurar e passar isso corretamente.

A primeira coisa é que o nosso usuário não é John Doe, o nosso usuário será postgres e sem senha. E não precisamos indicar o esquema public, podemos deixar o URL desse jeito.

Temos o nosso DATABASE_URL configurado corretamente, com o usuário postgres, @localhost na porta 5432, sendo que não é mydb o nosso banco de dados.

.env
# comentários omitidos

DATABASE_URL="postgresql://postgres@localhost:5432/mydb"
COPIAR CÓDIGO
Onde podemos pegar o nome correto do nosso banco de dados? No nosso Docker Compose. Na estrutura de projetos do lado esquerdo, vamos no docker-compose.yaml e temos na linha 17 uma variável de ambiente chamada Postgres_DB que diz qual é o banco de dados que será criado por padrão quando o Docker levantar esse Docker Compose. O nome do nosso banco de dados é codeconnect_dev.

Então, codeconnect_dev. Vamos substituir no .env o mybd por codeconnect_dev. Isso é suficiente para conectar e fazer com que as coisas se interliguem.

.env
# comentários omitidos

DATABASE_URL="postgresql://postgres@localhost:5432/codeconnect_dev"
COPIAR CÓDIGO
Agora podemos pedir para o Prisma de fato executar isso.

Abrimos o terminal. Vamos instalar o Prisma no nosso projeto que ainda não temos, npm i prisma.

npm i prisma
COPIAR CÓDIGO
Ele irá instalar e salvar no package.json. E agora ele estará corretamente.

Agora, solicitamos ao Prisma que execute e crie essa migration. Mas o que significa migration? Ele vai configurar e traduzir nossos modelos para uma linguagem compreensível pelo Postgres e, consequentemente, criar a aplicação. Em outras palavras, estamos indicando que nós, como desenvolvedores, não precisaremos intervir diretamente no Postgres.

Quem vai fazer isso é o Prisma. Desde a manipulação dos dados quanto mesmo a criação das tabelas. Então, quando pedimos para o npx executar o script de Prisma, pedindo para ele rodar as migrações e passamos um nome, porque, no nosso caso, a primeira Migration é a Migration inicial.

Estamos criando as primeiras tabelas. Mas o banco de dados pode evoluir e ir ganhando novos modelos, ganhando novos campos e o Prisma vai saber o que ele tem que fazer para manter tudo atualizado.

npx prisma migrate dev --name init
COPIAR CÓDIGO
Então, se executarmos esse comando, o Prisma vai começar a executar o que precisa, está executando os comandos.

Your database is now in sync with your schema
Como deu tudo certo, o retorno no terminal está escrito na cor verde, o banco de dados está sincronizado com o seu schema, podemos começar mesmo a inserir e ler dados dentro desse banco.

Mas como fazemos para ter certeza absoluta que o Prisma realmente criou essas tabelas lá? Vamos abrir um software que se chama DBeaver. Se você ainda não conhece ele ou se você ainda não instalou, deixamos bem explicado no preparando o ambiente como é que temos que fazer para baixar e instalar essa aplicação.

E o que vamos fazer? Desejamos criar agora uma nova conexão. Repare, não conseguimos dar um zoom muito grande, mas ele tem um ícone que está uma tomada com um ícone de mais na parte superior esquerda, que é exatamente uma nova conexão com o banco de dados. Informamos que esse banco de dados é Postgres selecionando essa opção, e clicamos em "Next".

O campo "host" é o localhost, está certo, a porta 5432 está correta, o banco de dados não é o Postgres, é aquele mesmo que está no nosso Docker Compose. Podemos pegar de lá, voltamos no VS Code, no Docker Compose, fechamos o terminal, copiamos na linha 17 o nosso codeconnect_dev, voltamos no DBeaver, colamos no campo "Database" para ficar bem correto e clicamos em Finish, que ele vai finalizar essa conexão.

Se esta é a primeira vez que você está fazendo isso, provavelmente ele fará o download do conector do Postgres, mas apenas na primeira vez. No meu caso, como já o fiz anteriormente, ele nem solicitou nada. Agora podemos explorar a parte do banco de dados. Ele já exibiu o codeconnect_dev.

Vamos abrir o schema do lado esquerdo, public, tabelas e lá está nossa tabela de posts, corretamente com os campos ID, cover, title, slug, body, markdown e todos os outros que adicionamos. O mesmo ocorre com a tabela de User, que contém os campos ID, name, username e avatar.

E reparem que tem uma outra tabela chamada _prisma_migrations, que tem uma estrutura própria, e inclusive tem até dados de uma migration que foi executada, ou seja, a nossa migration inicial. É através dessa tabela que o Prisma vai saber o que ele tem que fazer ou o que ele já fez de migration relacionadas ao nosso modelo.

Conclusão e Próximos Passos
Com o banco de dados configurado e funcionando com as tabelas necessárias para escrever e ler dados, podemos avançar para o próximo passo. Já temos nossos posts configurados e o Prisma que traduzirá nosso código JavaScript em instruções SQL.

Agora, precisamos fazer com que o Next.js se conecte ao Prisma para acessar esses dados. Esta será nossa próxima tarefa, que abordaremos na próxima aula. Contamos com a sua presença lá!

@@06
Para saber mais: Node e variáveis de ambiente

No mundo Node.js, as variáveis de ambiente são nossas melhores amigas para manter as coisas seguras e práticas. Estou falando de configurar URLs, chaves secretas, senhas... essas coisas que a gente não muda toda hora.
Bora criar variáveis de ambiente?

No Node, é tranquilo trabalhar com variáveis de ambiente. Elas estão ali, prontas para uso, graças ao objeto env que mora no process, um objeto global que o Node disponibiliza.

Por exemplo, se eu quiser guardar a combinação da minha mala (porque né, sempre esqueço), eu faria algo assim: process.env.COMBINACAO_BAGAGEM="12345".

Ah, uma dica: as pessoas desenvolvedoras costumam escrever o nome das variáveis em CAIXA ALTA, beleza? Isso é mais para um teste rápido. Na vida real, ou melhor, nas aplicações de verdade, a gente usa outras formas para definir esses valores. Como o arquivo .env que manipulamos em aula:


DB_HOST=localhost
DB_USER=admin
DB_PASSWORD=password
COPIAR CÓDIGO
Só não esquece de manter esse arquivo longe dos olhos maldosos, especialmente do GitHub. Uma opção é adicionar o .env ao .gitignore para não compartilhar sem querer informações sensíveis (como os dados de acesso ao banco de produção!).

@@07
Variáveis de ambiente em aplicações Next.js

Considerando as boas práticas e cuidados com a segurança em aplicações Next.js, você recebeu a tarefa de revisar o código da aplicação para garantir que as variáveis de ambiente estão sendo manuseadas corretamente. Qual das seguintes abordagens você julga ser a mais adequada para garantir a segurança das variáveis de ambiente?


Publicar as variáveis de ambiente em um repositório Git público para facilidade de acesso pelos membros da equipe.
 
Publicar variáveis de ambiente em um repositório público é altamente inseguro, pois expõe informações sensíveis a qualquer pessoa com acesso à internet.
Alternativa correta
Criptografar as variáveis de ambiente antes de armazená-las no código e descriptografá-las em tempo de execução.
 
Apesar da criptografia adicionar uma camada de segurança, armazenar variáveis de ambiente criptografadas diretamente no código ainda representa um risco de segurança e não é uma prática recomendada.
Alternativa correta
Armazenar as variáveis de ambiente diretamente no código e acessá-las usando process.env.NAME em todo o aplicativo.
 
Armazenar variáveis de ambiente diretamente no código viola as práticas de segurança, pois torna sensíveis informações acessíveis e comprometíveis caso o código seja exposto.
Alternativa correta
Utilizar um arquivo .env para armazenar variáveis de ambiente e acessá-las usando process.env.NAME, garantindo que este arquivo não seja incluído no controle de versão.
 
Utilizar um arquivo .env para armazenar as variáveis e garantir que ele não seja incluído no controle de versão é uma prática recomendada em Next.js, pois protege informações sensíveis e as mantém fora do repositório de código.

@@08
Faça como eu fiz: preparando o ambiente de dev

Vamos então preparar o ambiente de dev? Podemos definir 3 passos importantes para esse momento:
Levantar o postgres via docker compose
Iniciar a configuração do prisma
Definir os modelos de User e Post, incluindo o relacionamento
Bora lá? Lembrando que essa é a hora de praticar para consolidar o conhecimento. Se precisar de ajuda, lembre-se que temos o fórum e o discord esperando por você!

Como sempre, teu amigo aqui, o dev careca e barbudo que você mais gosta, não vai te deixar na mão!
Aqui no GitHub você consegue pegar direitinho todas as alterações que eu fiz em aula.

Só relembrando os comandos que usei:

Pra levantar o postgres:

docker compose up -d
COPIAR CÓDIGO
Para iniciar a configuração do Prisma:

npx prisma init
COPIAR CÓDIGO
Para instalar o Prisma:

npm i prisma
COPIAR CÓDIGO
E para executar as migrations:


npx prisma migrate dev --name init

https://github.com/alura-cursos/3498-next-14-ssr-codeconnect-parte-2/compare/main...aula-1

@@09
O que aprendemos?

Nessa aula, você aprendeu como:
Levantar um container docker do postgres;
Configurar o ambiente de desenvolvimento do Prisma;
Modelar entidades e relacionamentos no Prisma;
Executar migrations para inicializar o banco de dados.

#### 15/07/2024

@02-Prisma client

@@01
Projeto da aula anterior

Caso queira começar daqui, você pode acessar o projeto da aula anterior, ou, se preferir, fazer o download do arquivo.

https://github.com/alura-cursos/3498-next-14-ssr-codeconnect-parte-2/tree/aula-1

https://github.com/alura-cursos/3498-next-14-ssr-codeconnect-parte-2/archive/refs/heads/aula-1.zip

@@02
Gerando o Prisma client

Agora que já executamos nossas migrations e o Prisma criou as tabelas no nosso banco de dados, podemos, de fato, conectar o Next ao Postgres, diretamente, usando o Prisma. Como vamos fazer isso?
Como estamos delegando a responsabilidade de escrever SQL para o próprio Prisma, precisamos executar uma instrução para informá-lo que a base está pronta, o banco de dados está atualizado e ele pode gerar o client e tudo que for necessário para o client funcionar, pois está tudo certo do nosso lado.

Há um detalhe importante: essa geração do Prisma Client deve ser feita ou refeita sempre que houver uma alteração no banco de dados.

No nosso caso, foi a primeira alteração e o que fizemos foi criar a estrutura de banco de dados. Agora que está tudo mapeado, nossa model está correta, o Postgres já recebeu as tabelas, vamos criar esse client.

Vamos ao terminal do VS Code e novamente pediremos ajuda do NPX para executar o script do Prisma. Desta vez, não será o migrate, será o generate:

npx prisma generate
COPIAR CÓDIGO
Esse comando lerá nossa estrutura e gerará as consultas e tudo que for necessário para que o Prisma funcione corretamente.

Após a execução do comando, o terminal informa que tudo foi gerado corretamente e que agora podemos importar o Prisma Client, que é uma classe, instanciá-la e tudo ficará perfeito. Vamos fazer isso.

Primeiro, copiamos a sugestão de código do próprio terminal, para evitar a fadiga de digitar tudo de novo:

import { PrismaClient } from '@prisma/client'
const prisma = new PrismaClient()
COPIAR CÓDIGO
Copiamos essa instrução, fechamos o terminal e, na nossa estrutura de pastas, dentro do Prisma, criamos um novo arquivo chamado db.js, que representará nosso banco de dados.

Colamos o código que copiamos do terminal, com a única diferença de, ao invés de chamar o Prisma Client de prisma, chamamos de db. Por fim, na última linha, adicionamos um export default db.

db.js
import { PrismaClient } from '@prisma/client'
const db = new PrismaClient()

export default db
COPIAR CÓDIGO
Com isso, estamos criando uma única instância do Prisma Client. Agora, se qualquer parte da nossa aplicação precisar se conectar ao banco, é só importar esse db e tudo funcionará perfeitamente.

Agora que geramos nosso client e o instanciamos e disponibilizamos via export default, podemos ir para o nosso componente Next e realizar uma troca: fazer com que a nossa fonte de dados, agora, passe a ser o Postgres, ao invés de fazer aquelas chamadas HTTP da API.

Dessa forma, damos mais um passo na direção de transformar a nossa aplicação Next numa aplicação Fullstack. Nos encontramos no próximo vídeo.

@@03
Obtendo os Posts do Postgres

Após configurar toda a nossa infraestrutura, agora podemos remover nossa primeira integração via Fetch com a API REST e substituí-la pelo nosso Prisma Client. Vamos fazer isso?
Voltamos para o VS Code. Dentro do projeto, temos uma pasta "src > app" com um arquivo chamado page.js. Entramos nele.

No último curso, extraímos a função getAllPosts, responsável por obter esses dados. Como isso já está encapsulado para a nossa página, pouco importa de onde eles estão vindo, desde que venham no formato esperado.

Conforme verificamos na linha 19 desse arquivo, a nossa página espera que o nosso array de posts venha numa propriedade chamada data, e espera a página anterior e a próxima, que são prev e next: const { data: posts, prev, next } = await getAllPosts(currentPage). Essa é a estrutura que precisamos retornar.

Então, vamos limpar tudo que está dentro de getAllPosts para substituir o método.

Código de getaAllPosts deletado do arquivo page.js
async function getAllPosts (page) {
  const response = await fetch(`http://localhost:3042/posts?_page=${page}&_per_page=6`)
  if (!response.ok) {
    logger.error('Ops, alguma coisa correu mal')
    return []
  }
  logger.info('Posts obtidos com sucesso')
  return response.json()
}
COPIAR CÓDIGO
A primeira coisa que vamos fazer dentro do getAllPosts agora é inserir um bloco trycatch, para começarmos a programar defensivamente.

Após ter colocado o bloco trycatch, vamos chamar o logger.error no catch. Ou seja, se algo deu errado, podemos explicitar o que aconteceu passando uma mensagem de "Falha ao obter posts", por exemplo, e um objeto de erro como segundo argumento, o error. Isso resulta em: logger.error('Falha ao obter posts', { error }).

Para fazer com que a nossa aplicação funcione, ou seja, ela não quebre e, por exemplo, exiba que não tem post nenhum, podemos fazer um return do nosso objeto com a propriedade data, que é um array vazio, e podemos passar prev, que será nulo, assim como next. Então: return { data: [], prev: null, next: null }.

No caso de sucesso, podemos copiar esse return, porque será muito parecido, exceto pelo data, que não será um array vazio, mas um array de posts: return { data: posts, prev: null, next: null }.

E como vamos fazer para, utilizando o nosso Prisma Client, consumir esses dados?

Primeiro, vamos chamar o db. O VS Code sugere o db que está dentro da pasta "prisma", e é esse mesmo que queremos, então o selecionamos. O import é feito automaticamente: import db from "../../prisma/db".

Vamos adicionar um ponto . após o db, e teremos algumas sugestões de autocomplete. Tem algumas coisas que podemos fazer com o db que vão além de obter dados, mas o que queremos nesse momento é acessar o nosso módulo post. Então, db.post.

Agora vamos dizer o que queremos fazer. Nesse cenário, o que queremos fazer é passar uma instrução SQL. Como queremos pegar quantos posts conseguirmos, podemos chamar o método findMany, ou seja, pegar todos que estiverem no banco de dados.

Por padrão, não passamos parâmetro nenhum para esse método. O que ele vai fazer é acessar o banco e buscar todos esses dados. Note que o retorno é uma promessa, ou seja, um PrismaPromise, de que ele vai tentar obter esses dados.

Acreditando que essa promessa será cumprida, podemos guardar esse retorno em algum lugar. Podemos criar uma constante chamada posts, que vai receber o db.post.findMany(), mas vamos aguardar por essa resposta com await. Isso resulta em const posts = await db.post.findMany().

async function getAllPosts (page) {
    try {
      const posts await db.post.findMany() =
      return { data: posts, prev: null, next: null }
        
    } catch (error) { 
      logger.error('Falha ao obter posts, { error })
      return { data: [], prev: null, next: null }
    }
}
COPIAR CÓDIGO
Nesse cenário, pegamos os dados, guardamos em uma variável chamada posts, e retornamos na mesma estrutura que a API REST retornava: data com os valores, prev para a página anterior, next para a página posterior.

Como ainda não estamos controlando a paginação, sendo esse um assunto para tratarmos mais adiante, vamos testar se a conexão vai funcionar ou se vai dar algum erro. Então, salvamos o arquivo.

Rodando a aplicação
Até esse momento, não estávamos executando a nossa aplicação Next, então, vamos colocá-la para rodar.

Abrimos o terminal local e entramos no diretório onde está o projeto. No caso do instrutor, o projeto está na área de trabalho, então rodou o comando cd Desktop/ com code-connect, que é o nome do projeto:

cd Desktop/code-connect/
COPIAR CÓDIGO
Em seguida, rodamos o seguinte comando para subir o nosso servidor Next:

npm run dev
COPIAR CÓDIGO
Ao terminar a execução, o retorno nos indicará a porta de execução do servidor. Clicamos no link http://localhost:3000 para abrir.

Com isso, uma nova janela do navegador se abre e carrega o nosso projeto. Não há nada na página além do fundo preto e a logo do CodeConnect no canto superior esquerdo, mas não há nenhum erro do Next, pelo menos.

Vamos clicar na página com o botão direito e selecionar a opção "Inspecionar". O console nos diz que vieram atributos extras do lado do servidor.

Vamos observar o nosso arquivo de log (error.log) no VS Code para verificar se está tudo certo como deveria. Ele está limpo, então está tudo certo.

O que aconteceu aqui foi que chamamos a função e ela não retornou post nenhum, pois não temos posts cadastrados no nosso banco de dados. A nossa tabela de posts está limpa. Se voltarmos ao nosso DBeaver, dentro da tabela de Posts, na aba de Data, verificaremos que ela está vazia.

O que temos que fazer é povoar esses dados de alguma forma. É muito comum em uma aplicação desse tipo querermos que desde o início existam dados disponíveis, sejam dados reais que vão ser utilizados em produção, por exemplo, ou dados fictícios que podem ser usados para testes ou para o próprio desenvolvimento.

Para montar essa estrutura de dados já inseridos no banco de dados, existe uma funcionalidade do Prisma que se chama Seed, que significa "semear" os dados da nossa aplicação. Em algum momento do desenvolvimento, vamos precisar "pré-popular" essas tabelas, e é isso que vamos fazer no próximo vídeo.

Até lá!

@@04
Seed de dados

Agora precisamos semear o nosso banco de dados com as informações - seja de autor, seja de post - que precisamos neste momento.
Instrução para o Seeding
Na documentação do Prisma, há uma seção específica sobre Seeding (semeadura), abordando como configuramos e montamos a estrutura que vai semear esses dados.

A primeira sugestão é criar uma instrução para o próprio Prisma, dentro do arquivo package.json, sobre o que ele deve executar quando o comando prisma db seed for executado.

Devemos adicionar uma entrada no package.json chamada prisma, que é um objeto, contendo um comando chamado seed que vai executar um arquivo .js.

"prisma": {
  "seed": "node prisma/seed.js"
},
COPIAR CÓDIGO
Atenção: a documentação possui instruções tanto para TypeScript quanto para JavaScript. Estamos usando o JS puro, então precisamos selecionar os códigos corretos para isso.
Vamos copiar esse código da documentação, abrir o VS Code, fechar todos os arquivos que estão abertos e acessar o nosso package.json.

Na linha 10, onde acaba a nossa instrução de script, vamos colar o que copiamos da documentação. Esse prisma executa um seed, que é o nome do comando que o prisma vai procurar, que contém uma instrução: pedir para o node executar um arquivo chamado seed.js, que está dentro de uma pasta chamada prisma.

Vamos criar esse arquivo, então. Podemos copiar seed.js do código e colar no nome de um novo arquivo criado dentro da pasta "prisma", que está na raiz do nosso projeto.

Arquivo seed.js
Vamos voltar para a documentação, porque ela tem um exemplo de como semear o nosso banco de dados.

O exemplo de prisma é parecido com o nosso, exceto pelos campos, que são um pouco diferentes. Mais abaixo na página, verificamos que o exemplo de arquivo seed.js tem uma função main() criando usuários e posts e, no final, executa esse main().

Documentação do Prisma - seed.js
const { PrismaClient } = require('@prisma/client')
const prisma = new PrismaClient()

async function main() {
  const alice = await prisma.user.upsert({
    where: { email: 'alice@prisma.io' },
    update: {},
    create: {
      email: 'alice@prisma.io',
      name: 'Alice',
      posts: {
        create: {
          title: 'Check out Prisma with Next.js',
          content: 'https://www.prisma.io/nextjs',
          published: true,
        },
      },
    },
  })

  const bob = await prisma.user.upsert({
    where: { email: 'bob@prisma.io' },
    update: {},
    create: {
      email: 'bob@prisma.io',
      name: 'Bob',
      posts: {
        create: [
          {
            title: 'Follow Prisma on Twitter',
            content: 'https://twitter.com/prisma',
            published: true,
          },
          {
            title: 'Follow Nexus on Twitter',
            content: 'https://twitter.com/nexusgql',
            published: true,
          },
        ],
      },
    },
  })
  console.log({ alice, bob })
}
main()
  .then(async () => {
    await prisma.$disconnect()
  })
  .catch(async (e) => {
    console.error(e)
    await prisma.$disconnect()
    process.exit(1)
  })
COPIAR CÓDIGO
Vamos copiar todo o código do arquivo seed.js da documentação e colá-lo no nosso arquivo seed.js no VS Code. No entanto, não queremos criar esses usuários alice e bob que estão no exemplo, então vamos limpar, deixando apenas um create com um objeto vazio na busca pela usuária ana.

Repare que esse código tem uma função upsert(), que é um pouco diferente. Basicamente, ela quer dizer que ou ele vai atualizar ou ele vai inserir esse usuário no banco de dados, dependendo do resultado da nossa cláusula de busca. Essa busca será feita a partir do campo Username, o campo único na nossa tabela. Então vamos substituir email por username.

Atenção: Para executar essa função e ela funcionar corretamente, sem erros, temos que fazer essa consulta em cima de um campo único.
seed.js
async function main() {
    const ana await prisma.user.upsert({
        where: username: 'alice@prisma.io'
        update: {},
        create: {},
    })
}
main()
  .then(async () => {
    await prisma.$disconnect()
  })
  .catch(async (e) => {
    console.error(e)
    await prisma.$disconnect()
    process.exit(1)
  })
COPIAR CÓDIGO
Semeando os dados
Agora precisamos digitar um monte de dados relacionados a Ana e aos posts. Para evitar toda essa digitação, disponibilizamos a massa de dados que vamos utilizar neste Gist, para você copiar e colar no seu código.

No começo desse Gist, temos a criação da Ana Beatriz. Começaremos por ela. Vamos copiar, retornar ao nosso seed.js e colar esses dados logo abaixo do main():

async function main() {

    const author = {
        name: "Ana Beatriz",
        username: "anabeatriz_dev",
        avatar: "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/authors/anabeatriz_dev.png",
    };
COPIAR CÓDIGO
Podemos trocar o username dado em const ana, que não é alice.prisma, mas o author.username - esseusername que está definido na linha 8, que pegamos da nossa massa de dados. Então: where: { username: author.username }.

E o que queremos fazer? Se for update, não queremos fazer nada, porque se a Ana Beatriz já está cadastrada no banco de dados, vamos deixá-la como está. Se não, vamos passar o nosso author para a instrução de create.

const ana = await prisma.user.upsert({
        where: { username: author.username },
        update: {},
        create: author,
})
COPIAR CÓDIGO
Além disso, além de criar a Ana, podemos criar uma massa de dados de posts. Vamos usar uma estrutura separada para isso, porque você pode querer manipular essa estrutura, criando os seus posts e os seus autores. Então, para ficar mais legível, vamos trazer o array de posts, disponibilizado no nosso Gist.

Esse array de JSON começa na linha 7 e termina na 104 do Gist. Vamos copiar e colar no VS Code, logo depois de fazer o upsert da ana. Depois, vamos pedir para o VS Code formatar o documento.

const posts = [
            {
                    "cover": "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts/introducao-ao-react.png",
                    "title": "Introdução ao React",
                    "slug": "introducao-ao-react",
                    "body": "Neste post, vamos explorar os conceitos básicos do React, uma biblioteca JavaScript para construir interfaces de usuário. Vamos cobrir componentes, JSX e estados.",
                    "markdown": "```javascript\nfunction HelloComponent() {\n  return <h1>Hello, world!</h1>;\n}\n```",
                    "authorId": ana.id
            },
            {
                    "cover": "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts/css-grid-na-pratica.png",
                    "title": "CSS Grid na Prática",
                    "slug": "css-grid-na-pratica",
                    "body": "Aprenda a criar layouts responsivos com CSS Grid. Este post aborda desde a definição de grid até a criação de layouts complexos de forma simples e eficaz.",
                    "markdown": "```css\n.grid-container {\n  display: grid;\n  grid-template-columns: auto auto auto;\n}\n```",
                    "authorId": ana.id
            },
            {
                    "cover": "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts/vuejs-para-iniciantes.png",
                    "title": "Vue.js para Iniciantes",
                    "slug": "vuejs-para-iniciantes",
                    "body": "Vue.js é um framework progressivo para a construção de interfaces de usuário. Este guia inicial cobre as funcionalidades essenciais do Vue.",
                    "markdown": "```javascript\nnew Vue({\n  el: '#app',\n  data: {\n    message: 'Olá Vue!'\n  }\n})\n```",
                    "authorId": ana.id
            },
            {
                    "cover": "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts/dicas-de-acessibilidade-web.png",
                    "title": "Dicas de Acessibilidade Web",
                    "slug": "dicas-de-acessibilidade-web",
                    "body": "Explorando a importância da acessibilidade na web, este post oferece dicas práticas para tornar seus sites mais acessíveis a todos os usuários.",
                    "markdown": "```html\n<a href=\"#\" aria-label=\"Saiba mais sobre acessibilidade\">Saiba mais</a>\n```",
                    "authorId": ana.id
            },
            {
                    "cover": "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts/introducao-ao-typescript.png",
                    "title": "Introdução ao TypeScript",
                    "slug": "introducao-ao-typescript",
                    "body": "Este post é um guia introdutório ao TypeScript, explicando como ele aumenta a produtividade e melhora a manutenção do código JavaScript.",
                    "markdown": "```typescript\nfunction greeter(person: string) {\n  return 'Hello, ' + person;\n}\n```",
                    "authorId": ana.id
            },
            {
                    "cover": "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts/otimizacao-de-performance-no-react.png",
                    "title": "Otimização de Performance no React",
                    "slug": "otimizacao-de-performance-no-react",
                    "body": "Discutindo técnicas avançadas para otimizar a performance de aplicações React, este post aborda conceitos como memoização e lazy loading.",
                    "markdown": "```javascript\nconst MemoizedComponent = React.memo(function MyComponent(props) {\n  /* render using props */\n});\n```",
                    "authorId": ana.id
            },
            {
                    "cover": "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts/explorando-flexbox-no-css.png",
                    "title": "Explorando Flexbox no CSS",
                    "slug": "explorando-flexbox-no-css",
                    "body": "Este post detalha o uso do Flexbox para criar layouts responsivos e flexíveis no CSS, com exemplos práticos para um entendimento fácil.",
                    "markdown": "```css\n.flex-container {\n  display: flex;\n  justify-content: space-around;\n}\n```",
                    "authorId": ana.id
            },
            {
                    "cover": "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts/angular-primeiros-passos.png",
                    "title": "Angular: Primeiros Passos",
                    "slug": "angular-primeiros-passos",
                    "body": "Ideal para iniciantes, este post introduz o Angular, um poderoso framework para desenvolvimento de aplicações web, com um exemplo básico.",
                    "markdown": "```typescript\n@Component({\n  selector: 'my-app',\n  template: '<h1>Olá Angular</h1>'\n})\nexport class AppComponent { }\n```",
                    "authorId": ana.id
            },
            {
                    "cover": "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts/gerenciamento-de-estado-com-redux.png",
                    "title": "Gerenciamento de Estado com Redux",
                    "slug": "gerenciamento-de-estado-com-redux",
                    "body": "Abordando um dos aspectos cruciais no desenvolvimento de aplicações React, este post ensina como gerenciar o estado de forma eficiente com Redux.",
                    "markdown": "```javascript\nconst reducer = (state = initialState, action) => {\n  switch (action.type) {\n    case 'ACTION_TYPE':\n      return { ...state, ...action.payload };\n    default:\n      return state;\n  }\n};\n```",
                    "authorId": ana.id
            },
            {
                    "cover": "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts/sass-simplificando-o-css.png",
                    "title": "Sass: Simplificando o CSS",
                    "slug": "sass-simplificando-o-css",
                    "body": "Este post explora como o pré-processador Sass pode simplificar e melhorar a escrita de CSS, através de variáveis, mixins e funções.",
                    "markdown": "```scss\n$primary-color: #333;\nbody {\n  color: $primary-color;\n}\n```",
                    "authorId": ana.id
            },
            {
                    "cover": "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts/webpack-um-guia-para-iniciantes.png",
                    "title": "Webpack: Um Guia para Iniciantes",
                    "slug": "webpack-um-guia-para-iniciantes",
                    "body": "Aprenda a configurar o Webpack, uma poderosa ferramenta de empacotamento de módulos, neste guia passo a passo para iniciantes.",
                    "markdown": "```javascript\nmodule.exports = {\n  entry: './path/to/my/entry/file.js'\n};\n```",
                    "authorId": ana.id
            },
            {
                    "cover": "https://raw.githubusercontent.com/viniciosneves/code-connect-assets/main/posts/construindo-spa-com-vuejs.png",
                    "title": "Construindo SPA com Vue.js",
                    "slug": "construindo-spa-com-vuejs",
                    "body": "Este post oferece um tutorial detalhado sobre como construir uma Single Page Application (SPA) eficiente e interativa usando o framework Vue.js.",
                    "markdown": "```javascript\nnew Vue({\n  el: '#app',\n  data: {\n    message: 'Bem-vindo à sua SPA Vue.js!'\n  }\n});\n```",
                    "authorId": ana.id
            }
    ];
COPIAR CÓDIGO
Repare que dentro dessa massa de dados que trouxemos, o authorId vai ser o ana.id. Esse ana é o resultado do nosso upsert, onde inserimos ou atualizamos a Ana, que é a nossa autora.

Agora, temos que fazer uma iteração. Para isso, vamos chamar o posts.forEach(). Vamos passar uma função assíncrona para ele, com async () => {}. Teremos acesso ao post, ou seja, vamos percorrer todos eles, e faremos um await do nosso prisma.post.upsert() - ou seja, vamos aguardá-lo.

Esse upsert vai receber um objeto, e vamos começar a construi-lo de forma muito parecida com a da documentação.

Nossa cláusula de where vai ter um objeto, e o campo único do post é o slug. Então: where: { slug: post.slug }.

Repare que IntelliSense, o autocomplete do VS Code, está sendo feito, por debaixo dos panos, pela extensão do Prisma que instalamos em um vídeo anterior, junto do prisma generate. Ou seja, o Prisma toma todo esse cuidado com a tipagem.
Voltando para a nossa cláusula de where, vamos procurar pelo slug e olhar para o post.slug. Ou seja: vamos pegar o slug do post da vez e verificar se já existe algum no banco de dados.

Se for o caso, vamos passar um update. Se for um update, não queremos fazer nada além de manter o original no banco de dados. Mas, se ele não existir, queremos criar. E, para o create, passamos o post em si, cuja classe estamos percorrendo.

posts.forEach(async (post) => {
        await prisma.post.upsert({
                where: { slug: post.slug },
                update: {},
                create: post
        })
})
COPIAR CÓDIGO
Agora podemos adicionar um pouco de informação para nós, pessoas que estão desenvolvendo esse código.

Na linha 16, depois de criar a Ana e antes do const posts, vamos chamar um console.log de 'Author created' (autor criado), que foi a Ana, e vamos passar como segundo parâmetro a própria ana:

console.log('Author created', ana)
COPIAR CÓDIGO
No final do código, apenas antes de main(), vamos fazer outro console.log dizendo 'Seed OK', porque criamos um número indeterminado de posts e não queremos poluir o nosso console.

console.log('Seed OK')
COPIAR CÓDIGO
Repare que estamos usando o console porque isso não vai ser executado dentro do contexto da nossa aplicação.

Executando e testando o seed
Vamos abrir agora outro terminal no VS Code e digitar o comando que consultamos na documentação:

npx prisma db seed
COPIAR CÓDIGO
O prisma db seed vai procurar no package.json a instrução que acabamos de definir. Nela, estamos dizendo que vamos pedir para o Node executar o seed.js que está dentro da pasta prisma, ou seja, o arquivo que acabamos de escrever.

Agora vamos executar o comando e verificar se isso vai funcionar do jeito que esperávamos ou se vamos ter que lidar com algum erro.

Ao pressionar "Enter", o console diz: "Estou rodando o comando seed". Depois, imprime "Author created", conforme o nosso console.log, e os dados da Ana. No final temos o "Seed OK", ou seja, aparentemente não tivemos erro.

Podemos fechar o terminal. Antes de passar para o DBeaver para verificar se os dados estão lá, repare que a última parte do arquivo é o que trouxemos da documentação:

main()
    .then(async () => {
        await prisma.$disconnect()
    })
    .catch(async (e) => {
        console.error(e)
        await prisma.$disconnect()
        process.exit(1)
    })
COPIAR CÓDIGO
A documentação sugeriu a criação de uma função assíncrona chamada main, onde escrevemos todo o nosso upsert, e na linha 129, chamamos essa função e fazemos um .then para desconectar do banco e, se der algum erro, fazemos um .catch com o console.error do que aconteceu, nos desconectamos do banco e saímos do processo Node.

Se formos ao DBeaver agora, dentro da tabela de Posts, na aba de Data, observaremos os 12 posts que trouxemos da nossa estrutura. Agora podemos confirmar se a nossa aplicação vai exibir esses dados.

De volta ao terminal local, vamos executar o comando npm run dev novamente apenas para garantir que o servidor está rodando.

De volta ao navegador, na página do CodeConnect, vamos recarregar a página. Tomamos um erro do Next dizendo: "Não foi possível ler propriedades de undefined; estou tentando ler avatar".

O Next especifica: "No componente CardPost/index, você está fazendo post.author.avatar, e não existe author". Ou seja: o Prisma não trouxe o autor do post.

Repare que na nossa página raiz (arquivo page.js no VS Code), onde fizemos o db.post.findMany, em nenhum momento dissemos para o Prisma que queríamos incluir esse autor.

Então, além de corrigir esse erro, temos que melhorar a experiência do usuário. Afinal, enquanto estamos desenvolvendo, visualizar o erro na página é legal. Mas para a pessoa usuária fica apenas uma tela preta. Não enviamos nenhuma informação para a pessoa que está lendo aquele blog de que algo deu errado.

Então, temos duas coisas para fazer: melhorar a experiência da pessoa leitora do blog e tratar, de fato, o erro. Vamos fazer isso no próximo vídeo!

https://www.prisma.io/docs/orm/prisma-migrate/workflows/seeding

@@05
Melhor experiência para o usuário

Pensando na experiência das pessoas leitoras do nosso blog, o CodeConnect, vamos criar uma página de erro personalizada.
A documentação do Next.js possui uma seção específica sobre como tratar erros de tempo de execução, ou seja, erros que acontecem enquanto a aplicação está rodando. Basicamente, é nisso que precisamos prestar atenção enquanto pessoas desenvolvedoras Next.

Criando uma página de erro
No VS Code, dentro da nossa estrutura do App Router (pasta "src > app"), podemos criar páginas específicas para os erros que ocorrerem.

Por exemplo, temos a pasta "posts" e temos a pasta raiz no projeto. Se ocorrer um erro específico na página de post, podemos mostrar uma página de erro para isso, e ela ficara na pasta "posts". Se ocorrer algum erro mais generalizado, vamos para a pasta raiz e pegamos a página de erro no nível mais alto.

O ponto importante é que, dentro da pasta "app", ele vai procurar um arquivo chamado error.js. Vamos criá-lo.

Há mais um detalhe sobre isso, baseado na documentação. Na versão do JS, encontramos um exemplo de arquivo error.js. Na primeira linha desse código, a instrução número 1 diz: componentes de erro precisam ser componentes do lado do cliente ('use client' // Error components must be Client Components).

Ou seja, esses componentes são diferentes do que estávamos fazendo até agora. Tanto que, na linha 3 do exemplo, temos um useEffect, um código React - como já sabemos, com base nas nossas formações anteriores.

Vamos copiar esse exemplo de código e colar no VS Code, no arquivo error.js que criamos dentro de "app".

Vamos fazer algumas mudanças.

Esse código está tentando fazer uma recuperação se algum erro acontecer, por meio do reset. Mas não queremos fazer o Try Again ("tentar de novo"), então vamos tirar toda a tag button do código, além do reset na função Error.

Podemos deixar o h2 com a mensagem "Something went wrong!" ("Algo deu errado"). A única coisa que vamos fazer é colocar um style para a fonte ficar branca e conseguirmos ler no nosso fundo escuro. Então, color: 'white'.

error.js
'use client' // Error components must be Client Components
 
import { useEffect } from 'react'
 
export default function Error({ error }) {
  useEffect(() => {
    // Log the error to an error reporting service
    console.error(error)
  }, [error])
 
  return (
    <div>
      <h2 style=({color: 'white'})>Something went wrong!</h2>
    </div>
  )
}
COPIAR CÓDIGO
Criamos o arquivo error.js com a mensagem de que algo deu errado, e também fizemos console.error. Esse é o console do navegador, porque está rodando do lado do cliente, e ele vai mostrar para a pessoa usuária o que foi esse erro.

Voltando ao CodeConnect no navegador, vamos fazer uma Inspeção. Vamos limpar o Console, na parte inferior da aba de inspeção, e recarregar a página.

A mensagem "Something went wrong!" é exibida em letras brancas no canto superior esquerdo da tela, ao lado da logo. Vários erros são impressos no console, então o nosso console.log está funcionando também.

O erro foi renderizado. Agora podemos tratá-lo e até colocar em uma página mais elegante.

Tratando o erro
Vamos migrar para a documentação do Prisma.

Quando temos um relacionamento entre modelos e queremos incluir o relacionamento, temos que passar uma propriedade chamada include para o nosso findMany, para dizer o que queremos incluir ou não.

Vamos voltar ao VS Code. No nosso cenário do page.js, que está na raiz do nosso "app", vamos passar um objeto para o findMany e chamar o include, que também vai receber um objeto.

Se pressionarmos "Ctrl + Espaço" dentro desse objeto, o VS Code consegue sugerir que um relacionamento que um post tem é o autor. Então, o que podemos incluir é o author. Então:

page.js
const posts = await db.post.findMany({
    include: {
        author: true
    }
})
COPIAR CÓDIGO
Ou seja: para cada post que vier, incluimos o relacionamento com o autor.

Voltamos ao CodeConnect no navegador e já podemos observar os posts sendo exibidos! Podemos até recarregar a página para ter certeza.

Repare que ele está exibindo os posts corretamente, sem aquele erro de leituras de propriedade de undefined, porque agora, para cada post, a propriedade avatar está preenchida porque incluímos o nosso autor. O que o Prisma fez por debaixo dos panos foi selecionar o autor de cada post.

Desafio
Antes de seguir para a próxima aula, propomos um desafio. Na página de error.js, deixamos um h2 com um estilo marretado e um texto genérico. Não é isso que deveríamos exibir para a nossa pessoa leitora. O que queremos fazer é exibir uma página de erro bem bonita.

No Figma do nosso projeto, temos duas páginas de erro desenhadas pela nossa designer (um abraço para a Isa!). Ela montou a página de erro 404, de página não encontrada ou inexistente, e uma página de erro genérico, que não estávamos esperando que acontecesse no ambiente produtivo.

Então, a nossa página de error.js deve adotar esse segundo estilo.

Deixaremos mais detalhes na atividade de texto desta aula, com link para download da imagem e algumas instruções a mais para esse desafio.

Adiantamos que, para forçar o erro, você pode remover o include da page.js. Ao fazer isso, ele vai tentar ler o "avatar of undefined" e vai quebrar.

O desafio está lançado! Exercite e pratique! Estaremos te esperando na próxima aula.

Até lá!

https://nextjs.org/docs/app/building-your-application/routing/error-handling

@@06
Mão na massa: estilizando páginas de erro

Bora praticar CSS? Temos que deixar nossa página de erro bem estilizada, igual temos no Figma:
A imagem exibe uma tela de erro em um website ou aplicativo. No topo, há uma ilustração de um robô humanoide com um semblante pensativo, apoiando o queixo com a mão, em um fundo que sugere um ambiente mecânico ou industrial. Abaixo da ilustração, o texto centralizado em branco diz "Opa! Um erro ocorreu." Segue-se uma explicação menor: "Não conseguimos carregar a página, volte para seguir navegando." Abaixo deste texto, há um link estilizado como um botão que diz "Voltar ao feed", indicando uma opção para o usuário retornar à página anterior ou principal do site ou aplicativo. O esquema de cores da página é escuro, com o robô e o texto em destaque sobre o fundo.

Você vai precisar dessa imagem:

500.png
E do ícone da seta para a esquerda:

export const ArrowBack = ({color = '#132E35'}) => {
    return <svg width="13" height="13" viewBox="0 0 13 13" fill="none" xmlns="http://www.w3.org/2000/svg">
        <path d="M12.5117 5.76172V7.23828H3.37109L7.55469 11.457L6.5 12.5117L0.488281 6.5L6.5 0.488281L7.55469 1.54297L3.37109 5.76172H12.5117Z" fill={color} />
    </svg>
}
COPIAR CÓDIGO

https://www.figma.com/file/k2uVg4mzbUmXMwHFQKzfLr/CodeConnect--%7C-Formação-Next?node-id=622%3A1688&mode=dev

Então, como foi? Deixou a página super estilosa? Para resolver esse desafio eu criei uma pasta para conter os ícones da aplicação. Também criei um componente chamado Heading, já que esses estilos se repetem em outros lugares do Code Connect.
O CSS da página de erro eu coloquei dentro de src/app/error/error.module.css.

Se quiser conferir na íntegra o código da minha versão, dá uma olhada aqui no GitHub.

https://github.com/alura-cursos/3498-next-14-ssr-codeconnect-parte-2/commit/39cc8d01c433385b80e41304eaf750e135562d98#diff-819fd744fd6aa4e9b10df12228d62ba30246cc70df1dc8a7d699cec1ebbcc380

@@07
Para saber mais: ORMs

Bora falar de Object-Relational Mapping, mais conhecido como ORM, uma ferramenta essencial no desenvolvimento de software. A criação dos ORMs foi motivada pela necessidade de superar as diferenças entre os sistemas de banco de dados relacional e a programação orientada a objetos, que são duas formas bastante distintas de pensar sobre dados.

Um ORM serve como uma ponte entre esses dois mundos, permitindo que desenvolvedores manipulem o banco de dados usando a linguagem de programação de sua escolha (no nosso caso, JavaScript), sem ter que escrever consultas SQL complexas e sem se preocupar com as diferenças de tipos de dados. Isso não só economiza um tempo valioso, mas também diminui a probabilidade de erros que podem ocorrer ao traduzir manualmente entre esses dois sistemas.

Além disso, os ORMs promovem um código mais limpo e organizado, encorajando o uso de boas práticas de programação, como o princípio DRY (Don't Repeat Yourself) e a separação de preocupações. Isso é particularmente útil em projetos grandes e complexos, onde a manutenção do código pode se tornar um desafio.

Outro aspecto importante dos ORMs é a sua capacidade de abstrair as especificidades do banco de dados. Isso significa que o mesmo código ORM pode ser usado com diferentes sistemas de banco de dados com poucas ou nenhuma modificação, tornando as aplicações mais portáveis e flexíveis.

Esse assunto é tão bacana que o meu amigo Gui Lima gravou esse Alura+ sobre o assunto:

ORM: O que é?
Vida longa e próspera.

@@08
Relacionamentos no Prisma

Considerando uma query do Prisma, qual das seguintes alternativas é a mais eficiente para recuperar informações de usuários, seus posts, e comentários relacionados utilizando o Prisma?
Selecione uma alternativa

Utilizar findMany com include para buscar usuários, seus posts e comentários em uma única query.
 
Esta abordagem é eficiente porque permite recuperar todas as informações necessárias em uma única operação de banco de dados, reduzindo o overhead de múltiplas queries e mantendo a simplicidade do código.
Alternativa correta
Aplicar o método aggregate para contabilizar os dados antes de buscar as informações detalhadas de usuários, posts e comentários.
 
Alternativa correta
Implementar raw SQL queries para obter os dados necessários em uma chamada ao banco de dados.
 
Alternativa correta
Usar findUnique em um loop para cada usuário e seus respectivos posts e comentários

@@09
Faça como eu fiz: queries e tratamento de erros

Seguindo com a nossa refatoração, precisamos evoluir o Code Connect para obter os dados do postgres ao invés da API rest. Vamos de novo definir 3 passos?
Gerar o Prisma Client e escrever a query para obter os dados
Montar e executar o nosso seed de dados
Criar a página de erro proposta no desafio
Bora lá? Lembrando que essa é a hora de praticar para consolidar o conhecimento. Se precisar de ajuda, lembre-se que temos o fórum e o discord esperando por você!

Como sempre, teu amigo aqui, o dev careca e barbudo que você mais gosta, não vai te deixar na mão!
Aqui no GitHub você consegue pegar direitinho todas as alterações que eu fiz em aula.

https://github.com/alura-cursos/3498-next-14-ssr-codeconnect-parte-2/compare/aula-1...aula-2

@@10
O que aprendemos?

Nessa aula, você aprendeu como:
Obter dados utilizando o método findMany;
Incluir relacionamentos de forma automática via Prisma;
Configurar e executar seed de dados com o prisma db seed.
