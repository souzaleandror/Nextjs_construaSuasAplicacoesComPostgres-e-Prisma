##### 14/07/2024

Curso de Next.js: construa suas aplicações com Postgres e Prisma

```
docker compose up -d
docker compose down
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