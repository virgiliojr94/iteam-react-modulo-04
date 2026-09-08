# Guia GitHub — My Daily Habits

> **Este guia é para o seu repositório pessoal do My Daily Habits.**
> Você não deve fazer commit neste repositório do curso.

A proposta aqui é ensinar somente o mínimo de Git/GitHub necessário para registrar e entregar o projeto durante o Módulo 04.

---

## 1. Crie sua conta no GitHub

Se ainda não tiver conta, crie uma em:

https://github.com/

Use uma conta que você consiga acessar durante todas as aulas.

---

## 2. Crie o repositório do projeto

No seu GitHub, crie um repositório chamado:

```text
my-daily-habits
```

Este será o repositório usado até o fim do módulo.

> Não crie um projeto novo a cada aula. Cada encontro continua a partir da versão funcional da aula anterior.

---

## 3. Clone o seu repositório

Copie a URL HTTPS do seu próprio repositório e execute:

```bash
git clone URL-DO-SEU-REPOSITORIO
cd my-daily-habits
```

Exemplo:

```bash
git clone https://github.com/seu-usuario/my-daily-habits.git
cd my-daily-habits
```

Depois disso, trabalhe sempre dentro dessa pasta.

---

## 4. Arquivos que não devem ir para o GitHub

No arquivo `.gitignore`, mantenha pelo menos:

```gitignore
node_modules
dist
.env
.env.*
!.env.example
```

`node_modules` contém dependências instaladas localmente. `dist` é gerado pelo build. Arquivos `.env` podem conter configurações locais e não devem ser enviados por engano.

Crie também:

```text
.env.example
```

com:

```env
VITE_API_BASE_URL=
```

O `.env.example` documenta o nome da variável, mas não guarda senha, token ou chave privada.

---

## 5. Como registrar o trabalho de cada aula

Antes de criar o commit, veja o que mudou:

```bash
git status
```

Adicione as mudanças:

```bash
git add .
```

Crie o commit:

```bash
git commit -m "feat: descreva a entrega da aula"
```

Envie para o seu GitHub:

```bash
git push
```

Depois abra **o seu repositório `my-daily-habits` no navegador** e confirme que o commit apareceu.

---

## 6. Exemplos de mensagens de commit

Use mensagens curtas que expliquem o que passou a funcionar.

```text
feat: cria primeira versão do My Daily Habits
refactor: organiza hábitos em componentes
feat: adiciona comunicação entre componentes
feat: adiciona estado aos hábitos
feat: adiciona formulário de hábitos
```

O texto pode mudar de acordo com a atividade. O importante é que a mensagem descreva a entrega daquele encontro.

---

## 7. Se algo der errado

Primeiro execute:

```bash
git status
```

Depois registre três coisas:

1. qual comando você executou;
2. qual mensagem apareceu;
3. o que você esperava que acontecesse.

Não apague arquivos aleatoriamente para tentar corrigir Git.

---

## Resumo do fluxo

```text
seu projeto local
      ↓
git status
      ↓
git add .
      ↓
git commit -m "feat: ..."
      ↓
git push
      ↓
seu repositório My Daily Habits no GitHub
```
