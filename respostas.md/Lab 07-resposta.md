Lab 7 — Reset, Restore e Revert

Critério de aceite: você sabe explicar, com suas palavras, por que reset foi usado num caso e revert no outro — documente essa decisão no respostas.md.

Se o commit não foi para o github é seguro dar reset, ele vai empurrar os commits mais  recente para trás, porém, se caso esse commit tivesse ja no github o mais seguro não seria dar reset e sim o git revert, ele não iria reescrever e sim cirar um novo commit.

Lab 15 — Bisect

Critério de aceite: você identificou corretamente o commit que introduziu o bug, documentando o hash no respostas.md.

Hash do commit: 906345a

Lab 18 — Git + CI/CD

No respostas.md, explique em suas palavras como a tag v1.0.0 que você criou no Lab 10 poderia, num projeto real, disparar um pipeline diferente do pipeline de push normal — por exemplo, um que só publica uma nova versão "oficial" quando uma tag no formato v*.*.* é criada, em vez de publicar a cada commit.

Critério de aceite: explicação escrita, referenciando a diferença entre o gatilho on: push (a cada commit) e um gatilho baseado em tags (só em lançamentos oficiais).

A tag v1.0.0 poderia ser usada para disparar um pipeline diferente do push normal. Em um projeto realpoderia apenas rodar testes e verificando o código a cada commit. Já quando uma tag no formato v*.*.* fosse criada, outro workflow seria executado para publicar oficialmente aquela versão do projeto. Assim, uma nova versão só seria publicada quando fosse criada uma tag, como v1.0.0, e não a cada commit.