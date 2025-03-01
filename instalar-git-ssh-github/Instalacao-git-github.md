0. **Abra o terminal**
<br>

1. Atualize o sistema:<br>
     `sudo apt update && sudo apt upgrade`   
2. Instalar o Git:<br>
     `sudo apt install git`
3. Confirmar versão do git:<br>
     `git --version`
4. Configurar sua identidade do Git (Nome de usuário e e-mail)<br>
    
    `git config --global user.name "monicagit-hub"`
    
    `git config --global user.email "githubmonica@gmail.com"`
    
5. Confirmar a configuração de identidade: <br>
    `cat .gitconfig`
    
6. Crie uma pasta e inicialize um repositório Git:<br>
    
    `mkdir NoteOn` (criando uma pasta chamada NoteOn)
    
    `cd NoteOn`  (acessa a pasta NoteOn)
    
    `git init` (inicializando o git dentro da pasta NoteOn)
    
7. Adicione um arquivo à pasta do seu projeto: <br>
    
    `touch readme.md` (cria um arquivo chamado readme.md)
    
    `ls` (lista tudo que tem dentro da pasta NoteOn)
    
    `git status` (Verifica o status dos arquivos)
    
8. Enviar esse arquivo para o repositorio git: <br>
    
    `git add .`
    
    `git commit -m "readme note on”`
    
    <img src="./images/1.png" width="600"><br>
<br>
    
9. **Acesse seu navegador e entre na sua conta do seu github ( [https://github.com](https://github.com/) ), clique no sinal `“ new ”` no canto superior esquerdo.** <br>
Siga as instruções para criar um novo repositório no GitHub, conforme imagem abaixo:
<br>
    <img src="./images/2.png" width="600"><br>
<br>

10. **Volte ao seu computador, abra um novo terminal, sem fechar o antigo.**<br>

    `Botão direito do mouse em cima do ícone → nova janela →  abrirá um novo terminal`

<br>

11. Nesse novo terminal, gere chaves SSH para enviarmos para a conta do GitHub.<br>
    **Observação**: dê apenas enter se não quiser adicionar uma senha<br>

    `ssh-keygen -t rsa`
    
12. Copie o conteúdo da sua chave pública:<br>

    `cat ~/.ssh/id_rsa.pub`

13. Volte a sua conta do github acessando: <br>
    
    `settings → SSH and GPG keys - new SSH key` <br>
    
    Cole a chave no campo **KEY**, conforme imagem abaixo:
    
    <img src="./images/3.png" width="600"><br>
    
<br>

**Caso tenha adicionado uma senha:**

1. Ao executar o **passo 12**. `cat ~/.ssh/id_rsa.pub` e der essa saída:

    ```jsx
    cat: /home/monicamarcal/.ssh/id_rsa.pub: Arquivo ou diretório inexistente
    ```

2. Use esse comando para exibir a chave pública e adicionar ao GitHub:
    
    **Observação**: É a senha que adicionou ai gerar o  `ssh-keygen -t rsa` <br>
        Procure por **Your public key has been saved in pink.pub** no meu caso o nome do meu arquivo é **pink.pub**,adicione o seu nome de arquivo no comando abaixo:<br><br>
        `cat pink.pub`
    
     <img src="./images/4.png" width="600"><br>
    
3. Copie a chave e cole no github
4. Mover a chave para o local padrão (opcional):
    
    `mkdir -p ~/.ssh`<br>
    `mv pink ~/.ssh/id_rsa`<br>
    `mv pink.pub ~/.ssh/id_rsa.pub`<br>
    `chmod 600 ~/.ssh/id_rsa`<br>
    `chmod 644 ~/.ssh/id_rsa.pub`<br>
    
5.  Adicionar a chave ao SSH Agent (se necessário)<br>
    Se estiver usando o SSH Agent, carregue a chave com:

    `eval "$(ssh-agent -s)”`

    `ssh-add ~/.ssh/id_rsa`

    <img src="./images/5.png" width="600"><br>
    <br>
6. Testar a conexão com o github:
    
    `ssh -T [git@github.com](mailto:git@github.com)`
    
    <img src="./images/6.png" width="600"><br>
    
<br><br>

**Retorne ao seu primeiro terminal e vamos  conectar seu repositório local ao repositório remoto**<br>

14. Adicione ao comando abaixo seu usuário do github e pasta do projeto:

    `git remote add origin git@github.com:monicagit-hub/NoteOn.git`

    <img src="./images/7.png" width="600"><br>

15. `git remote -v`
    
     <img src="./images/8.png" width="600"><br>
    
16. Verifique o nome da sua branch:
    
    `git branch`
    
17. Envie as alterações para o repositório GitHub:
    
    `git push -u origin master`
    
     <img src="./images/9.png" width="600"><br><br><br>
    

17. **Entre na sua conta Github, arquivo enviado com sucesso!**<br>

    <img src="./images/10.png" width="600"><br>