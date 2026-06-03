# Comandos Intermediários do Git

Comandos para quem já domina o fluxo básico e quer mais controle sobre o histórico, branches e colaboração em equipe.

> Pré-requisito: familiaridade com `add`, `commit`, `push`, `pull`, `switch` e `merge`.

---

## Rebase interativo

Permite reescrever, reorganizar, agrupar ou remover commits antes de publicar uma branch.

```bash
git rebase -i HEAD~4          # edita os últimos 4 commits
git rebase -i <hash>          # edita a partir de um commit específico
```

Opções disponíveis no editor:

| Opção      | O que faz                                      |
|------------|------------------------------------------------|
| `pick`     | mantém o commit como está                      |
| `reword`   | mantém, mas permite editar a mensagem          |
| `edit`     | pausa para editar arquivos e a mensagem        |
| `squash`   | agrupa com o commit anterior (mantém mensagens)|
| `fixup`    | agrupa com o commit anterior (descarta mensagem)|
| `drop`     | remove o commit                                |

```bash
git rebase --continue         # continua após resolver conflito
git rebase --abort            # cancela e volta ao estado anterior
```

> Nunca faça rebase em branches já publicadas e compartilhadas com outras pessoas.

---

## Merge com estratégias

```bash
git merge feat/login                   # merge padrão (fast-forward quando possível)
git merge --no-ff feat/login           # força criação de merge commit
git merge --squash feat/login          # junta todos os commits em um só antes de commitar
git merge --abort                      # cancela o merge em andamento
```

Quando usar cada um:
- `--no-ff` — preserva o contexto de que existiu uma branch (útil para features)
- `--squash` — entrega o conteúdo limpo sem poluir o histórico com commits de WIP

---

## Cherry-pick

Aplica um ou mais commits específicos na branch atual sem fazer merge completo.

```bash
git cherry-pick <hash>                 # aplica um commit
git cherry-pick <hash1> <hash2>        # aplica múltiplos commits
git cherry-pick <hash1>..<hash2>       # aplica um intervalo de commits
git cherry-pick --no-commit <hash>     # aplica as mudanças sem criar commit
git cherry-pick --abort                # cancela
```

---

## Reflog

Registro local de todos os movimentos do HEAD — incluindo resets e branches deletadas.
Útil para recuperar trabalho "perdido".

```bash
git reflog                             # histórico completo do HEAD
git reflog show feat/login             # histórico de uma branch específica
git switch -c recuperada HEAD@{3}      # recupera estado de 3 movimentos atrás
```

---

## Reset avançado

```bash
git reset --soft HEAD~2       # desfaz 2 commits, mantém tudo no stage
git reset --mixed HEAD~2      # desfaz 2 commits, mantém arquivos modificados (padrão)
git reset --hard HEAD~2       # desfaz 2 commits e descarta todas as alterações ⚠️
git reset <hash>              # volta para um commit específico
```

> Após um `reset --hard`, use `git reflog` para recuperar os commits se necessário.

---

## Stash avançado

```bash
git stash push -m "WIP: formulário de login"     # stash com nome
git stash push -u -m "inclui não rastreados"     # inclui arquivos novos
git stash branch feat/login stash@{0}            # cria branch a partir de um stash
git stash show -p stash@{0}                      # mostra o diff do stash
```

---

## Log avançado

```bash
git log --oneline --graph --all            # grafo completo de todas as branches
git log --stat                             # mostra arquivos alterados por commit
git log -p                                 # mostra o diff de cada commit
git log --since="2 weeks ago"             # commits das últimas 2 semanas
git log --until="2025-01-01"              # commits até uma data
git log --grep="fix"                       # commits cuja mensagem contém "fix"
git log -S "função buscada"               # commits que adicionaram/removeram o texto
git log --author="Monica"                  # commits de um autor
git log main..feat/login                   # commits que estão em feat/login mas não em main
```

---

## Diff avançado

```bash
git diff HEAD~3                            # diff entre agora e 3 commits atrás
git diff main..feat/login                  # diff entre duas branches
git diff --stat                            # resumo de arquivos alterados
git diff --word-diff                       # mostra diferenças palavra por palavra
git diff <hash1> <hash2> -- <arquivo>      # diff de um arquivo entre dois commits
```

---

## Bisect (encontrar o commit do bug)

Faz uma busca binária no histórico para localizar qual commit introduziu um problema.

```bash
git bisect start
git bisect bad                   # o commit atual tem o bug
git bisect good v1.2.0           # essa tag/hash funcionava

# Git vai apontar commits para você testar
git bisect good                  # o commit atual está ok
git bisect bad                   # o commit atual tem o bug

git bisect reset                 # termina a sessão
```

**Modo automatizado:**
```bash
git bisect run npm test          # roda o comando em cada commit — bad se exit code != 0
```

---

## Blame

Mostra quem alterou cada linha de um arquivo e em qual commit.

```bash
git blame <arquivo>
git blame -L 20,40 <arquivo>     # apenas as linhas 20 a 40
git blame --since="1 month ago" <arquivo>
```

---

## Fetch com limpeza

```bash
git fetch origin                           # baixa atualizações do remoto
git fetch --prune                          # remove rastreamento de branches deletadas no remoto
git fetch --all --prune                    # todos os remotos + limpeza
```

---

## Múltiplos remotos

```bash
git remote add upstream <url>             # adiciona um segundo remoto (ex: repositório original de um fork)
git fetch upstream
git merge upstream/main                   # atualiza o fork com o repositório original
```

---

## Aliases úteis

Configure atalhos para os comandos que você mais usa:

```bash
git config --global alias.st "status -s"
git config --global alias.lg "log --oneline --graph --all"
git config --global alias.undo "reset --soft HEAD~1"
git config --global alias.unstage "restore --staged"
git config --global alias.aliases "config --get-regexp alias"
```

Usar:
```bash
git st
git lg
git undo
```

---

## Tags anotadas e versionamento

```bash
git tag -a v2.1.0 -m "Release 2.1.0 — adiciona suporte a OAuth"
git tag -a v2.0.1 <hash> -m "Hotfix 2.0.1"   # tag em commit antigo
git show v2.1.0                                # detalhes da tag
git push origin --tags                         # envia todas as tags pendentes
```

---

> Próximo nível: [Comandos Avançados](./Comandos-avancados-git.md)
