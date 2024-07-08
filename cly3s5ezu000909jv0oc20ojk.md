---
title: "Vulnerabilidade crítica em OpenSSH "regreSSHion", cheque se você está em risco"
datePublished: Tue Jul 02 2024 02:18:45 GMT+0000 (Coordinated Universal Time)
cuid: cly3s5ezu000909jv0oc20ojk
slug: vulnerabilidade-critica-em-openssh-regresshion-cheque-se-voce-esta-em-risco-1-1
canonical: https://sredevops.org/br/vulnerabilidade-critica-em-openssh-regresshion-cheque-se-voce-esta-em-risco/
cover: https://cdn.hashnode.com/res/hashnode/imageupload/v1719886725132/60378736-8a35-4c1a-b442-059205dc874a.webp
tags: linux, opensource, security, devsecops, brasil, cve

---

TL/DR;
------

![Vulnerabilidade crítica em OpenSSH "regreSSHion", cheque se você está em risco](https://cdn.hashnode.com/res/hashnode/imageupload/v1719886723859/81aae3ae-92c1-4061-b905-bc9d2b1dcecb.webp)

Uma vulnerabilidade crítica ([CVE-2024-6387](https://www.qualys.com/regresshion-cve-2024-6387/?ref=sredevops.org)), apelidada de "regreSSHion", ressurgiu nas versões do servidor **OpenSSH 8.5p1 a 9.8p1,** o que poderia permitir a execução remota de código como **root** sem autenticação em sistemas Linux vulneráveis. Este **bug**, uma regressão da vulnerabilidade CVE-2006-5051 previamente corrigida, destaca a importância de realizar testes de regressão completos. **Estima-se que aproximadamente 700.000 servidores OpenSSH expostos à Internet são vulneráveis.** Este artigo detalha a vulnerabilidade, seu impacto e as etapas para mitigar.

O que é a vulnerabilidade regreSSHion?
--------------------------------------

A vulnerabilidade regreSSHion ([CVE-2024-6387](https://www.qualys.com/regresshion-cve-2024-6387/?ref=sredevops.org)) é "um _regresso ao passado"_, uma condição de corrida (_race conditions, raft_) de gerenciadores de sinais no servidor OpenSSH (sshd) que permite que invasores remotos executem potencialmente código arbitrário com privilégios de **root**. O erro ressurgiu nas versões 8.5p1 a 9.8p1 devido à remoção acidental de um componente crítico do código. Essa vulnerabilidade permite que invasores não autenticados obtenham controle total dos sistemas afetados. O nome "regreSSHion" é um jogo de palavras inteligente que destaca que essa vulnerabilidade é uma regressão de uma falha corrigida anteriormente, CVE-2006-5051.

[

OpenSSH Vulnerability: CVE-2024-6387 FAQs and Resources | Qualys, Inc.

Discover what the OpenSSH vulnerability, CVE-2024-6387, is as well as resources and tools to help detect and mitigate vulnerabilities in your network.

![Vulnerabilidade crítica em OpenSSH "regreSSHion", cheque se você está em risco](https://cdn.hashnode.com/res/hashnode/imageupload/v1719886724163/c2c3c100-8ddf-4b04-b333-989d21c34433.png)Qualys

![Vulnerabilidade crítica em OpenSSH "regreSSHion", cheque se você está em risco](https://cdn.hashnode.com/res/hashnode/imageupload/v1719886724296/d938374a-5cc9-4eaa-b1d8-1d48b361c970.jpeg)

](https://www.qualys.com/regresshion-cve-2024-6387/?ref=sredevops.org)

Como funciona o regreSSHion?
----------------------------

Esta vulnerabilidade explora uma condição de corrida na forma como o OpenSSH lida com os sinais durante o processo de autenticação. Ao enviar uma sequência específica de sinais no momento certo, um invasor pode forçar o servidor OpenSSH a executar código malicioso com privilégios de **root**. Isso significa que um invasor pode potencialmente assumir o controle total do seu servidor sem a necessidade de nenhuma credencial de acesso válida.

Qual é o impacto do regreSSHion?
--------------------------------

A vulnerabilidade regreSSHion representa uma séria ameaça para organizações em todo o mundo, pois pode levar à completa **compromission** do sistema, vazamentos de dados e interrupções de serviço. Ao explorar essa vulnerabilidade, os invasores podem executar código arbitrário com os privilégios mais altos, o que lhes permite:

*   **Instalar malware:** Injetar **software** malicioso para roubar dados, espionar ou lançar novos ataques.
*   **Manipular dados:** Alterar ou excluir dados confidenciais, interromper as operações e causar danos à reputação.
*   **Criar backdoors:** Estabelecer acesso persistente aos sistemas comprometidos para futura exploração.
*   **Propagar-se lateralmente:** Usar o sistema comprometido como plataforma de lançamento de ataques a outros sistemas da rede.

Essa vulnerabilidade é especialmente preocupante porque afeta uma tecnologia amplamente utilizada, o OpenSSH, no qual muitas vezes se confia para administração remota segura e transferência de dados.

Como mitigar a vulnerabilidade regreSSHion
------------------------------------------

Abordar a vulnerabilidade regreSSHion requer ação imediata. As organizações devem priorizar a aplicação de **patches** aos sistemas afetados como principal estratégia de mitigação. A seguir, é apresentado um detalhamento das etapas principais:

*   **Gerenciamento de patches:** A mitigação mais eficaz é aplicar os **patches** oficiais lançados pelo OpenSSH. Atualize o OpenSSH para a versão 9.8p1 ou posterior imediatamente. Para versões anteriores, consulte o aviso de segurança do OpenSSH para obter os **patches** específicos.
*   **Segmentação da rede:** Isole os sistemas críticos e segmente sua rede para limitar o impacto potencial de um ataque. Isso ajuda a evitar que invasores se movam lateralmente dentro da sua rede se um sistema for comprometido.
*   **Sistemas de detecção e prevenção de intrusões:** Implante sistemas robustos de detecção e prevenção de intrusões para monitorar o tráfego de rede em busca de atividades suspeitas. Configure esses sistemas para detectar e bloquear tentativas de explorar a vulnerabilidade regreSSHion.
*   **Monitoramento e análise de logs:** Revise periodicamente os **logs** do sistema e de segurança em busca de qualquer indicador de **compromission**. Procure por tentativas incomuns de conexão SSH ou falhas de autenticação.
*   **Princípio do privilégio mínimo:** Siga o princípio do privilégio mínimo, concedendo aos usuários e processos apenas o nível mínimo de acesso necessário para realizar suas tarefas. Isso limita os danos potenciais que um invasor pode infligir se comprometer uma conta de usuário.

Ideias recolhidas
-----------------

A vulnerabilidade regreSSHion é um claro lembrete de que mesmo o **software** bem conservado e amplamente utilizado pode ter vulnerabilidades críticas. Medidas de segurança proativas, como aplicação oportuna de **patches**, ferramentas de segurança robustas e a adesão às melhores práticas de segurança são essenciais para mitigar essas ameaças. Ao tomar medidas rápidas para lidar com a vulnerabilidade regreSSHion, as organizações podem reduzir significativamente o risco de serem vítimas de ataques que explorem essa falha.

Referências
-----------

*   [Blog de Segurança da Qualys](https://blog.qualys.com/vulnerabilities-threat-research/2024/07/01/regresshion-remote-unauthenticated-code-execution-vulnerability-in-openssh-server?ref=sredevops.org)

[

regreSSHion: Remote Unauthenticated Code Execution Vulnerability in OpenSSH server | Qualys Security Blog

The Qualys Threat Research Unit (TRU) has discovered a Remote Unauthenticated Code Execution (RCE) vulnerability in OpenSSH’s server (sshd) in glibc-based Linux systems. CVE assigned to this…

![Vulnerabilidade crítica em OpenSSH "regreSSHion", cheque se você está em risco](https://cdn.hashnode.com/res/hashnode/imageupload/v1719886724595/c4568df8-fdd5-481a-ba77-63121161c564.png)Qualys, Inc.Bharat Jogi

![Vulnerabilidade crítica em OpenSSH "regreSSHion", cheque se você está em risco](https://cdn.hashnode.com/res/hashnode/imageupload/v1719886724715/3857b524-ee20-415d-84c0-b9a191f4562b.jpeg)

](https://blog.qualys.com/vulnerabilities-threat-research/2024/07/01/regresshion-remote-unauthenticated-code-execution-vulnerability-in-openssh-server?ref=sredevops.org)

*   [Segurança do OpenSSH](https://www.openssh.com/security.html?ref=sredevops.org)

OpenSSH: SecurityOpenSSH advisories

[

OpenSSH: Security

OpenSSH advisories

![Vulnerabilidade crítica em OpenSSH "regreSSHion", cheque se você está em risco](https://cdn.hashnode.com/res/hashnode/imageupload/v1719886724974/226e9f85-aca2-4e30-92d1-90673f30cad7.ico)



](https://www.openssh.com/security.html?ref=sredevops.org)