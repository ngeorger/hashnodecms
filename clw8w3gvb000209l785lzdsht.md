---
title: "Kubernetes Gateway API v1.1: Service mesh, GRPCRoute e muito mais"
datePublished: Thu May 16 2024 06:48:39 GMT+0000 (Coordinated Universal Time)
cuid: clw8w3gvb000209l785lzdsht
slug: kubernetes-gateway-api-v11-service-mesh-grpcroute-e-muito-mais
canonical: https://sredevops.org/br/kubernetes-gateway-api-v1-1-service-mesh-grpcroute-e-muito-mais/
cover: https://cdn.hashnode.com/res/hashnode/imageupload/v1715842118683/238679b3-9c2c-4e89-84d8-d2ffa15d0a19.png
tags: cloud, news, opensource, kubernetes, brasil

---

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute e muito mais](https://cdn.hashnode.com/res/hashnode/imageupload/v1715842114411/bd444288-6033-4719-be49-89ea67f264fd.png)

Após o lançamento GA do Gateway API em outubro passado, o Kubernetes SIG Network anuncia o lançamento da versão v1.1 do [Gateway API](https://gateway-api.sigs.k8s.io/?ref=sredevops.org). Neste lançamento, várias características passam a fazer parte do _Canal Padrão_ (GA), incluindo o suporte para service mesh e GRPCRoute. Também introduzem algumas características novas e experimentais, como persistência de sessão e verificação de certificados de cliente.

Novidades
---------

### Passagem para o Canal Padrão

Este lançamento inclui a passagem para o Canal Padrão de quatro características muito aguardadas. Isso significa que já não são conceitos experimentais; a inclusão no Canal Padrão denota um alto nível de confiança na API e proporciona garantias de compatibilidade. Claro, como qualquer outra API do Kubernetes, as características do Canal Padrão podem continuar evoluindo com adições retrocompatíveis e certamente virão mais refinamentos e melhorias para essas novas características no futuro. Para mais informações sobre como tudo isso funciona, consulte a [Política de Versionamento do Gateway API](https://gateway-api.sigs.k8s.io/concepts/versioning/?ref=sredevops.org).

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute e muito mais](https://cdn.hashnode.com/res/hashnode/imageupload/v1715842115216/3e801d47-8682-4b36-a640-c6d0d4c23a33.svg)

### [Suporte para Service Mesh](https://gateway-api.sigs.k8s.io/mesh/?ref=sredevops.org)

O suporte para service mesh no Gateway API permite aos usuários utilizarem a mesma API para gerenciar o tráfego de entrada e o _tráfego de malha_, reutilizando as mesmas interfaces de política e roteamento. No Gateway API v1.1, as rotas (como HTTPRoute) agora podem ter um Serviço como `parentRef`, para controlar como o tráfego se comporta para serviços específicos. Para mais informações, leia a [documentação de service mesh do Gateway API](https://gateway-api.sigs.k8s.io/mesh/?ref=sredevops.org) ou consulte a [lista de implementações do Gateway API](https://gateway-api.sigs.k8s.io/implementations/?ref=sredevops.org#service-mesh-implementation-status).

[

Traefik v3.0 já está disponível e inclui suporte para Kubernetes Gateway API, WebAssembly e mais. Veja aqui como atualizar para a v3.0

Este ano marca o nono aniversário do Traefik e hoje ele se tornou um dos gateways modernos mais utilizados, com mais de 3 bilhões de downloads e mais de 750 colaboradores. Está no Top 15 do DockerHub e tem 47.000 estrelas no GitHub. Aqui no SREDevOps.org, o Traefik

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute e muito mais](https://cdn.hashnode.com/res/hashnode/imageupload/v1715842115384/ae5e6d4c-e884-4fa6-8ceb-a2710a5ccee5.ico)SREDevOps.org - SRE, DevOps, Cloud, LinuxNicolás Georger

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute e muito mais](https://cdn.hashnode.com/res/hashnode/imageupload/v1715842116739/73373b3f-b49f-4982-ad4d-e97d5fb6c558.png)

](https://sredevops.org/br/traefik-v3-0-ja-esta-disponivel-e-inclui-suporte-para-kubernetes-gateway-api-webassembly-e-mais-veja-aqui-como-atualizar-para-a-v3-0/)

Como exemplo, pode-se fazer um deployment Canary de uma workload com um HTTPRoute da seguinte maneira:

    apiVersion: gateway.networking.k8s.io/v1
    kind: HTTPRoute
    metadata:
      name: color-canary
      namespace: faces
    spec:
      parentRefs:
        - name: color
          kind: Service
          group: ""
          port: 80
      rules:
      - backendRefs:
        - name: color
          port: 80
          weight: 50
        - name: color2
          port: 80
          weight: 50

Isso dividiria o tráfego enviado ao Serviço `color` no namespace `faces` 50/50 entre o Serviço `color` original e o Serviço `color2`, utilizando uma configuração portátil que é fácil de mover de uma malha para outra.

### [GRPCRoute](https://gateway-api.sigs.k8s.io/guides/grpc-routing/?ref=sredevops.org)

Se você já está utilizando a versão experimental do GRPCRoute, recomendamos esperar antes de atualizar para a versão do canal padrão do GRPCRoute até que os controladores que você está utilizando tenham sido atualizados para suportar a versão v1 do GRPCRoute. Até lá, é seguro atualizar para a versão experimental do GRPCRoute em v1.1 que inclui as versões de API v1alpha2 e v1.

### [Porta em ParentReference](https://gateway-api.sigs.k8s.io/reference/spec/?ref=sredevops.org#gateway.networking.k8s.io%2fv1.ParentReference)

Foi adicionado o campo `port` ao ParentReference, o que permite anexar recursos aos _**GatewayListeners**_, Serviços ou outros recursos principais (dependendo da implementação). Vincular a uma porta também permite anexar a múltiplos _Listeners_ ao mesmo tempo.

Por exemplo, você pode anexar um `HTTPRoute` a um ou mais `Listeners` específicos de um Gateway de acordo com o campo `port` do Listener, em vez do campo `name` do Listener.

Para mais informações, consulte [Attach to Gateways.](https://gateway-api.sigs.k8s.io/api-types/httproute/?ref=sredevops.org#attaching-to-gateways)

[

HTTPRoute - Kubernetes Gateway API

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute e muito mais](https://cdn.hashnode.com/res/hashnode/imageupload/v1715842117207/2d816fad-531d-46f9-8c87-32aa3bbb9bec.png)logo

![Kubernetes Gateway API v1.1: Service mesh, GRPCRoute e muito mais](https://cdn.hashnode.com/res/hashnode/imageupload/v1715842117430/8e436da3-f10b-47d8-8837-9c2f39cc5045.svg)

](https://gateway-api.sigs.k8s.io/api-types/httproute/?ref=sredevops.org#attaching-to-gateways)

### [Perfis e Relatórios de _Conformance_](https://gateway-api.sigs.k8s.io/concepts/conformance/?ref=sredevops.org#conformance-profiles)

A API Conformance foi ampliada com o campo `mode` (destinado a especificar o modo da implementação) e o `gatewayAPIChannel` (padrão ou experimental). Tanto `gatewayAPIVersion` quanto `gatewayAPIChannel` agora são preenchidos automaticamente pela API Machinery, junto com uma breve descrição do resultado dos testes. Os Relatórios foram reorganizados de uma maneira mais estruturada, e as implementações agora podem adicionar informações sobre como os testes foram executados e fornecer passos de reprodução.

Novas características no Canal Experimental
-------------------------------------------

### [Verificação de Certificados de Cliente em Gateway](https://gateway-api.sigs.k8s.io/geps/gep-91/?ref=sredevops.org)

Os Gateways agora podem configurar a verificação de certificados de cliente para cada Gateway`Listener,` através da introdução de um novo campo `frontendValidation` dentro de `tls`. Este campo suporta a configuração de uma lista de _Certificados CA_ **(`Certificate Authority`)** que podem ser utilizados como um ponto de confiança para validar os certificados apresentados pelo cliente.

✅ **Exemplo de validação de certificado de cliente no Gateway**

    apiVersion: gateway.networking.k8s.io/v1beta1
    kind: Gateway
    metadata:
      name: my-gateway
      namespace: default
    spec:
      gatewayClassName: my-gateway-class
      listeners:
        - name: https
          protocol: HTTPS
          port: 443
          tls:
            mode: Terminate
            certificateRefs:
              - name: example-cert
            options:
              group: example.com
              kind: TLSValidation
              name: validation
          frontendValidation:
            certificateRefs:
              - name: client-ca

### [Persistência de Sessão](https://gateway-api.sigs.k8s.io/geps/gep-1725/?ref=sredevops.org)

A persistência de sessão permite que os usuários controlem o comportamento de balanceamento de carga para sessões específicas, garantido que o tráfego seja roteado consistentemente para o mesmo backend. O campo `sessionAffinity` foi introduzido para `HTTPRoute` e `TLSRoute` com suporte inicial para afinidade baseada em cookies.

    apiVersion: gateway.networking.k8s.io/v1beta1
    kind: HTTPRoute
    metadata:
      name: example
    spec:
      rules:
        - matches:
            - path:
                value: "/"
          backendRefs:
            - name: foo
              port: 8080
          sessionAffinity:
            cookie:
              name: my-cookie
              path: "/"
              maxAge: 3600

Para mais informações, consulte a documentação de [persistência de sessão do Gateway API](https://gateway-api.sigs.k8s.io/concepts/persistencia-de-sess%C3%A3o/?ref=sredevops.org).

Obrigado!
---------

Estamos empolgados com este novo lançamento do Gateway API e com a incrível comunidade que tornou isso possível. Para mais informações, consulte a documentação do [Gateway API](https://gateway-api.sigs.k8s.io/?ref=sredevops.org).