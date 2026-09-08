# Exercícios — Aula 03
## Props e comunicação entre componentes

Faça pelo menos os **níveis 1 e 2**. O nível 3 é opcional.

Trabalhe sempre no seu **My Daily Habits**. Se quebrar alguma coisa, ótimo: descobrir por que quebrou é metade do aprendizado.

---

# Nível 1 — Fixação

Exercícios curtos. Faça, observe o resultado e **desfaça** antes de seguir.

### 1.1 — O valor padrão só vale para `undefined`

No `HabitCard.jsx`, a assinatura tem:

```jsx
goal = "Sem meta definida",
```

Faça, um de cada vez, e anote o que aparece na tela:

| Teste | No `habits.js`, deixe `goal` como | O que apareceu? |
|---|---|---|
| a | `goal: "8 copos"` | |
| b | remova a linha `goal` do objeto | |
| c | `goal: ""` (string vazia) | |

**Responda:** por que o teste **c** não mostrou "Sem meta definida"?

> 💡 Dica: valor padrão entra quando a prop chega como `undefined`. String vazia é um valor.

**Desfaça** e volte ao original.

---

### 1.2 — Props são somente leitura

Dentro do `HabitCard`, antes do `return`, escreva:

```jsx
title = "MUDEI!";
```

Salve e observe.

**Responda:** o que aconteceu? A mudança se manteve? Por quê?

> 💡 O cartão é redesenhado toda vez que o pai renderiza, e o pai sempre envia o valor original de volta. O filho não é dono do dado.

**Apague a linha** antes de seguir.

---

### 1.3 — Passar × executar

No `HabitCard.jsx`, troque temporariamente:

```jsx
onClick={() => onShowDetails(id)}
```

por:

```jsx
onClick={onShowDetails(id)}
```

Salve e **recarregue a página sem clicar em nada**.

**Responda:** o alerta apareceu sozinho? Quantas vezes? Por quê?

> 💡 Sem a arrow function, a chamada acontece durante a renderização — e o React renderiza um cartão por hábito.

**Volte a forma correta.** Este é o erro mais comum da semana; se ele acontecer de novo, você vai reconhecer.

---

### 1.4 — Espalhar objeto como props

No `HabitList.jsx`, troque:

```jsx
<HabitCard key={habit.id} {...habit} onShowDetails={onShowDetails} />
```

por:

```jsx
<HabitCard key={habit.id} onShowDetails={onShowDetails} {...habit} />
```

Repare que só mudou a **ordem**.

**Responda:** o clique continuou funcionando? E se o objeto `habit` tivesse uma chave chamada `onShowDetails`, o que aconteceria?

> 💡 Props seguem a ordem de escrita: a última declaração vence.

**Volte à ordem original**, com `{...habit}` antes.

---

# Nível 2 — Aplicação no projeto

Estes ficam no projeto. Faça commit ao final.

### 2.1 — Um quarto hábito

Adicione um quarto objeto ao array em `src/data/habits.js`, com `id` único.

**Critério:** o novo cartão aparece sozinho e o botão dele abre o alerta correto. Você **não** deve precisar mexer em nenhum componente.

**Responda:** por que nenhum arquivo de componente precisou mudar?

---

### 2.2 — Um hábito sem meta

Adicione um quinto hábito **sem a propriedade `goal`**.

**Critério:** o cartão aparece com "Sem meta definida" e a aplicação não quebra.

---

### 2.3 — Segundo `Panel`

No `App.jsx`, adicione outro `Panel` abaixo do primeiro:

```jsx
<Panel title="Sobre o projeto">
  <p>My Daily Habits — projeto do Módulo 04.</p>
</Panel>
```

**Critério:** os dois painéis aparecem com o mesmo visual, mas com conteúdos completamente diferentes.

**Responda:** o `Panel` precisou saber que agora existe um parágrafo dentro dele? Por quê?

> 💡 É esse desacoplamento que `children` oferece. Sem ele, você precisaria de uma prop nova para cada tipo de conteúdo.

---

### 2.4 — O alerta vira registro

No `App.jsx`, dentro de `handleShowDetails`, adicione antes do `window.alert`:

```jsx
console.log("Hábito solicitado:", habitId, habit);
```

Abra o console, clique em cada cartão e confirme que o `id` do console é sempre o do cartão clicado.

**Critério:** cinco cartões, cinco ids diferentes no console.

Este é o hábito de depuração que vai te salvar nas próximas aulas: **antes de achar que o React está errado, verifique qual dado chegou.**

---

# Nível 3 — Desafio

Opcional. Só depois de fechar os níveis 1 e 2.

### 3.1 — Um cartão que avisa duas coisas diferentes

Hoje o `HabitCard` recebe uma função: `onShowDetails`.

Adicione uma **segunda** função, `onCopyTitle`, que copia o título do hábito para o console. O cartão passa a ter dois botões.

**Faça o caminho completo:**

1. `App` cria a função `handleCopyTitle`
2. `App` envia para `HabitList`
3. `HabitList` encaminha para `HabitCard`
4. `HabitCard` liga no segundo botão

**Critério:** cada botão faz uma coisa diferente, e cada um recebe o `id` correto.

**Responda:** você precisou mudar alguma coisa na lógica do `HabitList`, ou só encaminhar mais uma prop? O que isso diz sobre o papel dele?

---

### 3.2 — `Panel` com rodapé

Faça o `Panel` aceitar, além de `children`, uma prop chamada `footer` que renderiza um rodapé no fim do painel — **só quando** a prop for enviada.

```jsx
<Panel title="Hábitos de hoje" footer={<small>5 hábitos</small>}>
  <HabitList ... />
</Panel>
```

**Critério:** o painel "Sobre o projeto", que não recebe `footer`, continua funcionando sem rodapé nenhum e sem espaço vazio sobrando.

> 💡 Props podem carregar JSX, não só texto e número.

---

### 3.3 — Investigação

Sem escrever código, responda com suas palavras:

> Por que o `HabitCard` **não** deve buscar o hábito sozinho no arquivo de dados, mesmo que isso funcionasse?

Escreva de três a cinco linhas. Não existe resposta única; existe raciocínio.

---

## Antes de entregar

```bash
git add .
git commit -m "feat: adiciona comunicação entre componentes"
git push
```

Abra seu repositório no navegador e **confirme que o commit apareceu**. Não confie no "acho que subiu".

---

[⬅️ Voltar para a Aula 03](./README.md)
