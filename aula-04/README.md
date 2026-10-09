# Aula 04 — Estado com `useState` e o legado das classes

> **Marco da aula:** clicar marca e desmarca um hábito; a tela lembra a mudança.

📖 Apostila: **capítulo 4**, seções 4.1 a 4.6. As classes aparecem apenas no **Box de atualização**, para leitura.

---

## Você já deve saber (vindo da Aula 03)

Continue no mesmo projeto `my-daily-habits`. A aula começa no checkpoint da [Aula 03](../aula-03/README.md):

- [ ] há três hábitos: `water`, `react-study` e `walk`;
- [ ] `App.jsx` mostra `1 de 3 hábitos concluídos`, usa `Panel` e tem `handleShowDetails`;
- [ ] `HabitList.jsx` encaminha `onShowDetails` e importa `HabitCard`;
- [ ] `HabitCard.jsx` mostra o status, usa `goal = "Sem meta definida"` e tem o botão “Ver detalhes”;
- [ ] os três botões abrem o alerta do hábito correspondente.

O início de `HabitList.jsx` é este. Repare no `import` da primeira linha:

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

Os dados, o `Panel` e o CSS já estão prontos e permanecem como estão. Se o checkpoint estiver diferente, volte à Aula 03 antes de começar.

---

## O que esta aula entrega

Ao final, você será capaz de:

1. guardar e atualizar uma lista com `useState`;
2. alternar o hábito certo sem alterar o array anterior;
3. encaminhar uma função do `App` até o cartão;
4. calcular o contador a partir do estado, sem criar outro estado;
5. reconhecer o padrão de estado em classes antigas, sem usá-las no projeto.

### A ideia central

```text
clicar  →  lembrar  →  renderizar
```

O cartão avisa qual `id` foi clicado; o `App`, dono dos dados, cria a próxima versão dos hábitos. O React renderiza a tela com ela.

---

## Conceitos desta aula

| Conceito | Em uma frase | Apostila |
|---|---|---|
| State e setter | valor lembrado pelo componente e função que pede uma atualização | §4.1 |
| `useState` e Regra dos Hooks | declara estado no topo do componente | §4.1 |
| Função atualizadora | recebe o estado mais recente para calcular o próximo | §4.2 |
| Imutabilidade e spread | cria um array e um objeto novos sem remendar os anteriores | §4.2 |
| O cartão pede a mudança | o evento envia o `id`; o pai atualiza o estado | §4.3 |
| Valor derivado | o contador é calculado a partir de `habits` | §4.4 |
| Checkpoint | fluxo completo do clique até a renderização | §4.5 |
| LabEx | prática opcional de estado com Hooks | §4.6 |
| Classes | leitura do exemplo legado no Box de atualização | Box de atualização |

### Box de atualização — classes (somente leitura)

Em código legado, reconheça `this.state`, `this.setState(...)` e `render()`. Em classes, `this.setState` pode mesclar parcialmente um objeto; o setter de `useState` substitui o valor guardado. Neste projeto, usamos uma função com `useState`. Não crie uma classe nem copie o exemplo para o projeto.

---

## Passo a passo no seu projeto

Trabalhe no projeto que terminou a Aula 03. Faça os passos na ordem. Os caminhos abaixo são relativos à pasta `my-daily-habits`.

### Passo 1 — Criar o estado no `App`

**Arquivo:** `src/App.jsx`

**Ação:** importe `useState`. No início de `App`, guarde os hábitos em estado e calcule o contador a partir de `habits`:

```jsx
import { useState } from "react";
```

```jsx
const [habits, setHabits] = useState(initialHabits);

const completedCount = habits.filter(
  (habit) => habit.completed,
).length;
```

No cabeçalho, use `{completedCount} de {habits.length} hábitos concluídos.`. No `<HabitList>`, troque `habits={initialHabits}` por `habits={habits}`.

**O que é:** `useState` devolve o valor desta renderização e o setter que solicita uma atualização. O array inicial é usado na primeira renderização; depois, o estado é a fonte dos dados exibidos.

**Você deve ver:** a tela permanece igual: “1 de 3 hábitos concluídos”. O editor pode avisar que `setHabits` ainda não é usado; vamos usá-lo no próximo passo.

**Se der erro:** `useState is not defined` indica que faltou o import. O Hook deve ficar no topo de `App`, fora de `if`, laços e funções aninhadas.

### Passo 2 — Criar a função atualizadora

**Arquivo:** `src/App.jsx`

**Ação:** apague `handleShowDetails` e coloque esta função no lugar:

```jsx
function handleToggleHabit(habitId) {
  setHabits((currentHabits) =>
    currentHabits.map((habit) => {
      if (habit.id === habitId) {
        return {
          ...habit,
          completed: !habit.completed,
        };
      }

      return habit;
    }),
  );
}
```

No `<HabitList>`, troque `onShowDetails={handleShowDetails}` por:

```jsx
onToggle={handleToggleHabit}
```

**O que é:** a função recebida pelo setter usa o estado mais recente. `map` cria um array novo; para o `id` clicado, o spread cria um objeto novo com `completed` invertido. Os outros objetos são devolvidos sem alteração.

**Você deve ver:** a tela ainda não muda. Se clicar agora, aparece **`onShowDetails is not a function`**. Isso é esperado até o Passo 4: `HabitList` e `HabitCard` ainda usam o nome antigo.

**Se der erro:** confira se o Passo 1 criou `setHabits` e se substituiu a função inteira. Não use `habit.completed = ...` para atualizar o estado.

### Passo 3 — Encaminhar `onToggle` pela lista

**Arquivo:** `src/components/HabitList.jsx`

**Ação:** substitua a assinatura e o cartão dentro do `map` por este arquivo completo. Mantenha o import:

```jsx
import HabitCard from "./HabitCard";

export default function HabitList({ habits, onToggle }) {
  if (habits.length === 0) {
    return <p>Nenhum hábito cadastrado.</p>;
  }

  return (
    <section className="habit-list" aria-label="Hábitos de hoje">
      {habits.map((habit) => (
        <HabitCard
          key={habit.id}
          {...habit}
          onToggle={onToggle}
        />
      ))}
    </section>
  );
}
```

**O que é:** a lista apenas encaminha a mesma função para cada cartão. Ela não decide como o estado muda.

**Você deve ver:** os cartões continuam visualmente iguais. Se clicar, o erro `onShowDetails is not a function` ainda é esperado até o Passo 4.

**Se der erro:** `HabitCard is not defined` indica que faltou `import HabitCard from "./HabitCard";`.

### Passo 4 — O cartão pede a mudança

**Arquivo:** `src/components/HabitCard.jsx`

**Ação:** faça somente estas três trocas e mantenha o restante do cartão da Aula 03:

1. Na assinatura, troque `onShowDetails` por `onToggle`.
2. No botão, passe uma função que chama `onToggle` com o `id`:

```jsx
<button type="button" onClick={() => onToggle(id)}>
```

3. Troque o texto “Ver detalhes” por:

```jsx
{completed ? "Desmarcar" : "Concluir"}
```

Mantenha `goal = "Sem meta definida"`, o status “Concluído/Pendente” e a `div.habit-actions`.

**O que é:** o cartão não possui o estado. Ele chama a função que recebeu do pai; o `App` altera os dados e passa os valores atualizados de volta.

**Você deve ver:** ao clicar em “Estudar React”, esse cartão fica verde, mostra “Concluído” e o botão vira “Desmarcar”. O topo passa a “2 de 3”. Clique de novo para desfazer. Os demais cartões não mudam.

**Se der erro:** `onToggle is not a function` indica que algum elo ainda usa o nome antigo. Confira `App → HabitList → HabitCard`. Se todos os cartões mudarem, confira `habit.id === habitId`.

---

## Como deve ficar o `App.jsx`

Use este arquivo para conferir o resultado final dos passos:

```jsx
import { useState } from "react";
import "./App.css";
import HabitList from "./components/HabitList";
import Panel from "./components/Panel";
import { initialHabits } from "./data/habits";

export default function App() {
  const [habits, setHabits] = useState(initialHabits);

  const completedCount = habits.filter(
    (habit) => habit.completed,
  ).length;

  function handleToggleHabit(habitId) {
    setHabits((currentHabits) =>
      currentHabits.map((habit) => {
        if (habit.id === habitId) {
          return {
            ...habit,
            completed: !habit.completed,
          };
        }

        return habit;
      }),
    );
  }

  return (
    <main className="app">
      <header className="hero">
        <p className="eyebrow">MY DAILY HABITS</p>
        <h1>Pequenos hábitos, progresso visível.</h1>
        <p>
          {completedCount} de {habits.length} hábitos concluídos.
        </p>
      </header>

      <Panel title="Hábitos de hoje">
        <HabitList
          habits={habits}
          onToggle={handleToggleHabit}
        />
      </Panel>
    </main>
  );
}
```

---

## Nota sobre a apostila

| No capítulo 4 | Nesta aula |
|---|---|
| O trecho de `HabitList` não mostra `import HabitCard` | Mantenha o import da primeira linha. |
| O trecho de `HabitCard` omite o status e o valor padrão de `goal` | Preserve “Concluído/Pendente” e `goal = "Sem meta definida"`; faça só as três trocas do Passo 4. |
| O checkpoint usa ternário dentro do `map` | Aqui usamos o `if` explícito de §4.2; o ternário é equivalente. |

---

## Arquivos que mudam hoje

```text
src/
├── App.jsx                    ← state, atualização e contador derivado
└── components/
    ├── HabitList.jsx          ← encaminha onToggle
    └── HabitCard.jsx          ← pede a alternância no clique
```

`Panel`, `src/data/habits.js` e os arquivos CSS não mudam.

---

## Critério de pronto

- [ ] cada hábito alterna entre “Pendente” e “Concluído” sem recarregar;
- [ ] só o cartão clicado muda;
- [ ] o contador acompanha os cliques e não tem um `useState` próprio;
- [ ] o console não mostra erro;
- [ ] o estado final tem os três hábitos, sem botões ou linhas dos exercícios;
- [ ] commit no seu projeto: `feat: adiciona estado aos hábitos` (os três arquivos listados acima).

No terminal do seu projeto:

```bash
git add src/App.jsx src/components/HabitList.jsx src/components/HabitCard.jsx
git commit -m "feat: adiciona estado aos hábitos"
git push
```

---

## Exercícios

👉 Faça os [exercícios da Aula 04](./exercicios.md). Todos os acréscimos são temporários e devem ser removidos antes do commit.

---

## Praticar na documentação oficial

| Página | Foco |
|---|---|
| [useState](https://react.dev/reference/react/useState) | declarar estado e pedir uma atualização |
| [Regras dos Hooks](https://react.dev/reference/rules/rules-of-hooks) | manter Hooks no topo e na mesma ordem |
| [Atualizar arrays no estado](https://react.dev/learn/updating-arrays-in-state) | criar a próxima versão sem mutar a anterior |

Links externos: **não verificados**. O LabEx opcional da §4.6, “React State with Hooks”, está [neste endereço](https://labex.io/tutorials/react-react-state-with-hooks-601742) (**não verificado**).

---

## Se você travou

| Sintoma | O que conferir |
|---|---|
| `useState is not defined` | o import de `useState` no topo de `App.jsx` |
| `onShowDetails is not a function` nos Passos 2 ou 3 | esperado até o Passo 4; depois, confira os nomes nos três componentes |
| `onToggle is not a function` depois do Passo 4 | o caminho `App → HabitList → HabitCard` e as props `onToggle` |
| todos os cartões mudam | a comparação `habit.id === habitId` e o `id` enviado pelo botão |
| o contador não acompanha | ele deve filtrar `habits`, não `initialHabits` |
| a tela não muda após o clique | crie um array e um objeto novos; não altere `habit.completed` diretamente |
| `HabitCard is not defined` | o import da primeira linha de `HabitList.jsx` |
| o cartão não fica verde | confira se o `className` condicional da Aula 03 foi preservado |

---

## Para ir além

- Faça o LabEx opcional da §4.6, se o link estiver disponível.
- Leia o Box de atualização e explique em palavras como estado e atualização aparecem na versão com `useState`.
- Na documentação de `useState`, observe por que uma atualização pode receber uma função quando depende do estado anterior.

---

## Prepare-se para a Aula 05

- [ ] deixe o checkpoint com três hábitos e sem elementos temporários dos exercícios;
- [ ] confirme que os três cartões alternam e o contador acompanha;
- [ ] faça o commit `feat: adiciona estado aos hábitos` no seu projeto.

Recarregar a página (F5) apaga as mudanças desta aula; persistir os dados fica para a **Aula 06**. Criar hábitos novos é o tema da **Aula 05**, com formulário controlado.

---

[⬅️ Aula 03](../aula-03/README.md) · [🏠 Início](../README.md)
