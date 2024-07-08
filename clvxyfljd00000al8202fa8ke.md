---
title: "Traefik v3.0 já está disponível e inclui suporte para Kubernetes Gateway API, WebAssembly e mais. Veja aqui como atualizar para a v3.0"
datePublished: Wed May 08 2024 15:08:36 GMT+0000 (Coordinated Universal Time)
cuid: clvxyfljd00000al8202fa8ke
slug: traefik-v30-ja-esta-disponivel-e-inclui-suporte-para-kubernetes-gateway-api-webassembly-e-mais-veja-aqui-como-atualizar-para-a-v30
canonical: https://sredevops.org/br/traefik-v3-0-ja-esta-disponivel-e-inclui-suporte-para-kubernetes-gateway-api-webassembly-e-mais-veja-aqui-como-atualizar-para-a-v3-0/
cover: https://cdn.hashnode.com/res/hashnode/imageupload/v1715180915990/c80c19ac-ead4-43ee-a27a-362f27e15c63.png
tags: cloud, apps, opensource, kubernetes, devops, sre, web-assembly, brasil

---

![Traefik v3.0 já está disponível e inclui suporte para Kubernetes Gateway API, WebAssembly e mais. Veja aqui como atualizar para a v3.0](https://cdn.hashnode.com/res/hashnode/imageupload/v1715180915399/07dbb4c8-3532-4413-81b3-c16c678e2fa8.png)

Este ano marca o nono aniversário do Traefik e hoje ele se tornou um dos _gateways_ modernos mais utilizados, com mais de 3 bilhões de downloads e mais de 750 colaboradores. Está no Top 15 do DockerHub e tem 47.000 estrelas no GitHub. **Aqui no SREDevOps.org, o Traefik é nosso IngressController favorito e já o utilizamos em nosso Cluster Kubernetes com k3s v1.29.4**.

O Traefik 1.0 foi lançado em 2016. Três anos depois, nasceu o Traefik 2.0. Hoje, após 5 anos de desenvolvimento, o Traefik 3.0 está disponível para o público em geral 🎉

**O Traefik v3.0 é um grande passo adiante no mundo cloud native, adicionando suporte para as mais recentes tecnologias como WASM, OpenTelemetry, Kubernetes Gateway API e SPIFFE.**

O registro de mudanças no Github é impressionante, com mais de 200 _pull requests_ mesclados, cada um com novos recursos. A lista de novas possibilidades é tão grande que a [TraefikLabs publicará uma série de artigos em seu blog](https://traefik.io/blog/?ref=sredevops.org), cada um aprofundando um recurso.

Como posso migrar do Traefik v2 para o v3?
------------------------------------------

Uma nova _major version_ é sempre algo muito aguardado: novo design, novos recursos, melhor experiência do usuário... Mas a desvantagem costuma ser a dor e os riscos da migração. Uma versão principal geralmente significa mudanças de última hora, mas isso não deveria implicar uma experiência de migração dolorosa, e com o **Traefik v3 é muito fácil migrar e atualizar**.

O Traefik v3 inclui um processo de transição simplificado a partir do v2. Como lembrete, o Traefik tem 2 tipos de configurações:

### A _configuração estática_ que é carregada quando o Traefik é iniciado e gerencia as opções globais

**Exemplo: Traefik + Kubernetes Ingress**

    
    ## Static configuration
    
    ## Todas as opções: 
    ## https://doc.traefik.io/traefik/providers/kubernetes-ingress/#provider-configuration
    
    providers:
      kubernetesIngress:
        namespaces:
          - "mynamespace"
    

    ## Exemplo de Kubernetes Ingress
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: myingress
      namespace: mynamespace
      annotations:
        # Traefik Entrypoint configurado na porta 80
        traefik.ingress.kubernetes.io/router.entrypoints: web 
        # Traefik Entrypoint configurado na porta 443
        traefik.ingress.kubernetes.io/router.entrypoints: websecure
        
    spec:
      rules:
        - host: example.com
          http:
            paths:
              - path: /bar
                pathType: Exact
                backend:
                  service:
                    name:  whoami
                    port:
                      number: 80 # Porta do serviço whoami
              - path: /foo
                pathType: Exact
                backend:
                  service:
                    name:  whoami
                    port:
                      number: 80 # Porta do serviço whoami
    
    

### A _configuração dinâmica_ pode ser atualizada enquanto o Traefik está em execução e contém todas as regras de roteamento

Foram feitas alterações mínimas em opções específicas da configuração estática, e é garantida a compatibilidade com a sintaxe do v2 na configuração dinâmica. Isso oferecerá uma mudança gradual para adotar a sintaxe v3, permitindo que os usuários migrem progressivamente seus _IngressResources_ do Kubernetes, tags do Docker, etc. para o novo formato.

Exemplo: Como migrar o Traefik do v2.11 para o v3.0 no Kubernetes
-----------------------------------------------------------------

⚠️ Certifique-se de realizar este exemplo em um ambiente de teste. Pessoalmente, recomendo usar o [****Lima (Linux Machines) e k3s****](https://github.com/lima-vm/lima/blob/master/examples/k3s.yaml?ref=sredevops.org)****, compatível com macOS e Linux, de uso muito fácil e rápido.****

### Passo 1: Preparar e testar

A primeira coisa a fazer é identificar como as mudanças feitas no v3 afetam sua configuração estática. As mudanças incompatíveis são mínimas e direcionadas a opções muito específicas. Em 90% dos casos de uso, esse processo deve levar apenas alguns minutos.

**Alguns exemplos de mudanças incompatíveis:**

Recurso

Deprecated

Remoção

Kubernetes CRD Provider API Version `traefik.io/v1alpha1`

3.0

4.0

Kubernetes Ingress API Version `networking.k8s.io/v1beta1`

N/A

3.0

CRD API Version `apiextensions.k8s.io/v1beta1`

N/A

3.0

**Você também deve considerar:**

*   Docker e Swarm agora são 2 providers distintos
*   HTTP/3 não é mais uma opção experimental
*   Rancher v1 foi abandonado, pois o projeto não é mais mantido ativamente, etc.

[Consulte a documentação sobre configuração estática](https://doc.traefik.io/traefik/migration/v2-to-v3/?ref=sredevops.org#static-configuration), bem como a seção de operações, para obter a lista completa e preparar sua nova configuração estática v3.

**Adicione este snippet à sua configuração estática atual para usar a sintaxe v2 por padrão em seus recursos atuais que usam o Traefik:**

    core:
      defaultRuleSyntax: v2
    

Quando estiver pronto para testar, inicie o Traefik v3 com essa nova configuração. Se você não obtiver nenhum registro de erro, está tudo certo. Caso contrário, não há problema, as opções de migração restantes são explicadas nos logs do Traefik e você pode aplicá-las.

Quando seu ambiente de teste não mostrar erros do Traefik no v3 e o acesso aos seus aplicativos, APIs, serviços, etc. estiver funcionando, você pode passar para a próxima etapa.

### Passo 2: Rolling Update

Agora que você testou sua configuração estática atualizada, é hora de migrar progressivamente suas instâncias de produção para o Traefik v3. Use o mecanismo de _rolling update_ do Kubernetes para substituir gradualmente os Pods atuais pelos novos.

⚠️ Antes de ativar qualquer alteração na produção, certifique-se de ****ter monitoramento sobre o tráfego de entrada**** para detectar instantaneamente qualquer problema. O Traefik é compatível com diferentes métodos de observabilidade, como ****OpenTelemetry, Prometheus e outros.****

Enquanto a atualização contínua estiver em andamento, monitore constantemente o tráfego de entrada em busca de erros inesperados (ou incomuns), **e tenha um rollback preparado** para voltar à configuração funcional. Em seguida, aproveite o logging, registros de depuração e acesso do Traefik para entender e solucionar o problema antes de atualizar novamente. Caso não tenha certeza, consulte a documentação ou [comente em nosso Discord](https://discord.com/invite/bK9rXFTvpk?ref=sredevops.org).

Depois que todos os pods forem atualizados... 🎉🎊🍾 você pode passar para a última etapa.

### Passo 3: Atualização Progressiva dos Ingresses

Agora que você está executando o Traefik v3, comece a migrar seus _ingresses_ para o novo formato.

📍 Lembre-se de que esta etapa pode ser feita posteriormente, pois o Traefik v3 é compatível com o formato v2 da __configuração dinâmica__. Mas, é claro, você pode começar a usar os novos recursos imediatamente nos novos __ingresses__ e migrar os __ingresses__ mais antigos depois.

> A configuração dinâmica no v3 tem algumas mudanças. Por exemplo, os _Router Rule Matchers_ têm uma sintaxe atualizada, o _Kubernetes Ingress API Group_ foi modificado e a opção TCP LoadBalancer `terminationDelay` foi removida. A lista completa pode ser encontrada na seção de configuração dinâmica da documentação de migração.

Progressivamente, altere cada roteador para a sintaxe v3, teste e atualize cada _IngressResource_ e verifique se o tráfego de entrada não é afetado. Depois de validar a migração de um _IngressResource_ v3, você pode remover o _IngressResource_ v2 e implantar a versão v3. Repita para cada _IngressResource_.

No final do processo, você pode remover com segurança o snippet adicionado no passo 1:

    core:
      defaultRuleSyntax: v2
    

...e pronto, você já está totalmente migrado para o Traefik v3 🎉! E você fez isso de forma progressiva, mantendo o controle durante todo o processo, com a opção de reverter qualquer alteração a qualquer momento.

Este exemplo com Kubernetes pode ser feito em qualquer orquestrador ou ambiente, o processo permanece o mesmo.

Em breve, publicaremos mais artigos sobre o suporte a WASM (e o plugin Web Application Firewall), OpenTelemetry, SPIFFE/Tailscale/HTTP/3 e Kubernetes Gateway API.

Links úteis
-----------

*   Traefik 3.0 no [GitHub](https://github.com/traefik/traefik/releases/tag/v3.0.0?ref=sredevops.org) & no [DockerHub](https://hub.docker.com/_/traefik?ref=sredevops.org)
*   [Documentação do Traefik](https://doc.traefik.io/traefik/?ref=sredevops.org), [Site](https://traefik.io/?ref=sredevops.org), & [GitHub](https://github.com/traefik/traefik?ref=sredevops.org)
*   [Fórum da comunidade](https://community.traefik.io/?ref=sredevops.org)