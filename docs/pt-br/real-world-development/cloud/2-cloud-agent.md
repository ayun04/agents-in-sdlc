---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas de dívida técnica. Eles podem incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidade que ficaram paradas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request de rascunho para que uma pessoa desenvolvedora revise o trabalho. Assim, é possível delegar tarefas tediosas, acelerar o desenvolvimento e se concentrar em questões mais abrangentes.

Neste exercício, você explorará como usar o agente de nuvem do Copilot para:

- personalizar o ambiente de geração de código;
- garantir que as operações sejam executadas com segurança;
- entender a importância de issues bem delimitadas;
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc às funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela deseja mostrar jogos relacionados — outros títulos da mesma categoria — na página de detalhes de cada jogo, para que as pessoas continuem navegando. Ainda não é necessária uma implementação refinada; basta algo que permita testar a experiência de usuário. Essa tarefa está bloqueada, mas há issues com prioridade maior no momento.

Esses são exemplos de tarefas que podem ser facilmente adiadas e que são ótimas candidatas para o agente de nuvem do Copilot. O agente trabalha nelas de forma assíncrona, permitindo que a pessoa desenvolvedora cuide de outras tarefas e depois revise o trabalho.

## Conhecer o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Você pode iniciar o trabalho de várias maneiras, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode explorar a sessão enquanto o trabalho está em andamento ou depois que ele termina. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções bem delimitadas

Não há mágica no GitHub Copilot — não é possível pular a etapa de raciocínio. Mesmo tarefas aparentemente simples podem se tornar complexas quando examinadas de perto. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação baseada em IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já concluiu a atividade prática sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, aproveite para examinar rapidamente o arquivo. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções do repositório para reconhecer sua influência mais adiante.

Abra os arquivos a seguir na interface web do GitHub ou em um codespace, se já tiver um em execução:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, principalmente as expectativas para convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe no pull request resultante os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções. Voltaremos a esse assunto ao revisar PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

Criar código geralmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo se aplica às tarefas atribuídas ao Copilot, que trabalha de forma semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] para configurar o ambiente de trabalho. Você pode personalizá-lo com um [workflow de configuração][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. Assim, o agente terá as ferramentas e dependências necessárias. Esse arquivo já foi configurado para o laboratório: ele disponibiliza o Node.js, as dependências do projeto, os navegadores para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para as etapas. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Se as etapas de configuração precisarem clonar o repositório, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se não houver clonagem, o Copilot fará isso automaticamente depois da conclusão destas etapas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do aplicativo: Node.js (o aplicativo agora é todo Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job, chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O acionador `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para verificar se o workflow funciona sem esperar o Copilot executá-lo.

## Adicionar documentação

Todo mundo entende a importância da documentação, mas muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica costuma não ser resolvida, reduzindo a produtividade e dificultando a manutenção da base de código e a integração de novas pessoas desenvolvedoras. O Copilot pode ajudar a criar documentação, por isso esta é uma issue adequada para atribuir ao agente de nuvem. Ele trabalhará em segundo plano, e você revisará as alterações em um exercício futuro.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuindo uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o workflow descrito anteriormente), analisando o projeto e decidindo como abordar o problema.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request para baixo e procure uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe a sessão em andamento e como o Copilot resolve o problema: explora o código e seu estado, decide qual abordagem seguir e cria código.

O trabalho provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona para que você possa se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, criar uma funcionalidade no aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem é dividir o trabalho: você pode se concentrar em uma tarefa enquanto ele cuida de outra. Mesmo que a seção de jogos relacionados para a equipe de design não leve muito tempo, esse tempo pode ser usado em outras atividades. Vamos atribuí-la ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuindo uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver o emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. Como o agente de nuvem funciona de forma semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo o tempo todo; basta revisar o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, é possível atribuir issues ao Copilot para execução assíncrona. Você pode resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código;
- garantir que as operações sejam executadas com segurança;
- entender a importância de issues bem delimitadas;
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha, você pode criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso a instruções personalizadas, apresentadas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
