---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, incluindo opções para os modos de alto contraste e claro. Vamos revisar as alterações e, se necessário, enviar feedback ao Copilot.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a inclusão da IA generativa. Sempre precisamos revisar o código gerado e seguir nosso processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para documentar o código, criar o recurso de jogos relacionados e implementar os recursos de acessibilidade antes de habilitar a revisão pela equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter o ambiente seguro. Entre elas:

- o Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código;
- o agente de nuvem é executado no GitHub Actions, que cria um ambiente separado e temporário para o trabalho;
- um ser humano precisa aprovar todos os workflows do GitHub Actions antes da execução;
- [o acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revisar a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o PR que adiciona documentação ao código. Você fará essa revisão na interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, talvez apareça um aviso de que o GitHub Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request conterá a indicação **[WIP]**. Aguarde até que o trabalho termine. Isso pode levar alguns minutos, então faça uma pausa ou reflita sobre o que aprendeu até aqui.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui os comentários TSDoc e outros elementos de documentação. As alterações exatas variam.

   Ao revisar o código, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles seguem os arquivos de instruções personalizadas que você examinou no Exercício 2.

6. Quando terminar a revisão e tudo parecer correto, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão ser enfileirados na seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma interação unidirecional. Você pode mencionar o Copilot em comentários do PR ou comentários em linha no código. Ele verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não há um texto prescritivo para usar. Você pode pedir, por exemplo, que o Copilot:

- adicione cabeçalhos de comentário no início de cada arquivo de código, descrevendo brevemente sua finalidade;
- adicione comentários TSDoc a arquivos TypeScript e Astro;
- crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa da equipe. Use uma das ideias acima ou faça outro pedido de documentação para a base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

   ![Opção Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Depois que o Copilot concluir as alterações, um novo commit deverá aparecer no PR.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você poderá marcar o PR como pronto para revisão e integrá-lo à branch principal.

![Opção para marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, volte ao PR que o Copilot criou para resolver a issue de exibir jogos relacionados na página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as atualizações, volte à guia **Conversation** e role para baixo.
6. Procure o indicador de workflows aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão ser enfileirados na seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode mudar seu codespace para essa branch e testar manualmente o recurso de jogos relacionados. Abra o terminal do codespace e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot (por exemplo, **copilot/fix-8**):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode iterar e pedir atualizações ao agente de nuvem. Por exemplo, solicite que ele altere o número de jogos relacionados ou adicione cabeçalhos e comentários TSDoc — lembre-se de que essa tarefa foi atribuída **antes** da atualização das instruções personalizadas. Para pedir alterações, adicione um comentário na guia **Conversation**; o Copilot iniciará outra sessão.

## Revisar os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado. Esse PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel central de agentes do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request conterá a indicação **[WIP]**. Aguarde até que o trabalho termine; isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Examine a implementação, com atenção especial a:
   - os componentes de interface para alternar entre os modos;
   - o uso do armazenamento local para persistir as preferências;
   - as alterações de CSS ou estilos para os modos claro e de alto contraste;
   - os atributos de acessibilidade (rótulos ARIA, navegação por teclado etc.);
   - o código JavaScript/TypeScript que gerencia a alternância entre modos.
6. Depois de revisar as atualizações, volte à guia **Conversation** e role para baixo.
7. Procure o indicador de workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão ser enfileirados na seção de verificações do pull request. Se tudo estiver correto, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** você pode mudar seu codespace para essa branch e testar manualmente os recursos de acessibilidade. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Em seguida, inicie o aplicativo e teste os seletores dos modos de alto contraste e claro no navegador. Confirme se funcionam e se as preferências são mantidas após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo boas práticas de acessibilidade. Se notar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar atualizações, como nos PRs anteriores.

## Exercício opcional: continuar delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e se concentrar em outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo;
- implementar paginação na página de listagem de jogos;
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Examinou os arquivos de instruções personalizados do repositório** para observar como influenciam o trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando opções para os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para acompanhar e orientar a sessão de acessibilidade.
- **Revisou e refinou os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, os outros percursos complementam o que você praticou aqui:

- **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e uma ponte opcional `/delegate` para voltar ao agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes;
- criar um agente personalizado para outro domínio;
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
