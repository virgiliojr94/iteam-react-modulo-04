# Aula 03 — Props e comunicação entre componentes

> **Marco da aula:** os cartões deixam de apenas exibir e passam a **avisar** quando são clicados.

📖 Apostila: **capítulo 3**, seções 3.1 a 3.6

---

## Você já deve saber (vindo da Aula 02)

Se algum item abaixo estiver falhando, resolva **antes** da aula.

- [ ] `npm run dev` roda sem erro
- [ ] a tela mostra três cartões de hábito
- [ ] os cartões vêm de um array, não estão escritos à mão
- [ ] cada `<HabitCard>` tem uma `key`
- [ ] o console do navegador não mostra aviso de `key`

**Seu `HabitList.jsx` deve estar assim** (arquivo completo — se o seu para no meio do `map()`, está incompleto):

```jsx
import HabitCard from "./HabitCard";

export default function HabitList({ habits }) {
  if (habits.length === 0) {
    return <p>Nenhum hábito cadastrado.</p>;
  }

  return (
    <section className="habit-list" aria-label="Hábitos de hoje">
      {habits.map((habit) => (
        <HabitCard
          key={habit.id}
          title={habit.title}
          goal={habit.goal}
          completed={habit.completed}
        />
      ))}
    </section>
  );
}
```

---

## O que esta aula entrega

Ao final você será capaz de:

1. diferenciar **dado local**, **prop** e **callback**;
2. desestruturar props na assinatura e definir valor padrão;
3. explicar por que não se altera uma prop dentro do filho;
4. enviar uma **função** do pai para o filho e executá-la no clique;
5. usar `children` para compor uma moldura reutilizável.

### A ideia central

```text
dados descem  →  ações sobem  →  children compõe
```

Nada "volta" pela árvore de componentes. O pai entrega uma função; o filho apenas a executa e avisa que algo aconteceu.

---

## Conceitos desta aula

| Conceito | Em uma frase | Apostila |
|---|---|---|
| Props | entradas que o pai escolhe e o filho lê | 3.1 |
| Somente leitura | o filho nunca altera uma prop | 3.1 |
| Valor padrão | vale só quando a prop chega como `undefined` | 3.1 |
| Pai → filho | dados percorrem a árvore para baixo | 3.2 |
| Callback | função desce; o evento sobe como execução | 3.3 |
| Spread `{...habit}` | espalha as chaves do objeto como props | 3.3 |
| `children` | o conteúdo escrito entre a abertura e o fechamento | 3.4 |

---

## Passo a passo no seu projeto

Continue no `my-daily-habits` da Aula 02. Deixe `npm run dev` aberto. Os caminhos abaixo são relativos à pasta do projeto. Faça cada passo antes de seguir para o próximo.

### Passo 1 — Definir uma meta padrão

**Arquivo:** `src/components/HabitCard.jsx`

**Ação:** substitua o arquivo inteiro:

```jsx
export default function HabitCard({
  title,
  goal = "Sem meta definida",
  completed,
}) {
  return (
    <article className={`habit-card ${completed ? "is-complete" : ""}`}>
      <div>
        <h2>{title}</h2>
        <p>Meta: {goal}</p>
      </div>
      <span className="habit-status">
        {completed ? "Concluído" : "Pendente"}
      </span>
    </article>
  );
}
```

**O que é:** o `App` fornece as props; o cartão lê os valores. O padrão de `goal` só entra quando a prop chega como `undefined`.

**Você deve ver:** os mesmos três cartões da Aula 02, com os status preservados.

**Se der erro:** confirme que manteve o `return` inteiro e que `goal` tem o mesmo nome usado em `HabitList`.

### Passo 2 — Criar a ação no pai

**Arquivo:** `src/App.jsx`

**Ação:** substitua o arquivo inteiro:

```jsx
import "./App.css";
import HabitList from "./components/HabitList";
import { initialHabits } from "./data/habits";

export default function App() {
  const completedCount = initialHabits.filter(
    (habit) => habit.completed,
  ).length;

  function handleShowDetails(habitId) {
    const habit = initialHabits.find((item) => item.id === habitId);

    if (habit) {
      const goal = habit.goal === undefined
        ? "Sem meta definida"
        : habit.goal;
      window.alert(`${habit.title} — Meta: ${goal}`);
    }
  }

  return (
    <main className="app">
      <header className="hero">
        <p className="eyebrow">MY DAILY HABITS</p>
        <h1>Pequenos hábitos, progresso visível.</h1>
        <p>
          {completedCount} de {initialHabits.length} hábitos concluídos.
        </p>
      </header>

      <HabitList
        habits={initialHabits}
        onShowDetails={handleShowDetails}
      />
    </main>
  );
}
```

**O que é:** `handleShowDetails` recebe o `id` do cartão clicado, procura o hábito e escolhe o texto do alerta. Por enquanto, `HabitList` ainda não encaminha a função.

**Você deve ver:** a tela continua igual; nenhum alerta aparece ao carregar.

**Se der erro:** confirme os imports de `HabitList` e `initialHabits` e não execute `handleShowDetails()` durante o `return`.

### Passo 3 — Encaminhar a função pela lista

**Arquivo:** `src/components/HabitList.jsx`

**Ação:** substitua o arquivo inteiro. Mantenha o `import` da primeira linha, ausente no trecho da apostila:

```jsx
import HabitCard from "./HabitCard";

export default function HabitList({ habits, onShowDetails }) {
  if (habits.length === 0) {
    return <p>Nenhum hábito cadastrado.</p>;
  }

  return (
    <section className="habit-list" aria-label="Hábitos de hoje">
      {habits.map((habit) => (
        <HabitCard
          key={habit.id}
          {...habit}
          onShowDetails={onShowDetails}
        />
      ))}
    </section>
  );
}
```

**O que é:** `{...habit}` envia `id`, `title`, `goal` e `completed` ao cartão. A lista também repassa `onShowDetails`. A `key` continua no elemento criado pelo `map()`.

**Você deve ver:** os três cartões ainda aparecem. O botão será acrescentado no próximo passo.

**Se der erro:** `HabitCard is not defined` indica que faltou o `import HabitCard from "./HabitCard";`.

### Passo 4 — Avisar o pai no clique

**Arquivo:** `src/components/HabitCard.jsx`

**Ação:** substitua o arquivo inteiro:

```jsx
export default function HabitCard({
  id,
  title,
  goal = "Sem meta definida",
  completed,
  onShowDetails,
}) {
  return (
    <article className={`habit-card ${completed ? "is-complete" : ""}`}>
      <div>
        <h2>{title}</h2>
        <p>Meta: {goal}</p>
      </div>
      <div className="habit-actions">
        <span className="habit-status">
          {completed ? "Concluído" : "Pendente"}
        </span>
        <button type="button" onClick={() => onShowDetails(id)}>
          Ver detalhes
        </button>
      </div>
    </article>
  );
}
```

**O que é:** `onClick` recebe uma função para executar depois. Ao clicar, o cartão chama a função do `App` com seu próprio `id`. O cartão não altera os dados.

**Você deve ver:** três botões. Cada clique abre um alerta com o título e a meta do cartão escolhido; o status continua visível.

**Se der erro:** alerta ao abrir a página indica `onClick={onShowDetails(id)}`. Use `onClick={() => onShowDetails(id)}`. Se aparecer `onShowDetails is not a function`, confira os passos 2 e 3.

### Passo 5 — Envolver a lista em um painel

**Arquivo:** `src/components/Panel.jsx`

**Ação:** crie o arquivo completo:

```jsx
export default function Panel({ title, children }) {
  return (
    <section className="panel">
      <header className="panel-header">
        <h2>{title}</h2>
      </header>
      <div className="panel-content">{children}</div>
    </section>
  );
}
```

**O que é:** `children` é o conteúdo colocado entre `<Panel>` e `</Panel>`. O painel conhece sua moldura; a lista continua responsável pelos cartões.

**Arquivo:** `src/App.jsx`

**Ação:** substitua o arquivo inteiro:

```jsx
import "./App.css";
import HabitList from "./components/HabitList";
import Panel from "./components/Panel";
import { initialHabits } from "./data/habits";

export default function App() {
  const completedCount = initialHabits.filter(
    (habit) => habit.completed,
  ).length;

  function handleShowDetails(habitId) {
    const habit = initialHabits.find((item) => item.id === habitId);

    if (habit) {
      const goal = habit.goal === undefined
        ? "Sem meta definida"
        : habit.goal;
      window.alert(`${habit.title} — Meta: ${goal}`);
    }
  }

  return (
    <main className="app">
      <header className="hero">
        <p className="eyebrow">MY DAILY HABITS</p>
        <h1>Pequenos hábitos, progresso visível.</h1>
        <p>
          {completedCount} de {initialHabits.length} hábitos concluídos.
        </p>
      </header>

      <Panel title="Hábitos de hoje">
        <HabitList
          habits={initialHabits}
          onShowDetails={handleShowDetails}
        />
      </Panel>
    </main>
  );
}
```

**Você deve ver:** o título do painel acima da lista; botões, status e contador continuam funcionando.

**Se der erro:** `Panel is not defined` indica que faltou o `import Panel from "./components/Panel";`.

### Passo 6 — Dar estilo ao painel e aos botões

**Arquivo:** `src/App.css`

**Ação:** substitua o arquivo inteiro:

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
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  padding: 20px;
  border: 1px solid #d7dde5;
  border-radius: 12px;
  background: #ffffff;
}

.habit-card.is-complete {
  border-color: #78b87a;
  background: #f0fff2;
}

.habit-status {
  font-weight: 700;
}

.habit-actions {
  display: flex;
  align-items: center;
  gap: 12px;
}

.habit-actions button {
  padding: 8px 12px;
  border: 1px solid #9aa9ba;
  border-radius: 8px;
  background: #ffffff;
  color: inherit;
  cursor: pointer;
}

.panel {
  overflow: hidden;
  border: 1px solid #d7dde5;
  border-radius: 16px;
  background: #ffffff;
}

.panel-header,
.panel-content {
  padding: 20px;
}

.panel-header {
  border-bottom: 1px solid #e5e7eb;
}

.panel-header h2 {
  margin: 0;
}

@media (max-width: 600px) {
  .habit-card {
    align-items: flex-start;
    flex-direction: column;
  }
}
```

**O que é:** `panel` cria a moldura, `habit-actions` agrupa status e botão. A regra final deixa o cartão caber em telas estreitas.

**Você deve ver:** lista dentro do painel, três status e três botões legíveis. O contador continua mostrando `1 de 3 hábitos concluídos`.

**Se der erro:** confirme o `import "./App.css";` no topo de `App.jsx` e mantenha `src/index.css` vazio.

---

## Arquivos que mudam hoje

```text
src/
├── App.jsx                    ← cria handleShowDetails e usa Panel
├── App.css                    ← estilo do painel e do botão
└── components/
    ├── HabitCard.jsx          ← recebe id e onShowDetails; ganha botão
    ├── HabitList.jsx          ← encaminha o callback
    └── Panel.jsx              ← ARQUIVO NOVO
```

---

## Critério de pronto

Rode isto no seu projeto depois da aula:

- [ ] os três cartões continuam mostrando "Concluído" ou "Pendente"
- [ ] clicar em "Ver detalhes" abre um alerta
- [ ] o alerta mostra o título e a meta **daquele** cartão, não sempre o mesmo
- [ ] a lista aparece dentro de um painel com borda e título
- [ ] o contador do topo continua funcionando
- [ ] console sem erros
- [ ] commit feito: `feat: adiciona comunicação entre componentes`

---

## Exercícios

👉 **[exercicios.md](./exercicios.md)**

Três níveis: fixação, aplicação no projeto e desafio. Faça pelo menos os níveis 1 e 2.

---

## Praticar na documentação oficial

Cada página abaixo termina com uma seção de **desafios**: enunciado, editor no navegador e solução comentada. Não precisa instalar nada.

| Página | Foco |
|---|---|
| [Passando props para um componente](https://pt-br.react.dev/learn/passing-props-to-a-component) | props, desestruturação, `children` |
| [Respondendo a eventos](https://pt-br.react.dev/learn/responding-to-events) | passar função × executar função |

> Role até o fim da página para achar os desafios.

---

## Se você travou

| Sintoma | O que conferir |
|---|---|
| `Cannot read properties of undefined` | o nome da prop está igual nos dois lados? |
| o alerta dispara sozinho ao abrir a página | você escreveu `onClick={f(id)}` em vez de `onClick={() => f(id)}` |
| todos os botões mostram o mesmo hábito | o `id` está chegando ao cartão? confira o `{...habit}` |
| `onShowDetails is not a function` | falta algum elo: App → HabitList → HabitCard |
| o painel aparece vazio | o conteúdo está entre `<Panel>` e `</Panel>`? |

**A regra para depurar:** siga a função de cima para baixo. Ela nasce no `App`, passa pelo `HabitList` e é executada no `HabitCard`. Se algum desses três não a menciona, o elo está quebrado ali.

---

## Para ir além

- [Passando props para um componente](https://pt-br.react.dev/learn/passing-props-to-a-component)
- [Respondendo a eventos](https://pt-br.react.dev/learn/responding-to-events)
- [Pensando em React](https://pt-br.react.dev/learn/thinking-in-react) — o passo 5, *fluxo de dados inverso*, é exatamente o que fizemos hoje
- Stefanov — capítulos sobre props, callbacks e `children`
- Elliott — composição e decomposição de problemas

---

## Prepare-se para a Aula 04

- [ ] confirme que "Ver detalhes" funciona em **todos** os cartões
- [ ] deixe o commit da Aula 03 no seu GitHub
- [ ] leia a seção **4.1** da apostila

**O que vem por aí:** hoje o clique avisa. Na próxima aula, o clique vai **mudar a tela**. Repare que `completed` está fixo no arquivo de dados — isso vai virar memória do componente.

---

[⬅️ Aula 02](../aula-02) · [🏠 Início](../) · [Aula 04 ➡️](../aula-04)
