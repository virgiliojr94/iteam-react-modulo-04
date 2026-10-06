# Aula 01 — Introdução ao React, Node.js, Vite e primeiro app

> **Marco da aula:** a aplicação **roda** no seu computador e o **primeiro commit** está visível no seu GitHub.

📖 Apostila: **capítulo 1**, seções 1.1 a 1.8

---

## Antes da aula

Confira no terminal. **Os três comandos precisam responder com uma versão:**

```bash
node -v
npm -v
git --version
```

- [ ] `node -v` mostra **v20.19 ou superior**, ou **v22.12 ou superior** (instale a versão **LTS** em https://nodejs.org)
- [ ] `git --version` responde (instale em https://git-scm.com)
- [ ] VS Code instalado
- [ ] conta no GitHub (veja o [GUIA-GITHUB.md](../GUIA-GITHUB.md))
- [ ] repositório **vazio** chamado `my-daily-habits` criado no seu GitHub — **sem README, sem licença e sem .gitignore**

Configure seu nome e e-mail no Git (uma vez só). Sem isso, o commit falha com `Author identity unknown`:

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu-email@exemplo.com"
```

> **Windows:** se o `npm` der erro de *"running scripts is disabled"* no PowerShell, troque o terminal do VS Code para **Command Prompt** ou **Git Bash**.

---

## O que esta aula entrega

Ao final você será capaz de:

1. dizer, em uma frase, o que **React**, **Node.js**, **npm**, **Vite** e **navegador** fazem;
2. reconhecer um **componente** e **JSX**;
3. criar e executar um projeto React com Vite;
4. apontar `App.jsx`, `App.css` e `package.json`;
5. ver a tela atualizar ao salvar um arquivo;
6. fazer o primeiro commit e encontrá-lo no GitHub.

### A ideia central

```text
você escreve  →  npm executa o Vite  →  o navegador exibe  →  Git guarda  →  GitHub publica
```

---

## Conceitos desta aula

| Conceito | Em uma frase | Apostila |
|---|---|---|
| React | descreve a interface a partir de componentes | 1.1, 1.2 |
| Componente | uma função que devolve um pedaço de tela; o nome começa com letra maiúscula | 1.2 |
| JSX | marcação parecida com HTML escrita dentro do JavaScript; toda tag precisa ser fechada | 1.2.1 |
| Node.js | executa JavaScript fora do navegador (roda as ferramentas) | 1.2 |
| npm | baixa as bibliotecas do projeto e executa os comandos dele | 1.2 |
| Vite | converte o JSX, serve o projeto no seu computador e atualiza ao salvar | 1.4 |
| Git × GitHub | Git guarda o histórico no seu computador; GitHub guarda uma cópia online | 1.7 |

---

## Passo a passo com código

Cada passo diz **qual arquivo**, **o que fazer** e **o que você deve ver**.

### Passo 1 — Criar o projeto

**Onde:** terminal do VS Code, em uma pasta de trabalho (**não** dentro de outro projeto).

```bash
git clone https://github.com/SEU-USUARIO/my-daily-habits.git
cd my-daily-habits
npm create vite@latest . -- --template react
```

O criador do Vite faz **duas perguntas**:

| Pergunta | Resposta |
|---|---|
| `Which linter to use?` | **Oxlint** (a primeira opção) |
| `Install with npm and start now?` | **No** |

Depois:

```bash
npm install
npm run dev
```

**Você deve ver** no terminal algo como `Local: http://localhost:5173/`. Abra esse endereço no navegador: aparece a página padrão do Vite com o botão **Count is 0**.

> **Deixe esse terminal aberto.** Ele é o servidor. Para rodar outros comandos (como os do Git), abra **um segundo terminal** no VS Code, pelo botão **+**.

### Passo 2 — Esvaziar `src/index.css`

**Arquivo:** `src/index.css`
**Ação:** apague **todo o conteúdo** e salve. **Não apague o arquivo.**
**Por quê:** o template traz estilos de demonstração que brigam com os nossos (e, no modo escuro do sistema, deixam os títulos quase invisíveis).

### Passo 3 — Substituir `src/App.jsx`

**Arquivo:** `src/App.jsx`
**Ação:** selecione tudo (Ctrl+A), apague e cole o arquivo **inteiro** abaixo. Salve **só depois de colar tudo**.

```jsx
import "./App.css";

export default function App() {
  return (
    <main className="app">
      <header className="hero">
        <p className="eyebrow">MY DAILY HABITS</p>
        <h1>Pequenos hábitos, progresso visível.</h1>
        <p>Hoje começamos com uma tela simples e funcional.</p>
      </header>

      <section className="habit-list" aria-label="Hábitos de hoje">
        <article className="habit-card">
          <h2>Beber água</h2>
          <p>Meta: 8 copos</p>
        </article>

        <article className="habit-card">
          <h2>Estudar React</h2>
          <p>Meta: 30 minutos</p>
        </article>

        <article className="habit-card">
          <h2>Caminhar</h2>
          <p>Meta: 20 minutos</p>
        </article>
      </section>
    </main>
  );
}
```

**Você deve ver:** o título e três hábitos, ainda sem estilo.

**Teste a atualização ao salvar:** troque o texto do `<h1>`, salve e olhe o navegador **sem dar F5**. Depois volte ao texto original.

### Passo 4 — Substituir `src/App.css`

**Arquivo:** `src/App.css`
**Ação:** selecione tudo, apague e cole o arquivo abaixo. Salve.

```css
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  min-width: 320px;
  min-height: 100vh;
  font-family: Arial, sans-serif;
  background: #f4f6f8;
  color: #1f2937;
}

.app {
  width: min(900px, 92%);
  margin: 0 auto;
  padding: 48px 0;
}

.hero {
  margin-bottom: 28px;
}

.eyebrow {
  font-weight: 700;
  letter-spacing: 0.12em;
}

.habit-list {
  display: grid;
  gap: 16px;
}

.habit-card {
  padding: 20px;
  border: 1px solid #d7dde5;
  border-radius: 12px;
  background: #ffffff;
}
```

**Você deve ver:** cartões brancos sobre fundo cinza-claro.

### Passo 5 — Primeiro commit

**Antes do commit**, acrescente ao **final** do arquivo `.gitignore` (o Vite já ignora `node_modules` e `dist`, mas **não** ignora `.env`):

```gitignore
# variáveis de ambiente locais
.env
.env.*
!.env.example
```

Crie na raiz do projeto (ao lado do `package.json`) o arquivo **`.env.example`**:

```env
VITE_API_BASE_URL=
```

> Variável com prefixo `VITE_` vai para o código que o navegador baixa. **Nunca coloque senha ou chave secreta nela.**

Em um **segundo terminal**:

```bash
git status
git add .
git status
git commit -m "feat: cria primeira versão do My Daily Habits"
git push
```

Abra o **seu** repositório `my-daily-habits` no navegador e confirme que o commit apareceu.

---

## Arquivos que mudam hoje

```text
my-daily-habits/
├── .gitignore            ← ganha 3 linhas
├── .env.example          ← ARQUIVO NOVO
└── src/
    ├── App.jsx           ← substituído
    ├── App.css           ← substituído
    └── index.css         ← esvaziado
```

Os demais arquivos que o Vite criou (`index.html`, `main.jsx`, `package.json`, `vite.config.js`, `README.md`, `public/`) **não** são alterados hoje.

---

## Critério de pronto

- [ ] `npm run dev` roda sem erro
- [ ] a página abre sem tela branca
- [ ] o título **MY DAILY HABITS** e os **três hábitos** aparecem
- [ ] salvar uma alteração em `App.jsx` muda a tela sem F5
- [ ] não sobrou nenhum `import` de logo do exemplo padrão
- [ ] o `.env.example` existe e o `.gitignore` ignora `.env`
- [ ] commit feito: `feat: cria primeira versão do My Daily Habits`
- [ ] o commit **aparece no seu GitHub**

---

## Se você travou

| O que aparece | Causa provável | O que fazer |
|---|---|---|
| `command not found` / `'npm' is not recognized` | Node não instalado, ou terminal aberto antes da instalação | instale o Node LTS e **feche e reabra o VS Code** |
| `running scripts is disabled` (PowerShell) | o PowerShell bloqueia scripts | use **Command Prompt** ou **Git Bash** no terminal do VS Code |
| `Current directory is not empty` | o repositório foi criado com README/licença | em um clone **recém-feito**, escolha *Remove existing files and continue* |
| Tela de erro: `Expected corresponding JSX closing tag for 'p'` | tag aberta e não fechada | vá à linha indicada e feche a tag |
| Tela de erro: `Adjacent JSX elements must be wrapped in an enclosing tag` | dois elementos vizinhos sem uma tag em volta | envolva em `<main>` ou em `<>...</>` |
| Tela de erro: `Unexpected token` perto do fim do arquivo | o arquivo foi colado pela metade | cole o `App.jsx` **inteiro** de novo |
| Tela de erro: `Could not resolve './assets/...'` | sobrou um `import` de logo do exemplo | apague essa linha de `import` |
| Erro de importação de `./index.css` | você apagou o **arquivo**, não só o conteúdo | crie um `src/index.css` vazio |
| Os textos parecem **sumir** | modo escuro do sistema + `index.css` do template | Passo 2 |
| Salvei e nada mudou | servidor parado, arquivo não salvo (bolinha na aba) ou arquivo errado | confira o `npm run dev`, salve com Ctrl+S e confirme `src/App.jsx` |
| Abriu em `localhost:5174` | já existe outro servidor na 5173 | use o endereço que o terminal mostra |
| `Author identity unknown` | nome e e-mail do Git não configurados | comandos em **Antes da aula** |
| `git push` pede login | o GitHub não aceita a senha da conta | entre pelo navegador quando o Git abrir a janela de login |

**A regra para depurar:** leia a mensagem inteira. Ela diz **o quê**, **onde** (arquivo e linha) e muitas vezes **como resolver**.

---

## Para ir além

- [React — Seu primeiro componente](https://pt-br.react.dev/learn/your-first-component)
- [React — Construindo a UI](https://pt-br.react.dev/learn/describing-the-ui)
- [Vite — Getting Started](https://vite.dev/guide/)
- LabEx — [Configuração do React e Primeiro App](https://labex.io/pt/tutorials/react-react-setup-and-first-app-598881) (reforço opcional)

---

## Prepare-se para a Aula 02

- [ ] confirme que `npm run dev` roda e que os **três cartões** aparecem
- [ ] confirme que o commit da Aula 01 está no **seu** GitHub
- [ ] leia a seção **2.1** da apostila

**O que vem por aí:** hoje os três cartões foram escritos **à mão**, um copiado do outro. Se amanhã fossem 30 hábitos, você copiaria 30 vezes? Na próxima aula essa repetição vira **componente + dados**.

---

[🏠 Início](../) · [Aula 02 ➡️](../aula-02)
