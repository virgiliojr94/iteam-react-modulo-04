# Exercícios — Aula 02
## Componentes, JSX, condicionais, listas e chaves

Faça os níveis **1 e 2** no seu `my-daily-habits`. O nível 3 é opcional. Comece com o projeto funcionando como no [README da Aula 02](./README.md). Em testes temporários, **desfaça a mudança antes de seguir**.

---

# Nível 1 — Fixação

### 1.1 — Um dado muda duas partes da tela

**Arquivo:** `src/data/habits.js`

**Ação:** no objeto “Beber água”, troque temporariamente `completed: true` por `completed: false`.

**Observe:** o cartão deve mostrar “Pendente”, perder o destaque verde e o topo deve passar de “1 de 3” para “0 de 3”.

**Responda:** quais componentes mostraram a mudança? Você precisou editar o JSX deles? **Volte para `true`.**

### 1.2 — O aviso de key

**Arquivo:** `src/components/HabitList.jsx`

**Ação:** remova temporariamente apenas a linha `key={habit.id}` do `<HabitCard>` criado dentro de `map()`.

Abra o console do navegador. **Responda:** apareceu um aviso sobre `key`? A tela continuou mostrando os cartões? Por que a aparência sozinha não basta para considerar a lista pronta?

**Reponha `key={habit.id}`** e confirme que não há aviso novo depois de recarregar.

### 1.3 — E se a lista estiver vazia?

**Arquivo:** `src/App.jsx`

**Ação:** substitua temporariamente somente `<HabitList habits={initialHabits} />` por:

```jsx
<HabitList habits={[]} />
```

**Observe:** a lista mostra “Nenhum hábito cadastrado.” O contador do topo continua calculado sobre `initialHabits`, porque você alterou apenas o dado enviado para `HabitList`.

**Responda:** em qual arquivo está a condição `habits.length === 0`? **Volte ao `initialHabits` antes de seguir.**

---

# Nível 2 — Aplicação no projeto

### 2.1 — Personalize uma meta

**Arquivo:** `src/data/habits.js`

**Ação:** altere a meta de um dos três hábitos para algo seu.

**Critério:** o texto do cartão muda sem editar `HabitCard.jsx` nem `HabitList.jsx`. Mantenha esta mudança.

### 2.2 — Um quarto hábito como teste

**Arquivo:** `src/data/habits.js`

**Ação:** adicione temporariamente outro objeto ao final de `initialHabits`, com `id` **diferente** dos três atuais, `title`, `goal` e `completed`.

**Critério:** um quarto cartão aparece e o contador passa a usar quatro como total. Se `completed` for `true`, o número de concluídos também aumenta.

**Responda:** quais arquivos de componente você precisou mudar? **Remova o quarto objeto após o teste**: a Aula 03 começa com os três hábitos de referência.

### 2.3 — Confira a entrega

Confirme: três cartões, contador correto, console sem aviso de `key` e `npm run dev` sem erro. Se ainda não registrou a Aula 02, use os comandos do [Critério de pronto](./README.md#critério-de-pronto). Se já registrou, faça outro commit para sua meta personalizada:

```bash
git status
git add src/data/habits.js
git commit -m "feat: personaliza meta de hábito"
git push
```

Abra o **seu** GitHub e confirme o commit. Não envie seu projeto para o repositório do curso.

---

# Nível 3 — Desafio opcional

### 3.1 — Um símbolo que aparece só para o concluído

**Arquivo:** `src/components/HabitCard.jsx`

**Ação:** imediatamente **antes** do `<span className="habit-status">`, insira:

```jsx
{completed && <span aria-label="Concluído">✓</span>}
```

**Critério:** somente cartões com `completed: true` exibem o símbolo. Troque um valor em `habits.js` para testar, depois restaure.

**Responda:** por que o símbolo aparece quando `completed` é `true` e desaparece quando é `false`? Como isso difere do texto que usa o ternário?

---

[⬅️ Voltar para a Aula 02](./README.md)
