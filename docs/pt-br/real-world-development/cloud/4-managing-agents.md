---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas diferentes para melhorar a experiência e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver todas as tarefas de agentes com pull requests abertos, examinar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem;
- orientar uma sessão em andamento para solicitar outra funcionalidade.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe percebeu que seria útil adicionar também um modo claro. Como o trabalho já envolve atualizar os estilos do site e adicionar uma opção de alternância, faz sentido incluir esse recurso. Você orientará o agente para garantir que ele implemente também o modo claro.

## Revisar as tarefas do agente de nuvem

Vamos verificar o estado atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel esquerdo. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código;
   - gerar APIs para modificar jogos;
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações executadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório;
   - criar o ambiente de trabalho do Copilot;
   - configurar servidores MCP;
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas listadas variam de acordo com o estado do trabalho e a abordagem usada pelo Copilot.

4. Observe também o painel de pull requests à direita. Ele permite consultar o PR e os arquivos alterados para acompanhar o trabalho.

## Orientar o agente de nuvem

Agora que você viu as tarefas em andamento, peça ao Copilot que inclua uma opção de tema claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no painel central de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até o agente concluir a configuração e começar o trabalho. Você perceberá isso quando surgirem mensagens semelhantes aos exemplos.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot confirmar o recebimento do prompt e incorporá-lo ao trabalho.

## Deixar o Copilot trabalhar

O Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa que estava executando quando você enviou a mensagem.

Como antes, isso levará alguns minutos. Aproveite para fazer uma pausa e refletir sobre o que aprendeu e explorou até aqui.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o painel central para acompanhar e orientar tarefas do agente de nuvem do GitHub Copilot. Com esse painel, é possível monitorar tarefas ativas e concluídas, revisar o trabalho e redirecionar tarefas em andamento para ajustar o escopo ou fornecer outras orientações.

Você explorou estes conceitos:

- usar o painel central e a página de agentes para acompanhar tarefas do agente de nuvem;
- redirecionar uma sessão em andamento para solicitar outra funcionalidade.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar nos pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
