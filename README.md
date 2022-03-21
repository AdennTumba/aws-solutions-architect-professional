# Aws Solutions Architect Professional - SAA-C02

Este repositório tem como objetivo servir como guia de estudos para a certificação aws solutions architect professional.

## Topicos 
1. [IAM](#IAM)
2. [EC2](#EC2)
3. [S3](#S3)
4. [VPC](#VPC)
5. [ROUTE 53](#ROUTE53)


# IAM 
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

# S3 
## S3 (Simple Storage Service)

- S3 (Simple Storage Service) é o serviço de armazenamento em nuvem padrão da AWS, oferecendo armazenamento de arquivos ("blob" opaco) de números arbitrários de arquivos de quase qualquer tamanho.

    - Namespace global (os nomes dos bucket devem ser exclusivos)
    - O tamanho mínimo dos objetos é 0 bytes, o máximo é 5 TB
    - Por padrão  o S3 tem  disponibilidade de 4 9s (99,99%), e durabilidade de 11 9s (99,999999999%)
    - Os arquivos podem ser carregados em buckets que nada mais são do que pastas que hospedam os arquivos
    - No upload bem-sucedido, S3 retorna um código de status HTTP 200
    - Por padrão, todos os bucket são privados

## S3 Consistency Model

- O Amazon S3 oferece uma forte consistência de leitura após gravação para solicitações de PUT e DELETE de objetos no bucket do Amazon S3 em todas as Regiões da AWS. Isso se aplica a ambas as gravações em novos objetos, bem como solicitações PUT que sobrescrevem objetos existentes e solicitações DELETE

- Consistência de leitura após gravação para novos objetos PUT (objetos recém-carregados têm a garantia de serem lidos imediatamente, sem qualquer estado desatualizado ou problemas)

- Consistência eventual para sobrescrever PUTs e DELETEs (modificações / exclusões eventualmente refletirão o estado mais recente --- pode haver um atraso de alguns segundos)

- O Amazon S3 atinge alta disponibilidade replicando dados entre vários servidores nos datacenters da AWS. Se uma solicitação PUT for bem-sucedida, os dados serão armazenados com segurança. Qualquer leitura (solicitação de GET ou LIST) iniciada após o recebimento de uma resposta PUT bem-sucedida retornará os dados escritos pelo PUT. Veja alguns exemplos desse comportamento:
    - Um processo grava um novo objeto no Amazon S3 e imediatamente lista as chaves em seu bucket. O novo objeto aparecerá na lista.
    - Um processo substitui um objeto existente e imediatamente tenta lê-lo. O Amazon S3 retorna os novos dados.
    - Um processo exclui um objeto existente e imediatamente tenta lê-lo. O Amazon S3 não retorna dados porque o objeto foi excluído.
    - Um processo exclui um objeto existente e imediatamente lista as chaves em seu bucket. O objeto não aparecerá na listagem.

## S3 Object Properties

- Key: Nome do objeto
- Value: Sequência de bytes / conteúdo do objeto
- Versioning: Versão do objeto
- Metadata: Os objetos podem ainda ter metadados (dados sobre dados) para classificar o conteúdo

## S3 Storage Tiers

- S3 Standard: O S3 Standard oferece um armazenamento de objetos com altos níveis de resiliência, disponibilidade e performance para dados acessados com frequência. Como fornece baixa latência e alto throughput, o S3 Standard é adequado para uma grande variedade de casos de uso, como aplicativos na nuvem, sites dinâmicos, distribuição de conteúdo, aplicativos móveis e de jogos e dados analíticos de big data.

    ### Key Features:
    
    - Performance com baixa latência e alto throughput
    - Projetada para fornecer 99,999999999% de resiliência de objetos em várias zonas de disponibilidade
    - Resiliência contra eventos que causam impactos em uma zona de disponibilidade inteira
    - Projetada para fornecer disponibilidade de 99,99% em um determinado ano
    - Suporte a SSL para dados em trânsito e criptografia para dados ociosos
    - Gerenciamento de ciclo de vida do S3 para migração automática de objetos a outras classes de armazenamento do S3 

- S3 Intelligent Tiering: A Amazon S3 Intelligent-Tiering (S3 Intelligent-Tiering) é o primeiro armazenamento em nuvem que reduz automaticamente os custos de armazenamento em um nível de objeto granular, movendo automaticamente os dados para o nível de acesso mais econômico com base na frequência de acesso, sem impacto sobre a performance, taxas de recuperação ou sobrecarga operacional. A S3 Intelligent-Tiering oferece latência em milissegundos e alta performance de taxa de transferência para dados acessados com muita frequência, com pouca frequência e agora raramente acessados nos níveis Frequent Access, Infrequent Access e o novo Archive Instant Access. Agora, você pode usar a S3 Intelligent-Tiering como a classe de armazenamento padrão para praticamente qualquer workload, especialmente data lakes, análise de dados, novas aplicações e conteúdo gerado pelo usuário.

    ### Key Features:

    - Os níveis de acesso frequente e infrequente têm a baixa latência e a alta performance de taxa de transferência iguais às do S3 Standard
    - O nível de acesso infrequente economiza até 40% em custos de armazenamento
    - O nível Archive Instant Access economiza até 68% em custos de armazenamento
    - Ativa recursos de arquivamento assíncrono automático opcionais para objetos que se tornam raramente acessados
    - Os níveis de acesso para arquivamento e acesso para arquivamento profundo têm o mesmo desempenho que as classes Glacier e Glacier Deep Archive e economizam até 95% para objetos raramente acessados
    - Sem sobrecarga operacional, sem encargos de ciclo de vida, sem encargos de recuperação e sem duração mínima de armazenamento

- S3 IA (Infrequently accessed): O S3 Standard-IA é indicado para dados acessados com menos frequência, mas que exigem acesso rápido quando necessários. A categoria S3 Standard – IA oferece os altos níveis de resiliência e throughput e a baixa latência da categoria S3 Standard, com taxas reduzidas por GB de armazenamento e GB de recuperação. A combinação de baixo custo e alta performance tornam a classe S3 Standard-IA ideal para armazenamento de longa duração, backups e datastores para arquivos de recuperação de desastres.

    ### Key Features:

    - A mesma performance de baixa latência e alto throughput da categoria S3 Standard
    - Projetada para fornecer 99,999999999% de resiliência de objetos em várias zonas de disponibilidade
    - Resiliência contra eventos que causam impactos em uma zona de disponibilidade inteira
    - Resiliência de dados em caso de destruição de uma zona de disponibilidade inteira
    - Projetada para fornecer disponibilidade de 99,9% em um determinado ano
    - Suporte a SSL para dados em trânsito e criptografia para dados ociosos
    - Gerenciamento de ciclo de vida do S3 para migração automática de objetos a outras classes de armazenamento do S3

- S3 IA 1 Zone: O S3 One Zone-IA é indicado para dados acessados com menos frequência, mas que exigem acesso rápido quando necessários. Ao contrário de outras classes de armazenamento do S3, que armazenam dados em no mínimo três Zonas de disponibilidade (AZs), a S3 One Zone-IA armazena dados em uma única AZ, com um custo 20% inferior ao S3 Standard-IA. A classe S3 One Zone-IA é ideal para clientes que querem uma opção de menor custo para dados acessados com pouca frequência, mas não precisam da disponibilidade e da resiliência S3 Standard ou S3 Standard-IA. É uma ótima opção para armazenar cópias de backup secundária de dados locais ou que possam ser recriados com facilidade. 

    ### Key Features:

    - A mesma performance de baixa latência e alto throughput da categoria S3 Standard
    - Projetada para fornecer 99,999999999% de resiliência de objetos em uma única zona de disponibilidade.
    - Projetada para fornecer disponibilidade de 99,5% em um determinado ano
    - Suporte a SSL para dados em trânsito e criptografia para dados ociosos
    - OBS: Como a categoria S3 One Zone – IA armazena dados em uma única zona de disponibilidade da AWS, os dados armazenados nessa categoria de armazenamento serão perdidos em caso de destruição da zona de disponibilidade.

- S3 Glacier Instant Retrieval: Amazon S3 Glacier Instant Retrieval é uma nova classe de armazenamento de arquivos que oferece o armazenamento de custo mais baixo para dados de longa duração, que raramente são acessados e exigem recuperação em milissegundos. Com a S3 Glacier Instant Retrieval, você pode economizar até 68% nos custos de armazenamento em comparação com o uso da classe de armazenamento S3 Standard-Infrequent Access (S3 Standard-IA), quando seus dados são acessados uma vez por trimestre. O S3 Glacier Instant Retrieval é ideal para arquivar dados que precisam de acesso imediato, como imagens médicas, recursos de mídia de notícias ou arquivos de conteúdo gerado pelo usuário.

    ### Key Features:

    - Recuperação de dados em milissegundos com o mesmo desempenho do S3 Standard
    - Projetada para fornecer 99,999999999% de resiliência de objetos em várias zonas de disponibilidade
    - Resiliência de dados em caso de destruição de uma zona de disponibilidade inteira
    - Projetado para 99,9% de disponibilidade de dados em um determinado ano
    - 128 KB de tamanho mínimo do objeto

- S3 Glacier Flexible Retrieval (anteriormente S3 Glacier): O S3 Glacier Flexible Retrieval oferece armazenamento de baixo custo, custo até 10% menor (do que o S3 Glacier Instant Retrieval), para dados de arquivamento que são acessados 1 a 2 vezes por ano e recuperados de forma assíncrona. Para dados de arquivo que não exigem acesso imediato, mas precisam de flexibilidade para a recuperação de grandes conjuntos de dados sem custo, como casos de uso de backup ou recuperação de desastres, o S3 Glacier Flexible Retrieval (a antiga S3 Glacier) é a classe de armazenamento ideal. A S3 Glacier Flexible Retrieval oferece o maior número de opções de velocidade de recuperação que equilibram o custo com tempos de acesso que variam de minutos a horas e com recuperações gratuitas em massa.  uma solução ideal para backup, recuperação de desastres, necessidades de armazenamento externo de dados e para quando alguns dados ocasionalmente precisam ser recuperados em minutos e você não quer se preocupar com custos.

    ### Key Features:

    - Projetada para fornecer 99,999999999% de resiliência de objetos em várias zonas de disponibilidade
    - Resiliência de dados em caso de destruição de uma zona de disponibilidade inteira
    - Suporte a SSL para dados em trânsito e criptografia para dados ociosos
    - Ideal para casos de uso de backup e recuperação de desastres quando grandes conjuntos de dados ocasionalmente precisam ser recuperados em minutos, sem preocupação com custos
    - Tempos de recuperação configuráveis, de minutos a horas, com recuperações em massa gratuitas

- S3 Glacier deep archive: O S3 Glacier Deep Archive é a classe de armazenamento mais barata do Amazon S3 e oferece suporte à retenção e preservação digitais de longo prazo para dados que podem ser acessados uma ou duas vezes por ano. Essa classe é projetada para clientes que mantêm conjuntos de dados por 7 a 10 anos ou mais para cumprir requisitos de conformidade normativa, especialmente em setores altamente regulados como serviços financeiros, saúde e setores públicos.

    ### Key Features:

    - Projetada para fornecer 99,999999999% de resiliência de objetos em várias zonas de disponibilidade
    - A classe de armazenamento com custo mais baixo projetada para retenção de dados em longo prazo que serão mantidos por 7 a 10 anos.
    - Alternativa ideal às bibliotecas de fitas magnéticas
    - Tempo de recuperação de até 12 horas
    - API PUT do S3 para uploads diretos ao S3 Glacier Deep Archive e gerenciamento de ciclo de vida do S3 para migração automática de objetos

## S3 vs Glacier, EBS, and EFS:

- A AWS oferece muitos serviços de armazenamento e vários, além do S3, oferecem abstrações de tipo de arquivo. 
- O Glacier é para armazenamento de arquivos mais barato e raramente acessado. 
- O EBS, ao contrário do S3, permite acesso aleatório ao conteúdo do arquivo por meio de um sistema de arquivos tradicional, mas só pode ser anexado a uma instância do EC2 por vez. 
- EFS é um sistema de arquivos de rede ao qual muitas instâncias podem se conectar, mas a um custo mais alto.

## S3 Security Policies

- O acesso ao objeto / intervalo S3 pode ser controlado com ACL control lists ou Bucket Policies
- As Policies funcionam no nível do bucket , mas as ACL control lists podem ir até objetos individuais
- O log de acesso pode ser configurado para buckets S3, que registra todas as solicitações de acesso para S3, feitas por diferentes usuários
- A criptografia em trânsito é obtida por HTTPS (SSL / TLS)

A criptografia em repouso é obtida de duas maneiras

Criptografia do lado do serviço (pode ser gerenciada posteriormente pela AWS de três maneiras)

    1) Keys managed by S3 service for encryption (SSE-S3)

    2) Keys provisioned by user in KMS (SSE-KMS)

    3) User/Customer provided encryption keys can also be used (SSE-C)

Criptografia do lado do cliente, o próprio cliente gerencia a criptografia / descriptografia e carrega apenas os dados criptografado

## S3 Transfer Acceleration

- O S3 Transfer Acceleration é usado para acelerar o upload de grandes dados para o S3. Com isso, o usuário pode carregar os dados para o ponto de presença mais próximo, e o S3 garantirá que os dados sejam replicados para o bucket real para armazenamento final. Para Edge Location -> S3, a AWS usará a rede de backbone, que é bastante rápida do que a velocidade normal da Internet

## S3 Cross Region Replication

- O S3 Cross Region Replication é usado quando você deseja replicar o conteúdo de um bucket automaticamente para outro bucket (o bucket de destino também pode impor diferentes níveis de armazenamento nos objetos)
- O controle de versão deve ser habilitado obrigatoriamente nos bucket de origem e destino
- Excluir marcadores ou exclusões de objetos não são replicadas no bucket de destino. Isso é feito intencionalmente pela Amazon para replicar inadvertidamente a exclusão de objetos.

## O CloudFront

- O Amazon CloudFront é um serviço da web que acelera a distribuição do conteúdo estático e dinâmico da web, como arquivos .html, .css, .js e arquivos de imagem, para os usuários. O CloudFront distribui seu conteúdo por meio de uma rede global de datacenters denominados pontos de presença. Ao solicitar um conteúdo que você está veiculando com o CloudFront, o usuário é roteado para o ponto de presença com a menor latência (atraso de tempo) para que o conteúdo seja fornecido com o melhor desempenho possível.

- Se o conteúdo já estiver no ponto de presença com a menor latência, o CloudFront o entregará imediatamente.

- Se o conteúdo não estiver nesse ponto de presença, o CloudFront recuperará de uma origem que você definiu—como um bucket do Amazon S3, um canal do MediaPackage ou um servidor HTTP (por exemplo, um servidor web), que você identificou como a fonte para a versão definitiva do seu conteúdo.

- Por exemplo, suponha que você esteja exibindo uma imagem de um servidor web tradicional, e não do CloudFront. Por exemplo, você pode fornecer uma imagem, adenntumba.png, usando a URL http://example.com/adenntumba.png.

- Seus usuários podem navegar facilmente para esse URL e ver a imagem. Mas provavelmente não sabem que sua solicitação foi roteada de uma rede para outra—por meio do conjunto complexo de redes interconectadas que compõem a Internet—até a imagem ser encontrada.

- O CloudFront acelera a distribuição do seu conteúdo encaminhando cada pedido de usuário por meio da rede backbone da AWS para o ponto de presença que veicule melhor seu conteúdo. Normalmente, esse é um servidor de ponto do CloudFront que fornece a entrega mais rápida ao visualizador. Usar a rede da AWS reduz drasticamente o número de redes pelas quais as solicitações dos usuários devem passar, melhorando o desempenho. Os usuários obtêm menos latência—o tempo que leva para carregar o primeiro byte do arquivo—e taxas de transferência de dados maiores.

- Você também pode obter mais confiabilidade e disponibilidade porque as cópias de seus arquivos (também conhecidos como objetos) agora são mantidos (ou armazenados em cache) em vários pontos de presença em todo o mundo.

## Storage Gateway

- O Storage Gateway é um conjunto de serviços de nuvem híbrida que oferece acesso on-premises a armazenamento na nuvem praticamente ilimitado. Os clientes usam o Storage Gateway para integrar o armazenamento da Nuvem AWS com workloads locais para que possam simplificar o gerenciamento do armazenamento e reduzir os custos de casos de uso de armazenamento fundamentais na nuvem híbrida. 

- Esses casos de uso incluem mover os backups para a nuvem, usar compartilhamentos de arquivo on-premises com respaldo do armazenamento na nuvem e fornecer acesso de baixa latência aos dados na AWS para aplicações on-premises.

- Para oferecer suporte a esses casos de uso, o Storage Gateway oferece quatro tipos diferentes de gateways: 
    - Amazon S3 File Gateway 
    - Amazon FSx File Gateway
    - Tape Gateway 
    - Volume Gateway

## AWS Snowball

- O AWS Snowball é um serviço que fornece dispositivos robustos e seguros para que você possa utilizar os recursos de armazenamento e computação da AWS em seus ambientes de borda e transferir dados de e para a AWS. Esses dispositivos robustos são comumente chamados de dispositivos AWS Snowball ou AWS Snowball Edge.


# Route 53
## Route 53 

- O Amazon Route 53 é um web service Domain Name System (DNS) na nuvem altamente disponível e escalável. Ele foi projetado para oferecer aos desenvolvedores e empresas uma maneira altamente confiável e econômica de direcionar os usuários finais aos aplicativos de Internet, convertendo nomes como www.example.com para endereços IP numéricos como 192.0.2.1, usados pelos computadores para se conectarem entre si. O Amazon Route 53 também é totalmente compatível com o IPv6.

- O sistema DNS da internet funciona praticamente como uma agenda de telefone ao gerenciar o mapeamento entre nomes e números. Os servidores DNS convertem solicitações de nomes em endereços IP, controlando qual servidor um usuário final alcançará quando digitar um nome de domínio no navegador da web. Essas solicitações são chamadas consultas.

### Tipos de serviço DNS
- DNS autoritativo: um DNS autoritativo disponibiliza um mecanismo de atualização que os desenvolvedores usam para gerenciar seus nomes DNS públicos. Em seguida, ele responde a consultas do DNS, convertendo nomes de domínio em endereço IP de forma que os computadores possam se comunicar entre si. O DNS autoritativo tem a autoridade final sobre o domínio, além de ser o responsável pela disponibilização de respostas para os servidores DNS recursivos com informações de endereço IP. O Amazon Route 53 é um sistema DNS autoritativo.

- DNS recursivo: geralmente, os clientes não fazem consultas diretamente para os serviços DNS autoritativos. Em vez disso, eles se conectam de modo geral a outro tipo de serviço DNS conhecido como resolvedor ou serviço DNS recursivo. Um serviço DNS recursivo age como o concierge de um hotel: embora não tenha nenhum registro DNS, ele atua como um intermediário que pode obter informações de DNS por você. Se um DNS recursivo tiver a referência do DNS armazena em cache, ou armazenada durante um período, ele responderá a consulta do DNS ao disponibilizar as informações sobre a origem ou o IP. Caso contrário, ele encaminhará a consulta para um ou mais servidores DNS autoritativos para encontrar as informações.

### Existem diferentes tipos de registros usados ​​no sistema DNS:
- SOA records
    - O registro SOA especifica informações autoritativas sobre uma zona DNS, incluindo o servidor de nome primário e o e-mail do administrador do domínio.

- A records
    - O registro A é o tipo principal, conhecido também como registro do host. Ele faz o vínculo do domínio com o endereço IP do servidor físico que hospeda o site e outros serviços desse domínio.

- CNAME records
    - O registro CNAME, conhecido também como registro de nome canônico, faz o vínculo de um alias de domínio ou subdomínio para um outro domínio, ou seja, nunca devem apontar para número IP.

- MX records
    - O registro MX é responsável por direcionar os emails do domínio (ex: contato@dominio.com.br) para o servidor que hospeda todas as contas e configurações relacionadas. Você pode, por exemplo, usar um serviço exclusivo de email em um servidor e deixar os arquivos do site em outro.

- PTR records
    - Um registro de ponteiro (PTR) DNS fornece o nome de domínio associado a um endereço de IP. Um registro PTR DNS é exatamente o oposto do registro "A", que fornece o endereço de IP associado a um nome de domínio. O seja, Os registros PTR atribuem endereços IP a um nome de servidor, em vez de associar um nome de servidor a um endereço IP.

- Alias records
    - Um registro ALIAS é um tipo de registro DNS que aponta seu nome de domínio para um nome de host em vez de um endereço IP.

- NS records
    - O registro no nameserver indica qual servidor de DNS é autoritativo para o domínio em questão (por exemplo, qual servidor contém os registros DNS reais). Basicamente, os registros no NS dizem à internet onde encontrar o endereço IP de um domínio. 

 ### O que é o TTL?
 - O Time to Live ou simplesmente o TTL, como objetivo terminar o tempo de expiração de um determinado registro DNS. O TTL é utlizado para informar a um DNS Resolver. Como por exempo o (8.8.8.8). No entanto, o tempo deve manter o registro em cache. Quanto maior o TTL, maior será o tempo em cache. 

### Diferentes tipos de policies de resolução suportadas pelo Route53
- Simple Routing Policy
    - O Simple Routing Policy, também pode fornecer vários endereços IP, mas não pode associar a verificação de integridade de todos os endereços IP. O AWS Route 53 responde às consultas de DNS com base nos valores no conjunto de registros de recursos. Por exemplo, endereço IP em um registro A

- Weighted Routing Policy
    - Usando o Weighted Routing Policy, pode-se rotear o tráfego com base no peso atribuído a diferentes endereços IP. Por exemplo, 70% do tráfego para IP X e outros 30% para IP Y, que é então roteado/distribuído proporcionalmente.

- Latency Based Routing Policy
    - O Latency Based Routing Policy como o proprio nome ja diz, é baseada na latência de resposta do local do usuário. Usando essas informações, a solicitação pode ser roteada para diferentes servidores.

- Failover Based Routing Policy
    - O Failover Based Routing Policy, funciona como um ativo, um passivo. Se a verificação de integridade ativa começar a falhar, o tráfego roteará para a configuração passiva.

- Geographical Routing Policy
    - Com Geographical Routing Policy, podemos definir que o usuário na América do Sul deve ser roteado apenas para o servidor da América de Sul. No entanto isso é diferente da política de roteamento baseada em latência, e é mais codificado.

- Geographical Proximity Routing Policy
    - O Geographical Proximity Routing Policy, é usado  quando quisermos encaminhar o tráfego com base no local de seus recursos e, opcionalmente, alternar o tráfego de recursos em um local para recursos em outro local.

- Multivalue Answer Routing Policy
    - O Multivalue Answer Routing Policy, é usado quando quiser que o Route 53 responda a consultas de DNS com até oito registros íntegros selecionados aleatoriamente.