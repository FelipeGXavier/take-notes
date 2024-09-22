## OAuth/extensões e terminologias

O OAuth 2.0 é um protocolo de autorização que padroniza o fluxo de permissões entre dois sistemas. Ele define os formatos e regras que devem ser seguidos para que as partes possam trocar informações de forma segura. Geralmente, o cenário envolve um serviço A querendo acessar recursos protegidos de um serviço B. Dependendo do contexto, existem diferentes fluxos de autorização, como o caso de aplicações que rodam no navegador, comunicação entre servidores (server-to-server), entre navegador e servidor (browser-to-server), ou até mesmo quando o servidor não está associado a uma identidade de usuário específica. Além do OAuth em si há outras extensões dele como o OIDC (OpenID Connect) e o UMA (User Managed Access).

Resource owner: seria o usuário final muita das vezes, é o detentor da informação que está protegida em um sistema e outros querem interagir.

Authorization server: é o servidor que o usuário (resource owner) interage para consentir uma série de escopos para que uma aplicação possa interagir, é ele que emite o token de acesso.

Resource server: as vezes o mesmo servidor que o authorization server é ele provém uma API para interagir com os recursos protegidos pelo token emitido e consetido ao sistema.

Client: é a aplicação que quer interagir com o sistema sendo integrado

Redirect URI: é a URL geralmente redirecionada para o client quando as permissões são consentidas a partir do authorization server

Access token: é o token emitido pelo authorization server e que é enviado ao resource server para interagir com os recursos

Fluxos: existem 4 principais fluxos

- Implicit flow: quando a aplicação sendo integrada roda estritamente no navegador. Nesse cenário após o authorization server retorna o access token direto ao client rodando do navegador que também fará as requisições a partir do próprio navegador.
- Authorization code: o mais comum, nesse cenário o authorization server retorna um authorization code ao client da aplicação, normalmente o navegador, que é enviado a seu serviço backend que juntamente com o secret é utilizado para então emitir o access token
- Resource owner credentials: nesse fluxo o usuário informa diretamente as credenciais de acesso no client que troca pelo token de acesso ao authorization server. Esse fluxo só é recomendado se o client for conhecido e seguro.
- Client credentials: nesse fluxo geralmente o client é um serviço sem identidade como um daemon, job, serviço de background etc. o token é emitido através do client id + client secret

OpenID Connect: é uma extensão em cima do OAuth 2.0 que adiciona uma camada para lidar com autenticação. Nesse cenário o authorization server (analogamente) retorna um id token, um token JWT contendo informações de perfil sobre o usuário, e por vezes também um access token. Esse id token pode ser utilizado para obter as informações de perfil assim como ser utilizado como autenticidade verificando a assinatura do token para saber se é válido ou não.

UMA: é um protocolo direcionado a autorização de recursos gerenciado pelo papel do resource owner. É criado em cima do OAuth onde o resource owner define regras de como e de qual forma um recurso deve ser protegido.

RBAC: é um modelo de controle de acesso baseado em papéis, roles, onde são definidas permissões atrelando usuário a roles. Essas informaçẽos são utilizadas para assegurar se um recurso pode ser acessado ou não.