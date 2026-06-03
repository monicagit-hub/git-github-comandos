# Comandos Básicos do Git

Referência prática dos comandos Git mais usados no dia a dia, atualizada para 2026.

> **Versão recomendada:** Git 2.45+. Verifique com `git --version`.

---

## Configuração inicial

Execute uma vez após instalar o Git:

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu@email.com"
git config --global core.editor "code --wait"   # define VS Code como editor padrão
git config --global init.defaultBranch main      # define 'main' como branch padrão
```

Ver todas as configurações:
```bash
git config --list
```

---

## Iniciando um repositório

```bash
git init                    # inicia um repositório local na pasta atual
git clone <url>             # clona um repositório remoto
git clone <url> minha-pasta # clona em uma pasta com nome específico
```

---

## Ciclo básico de trabalho

### Verificar estado dos arquivos
```bash
git status          # mostra arquivos modificados, staged e não rastreados
git status -s       # versão resumida
```

### Adicionar arquivos ao stage
```bash
git add <arquivo>   # adiciona um arquivo específico
git add .           # adiciona todos os arquivos da pasta atual
git add -p          # adiciona por partes (interativo — recomendado para revisão)
```

### Criar um commit
```bash
git commit -m "feat: adicionar página de login"
git commit          # abre o editor configurado para mensagem mais detalhada
```

### Ver diferenças
```bash
git diff            # diferenças ainda não adicionadas ao stage
git diff --staged   # diferenças já no stage (a serem commitadas)
git diff main..feat/login  # compara duas branches
```

---

## Histórico de commits

```bash
git log                          # histórico completo
git log --oneline                # uma linha por commit
git log --oneline --graph        # histórico com grafo de branches
git log --oneline -10            # últimos 10 commits
git log --author="Nome"          # commits de um autor específico
git log -- <arquivo>             # histórico de um arquivo específico
git show <hash>                  # detalhes de um commit específico
```

---

## Branches

> A partir do Git 2.23, prefira `git switch` e `git restore` ao invés de `git checkout` para essas operações.

```bash
git branch                       # lista branches locais
git branch -a                    # lista locais e remotas
git branch feat/login            # cria uma nova branch
git branch -d feat/login         # deleta branch (segura — só se já mergeada)
git branch -D feat/login         # deleta branch à força
git branch -m novo-nome          # renomeia a branch atual
```

### Navegar entre branches

```bash
git switch feat/login            # muda para a branch
git switch -c feat/login         # cria e muda para a branch (substitui checkout -b)
git switch -                     # volta para a branch anterior
```

### Restaurar arquivos

```bash
git restore <arquivo>            # descarta alterações não commitadas no arquivo
git restore --staged <arquivo>   # remove do stage (mantém as alterações)
git restore .                    # descarta todas as alterações não commitadas
```

---

## Merge e Rebase

```bash
# Merge — une as histórias preservando os commits da branch
git switch main
git merge feat/login

# Rebase — reaplica commits em cima de outra base (histórico linear)
git switch feat/login
git rebase main

# Rebase interativo — editar, reordenar ou agrupar commits
git rebase -i HEAD~3             # rebase interativo nos últimos 3 commits
```

> **Regra:** nunca faça rebase em branches públicas/compartilhadas.

---

## Desfazer alterações

```bash
git restore <arquivo>            # desfaz alterações no working directory
git restore --staged <arquivo>   # remove do stage

git revert <hash>                # cria um novo commit desfazendo o commit indicado (seguro)
git reset --soft HEAD~1          # desfaz o último commit, mantém arquivos no stage
git reset --mixed HEAD~1         # desfaz o último commit, mantém arquivos modificados
git reset --hard HEAD~1          # desfaz o último commit e descarta as alterações ⚠️
```

> `revert` é mais seguro em branches compartilhadas pois não reescreve o histórico.

---

## Stash (guardar alterações temporariamente)

```bash
git stash                        # guarda alterações não commitadas
git stash push -m "mensagem"     # guarda com descrição
git stash list                   # lista os stashes salvos
git stash pop                    # aplica o stash mais recente e o remove da lista
git stash apply stash@{1}        # aplica um stash específico sem removê-lo
git stash drop stash@{0}         # remove um stash específico
git stash clear                  # remove todos os stashes
```

---

## Repositório remoto

```bash
git remote -v                          # lista os remotos configurados
git remote add origin <url>            # adiciona um remoto chamado 'origin'
git remote set-url origin <nova-url>   # altera a URL do remoto

git fetch origin                       # baixa atualizações sem aplicar
git pull                               # fetch + merge da branch atual
git pull --rebase                      # fetch + rebase (histórico mais limpo)

git push origin main                   # envia branch para o remoto
git push -u origin feat/login          # envia e configura rastreamento
git push --force-with-lease            # push forçado seguro (⚠️ use no lugar de --force)
git push origin --delete feat/login    # deleta branch remota
```

---

## Tags

```bash
git tag                            # lista todas as tags
git tag v1.0.0                     # cria tag leve
git tag -a v1.0.0 -m "Release 1.0.0"  # cria tag anotada (recomendado)
git push origin v1.0.0             # envia uma tag para o remoto
git push origin --tags             # envia todas as tags
git tag -d v1.0.0                  # deleta tag local
git push origin --delete v1.0.0   # deleta tag remota
```

---

## Comandos úteis do dia a dia

```bash
# Corrigir o último commit (mensagem ou arquivos esquecidos)
git commit --amend -m "nova mensagem"   # ⚠️ apenas em commits ainda não enviados

# Aplicar um commit específico de outra branch
git cherry-pick <hash>

# Buscar qual commit introduziu um bug (busca binária)
git bisect start
git bisect bad                     # commit atual tem o bug
git bisect good <hash>             # commit antigo que funcionava

# Ver quem alterou cada linha de um arquivo
git blame <arquivo>

# Limpar arquivos não rastreados
git clean -n                       # simula o que seria removido
git clean -fd                      # remove arquivos e pastas não rastreados ⚠️

# Encontrar texto em toda a base de código
git grep "texto buscado"
```

---

## .gitignore

Arquivo que lista o que o Git deve ignorar:

```
# dependências
node_modules/
vendor/

# build
dist/
*.class

# ambiente
.env
.env.local

# sistema operacional
.DS_Store
Thumbs.db

# IDEs
.vscode/
.idea/
```

Criar um `.gitignore` global para sua máquina:
```bash
git config --global core.excludesFile ~/.gitignore_global
```

---

## Referência rápida

| Situação                              | Comando                          |
|---------------------------------------|----------------------------------|
| Ver o que mudou                       | `git status` / `git diff`        |
| Salvar progresso                      | `git add . && git commit -m "..."` |
| Criar e entrar em nova branch         | `git switch -c feat/nome`        |
| Atualizar branch com o remoto         | `git pull --rebase`              |
| Guardar trabalho incompleto           | `git stash push -m "descrição"`  |
| Desfazer último commit (não enviado)  | `git reset --soft HEAD~1`        |
| Desfazer commit já enviado            | `git revert <hash>`              |
| Enviar branch pela primeira vez       | `git push -u origin feat/nome`   |

---

> Documentação oficial: [git-scm.com/doc](https://git-scm.com/doc)

---

**Próximos níveis:**
- [Comandos Intermediários](./Comandos-intermediarios-git.md) — rebase interativo, cherry-pick, bisect, aliases, múltiplos remotos
- [Comandos Avançados](./Comandos-avancados-git.md) — hooks, worktree, filter-repo, signing, submodules, bundle
