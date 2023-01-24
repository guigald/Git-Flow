# Git flow
## Boas práticas

### Nomenclatura de Branches

- `feat/bcl-456-novo-modal-sms`
- `fix/bcl-456-evento-nao-carrega`
- `feat/novo-modal-sms`
- `fix/evento-nao-carrega`
- `refac/troca-loading-modal`

### Mensagem de Commit

#### Atomic Commit

Commits atômicos estão entre regras e boas práticas, você deve usar com astúcia e parcimônia seguindo 3 regras:

1. Única unidade irredutível;
2. Tudo deve funcionar;
3. Claro e conciso.

#### Título do commit

- Separe o título do corpo com uma linha em branco;
- Limite o título a 50 caracteres;
- Limite a coluna do corpo a 72 caracteres;
- Títulos começando com letra maiúscula;
- Não coloque ponto no final do título;
- Utilize o verbo no imperativo. Dica crie o título com este pensamento inicial "Este commit…" (Não é para escrever isto no commit!);
- Explique o "o quê" e o "por quê" e não o "como";

#### Texto do commit

- Descrever o porquê da mudança feita;
- Porque o commit resolve este problema;
- O que afeta esse commit;
- A primeira linha é a mais importante;
- Descreva as limitações do commit;
- Não assuma que colegas entendam o problema original;
- Não assuma que o código é autoexplicativo;
- Não utilize o git commit -m.

Exemplo:
```
Implementa validação de CPF via serviço

Foi criado um novo validador que implementa o serviço de validação
da receita para consultar e avaliar se um CPF é válido no momento do
cadastro.

Este patch implementa a validação de CPF via serviço, a validação
anterior estava dando vários problemas porque a base de dados não esta
completa e muitos usuários estavam sendo bloqueados.

Para esta implementação funcionar será necessário criar uma variável
de ambiente com o token da receita. Esta descrito na issue #123 o 
token e o nome da variável deve ser TOKEN_RECEITA.

Esta implementação foi feita usando injeção de dependência, uma
técnica que ainda não foi usada nesta API. Caso tenha alguma dúvida 
veja o trecho do código onde estão as configurações para entender como 
as dependências são injetadas.

Resolves: #123
See also: #456, #789
```

## Guia prático

### Passo 1: Começar uma nova funcionalidade ou correção

```bash
1. git checkout develop
   # Muda para a branch develop

2. git fetch origin -a
   # Atualiza as referências entre os repositórios remoto e local

3. git reset --hard FETCH_HEAD
   # traz todos os commits da branch develop

4. git checkout -b [feat/fix/refac]/[cardId]-[nome-da-funcionalidade]`
   # Cria sua nova branch feature

5. Desenvolva, desenvolva e desenvolva!

6. git status
   git add
   git commit
   # Verifica suas mudanças, adiciona no stage e faz o commit
```

### Passo 2: Enviar sua nova funcionalidade
```bash
1. git checkout develop
   # Muda para a branch develop

2. git fetch origin
   # Atualiza as referências entre os repositórios remoto e local

3. git pull origin develop
   # Traz todos os commits da branch develop

4. git checkout -b [feat/fix/refac]/[cardId]-[nome-da-funcionalidade]
   # Volta para a branch que você estava trabalhando

5. git rebase origin/develop
   # Atualiza sua branch atual sem o merge, mantendo o histórico em ordem
   # Se houver conflitos, resolva-os antes de continuar!
   # O código que você fez sempre será o "current" e o que veio da branch develop será o "incoming"

6. git status
   git add
   git commit
   # Verifica suas mudanças, adiciona no stage e faz o commit

7. git push origin [feat|fix|refac]/[cardId]-[nome-da-funcionalidade]
   # Envia suas alterações para o repositório remoto

8. Faça o seu PR! Lembrem-se de usar o template de descrição.
```
### Problemas comuns

#### Cenário 1: Seu colega fez uma alteração no repositório remoto e você está no meio do seu desenvolvimento, mas precisa atualizar a sua branch:
```bash
1. git fetch origin
   # Atualiza as referências entre os repositórios remoto e local

2. git stash
   # Cria uma branch temporária contendo suas modificações até o momento.

3. git rebase origin/[develop|branch-do-colega]
   # Atualiza sua branch atual sem o merge, mantendo o histórico em ordem
   # Se houver conflitos, resolva-os antes de continuar!

4. git stash pop
   # Retorna suas modificações que estavam na branch temporária para a sua branch atual, porém agora atualizada.
```

#### Cenário 2: Caso o seu git rebase tenha dado conflito:
```bash
1. Resolva os conflitos. Caso necessário chame o autor das alterações para solucionarem as modificações necessárias.
2. git add [+ caminho dos arquivos que os conflitos foram resolvidos]
   # Adiciona os arquivos no stage

3. git rebase --continue
   # Conclui o rebase após a resolução dos conflitos ocorridos
```
#### Cenário 3: Caso o conflito tenha ocorrido após o `git stash pop`
```bash
1. Resolva os conflitos. Caso necessário chame o autor das alterações para solucionarem as modificações necessárias.
2. git add [+ caminho dos arquivos que os conflitos foram resolvidos]
   # Adiciona os arquivos no stage

3. Continue seu desenvolvimento normalmente.
```
