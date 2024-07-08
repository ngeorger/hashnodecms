---
title: "O WebAssembly está pronto para a produção? WASI Preview 2 torna isso possível"
datePublished: Mon Apr 15 2024 12:08:50 GMT+0000 (Coordinated Universal Time)
cuid: clv0wvt4i000108js55swg1vj
slug: o-webassembly-esta-pronto-para-a-producao-wasi-preview-2-torna-isso-possivel
canonical: https://sredevops.org/br/o-webassembly-esta-pronto-para-a-producao-wasi-preview-2-torna-isso-possivel/
cover: https://cdn.hashnode.com/res/hashnode/imageupload/v1713182929313/cd9c68e3-1fe1-4084-960c-850a1732864d.png
tags: cloud, news, opensource, web-assembly, brasil

---

> Resumo:  
>   
> O WebAssembly, ou Wasm, há muito tempo promete revolucionar o desenvolvimento de aplicativos. O sonho era simples, mas poderoso (e familiar): escrever seu código uma vez e fazê-lo funcionar em qualquer lugar. O Wasm também prometia executar o código em velocidades quase nativas. Melhor ainda, ao contrário das tentativas anteriores de tempos de execução universais, como o Java, o Wasm oferecia garantias de segurança mais altas e uma postura de segurança padrão de negação por padrão (o acesso aos recursos do sistema é desativado, a menos que seja orientado de outra forma por um desenvolvedor).  
> Até recentemente, a realidade do Wasm estava aquém de seu potencial prometido. Ainda faltavam as ferramentas e os recursos essenciais necessários para uma adoção mais ampla. No entanto, o WASI Preview 2 preencheu essas lacunas e abriu as portas para a adoção em massa do Wasm em ambientes de produção.  
>   
> O modelo de componente e o suporte a HTTP incorporados ao WASI Preview 2 simplificam determinados aspectos em comparação com o POSIX, tornando o WASI uma interface mais moderna e portátil para aplicativos da Web e de back-end. Com essa tecnologia, os desenvolvedores podem escolher bibliotecas de qualquer ecossistema ou linguagem, compilá-las como módulos e estruturá-las em um único aplicativo. Isso permite que as equipes de uma organização desenvolvam sistemas com a pilha de sua escolha e, em seguida, unifiquem seus componentes e módulos em um aplicativo monolítico.  
>   
> Não é difícil ver como o WASI Preview 2 transformou a possibilidade do futuro da Wasm. Essa tecnologia deve impulsionar um rápido aumento na adoção e na implementação do Wasm, o que beneficiará os desenvolvedores e os investidores em tecnologia de todo o mundo. O WASI Preview 2 é o ponto de inflexão que leva a uma era de desenvolvimento de software mais fácil, mais rápido e mais seguro para todos.

### Por que o WASI Preview 2 torna o WebAssembly pronto para a produção?

![O WebAssembly está pronto para a produção? WASI Preview 2 torna isso possível](https://cdn.hashnode.com/res/hashnode/imageupload/v1713182928053/082fa527-7fdd-4dc4-bd7a-45c2b8cf16aa.png)

O WebAssembly, ou Wasm, prometeu revolucionar o desenvolvimento de aplicativos. O sonho era simples, mas poderoso (e familiar): escrever seu código uma vez e fazê-lo funcionar em qualquer lugar. O Wasm também prometia executar o código em velocidades quase nativas. Melhor ainda, ao contrário das tentativas anteriores de tempos de execução universais, como o Java, o Wasm oferecia garantias de segurança mais altas e uma postura de segurança padrão de negação por padrão (o acesso aos recursos do sistema é desativado, a menos que seja orientado de outra forma por um desenvolvedor). Apesar do enorme potencial e da crescente comunidade, o Wasm não tinha as ferramentas, as interfaces e as APIs necessárias para torná-lo realmente pronto para a produção. Os desenvolvedores que quisessem usar o Wasm tinham que se tornar especialistas em Wasm, entrando em águas turvas e não documentadas, ou recorrer a empresas especializadas em Wasm, como a Cosmonic ou a Fermyon, que oferecem PaaS que simplificam a curva de aprendizado inicial.

### Mais do que uma prévia: estabilizando o modelo de componentes

A WASI Preview 2, embora chamada de "prévia", é apenas no nome. O WASI Preview 2 é o elo perdido que a Wasm precisava para se tornar uma opção viável para casos de uso de produção. WASI, que significa WebAssembly System Interface, é um conjunto de padrões que define uma maneira padrão e segura de os módulos Wasm interagirem com os recursos do sistema. O WASI Preview 2 representa um marco significativo na evolução do Wasm porque fornece um padrão sólido a partir do qual os desenvolvedores podem construir com confiança, sabendo que toda a plataforma não terá alterações que envolvam refatoração ou incompatibilidades (semelhante ao controle de versão da API). Essa peça crucial do quebra-cabeça oferece uma maneira de compor módulos Wasm em componentes maiores, mesmo que tenham sido programados em linguagens diferentes. É um grande avanço em termos de flexibilidade e compatibilidade. O modelo de componente define uma IAB (interface de aplicativo binário) canônica que padroniza a maneira como os componentes se comunicam entre si e impede que eles acessem as memórias de outros componentes. Isso elimina os tipos mais comuns de bugs e vulnerabilidades de segurança. Outro aspecto importante do WASI Preview 2 é a estabilização das APIs. Isso garante a compatibilidade futura dos aplicativos Wasm, dando aos desenvolvedores a confiança necessária para construir sobre o Preview 2 sem se preocupar com futuros desastres. É análogo ao modo como o POSIX (Portable System Interface) padronizou as interfaces nos sistemas operacionais Unix, facilitando muito a criação de software "portátil".

### Além do 'POSIX para Wasm'

Mas a WASI pretende ir além, incluindo interfaces para todas as coisas que não são necessariamente recursos do sistema. A WASI inclui o HTTP como uma interface de primeira classe, um recurso crítico de rede e conexão que não está presente no POSIX, oferecendo melhores opções para manter o controle do que um módulo pode fazer, em vez de lidar com ele como um soquete (é claro que você também pode fazer isso). Ele também simplifica alguns aspectos em comparação com o POSIX, o que torna o WASI Preview 2 uma interface mais moderna e portátil, tanto para aplicativos da Web quanto de back-end. As implicações práticas do WASI Preview 2 são enormes. Anteriormente, os desenvolvedores tinham dificuldades para criar aplicativos Wasm fora de sistemas especializados (como o PaaS, que mascarava a complexidade). A falta de APIs e ferramentas estáveis dificultava a confiança no Wasm como uma tecnologia pronta para a produção. Em resumo, ao criar aplicativos Wasm, agora você pode escolher bibliotecas (ou bibliotecas com nomes errados) de qualquer ecossistema ou linguagem, compilá-las como módulos e estruturá-las para criar um único aplicativo. As equipes da sua organização podem desenvolver os sistemas pelos quais são responsáveis com a pilha de sua escolha. Em seguida, por meio de componentes e módulos, os desenvolvedores e as equipes podem unificá-los em um aplicativo monolítico com a confiança de que as peças se encaixarão e se comportarão de maneira previsível. Até o momento, esse potencial tem sido contido por lacunas em ferramentas, estabilidade e recursos essenciais. O WASI Preview 2 preenche muito bem essas lacunas, abrindo caminho para que o Wasm cumpra sua promessa de facilidade de desenvolvimento em ambientes de produção. O WASI Preview 2 e o modelo de componentes devem impulsionar um rápido aumento na adoção e na implementação do Wasm. Acima de tudo, é um grande passo à frente que deve deixar qualquer desenvolvedor ou líder de tecnologia entusiasmado com o futuro do Wasm e incentivá-lo a colocar a mão na massa para criar aplicativos Wasm. O WASI Preview 2 deve ser esse ponto de inflexão para o Wasm. Ele tornará o desenvolvimento de software mais fácil, mais rápido e mais seguro para os desenvolvedores de todos os lugares.

Adaptado do original de Oscar Spencer em The New Stack:

[

Why WASI Preview 2 Makes WebAssembly Production Ready

Until recently, Wasm’s reality didn’t live up to the hype. Preview 2 is the missing link that Wasm needed to become viable for production use cases.

![O WebAssembly está pronto para a produção? WASI Preview 2 torna isso possível](https://cdn.hashnode.com/res/hashnode/imageupload/v1713182928588/cbc98088-a509-4e3f-8f68-881a0f3431f6.ico)The New StackOscar Spencer

![O WebAssembly está pronto para a produção? WASI Preview 2 torna isso possível](https://cdn.hashnode.com/res/hashnode/imageupload/v1713182929054/701ad456-bcdc-45ed-b808-441e747d40ef.png)

](https://thenewstack.io/why-wasi-preview-2-makes-webassembly-production-ready/?ref=sredevops.org)