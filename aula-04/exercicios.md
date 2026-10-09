# Exercícios — Aula 04
## Estado com `useState`

Comece depois do Passo 4 do [README da Aula 04](./README.md). Faça os níveis 1 e 2; o nível 3 é opcional.

**Todos os exercícios são temporários.** Antes de entregar, remova cada estado, função, botão, parágrafo e alteração de dados que você acrescentou. O projeto deve voltar ao checkpoint com três hábitos e sem botões extras.

---

# Nível 1 — Fixação

### 1.1 — Um contador que a tela lembra

**Arquivo:** `src/App.jsx`

**Ação:** temporariamente, declare este estado dentro de `App`:

```jsx
const [cliques, setCliques] = useState(0);
```

Acrescente no JSX um botão:

```jsx
<button type="button" onClick={() => setCliques(cliques + 1)}>
  Cliques: {cliques}
</button>
```

**Observe:** cada clique atualiza o número na tela. Isso contrasta com o `let cliques = 0` da demonstração da aula, que não provocava uma nova renderização.

**Desfaça:** remova o estado e o botão antes do próximo exercício.

### 1.2 — “Desmarcar todos”

**Arquivo:** `src/App.jsx`

**Ação:** crie temporariamente esta função:

```jsx
function handleResetAll() {
  setHabits((currentHabits) =>
    currentHabits.map((habit) => ({ ...habit, completed: false })),
  );
}
```

Acrescente um botão que a chama:

```jsx
<button type="button" onClick={handleResetAll}>
  Desmarcar todos
</button>
```

**Observe:** todos os cartões ficam “Pendente” e o topo mostra “0 de 3”.

**Desfaça:** remova a função e o botão.

### 1.3 — Mostrar quantos estão pendentes

**Arquivo:** `src/App.jsx`

**Ação:** calcule o valor sem criar outro estado:

```jsx
const pendingCount = habits.length - completedCount;
```

Mostre-o temporariamente no JSX:

```jsx
<p>Pendentes: {pendingCount}</p>
```

**Observe:** o total de pendentes acompanha os cliques nos cartões. `pendingCount` é derivado de `habits` e `completedCount`.

**Desfaça:** remova a variável e o parágrafo.

---

# Nível 2 — Aplicação no projeto

### 2.1 — Prever e testar uma mutação

**Arquivo:** `src/App.jsx`

Antes de testar, responda: se um objeto for alterado diretamente e o mesmo array for enviado ao setter, a tela vai atualizar?

**Ação:** temporariamente, use este corpo em uma função de teste chamada por um botão:

```jsx
const habit = habits.find((item) => item.id === "react-study");
habit.completed = !habit.completed;
setHabits(habits);
console.log(habit.completed);
```

**Observe:** a tela não atualiza com esse clique, mas o valor do objeto foi alterado. O setter recebeu o mesmo array. A resposta prevista é **não**.

**Desfaça imediatamente:** remova a função e o botão de teste e recarregue a página para voltar aos dados iniciais.

### 2.2 — Alternar dois cartões

**Ação:** clique em um cartão e depois em outro diferente.

**Observe:** os dois cartões escolhidos refletem seus próprios cliques; o terceiro não muda. Clique nos mesmos dois outra vez para conferir que cada um também volta ao estado anterior.

**Desfaça:** recarregue a página. Como o estado desta aula vive na memória, ela volta aos três valores iniciais.

---

# Nível 3 — Desafio opcional

### 3.1 — “Inverter todos”

**Arquivo:** `src/App.jsx`

Crie temporariamente um botão que inverta `completed` de cada hábito usando `map` e spread. O botão deve ser reversível: ao clicar novamente, todos voltam ao estado anterior.

**Critério:** o contador acompanha a inversão e nenhum objeto do array anterior é alterado.

### 3.2 — Por que os Hooks ficam no topo?

Leia [Regras dos Hooks](https://react.dev/reference/rules/rules-of-hooks). Sem escrever código, explique por que `useState` precisa ser chamado no topo do componente e na mesma ordem em toda renderização.

### 3.3 — O Box das classes

Leia o Box de atualização no [README da aula](./README.md). Em três ou quatro linhas, descreva a ideia equivalente no componente com `useState`. Não crie uma classe no projeto.

---

## Antes de entregar

- [ ] removi o contador e seu botão;
- [ ] removi “Desmarcar todos” e o botão “Inverter todos”, se fiz o desafio;
- [ ] removi o parágrafo “Pendentes” e a variável derivada;
- [ ] removi toda função e botão de teste de mutação;
- [ ] mantive três hábitos, sem dados ou botões extras;
- [ ] confirmei a alternância individual, o contador e o console sem erro;
- [ ] fiz o commit no meu projeto com a mensagem `feat: adiciona estado aos hábitos` e somente `src/App.jsx`, `src/components/HabitList.jsx` e `src/components/HabitCard.jsx`.

---

[⬅️ Voltar para a Aula 04](./README.md)
