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

## Arquivos que mudam hoje

```text
src/
├── App.jsx                    ← ganha handleShowDetails
└── components/
    ├── HabitCard.jsx          ← recebe id e onShowDetails; ganha botão
    ├── HabitList.jsx          ← encaminha o callback
    └── Panel.jsx              ← ARQUIVO NOVO
```

---

## Critério de pronto

Rode isto no seu projeto depois da aula:

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
