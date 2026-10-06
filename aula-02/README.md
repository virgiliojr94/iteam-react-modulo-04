# Aula 02 — Componentes, JSX, renderização condicional, listas e chaves

> **Marco da aula:** os três cartões deixam de ser cópias escritas à mão. Um array guarda os hábitos; os componentes mostram cada um.

📖 Apostila: **capítulo 2**, seções 2.1 a 2.7

---

## Você já deve saber (vindo da Aula 01)

Abra o **seu** projeto `my-daily-habits`. Continue nele; não crie outro.

- [ ] `npm run dev` inicia e os três cartões aparecem
- [ ] `src/App.jsx` contém três blocos `<article>` escritos à mão
- [ ] o primeiro commit está visível no seu GitHub

Se algum item falhar, volte ao [passo a passo da Aula 01](../aula-01/README.md).

---

## O que esta aula entrega

Ao final você será capaz de:

1. criar e usar `HabitCard` e `HabitList`;
2. exibir valores JavaScript no JSX com chaves;
3. guardar hábitos em objetos com `id` estável;
4. mostrar “Concluído” ou “Pendente” conforme `completed`;
5. usar `map()` e `key` para mostrar a lista;
6. adicionar um hábito alterando apenas os dados.

### A ideia central

```text
initialHabits  →  HabitList usa map()  →  HabitCard mostra cada hábito
```

`completed` é um dado fixo nesta aula. O clique que o altera vem na Aula 04.

---

## Conceitos desta aula

| Conceito | Em uma frase | Apostila |
|---|---|---|
| Componente | função que descreve uma parte da tela | 2.1 |
| JSX e `{}` | marcação que mostra valores JavaScript | 2.2 |
| Dados iniciais | objetos com `id`, `title`, `goal` e `completed` | 2.3 |
| Prop | informação recebida de outro componente | 2.4 |
| Condicional | escolhe o que mostrar conforme um dado | 2.4 |
| `map()` | produz um cartão por objeto do array | 2.5 |
| `key` | identidade estável de um item da lista | 2.5 |

---

## Passo a passo no seu projeto

Deixe `npm run dev` aberto. Todos os caminhos abaixo são relativos à pasta `my-daily-habits`.

### Passo 1 — Extrair o primeiro cartão

**Arquivo:** `src/components/HabitCard.jsx`

**Ação:** crie a pasta `components` e o arquivo completo:

```jsx
export default function HabitCard() {
  return (
    <article className="habit-card">
      <h2>Beber água</h2>
      <p>Meta: 8 copos</p>
    </article>
  );
}
```

**O que é:** um componente é uma função com nome que devolve uma parte da tela.

**Arquivo:** `src/App.jsx`

**Ação:** insira o `import` logo depois de `import "./App.css";`:

```jsx
import HabitCard from "./components/HabitCard";
```

Ainda em `App.jsx`, substitua **somente** o primeiro bloco `<article>...</article>`, o de “Beber água”, por:

```jsx
<HabitCard />
```

**Você deve ver:** os mesmos três cartões. O primeiro já vem do novo arquivo.

**Se der erro:** confira a pasta `src/components` e a maiúscula em `HabitCard`.

### Passo 2 — Usar valores dentro do JSX

**Arquivo:** `src/components/HabitCard.jsx`

**Ação:** substitua o arquivo inteiro:

```jsx
const habit = {
  title: "Beber água",
  goal: "8 copos",
};

export default function HabitCard() {
  return (
    <article className="habit-card">
      <h2>{habit.title}</h2>
      <p>Meta: {habit.goal}</p>
    </article>
  );
}
```

**O que é:** `{habit.title}` e `{habit.goal}` inserem o resultado de expressões JavaScript no JSX.

**Você deve ver:** o mesmo cartão. A tela ainda não mudou, mas os textos agora vêm do objeto.

**Se der erro:** `habit is not defined` indica que o objeto local foi omitido ou renomeado.

### Passo 3 — Separar os dados iniciais

**Arquivo:** `src/data/habits.js`

**Ação:** crie a pasta `data` e este arquivo completo:

```js
export const initialHabits = [
  {
    id: "water",
    title: "Beber água",
    goal: "8 copos",
    completed: true,
  },
  {
    id: "react-study",
    title: "Estudar React",
    goal: "30 minutos",
    completed: false,
  },
  {
    id: "walk",
    title: "Caminhar",
    goal: "20 minutos",
    completed: false,
  },
];
```

**O que é:** cada objeto representa um hábito. O `id` permanecerá igual mesmo que a posição do hábito mude.

**Você deve ver:** nenhuma mudança na tela ainda; o `App` ainda não usa este arquivo.

**Se der erro mais adiante:** confirme `export const initialHabits` e os nomes das quatro propriedades.

### Passo 4 — Receber dados e decidir o status

**Arquivo:** `src/App.jsx`

**Ação:** antes de substituir o cartão, troque **somente** a linha `<HabitCard />` por:

```jsx
<HabitCard title="Beber água" goal="8 copos" completed />
```

Esses valores são **props**, informações que o `App` entrega ao cartão. `completed` sem valor explícito significa `true`.

**Arquivo:** `src/components/HabitCard.jsx`

**Ação:** substitua o arquivo inteiro:

```jsx
export default function HabitCard({ title, goal, completed }) {
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

**O que é:** `? :` escolhe entre duas saídas. A classe `is-complete` ganhará estilo no Passo 7.

**Você deve ver:** o primeiro cartão mostra “Concluído”; os outros dois ainda são blocos manuais.

**Se der erro:** `title is not defined` indica que a assinatura não recebeu `{ title, goal, completed }`.

> Para mostrar algo apenas quando uma condição for verdadeira, existe também `completed && <span>✓</span>`. Com números, use `count > 0 && ...`: `0 && ...` pode exibir o zero na tela.

### Passo 5 — Transformar objetos em cartões

**Arquivo:** `src/components/HabitList.jsx`

**Ação:** crie o arquivo **inteiro** abaixo. O deck divide este código em dois slides; não salve só a primeira metade.

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

**O que é:** `map()` cria um `HabitCard` para cada objeto. `key={habit.id}` dá identidade estável ao item; ela não aparece na tela nem chega ao `HabitCard` como prop. A lista vazia recebe uma mensagem.

**Você deve ver:** a tela ainda igual; no passo seguinte o `App` usará `HabitList`.

**Se der erro:** `HabitCard is not defined` indica que faltou o `import`. Se o arquivo terminar na linha do `map()`, cole-o inteiro.

### Passo 6 — Compor a tela com os dados

**Arquivo:** `src/App.jsx`

**Ação:** substitua **todo** o arquivo:

```jsx
import "./App.css";
import HabitList from "./components/HabitList";
import { initialHabits } from "./data/habits";

export default function App() {
  const completedCount = initialHabits.filter(
    (habit) => habit.completed,
  ).length;

  return (
    <main className="app">
      <header className="hero">
        <p className="eyebrow">MY DAILY HABITS</p>
        <h1>Pequenos hábitos, progresso visível.</h1>
        <p>
          {completedCount} de {initialHabits.length} hábitos concluídos.
        </p>
      </header>

      <HabitList habits={initialHabits} />
    </main>
  );
}
```

**O que é:** o `App` importa dados, calcula o total concluído e entrega o array à lista. O contador vem dos dados; não precisa de outro estado.

**Você deve ver:** três hábitos, “1 de 3 hábitos concluídos” e o status de cada cartão. `App.jsx` já não repete três blocos `<article>`.

**Se der erro:** confira os caminhos de `import` e o nome `initialHabits` no arquivo de dados.

### Passo 7 — Destacar o hábito concluído

**Arquivo:** `src/App.css`

**Ação:** substitua o arquivo inteiro, preservando o estilo da Aula 01 e acrescentando o status:

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
```

**O que é:** `is-complete` dá cor ao concluído; `habit-status` destaca a palavra do status.

**Você deve ver:** o cartão “Beber água” em verde claro; os outros dois mostram “Pendente”.

**Se der erro:** confirme que `App.jsx` importa `"./App.css"` e que `src/index.css` continua vazio desde a Aula 01.

---

## Arquivos que mudam hoje

```text
src/
├── App.jsx                    ← importa a lista e os dados; mostra o total
├── App.css                    ← estilo do cartão concluído
├── components/
│   ├── HabitCard.jsx          ← NOVO; mostra um hábito
│   └── HabitList.jsx          ← NOVO; usa map() e key
└── data/
    └── habits.js              ← NOVO; dados com id estável
```

---

## Critério de pronto

- [ ] `npm run dev` inicia e aparecem três cartões
- [ ] “Beber água” está “Concluído”; os outros, “Pendente”
- [ ] o topo mostra “1 de 3 hábitos concluídos”
- [ ] `App.jsx` não contém três cópias de `<article>`
- [ ] cada hábito tem `id` próprio e a lista usa `key={habit.id}`
- [ ] o console do navegador não mostra aviso de `key` ou outro erro
- [ ] um quarto objeto em `initialHabits` cria um cartão sem mudar JSX; **remova-o após o teste** para começar a Aula 03 com os três hábitos de referência
- [ ] commit feito: `refactor: organiza hábitos em componentes`

No **seu** projeto, em um segundo terminal:

```bash
git status
git add src/App.jsx src/App.css src/components src/data
git commit -m "refactor: organiza hábitos em componentes"
git push
```

Abra o **seu** repositório `my-daily-habits` no navegador e confirme o commit.

---

## Exercícios

👉 **[exercicios.md](./exercicios.md)**

Três níveis: fixação, aplicação e desafio. Faça pelo menos os níveis 1 e 2. Desfaça os experimentos temporários antes da entrega.

---

## Praticar na documentação oficial

Cada página contém exemplos e desafios editáveis no navegador:

| Página | Foco |
|---|---|
| [Seu primeiro componente](https://react.dev/learn/your-first-component) | criar e usar um componente |
| [Renderização condicional](https://react.dev/learn/conditional-rendering) | `if`, ternário e `&&` |
| [Renderizando listas](https://react.dev/learn/rendering-lists) | `map()` e `key` |

O [LabEx React para Iniciantes](https://labex.io/pt/learn/react) é reforço opcional. A entrega oficial continua no seu GitHub.

---

## Se você travou

| Sintoma | O que conferir |
|---|---|
| tela branca | abra o console do navegador e leia o primeiro erro |
| `Failed to resolve import` | caminho, extensão e maiúsculas dos arquivos |
| `HabitCard is not defined` | o `import HabitCard from "./HabitCard";` no topo de `HabitList.jsx` |
| `initialHabits is not defined` | o `export` em `habits.js` e o `import` em `App.jsx` |
| `Cannot read properties of undefined` | `habits={initialHabits}` foi passado a `HabitList`? |
| aviso “Each child in a list should have a unique key” | `key={habit.id}` está no `HabitCard` criado dentro do `map()`? |
| todos os cartões mostram “Beber água” | `HabitCard` ainda usa o objeto local do Passo 2; substitua pelo Passo 4 |
| o status muda, mas o total não | o contador filtra `initialHabits` no `App.jsx` completo do Passo 6 |

**A regra para depurar:** siga um hábito: `initialHabits` → `App` → `HabitList` → `HabitCard` → navegador. Confira o nome do dado em cada passagem.

---

## Prepare-se para a Aula 03

- [ ] `HabitCard.jsx` e `HabitList.jsx` existem em arquivos separados
- [ ] os três hábitos vêm de `src/data/habits.js`
- [ ] todos têm `id` próprio e a lista usa `key={habit.id}`
- [ ] o commit da Aula 02 aparece no seu GitHub
- [ ] leia a seção **3.1** da apostila

**O que vem por aí:** hoje os dados descem até os cartões. Na Aula 03, o cartão receberá uma função e avisará ao `App` quando alguém clicar em “Ver detalhes”.

---

[⬅️ Aula 01](../aula-01) · [🏠 Início](../) · [Aula 03 ➡️](../aula-03)
