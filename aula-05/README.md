# Aula 05 — Eventos e formulários controlados

> **Marco da aula:** digitar → validar → adicionar. O aluno cadastra um hábito sem recarregar a página.

📖 Apostila: **capítulo 5, seções 5.1 a 5.7**.

---

## Você já deve saber (vindo da Aula 04)

Continue **no mesmo** `my-daily-habits`. Não crie outro projeto.

- [ ] `App.jsx` usa `useState(initialHabits)`, `handleToggleHabit` e `Panel`;
- [ ] `HabitList` repassa `onToggle`; `HabitCard` possui “Concluir/Desmarcar”;
- [ ] há **3 hábitos** (`water`, `react-study`, `walk`), contador “1 de 3” e cliques individuais funcionando;
- [ ] o commit `feat: adiciona estado aos hábitos` aparece no seu GitHub.

Se não estiver assim, volte ao [checkpoint da Aula 04](../aula-04/README.md).

---

## O que esta aula entrega

1. Ligar `onChange` e `onSubmit` a handlers, sem executá-los na renderização.
2. Criar dois campos **controlados** por um objeto de estado.
3. Preservar o outro campo com spread e chave calculada.
4. Normalizar, validar, cadastrar e limpar somente após o sucesso.
5. Manter a alternância e o contador das aulas anteriores.

### A ideia central

```text
digitar → onChange → setForm → value
enviar → preventDefault → trim → validar → onAddHabit → setHabits → lista
```

O formulário controla os **campos**; o `App` controla os **hábitos**.

---

## Conceitos desta aula

| Conceito | Em uma frase | Apostila |
|---|---|---|
| Handler | Função passada ao evento; não chamada durante a renderização. | §5.1 |
| Passar × executar | `onSubmit={handleSubmit}` espera; `onSubmit={handleSubmit()}` não. | §5.1 |
| `preventDefault()` | Impede o envio padrão do formulário. | §5.1 |
| Campo controlado | `value` vem do state; `onChange` atualiza o state. | §5.2 |
| Estado em objeto | `form` guarda `title` e `goal`. | §5.2 |
| `event.target`, `name`, `[name]` | Identificam qual campo atualizar. | §5.3 |
| Spread | `...currentForm` preserva a outra propriedade. | §5.3 |
| `trim()` e validação | Impedem enviar campos vazios ou só com espaços. | §5.4 |
| `crypto.randomUUID()` | Cria identidade própria para o novo hábito. | §5.4 |
| Callback | `onAddHabit` entrega o objeto ao `App`. | §5.5 |
| Controlado × não controlado | `value` acompanha state; `defaultValue` indica valor inicial. | §5.6 |

### Box de atualização — SyntheticEvent (somente leitura)

React expõe uma interface de eventos `SyntheticEvent`. Em exemplos legados aparece `event.persist()` devido ao pooling de eventos de versões anteriores. No projeto, leia `event.target` **dentro do handler**. Veja a §5.6; não copie código legado.

---

## Passo a passo no seu projeto

Todos os caminhos abaixo são relativos a `my-daily-habits`. Deixe `npm run dev` aberto.

### Passo 1 — Criar o formulário

**Arquivo:** `src/components/HabitForm.jsx` (**novo**). **Ação:** crie o arquivo **inteiro**:

```jsx
import { useState } from "react";

export default function HabitForm({ onAddHabit }) {
  const [form, setForm] = useState({ title: "", goal: "" });
  const [error, setError] = useState("");

  function handleChange(event) {
    const { name, value } = event.target;

    setForm((currentForm) => ({
      ...currentForm,
      [name]: value,
    }));
  }

  function handleSubmit(event) {
    event.preventDefault();

    const title = form.title.trim();
    const goal = form.goal.trim();

    if (!title || !goal) {
      setError("Preencha o hábito e a meta.");
      return;
    }

    onAddHabit({
      id: crypto.randomUUID(),
      title,
      goal,
      completed: false,
    });

    setForm({ title: "", goal: "" });
    setError("");
  }

  return (
    <form className="habit-form" onSubmit={handleSubmit}>
      <div className="field">
        <label htmlFor="title">Hábito</label>
        <input
          id="title"
          name="title"
          value={form.title}
          onChange={handleChange}
          placeholder="Ex.: Ler"
        />
      </div>

      <div className="field">
        <label htmlFor="goal">Meta</label>
        <input
          id="goal"
          name="goal"
          value={form.goal}
          onChange={handleChange}
          placeholder="Ex.: 20 minutos"
        />
      </div>

      {error && <p className="form-error">{error}</p>}

      <button type="submit">Adicionar hábito</button>
    </form>
  );
}
```

**O que é:** `handleChange` copia `name` e `value`; o spread evita perder o segundo campo. `handleSubmit` impede recarga, normaliza, valida, envia um objeto com id único e **só então** limpa os campos e o erro.

**Você deve ver:** ainda **nada novo** na página: o `App` não importa o formulário.

**Se der erro:** `Unexpected token` pode indicar arquivo colado parcialmente. O deck divide o código para ensinar, mas o arquivo precisa ficar completo.

### Passo 2 — Conectar o formulário ao App

**Arquivo:** `src/App.jsx`. **Ação:** faça três mudanças e preserve `handleToggleHabit`:

1. **Import**, junto aos outros imports, antes de `HabitList`:

```jsx
import HabitForm from "./components/HabitForm";
```

2. **Função**, dentro de `App`, após `handleToggleHabit` e antes do `return`:

```jsx
function handleAddHabit(newHabit) {
  setHabits((currentHabits) => [
    ...currentHabits,
    newHabit,
  ]);
}
```

3. **Painel**, acima do painel “Hábitos de hoje”:

```jsx
<Panel title="Novo hábito">
  <HabitForm onAddHabit={handleAddHabit} />
</Panel>
```

**O que é:** `HabitForm` recebe uma função por prop e informa o objeto novo; o `App` acrescenta o hábito com um **novo array**, sem mutar o anterior.

**Você deve ver:** formulário acima da lista. Enviar “Ler” / “20 minutos” cria um cartão **sem recarregar**.

**Se der erro:** `HabitForm is not defined` indica import ausente; `onAddHabit is not a function` indica prop com nome divergente.

### Passo 3 — Acrescentar o CSS

**Arquivo:** `src/App.css`. **Ação:** adicione **ao final**, sem apagar os estilos anteriores:

```css
.habit-form {
  display: grid;
  gap: 16px;
}

.field {
  display: grid;
  gap: 6px;
}

.field input {
  width: 100%;
  padding: 10px 12px;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  font: inherit;
}

.form-error {
  margin: 0;
  color: #b42318;
  font-weight: 700;
}
```

**Você deve ver:** campos organizados, erro em vermelho. O botão mantém **estilo padrão do navegador**: isso é esperado pela apostila.

**Se der erro:** confira `className="habit-form"`, `className="field"` e `className="form-error"`.

---

## Como deve ficar o `App.jsx`

Compare este checkpoint após os três passos. `HabitList`, `HabitCard`, `Panel` e `src/data/habits.js` não mudam:

```jsx
import { useState } from "react";
import "./App.css";
import HabitForm from "./components/HabitForm";
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

  function handleAddHabit(newHabit) {
    setHabits((currentHabits) => [
      ...currentHabits,
      newHabit,
    ]);
  }

  return (
    <main className="app">
      <header className="hero">
        <p className="eyebrow">MY DAILY HABITS</p>
        <h1>Pequenos hábitos, progresso visível.</h1>
        <p>{completedCount} de {habits.length} hábitos concluídos.</p>
      </header>

      <Panel title="Novo hábito">
        <HabitForm onAddHabit={handleAddHabit} />
      </Panel>

      <Panel title="Hábitos de hoje">
        <HabitList habits={habits} onToggle={handleToggleHabit} />
      </Panel>
    </main>
  );
}
```

---

## Nota sobre a apostila

Os códigos seguem o **capítulo 5**; o texto apenas explicita onde inserir **import, handler e painel** no `App` que veio da Aula 04. A lógica de alternar os hábitos é preservada.

---

## Arquivos que mudam hoje

```text
src/
├── App.jsx                 ← import, handler e painel
├── App.css                 ← regras novas de formulário
└── components/
    └── HabitForm.jsx      ← novo arquivo
```

Nenhuma alteração em `HabitList`, `HabitCard`, `Panel`, `src/data/habits.js` ou `main.jsx`.

---

## Critério de pronto

- [ ] Envio **vazio** mostra “Preencha o hábito e a meta.” e não adiciona cartão.
- [ ] Preencher **só um campo** mantém o texto e mostra a mensagem.
- [ ] Dois campos válidos criam **um cartão**, limpam campos e erro.
- [ ] O contador acompanha o cadastro; o hábito novo também alterna concluído/pendente.
- [ ] Envio não recarrega a página; console sem erros nem aviso de `key`.
- [ ] Commit visível no GitHub do aluno.

```bash
git status
git add src/App.jsx src/App.css src/components/HabitForm.jsx
git commit -m "feat: adiciona formulário de hábitos"
git push
```

---

## Exercícios

Faça os [exercícios da Aula 05](./exercicios.md). **Todos são temporários; restaure as alterações antes do commit.**

---

## Praticar na documentação oficial

| Referência | Foco |
|---|---|
| [Respondendo a eventos](https://react.dev/learn/responding-to-events) | Passar handlers. |
| [Atualizando objetos no estado](https://react.dev/learn/updating-objects-in-state) | Spread e imutabilidade. |
| [Referência de input](https://react.dev/reference/react-dom/components/input) | `value` e `onChange`. |
| [LabEx: React para iniciantes](https://labex.io/pt/learn/react) | Laboratórios **Event Handling** e **Forms Basics** da §5.7. |

Links externos: **não verificados**. Links individuais dos laboratórios: **não verificados**.

---

## Se você travou

| Sintoma | Confira |
|---|---|
| `HabitForm is not defined` | Import no `App.jsx`. |
| `onAddHabit is not a function` | Nome da prop nos dois componentes. |
| Campo não aceita digitação | `value` exige `onChange` atualizado por setter. |
| Digitar em um campo apaga o outro | Spread `...currentForm`. |
| Erro de `trim` em `undefined` | Propriedade ausente do objeto do formulário. |
| Página recarrega no submit | `event.preventDefault()`. |
| Novo hábito não aparece | `handleSubmit → onAddHabit → setHabits → HabitList`. |
| Hábito novo não alterna | Preserve `onToggle` da Aula 04. |
| Aviso de `key` | Id único criado no cadastro. |

---

## Para ir além

Leia `§5.6`: explique `value` × `defaultValue` e o Box sobre `SyntheticEvent`. Não altere a entrega para usar campos não controlados.

---

## Prepare-se para a próxima aula (Aula 06)

- [ ] Restaure o projeto para os três hábitos iniciais no arquivo de dados, sem mudanças temporárias.
- [ ] Confirme o formulário e o toggle; envie o commit da Aula 05.
- [ ] Cadastre um novo hábito e pressione **F5**: ele **desaparece**.

A **Aula 06** resolverá essa perda com `useEffect` e `localStorage`.

---

[⬅️ Aula 04](../aula-04/README.md) · [🏠 Início](../README.md) · [➡️ Aula 06](../aula-06/README.md)
