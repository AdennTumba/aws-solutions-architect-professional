# Aws Solutions Architect Professional - SAA-C02

Este repositório tem como objetivo servir como guia de estudos para a certificação aws solutions architect professional.

## Topicos 
1. [IAM](#IAM)

## IAM (Identity and Access management)
- IAM é o serviço que você usa para gerenciar contas e permissões para AWS.
- O IAM, como outros serviços da AWS, é eventualmente consistente, pois esses dados são replicados em vários servidores. IAM é um serviço global (sem escopo por região). 
- As identidades do IAM incluem usuários (pessoas ou serviços que estão usando AWS), grupos (contêineres para conjuntos de usuários e suas permissões) e roles (contêineres para permissões atribuídas a instâncias de serviço da AWS). As permissões para essas identidades são regidas por policies. Você pode usar policies predefinidas da AWS ou policies personalizadas criadas por você.

    - Usuarios - pode ser um usuário IAM ou uma aplicação acessando os recursos da AWS.
    - Grupos  - grupo de usuários que podem ter permissão coletiva de algumas ações, serviços etc.
    - Roles - Isso é algo que pode ser assumido por uma entidade como usuário ou grupo (isso é semelhante à autenticação)
    - Policies - define as permissões que você tem nos recursos que deseja acessar (semelhante à autorização).

- As políticas podem ser gerenciadas de duas maneiras:
    - Policies gerenciadas: AWS gerenciado e gerenciado pelo cliente
    - Policies em linha
  
- As políticas podem ser de dois tipos:

    - Policies baseadas em identidade: Estas são anexadas diretamente a Identidades como Usuário / Grupos, etc. Elas podem ser gerenciadas ou policies embutidas.

    - Policies baseadas em recursos: são policies em linha, aplicadas diretamente no recurso que deve ser acessado da mesma / de outras contas. Isso é usado principalmente para acesso a recursos entre contas. Ex: Bucket S3.

    - Limite de permissões: Este tipo de policie define o número máximo de permissões que as policies baseadas em identidade podem conceder a uma entidade.  

    - Policies baseadas em sessão: Esta policies transmite em um parâmetro ao criar de forma programática uma sessão temporária para uma role ou usuário federado.

- IAM tem a hierarquia de permissão de:
    - Explicit deny: a política mais restritiva vence.
    - Explicit allow: as permissões de acesso a qualquer recurso devem ser fornecidas explicitamente.
    - Implicit deny: todas as permissões são negadas implicitamente por padrão.

- A permissão padrão da AWS é deny. Ou seja, tudo que precisar ser feito precisará ser liberado. 

- Controle de versão de política: as políticas gerenciadas pelo cliente normalmente podem ter apenas 5 versões gerenciadas em um único ponto do tempo. Isso é útil quando você faz uma alteração em uma política e isso quebra algo, você pode definir rapidamente a configuração padrão para uma política usada anteriormente.

