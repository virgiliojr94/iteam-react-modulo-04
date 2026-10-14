# Aula 06 — `useEffect` e o ciclo de vida

> **Marco da aula:** recarregar → manter → sincronizar. Os hábitos sobrevivem ao F5, e o título da aba acompanha o progresso.

📖 Apostila: **capítulo 6, seções 6.1 a 6.6**.

---

## Você já deve saber (vindo da Aula 05)

Continue **no mesmo** `my-daily-habits`:

- [ ] `HabitForm` possui dois inputs controlados, validação com `trim()` e `onAddHabit`;
- [ ] o `App` guarda `habits` com `useState(initialHabits)` e possui `handleToggleHabit` e `handleAddHabit`;
- [ ] formulário e lista estão em painéis próprios; contador acompanha cadastro e alternância;
- [ ] o commit `feat: adiciona formulário de hábitos` está no seu GitHub;
- [ ] um hábito cadastrado ainda **desaparece ao pressionar F5**.

Se não estiver assim, volte à [Aula 05](../aula-05/README.md).

---

## O que esta aula entrega

1. Diferenciar **renderização, evento e efeito**.
2. Ler os hábitos salvos uma vez na inicialização, com tratamento de JSON inválido.
3. Salvar o array com `useEffect` e dependências corretas.
4. Manter cadastro e conclusão depois do F5.
5. Atualizar `document.title` e executar limpeza (`cleanup`).

### A ideia central

```text
iniciar → ler localStorage → criar estado
mudar estado → React renderiza → Effect grava localStorage
mudar progresso → Effect atualiza título → cleanup restaura
```

O contador `completedCount` é derivado de `habits` durante a **renderização**; não precisa de outro state nem Effect.

---

## Conceitos desta aula

| Conceito | Em uma frase | Apostila |
|---|---|---|
| Renderização | Calcula JSX e contagem de hábitos. | §6.1 |
| Evento | Responde a uma ação, como cadastrar. | §6.1 |
| Effect | Sincroniza após a renderização com algo fora do React. | §6.1 |
| `localStorage` | Armazena dados em forma de texto nesta origem do navegador. | §6.2 |
| `JSON.parse` e `JSON.stringify` | Transformam texto em dados e dados em texto. | §6.2–6.3 |
| `try/catch` | Impede uma falha de JSON de derrubar a tela. | §6.2 |
| Inicialização preguiçosa | `useState(loadHabits)` passa a função inicializadora. | §6.2 |
| Dependências | Declaram valores reativos usados pelo Effect. | §6.3–6.4 |
| Cleanup | Função retornada pelo Effect para desfazer a sincronização. | §6.4 |
| Classes legadas | `componentDidMount`, `componentDidUpdate`, `componentWillUnmount`: leitura. | Box do cap. 6 |

### Box de atualização — ciclo de vida das classes (somente leitura)

Os métodos de classe representam fases de montagem, atualização e desmontagem. `useEffect` organiza a lógica por **sincronização com sistemas externos e limpeza**. A equivalência **não** é linha a linha. **Não implemente classes.**

**StrictMode (deck):** em desenvolvimento, o React pode repetir chamadas de inicializadores e ciclos de setup/cleanup para encontrar problemas. Não remova dependências para esconder execuções repetidas. A contagem exata de logs depende do ambiente.

---

## Passo a passo no seu projeto

**Só `src/App.jsx` muda hoje.** Os arquivos do formulário, dos cartões e CSS permanecem intactos.

### Passo 1 — Ler os dados salvos com segurança

**Arquivo:** `src/App.jsx`. **Ação:** faça três mudanças.

**1.1. Import:** substitua o import do React por:

```jsx
import { useEffect, useState } from "react";
```

**1.2. Entre os imports e o `export default function App()`:** inclua a constante e função completas:

```jsx
const STORAGE_KEY = "my-daily-habits:habits";

function loadHabits() {
  const savedHabits = localStorage.getItem(STORAGE_KEY);

  if (!savedHabits) {
    return initialHabits;
  }

  try {
    const parsedHabits = JSON.parse(savedHabits);
    return Array.isArray(parsedHabits)
      ? parsedHabits
      : initialHabits;
  } catch {
    return initialHabits;
  }
}
```

**1.3. No início do `App`:** troque `useState(initialHabits)` por:

```jsx
const [habits, setHabits] = useState(loadHabits);
```

**O que é:** `loadHabits` retorna `initialHabits` quando não há chave, o JSON é inválido ou o valor salvo não é array. Passar `loadHabits` **sem parênteses** evita invocar a leitura a cada renderização.

**Você deve ver:** a mesma tela da Aula 05. Ainda não há gravação.

**Se der erro:** `loadHabits is not defined` indica função fora do lugar; `initialHabits is not defined` indica import ausente.

### Passo 2 — Gravar os hábitos

**Arquivo:** `src/App.jsx`. **Ação:** dentro de `App`, **depois** do cálculo de `completedCount` e **antes** dos handlers, adicione:

```jsx
useEffect(() => {
  localStorage.setItem(
    STORAGE_KEY,
    JSON.stringify(habits),
  );
}, [habits]);
```

**O que é:** após a renderização, o Effect grava `habits` como texto. A lista de dependências contém `habits` porque é o valor reativo que o Effect lê. O Effect roda **também na montagem**, gravando os dados carregados.

**Você deve ver:** em **F12 → Application → Local Storage**, a chave `my-daily-habits:habits` aparece e muda a cada ação. Depois de cadastrar ou marcar, **F5 mantém os dados**.

**Se der erro:** confira `STORAGE_KEY`, `JSON.stringify`, `localStorage.setItem` e `[habits]`. Com `[]`, o salvamento não acompanha novas alterações.

### Passo 3 — Título da aba e limpeza

**Arquivo:** `src/App.jsx`. **Ação:** logo após o Effect de salvar, adicione:

```jsx
useEffect(() => {
  const previousTitle = document.title;

  document.title = `${completedCount}/${habits.length} hábitos concluídos`;

  return () => {
    document.title = previousTitle;
  };
}, [completedCount, habits.length]);
```

**O que é:** o Effect lê `completedCount` e `habits.length` e atualiza `document.title`. O retorno é o **cleanup**: restaura o título anterior antes de novo setup e ao desmontar.

**Você deve ver:** ao iniciar com três hábitos, a aba exibe **“1/3 hábitos concluídos”**. O título acompanha cliques e cadastros.

**Se der erro:** `useEffect is not defined` pede correção do import; título parado indica dependências ou contagem incorretas.

---

## Como deve ficar o `App.jsx`

Confira este arquivo ao final do Passo 3:

```jsx
import { useEffect, useState } from "react";
import "./App.css";
import HabitForm from "./components/HabitForm";
import HabitList from "./components/HabitList";
import Panel from "./components/Panel";
import { initialHabits } from "./data/habits";

const STORAGE_KEY = "my-daily-habits:habits";

function loadHabits() {
  const savedHabits = localStorage.getItem(STORAGE_KEY);

  if (!savedHabits) {
    return initialHabits;
  }

  try {
    const parsedHabits = JSON.parse(savedHabits);
    return Array.isArray(parsedHabits)
      ? parsedHabits
      : initialHabits;
  } catch {
    return initialHabits;
  }
}

export default function App() {
  const [habits, setHabits] = useState(loadHabits);

  const completedCount = habits.filter(
    (habit) => habit.completed,
  ).length;

  useEffect(() => {
    localStorage.setItem(
      STORAGE_KEY,
      JSON.stringify(habits),
    );
  }, [habits]);

  useEffect(() => {
    const previousTitle = document.title;
  
    document.title = `${completedCount}/${habits.length} hábitos concluídos`;
  
    return () => {
      document.title = previousTitle;
    };
  }, [completedCount, habits.length]);

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
        <p>
          {completedCount} de {habits.length} hábitos concluídos.
        </p>
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

- Coloque `STORAGE_KEY` e `loadHabits` **fora** do `App`, depois dos imports.
- A §6.5 reescreve o toggle usando **ternário** e variável `current`. O `if` e `currentHabits` da Aula 04 são **equivalentes**: não substitua a lógica que já funciona.
- Persistência é **local ao navegador**: não é banco de dados nem autenticação. **Não armazene senhas ou tokens no `localStorage`.**

---

## Arquivos que mudam hoje

```text
src/
└── App.jsx  ← import, loadHabits, Effects, dependências e cleanup
```

Não altere `src/App.css`, `HabitForm`, `HabitList`, `HabitCard`, `Panel` nem `src/data/habits.js`.

---

## Critério de pronto

- [ ] Cadastrar e pressionar **F5** mantém o hábito novo.
- [ ] Marcar/desmarcar e pressionar **F5** mantém o status.
- [ ] Existe a chave `my-daily-habits:habits` em Local Storage.
- [ ] JSON inválido (`{oops`) não derruba a tela; aparecem os **três hábitos iniciais**.
- [ ] O título da aba acompanha concluídos/total.
- [ ] Console sem erros nem loop; commit visível no GitHub.

```bash
git status
git add src/App.jsx
git commit -m "feat: persiste hábitos no navegador"
git push
```

---

## Exercícios

Faça os [exercícios da Aula 06](./exercicios.md). **Todos temporários**: restaure `[habits]` e `useState(loadHabits)` e remova logs antes da entrega.

---

## Praticar na documentação oficial

| Página | Foco |
|---|---|
| [Sincronizando com Effects](https://react.dev/learn/synchronizing-with-effects) | Sincronização após a renderização. |
| [Talvez você não precise de um Effect](https://react.dev/learn/you-might-not-need-an-effect) | Por que a contagem é derivada. |
| [Referência do useEffect](https://react.dev/reference/react/useEffect) | Dependências e limpeza. |

A §6.6 não tem LabEx; a prática oficial ocorre no projeto. **Links externos não verificados.**

---

## Se você travou

| Sintoma | Confira |
|---|---|
| `useEffect is not defined` | Import de `useEffect` e `useState`. |
| F5 perde dados | Chave, stringify e dependências `[habits]`. |
| JSON inválido derruba a tela | `JSON.parse` deve ficar protegido por `try/catch`. |
| Grava só ao montar | `[]` em vez de `[habits]`. |
| Título não muda | Dependências `[completedCount, habits.length]`. |
| Effect roda mais vezes em desenvolvimento | `StrictMode` pode repetir setup; verifique idempotência. |
| Loop de renderização | Verifique se um Effect atualiza continuamente o estado que observa. **Não provoque esse loop para experimentar.** |

---

## Para ir além

Leia o Box de classes da apostila. Explique por que `completedCount` não exige Effect. Compare armazenar dados do exercício no navegador com dados persistidos em um backend; identifique os riscos de colocar senhas no `localStorage`.

---

## Prepare-se para a próxima aula (Aula 07)

- [ ] Remova códigos temporários e faça o commit da Aula 06.
- [ ] Confirme cadastro, alternância e persistência.
- [ ] Para voltar à base de três hábitos, **apague a chave** `my-daily-habits:habits` no DevTools e recarregue.

Na **Aula 07**, o `App` deixará de concentrar estado e handlers; a responsabilidade será reorganizada com **Context**, preservando as funções existentes.

---

[⬅️ Aula 05](../aula-05/README.md) · [🏠 Início](../README.md)
