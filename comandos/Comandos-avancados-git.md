# Comandos Avançados do Git

Técnicas para controle fino do histórico, automação com hooks, otimização de repositórios grandes e fluxos profissionais.

> Pré-requisito: domínio de rebase interativo, cherry-pick, reflog e estratégias de merge.

---

## Worktree (múltiplos diretórios de trabalho)

Permite ter várias branches abertas simultaneamente em pastas diferentes — sem precisar fazer stash ou commit de trabalho incompleto.

```bash
git worktree add ../hotfix-branch hotfix/v2.1.1   # abre a branch em outra pasta
git worktree list                                  # lista os worktrees ativos
git worktree remove ../hotfix-branch               # remove o worktree
```

Caso de uso: trabalhar em um hotfix urgente sem interromper o que está em andamento na branch atual.

---

## Sparse Checkout (repositórios grandes)

Faz checkout apenas dos diretórios necessários, sem baixar o repositório inteiro.

```bash
git clone --no-checkout <url> meu-repo
cd meu-repo
git sparse-checkout init --cone
git sparse-checkout set src/frontend src/shared   # baixa apenas essas pastas
git checkout main
```

Útil em monorepos com dezenas de módulos onde você trabalha em apenas um.

---

## Shallow Clone (histórico parcial)

Clona apenas os últimos N commits, reduzindo tempo e espaço em disco.

```bash
git clone --depth 1 <url>                  # só o commit mais recente
git clone --depth 50 <url>                 # últimos 50 commits
git fetch --unshallow                      # converte para clone completo quando necessário
```

Muito usado em pipelines de CI/CD onde o histórico completo não é necessário.

---

## Filter-repo (reescrever histórico)

O `git filter-repo` substitui o depreciado `git filter-branch` para reescrita de histórico.

```bash
# Instalar (não vem com o Git)
pip install git-filter-repo

# Remover um arquivo de TODO o histórico (ex: credential commitada por engano)
git filter-repo --path senha.txt --invert-paths

# Remover uma pasta inteira do histórico
git filter-repo --path config/secrets/ --invert-paths

# Alterar e-mail em todos os commits
git filter-repo --email-callback '
    return email if email != b"antigo@email.com" else b"novo@email.com"
'
```

> Após reescrever o histórico, todos os colaboradores precisam re-clonar o repositório.

---

## Rerere (reutilizar resoluções de conflito)

Memoriza como você resolveu um conflito e aplica automaticamente se o mesmo conflito aparecer novamente.

```bash
git config --global rerere.enabled true    # ativa globalmente

git rerere                                  # exibe conflitos memorizados
git rerere forget <arquivo>                 # esquece a resolução de um arquivo
```

Muito útil em rebases frequentes de branches de longa duração.

---

## Hooks (automação por evento)

Scripts executados automaticamente em eventos do Git. Ficam em `.git/hooks/`.

Hooks mais usados:

| Hook                  | Quando executa                              |
|-----------------------|---------------------------------------------|
| `pre-commit`          | antes de criar o commit                     |
| `commit-msg`          | após digitar a mensagem, antes de commitar  |
| `pre-push`            | antes de enviar para o remoto               |
| `post-merge`          | após um merge bem-sucedido                  |
| `prepare-commit-msg`  | antes de abrir o editor de mensagem         |

**Exemplo — validar mensagem de commit (commit-msg):**
```bash
#!/bin/sh
# .git/hooks/commit-msg
MSG=$(cat "$1")
PATTERN="^(feat|fix|docs|style|refactor|test|chore|perf|build|ci|revert)(\(.+\))?(!)?: .{1,72}"

if ! echo "$MSG" | grep -qE "$PATTERN"; then
  echo "Mensagem fora do padrão Conventional Commits. Exemplo:"
  echo "  feat(auth): adicionar login com Google"
  exit 1
fi
```

```bash
chmod +x .git/hooks/commit-msg    # tornar executável
```

**Compartilhar hooks com o time** (Git 2.9+):
```bash
mkdir .githooks
git config core.hooksPath .githooks    # aponta para pasta versionada
```

---

## Signing (assinar commits e tags)

Garante autenticidade — prova que um commit foi feito por você.

**Com chave SSH (recomendado a partir do Git 2.34):**
```bash
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519.pub
git config --global commit.gpgsign true     # assina todos os commits automaticamente
git config --global tag.gpgsign true
```

**Com chave GPG:**
```bash
gpg --list-secret-keys --keyid-format=long
git config --global user.signingkey <ID_DA_CHAVE>
git config --global commit.gpgsign true
```

Verificar assinatura:
```bash
git log --show-signature
git verify-commit <hash>
git verify-tag v1.0.0
```

---

## Submodules

Incorpora outro repositório Git dentro do seu.

```bash
git submodule add <url> libs/componentes      # adiciona submodule
git submodule update --init --recursive       # inicializa após clonar
git submodule update --remote                 # atualiza para o commit mais recente do remoto

git clone --recurse-submodules <url>          # clona já inicializando os submodules
```

Remover um submodule:
```bash
git submodule deinit libs/componentes
git rm libs/componentes
```

---

## Subtree (alternativa ao submodule)

Integra outro repositório sem complexidade de submodules — o conteúdo fica diretamente na árvore.

```bash
git subtree add --prefix=libs/ui <url> main --squash
git subtree pull --prefix=libs/ui <url> main --squash
git subtree push --prefix=libs/ui <url> main
```

---

## Notes (anotações em commits)

Adiciona metadados a um commit sem alterar seu hash.

```bash
git notes add -m "aprovado em code review por João" <hash>
git notes show <hash>
git log --show-notes
git push origin refs/notes/commits    # compartilhar notas com o remoto
```

---

## Maintenance (manutenção do repositório)

```bash
git gc                          # coleta de lixo — compacta objetos
git gc --aggressive             # compactação mais profunda (lenta)
git fsck                        # verifica integridade do repositório
git prune                       # remove objetos inacessíveis
git count-objects -vH           # mostra tamanho do repositório

git maintenance start           # ativa manutenção automática em background (Git 2.30+)
git maintenance stop
```

---

## Bundle (exportar repositório sem rede)

Empacota o repositório em um arquivo único para transferência offline.

```bash
git bundle create repo.bundle --all              # exporta tudo
git bundle create main.bundle main               # exporta apenas a branch main
git bundle verify repo.bundle                    # verifica o bundle
git clone repo.bundle repositorio-restaurado     # restaura a partir do bundle
git pull repo.bundle main                        # atualiza a partir do bundle
```

---

## Archive (exportar código sem histórico)

```bash
git archive --format=zip HEAD -o release.zip           # exporta HEAD como .zip
git archive --format=tar.gz v2.0.0 -o v2.0.0.tar.gz   # exporta uma tag
git archive --prefix=projeto/ HEAD | gzip > projeto.tar.gz
```

---

## Bisect automatizado com script

```bash
#!/bin/sh
# bisect-script.sh — retorna 0 se ok, 1 se com bug
npm test -- --testPathPattern="checkout" --silent
```

```bash
git bisect start
git bisect bad HEAD
git bisect good v1.0.0
git bisect run sh bisect-script.sh
git bisect reset
```

---

## Config avançado

```bash
# Rebase automático no pull
git config --global pull.rebase true

# Detectar renomeações com mais precisão
git config --global diff.renames copies

# Push apenas da branch atual (mais seguro)
git config --global push.default current

# Exibir diff durante rebase interativo
git config --global rebase.instructionFormat "[%an] %s"

# Salvar credenciais em cache por 1 hora
git config --global credential.helper "cache --timeout=3600"

# Correção automática de comandos com typo
git config --global help.autocorrect 20   # aguarda 2s antes de executar
```

---

## Refspec personalizado

Controla exatamente o que é enviado e recebido do remoto.

```bash
# Buscar uma Pull Request do GitHub localmente
git fetch origin pull/42/head:pr-42
git switch pr-42

# Configurar fetch automático de PRs no .git/config
[remote "origin"]
    fetch = +refs/pull/*/head:refs/remotes/origin/pr/*
```

---

> Referências:
> - [Pro Git Book](https://git-scm.com/book/pt-br/v2)
> - [git filter-repo](https://github.com/newren/git-filter-repo)
> - [Conventional Commits](../convencional-commits/Convencional-commits.md)
