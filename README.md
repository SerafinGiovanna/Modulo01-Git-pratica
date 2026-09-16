# TRILHA DE APRENDIZADO

Neste repositório vai conter as Lab's e o arquivo de resposta.md (contendo a parte escrita do módulo), feitas na Trilha de aprendizado.

# Índice
   
## Módulo 01-Git-Prática
###### --------------------------------------------------------------------
- [ Lab - 01  Fundamentos e primeiros commits.](#fundamentos-e-primeiros-commits)
- [ Lab - 02 Branch.](#branch)
- [ Lab - 03 Remote, fetch/pull/push.](#Remote-,fetch-/pull/-push)
- [ Lab - 04 Merge.](#merge)
- [ Lab - 05  Gerando e resolvendo um conflito de verdade.](#gerando-e-resolvendo-um-conflito-de-verdade)
- [ Lab - 06 Rebase.](#rebase)
- [ Lab - 07 Reset, Restore e Revert.](#Reset,-Restore-e-Revert)
- [ Lab - 08 Stash.](#stash.)
- [ Lab - 09 Histórico e Reflog.](#Histórico-e-Reflog.)
- [ Lab - 10 Tags.](#tags)
- [ Lab - 11 Conventional Commits (revisão).](#Conventional-Commits-(revisão))
- [ Lab - 12 Pull Request e Code Review.](#Pull-Request-e-Code-Review)
- [ Lab - 13 Git Flow x Trunk Based.](#Git-Flow-x-Trunk-Based)
- [ Lab - 14 Cherry-pick.](#Cherry-pick)
- [ Lab - 15 Bisect.](#bisect)
- [ Lab - 16 Worktree.](#worktree)
- [ Lab - 17 Hooks.](#hooks)
- [ Lab - 18 Git + CI/CD.](#Git-+-CI/CD)


# Lab's
## Nível Básico 
***

## Fundamentos e primeiros commits.
Dentro de lanchonete-beira-rio:

**git init**

**git status**

git status deve mostrar cardapio.txt como não rastreado. Adicione e faça o primeiro commit:

**git add cardapio.txt**

**git commit -m "feat: adicionar cardapio inicial"**

Agora edite cardapio.txt (Bloco de Notas) adicionando uma seção "Lanches" com 3 itens e preços, e faça um commit propositalmente ruim — sem git add de nada de específico e com mensagem vaga, só pra você ver o problema na prática:

**git add .**

**git commit -m "arruma coisas"**

Esse commit ruim volta no Lab 7 pra ser corrigido — não conserte ainda.

**Critério de aceite:** 
git log --oneline mostra 2 commits; o segundo tem uma mensagem que não segue o padrão de prefixo do módulo 02 (de propósito).
***

## Branch.
A lanchonete quer adicionar uma seção de bebidas, mas sem mexer na main até estar pronta:

**git branch feature/cardapio-bebidas**

**git switch feature/cardapio-bebidas**

Edite cardapio.txt no Bloco de Notas, adicionando uma seção "Bebidas" com 3 itens. Commit seguindo o padrão do módulo 02:

**git add cardapio.txt**

**git commit -m "feat: adicionar secao de bebidas ao cardapio"**

**Critério de aceite:**
git branch mostra duas branches, com * marcando feature/cardapio-bebidas como a atual; main ainda não tem a seção de bebidas (confira trocando pra main com git switch main e abrindo o arquivo).
***
## Remote, fetch/pull/push.
Crie um repositório vazio no GitHub chamado lanchonete-beira-rio (pela interface web, sem inicializar com README). Conecte seu repositório local a ele:

**git remote add origin https://github.com/SEU-USUARIO/lanchonete-beira-rio.git**

**git remote -v**

**git switch main**

**git push -u origin main**

O -u cria o vínculo de tracking entre a main local e a origin/main remota — depois disso, git push/git pull sozinhos (sem especificar remoto/branch) já sabem pra onde ir. Envie a branch de feature também:

**git switch feature/cardapio-bebidas**

**git push -u origin feature/cardapio-bebidas**

**Critério de aceite:** 
as duas branches aparecem no GitHub; git remote -v mostra origin listada duas vezes (fetch e push).
***
## Merge.
A seção de bebidas está pronta — hora de trazer pra main:

**git switch main**

**git merge feature/cardapio-bebidas**

**git push**

**Critério de aceite:** 
cardapio.txt na main agora tem lanches e bebidas; git log --graph --oneline mostra os dois caminhos se juntando.
***
## Gerando e resolvendo um conflito de verdade.
Situação real: duas pessoas (aqui, você mesmo, trocando de branch) editam a mesma linha do cardápio ao mesmo tempo.

**git switch main**

**git branch ajuste-preco-main**

**git switch ajuste-preco-main**

No Bloco de Notas, mude o preço do primeiro item de "Lanches" (ex: de R$ 15,00 pra R$ 17,00).

**git add cardapio.txt**

**git commit -m "fix: corrigir preco do lanche principal"**

**git switch main**

**git branch ajuste-preco-feature main**

**git switch ajuste-preco-feature**

Agora mude o mesmo item, mas pra um preço diferente (ex: R$ 16,50) — simulando a segunda pessoa editando por cima.

**git add cardapio.txt**

**git commit -m "fix: atualizar preco do lanche principal"**

**git switch main**

**git merge ajuste-preco-main**

**git merge ajuste-preco-feature**

O segundo merge vai gerar conflito. Abra cardapio.txt no Bloco de Notas — você vai ver os marcadores **<<<<<<<, =======, >>>>>>>** em volta da linha do preço. Escolha o preço correto (ou combine os dois, sua decisão), apague os três marcadores manualmente, salve.

**git add cardapio.txt**

**git commit -m "merge: resolver conflito de preco do lanche principal"**

**git push**

**Critério de aceite:** 
git log --graph --oneline mostra claramente um commit de merge com dois pais; cardapio.txt não tem mais nenhum marcador <<<<<<</=======/>>>>>>> sobrando.
***
# Nível Intermediário
## Rebase.
Crie uma branch pra adicionar uma seção "Sobremesas", mas desta vez, em vez de merge, use rebase pra manter o histórico linear (apropriado aqui porque é uma branch só sua, ainda não compartilhada com mais ninguém):

**git switch main**

**git branch feature/sobremesas**

**git switch feature/sobremesas**

Edite o cardápio adicionando "Sobremesas" (2 itens), commit. Enquanto isso, imagine que a main recebeu outro commit direto (simule voltando pra main, editando algo pequeno tipo o nome da lanchonete no topo do arquivo, e commitando lá). Depois:

**git switch feature/sobremesas**

**git rebase main**

**[ATENÇÃO]** Rebase reescreve o histórico da branch — nunca faça isso numa branch que outra pessoa já baixou e está usando; aqui é seguro porque feature/sobremesas ainda não foi enviada.

**Critério de aceite:** 
depois do rebase, git log --oneline feature/sobremesas mostra o commit da sobremesa depois do commit que você fez na main, num histórico linear (sem bifurcação).
***
## Reset, Restore e Revert.
Hora de lidar com o commit ruim do Lab 1 ("arruma coisas") — mas de duas formas diferentes, dependendo de onde ele está:

Se esse commit ainda não foi enviado ao GitHub (só existe local): pode reescrever a história com reset. Rode git log --oneline pra achar o hash do commit antes dele, e:

**git reset --soft <hash-do-commit-anterior>**

Isso desfaz o commit ruim mas mantém as mudanças dele "na área de stage", prontas pra você recommitar com uma mensagem decente:

**git commit -m "feat: adicionar secao de lanches ao cardapio"**

Se esse commit já foi enviado/compartilhado (ex: alguém já deu pull): reescrever a história com reset é perigoso — use revert, que cria um novo commit desfazendo o anterior sem apagar nada do histórico:

**git revert <hash-do-commit-ruim>**

Pratique também restore, pra descartar uma mudança que você nem chegou a commitar: edite cardapio.txt com qualquer besteira, sem dar add, e rode git restore cardapio.txt — a mudança desaparece, voltando ao último commit.

**Critério de aceite:** 
você sabe explicar, com suas palavras, por que reset foi usado num caso e revert no outro — documente essa decisão no respostas.md.
***
## Stash.
Você está no meio de editar cardapio.txt (uma nova seção "Promoções", ainda incompleta, sem commitar) quando surge um pedido urgente pra trocar de branch e corrigir outra coisa. Você não quer perder o que já escreveu, mas também não quer commitar algo pela metade:

**git stash**

**git switch main**

(faça uma mudança rápida qualquer na main, commit, git switch de volta pra onde estava)

**git switch feature/sobremesas**

**git stash pop**

**Critério de aceite:** 
git stash list estava com 1 item antes do pop, e vazio depois; a seção "Promoções" incompleta voltou exatamente como você deixou.
***
## Histórico e Reflog.
Explore o histórico completo do projeto:

**git log --oneline**

**git log --oneline --graph --all**

**git log --stat -3**

Agora simule um susto real: rode git reset --hard HEAD~1 (isso descarta o último commit e as mudanças dele — cuidado, é destrutivo de propósito aqui só pra praticar recuperação). Percebeu que foi um erro? O commit não sumiu de verdade — ele só ficou "sem referência":

**git reflog**

Ache o hash do commit que você "perdeu" na lista do reflog, e recupere:

**git reset --hard <hash-do-commit-perdido>**

**Critério de aceite:**
depois de recuperar, git log --oneline mostra o commit de volta, exatamente como estava antes do reset --hard acidental.
***
## Tags.
O cardápio está pronto pro lançamento oficial:

**git switch main**

**git tag -a v1.0.0 -m "Lancamento oficial do cardapio"**

**git push origin v1.0.0**

**Critério de aceite:**
a tag v1.0.0 aparece na página de "Releases/Tags" do repositório no GitHub.
***
## Pull Request e Code Review.
Crie uma última branch de feature (ex: feature/secao-contato, adicionando um bloco "Contato" ao cardápio com endereço/telefone fictícios), commit e envie:

**git switch main**

**git branch feature/secao-contato**

**git switch feature/secao-contato**

**(edite, commit seguindo Conventional Commits, depois:)**

**git push -u origin feature/secao-contato**

Na interface do GitHub, abra um Pull Request de verdade dessa branch pra main, escrevendo a descrição no formato do módulo 02 (o que foi feito / por que / como). Depois de revisar você mesmo o diff mostrado pelo GitHub (releia como se fosse um colega revisando), faça o merge do PR pela própria interface do GitHub (não pelo terminal desta vez).

**Critério de aceite:** 
o PR existe no histórico do repositório (mesmo já mesclado, ele continua listado em "Pull Requests" → "Closed"), com título e descrição completos.
***
# Nível Avançado

## Git Flow x Trunk Based
Depois de sincronizar (git pull) a main local com o merge feito pela interface no Lab 12, crie uma branch develop a partir dela — o ponto de partida de um fluxo estilo Git Flow:

**git switch main**

**git branch develop**

**git switch develop**

No respostas.md, compare em suas palavras: no Git Flow (com develop, branches de release/* e hotfix/* separadas da main), o que teria que acontecer pra uma correção urgente chegar em produção rápido? E no Trunk Based Development (todo mundo commitando direto numa branch principal única, com branches de vida curtíssima)? Qual seria mais adequado pra um projeto pequeno como o da lanchonete, e por quê?
***
## Cherry-pick.
Situação: você percebe um erro crítico de preço na branch develop (edite cardapio.txt lá, corrigindo um valor, e commit) — mas esse mesmo erro também existe na main, que já está em "produção" (publicada), e não dá tempo de esperar um merge completo de develop pra lá:

**git switch develop**

(edite o preço errado, commit: git commit -m "fix: corrigir preco incorreto de item critico")

**git switch main**

**git cherry-pick <hash-do-commit-do-fix>**

**Critério de aceite:** 
o mesmo commit de correção aparece tanto em develop quanto em main, com o mesmo conteúdo (você pode conferir com git log --oneline nas duas branches).
***
## Bisect.
Um "bug" foi introduzido em algum lugar dos últimos commits — pra simular, volte e adicione, espalhados em 2-3 commits diferentes ao longo do histórico (pode ser em qualquer branch, faça uns commits novos na main pra ter material), uma linha de texto quebrada (ex: um caractere estranho no meio do cardápio) em um deles especificamente, sem lembrar exatamente qual. Depois, ache:

**git bisect start**

**git bisect bad                    (o estado atual, com o bug)**

**git bisect good <hash-bem-antigo> (um commit de que você tem certeza que não tinha o bug)**

**O Git vai te colocar automaticamente no meio do intervalo — abra cardapio.txt a cada passo e responda:**

**git bisect good     (se esse commit NÃO tem o bug)**

**git bisect bad       (se esse commit TEM o bug)**

até o Git apontar exatamente o commit culpado. Encerre com:

**git bisect reset**

**Critério de aceite:** 
você identificou corretamente o commit que introduziu o bug, documentando o hash no respostas.md.
***
