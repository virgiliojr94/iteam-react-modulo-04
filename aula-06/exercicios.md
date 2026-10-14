# Exercícios — Aula 06
## `useEffect`, dependências e persistência

Inicie após o Passo 3 do [README da Aula 06](./README.md). Níveis 1 e 2 obrigatórios; nível 3 opcional.

**Todos são temporários. Faça → observe → desfaça**. Nunca crie um Effect que atualiza sem condição o próprio estado que observa: isso **pode travar a página**.

---

# Nível 1 — Fixação

### 1.1 — Inspecionar a chave

**Faça:** em **F12 → Application → Local Storage**, encontre `my-daily-habits:habits`; cadastre um hábito e alterne outro.

**Observe:** o texto JSON muda com os dados. **Não testável nesta revisão sem DevTools real.**

**Desfaça:** se precisar voltar ao estado inicial, apague a chave e recarregue.

### 1.2 — JSON inválido

**Faça:** troque o valor da chave por `{oops` e pressione F5.

**Observe:** `JSON.parse` falha, o `catch` retorna `initialHabits` e o Effect regrava JSON válido com **três hábitos**, sem tela branca.

**Desfaça:** o próprio Effect já corrigiu o valor; confirme os três hábitos iniciais.

### 1.3 — Dependências `[]`

**Faça:** troque `[habits]` por `[]` **apenas** no Effect de salvar. Apague a chave, recarregue, cadastre um hábito e pressione F5.

**Observe:** antes do F5, a tela tem **quatro** hábitos; o Storage conserva os **três** da montagem, então o novo desaparece.

**Desfaça:** restaure `[habits]` e repita cadastro + F5.

### 1.4 — Sem array de dependências

**Faça:** em `App.jsx`, logo abaixo do `useState` de `habits`, acrescente `const [tick, setTick] = useState(0);` e, dentro do `<header className="hero">`, um botão `<button type="button" onClick={() => setTick(tick + 1)}>tick {tick}</button>`. Coloque `console.log("gravou");` dentro do Effect de salvar. Clique em **tick** três vezes. Depois retire o array de dependências do Effect e clique em **tick** três vezes de novo.

**Observe:** com `[habits]`, clicar em “tick” **não** grava: o `App` renderiza, mas `habits` não mudou. **Sem array**, **cada clique grava**: o Effect roda após **toda** renderização. Com `[]`, nem o clique em um cartão grava (só a montagem). *Clicar nos cartões não mostra a diferença, porque cada clique muda `habits`.*

**Desfaça:** remova o botão, o `tick` e o log, e restaure `[habits]`.

---

# Nível 2 — Aplicação no projeto

### 2.1 — Inicialização preguiçosa

**Faça:** adicione `console.log("loadHabits");` no início de `loadHabits`. Compare `useState(loadHabits)` com `useState(loadHabits())` após dois cliques.

**Observe:** com `useState(loadHabits)`, o número de mensagens **não cresce** com os cliques. Com `useState(loadHabits())`, **cresce a cada renderização**. No desenvolvimento, o `StrictMode` pode chamar o inicializador **duas vezes**: na simulação realizada, a versão correta mostrou 2 mensagens e ficou em 2; a versão com parênteses passou de 2 para 6 depois de dois cliques. A contagem exata não é universal. O que importa é **crescer ou não crescer**.

**Desfaça:** restaure `useState(loadHabits)` e remova o log.

### 2.2 — Lista vazia

**Faça:** escreva `[]` na chave do Storage e pressione F5.

**Observe:** a tela mostra “Nenhum hábito cadastrado.”; `[]` é um array válido, não um erro de JSON.

**Desfaça:** apague a chave e recarregue para recuperar os três hábitos iniciais.

### 2.3 — `StrictMode` (deck)

**Faça:** coloque temporariamente um log no Effect de gravação e observe a montagem em desenvolvimento.

**Observe:** o React pode repetir o setup; gravar os mesmos dados novamente deve ser seguro. Isso não corresponde à execução do build final.

**Desfaça:** remova o log; mantenha o `StrictMode`.

---

# Nível 3 — Desafio opcional

### 3.1 — Classes e ciclo de vida

**Faça:** leia o Box das classes na apostila e compare `componentDidMount` / `componentWillUnmount` com a ideia de setup e cleanup de `useEffect`.

**Observe:** não há equivalência perfeita linha a linha.

**Desfaça:** não crie classes.

### 3.2 — O contador precisa de Effect?

**Faça:** explique por que `completedCount` é calculado diretamente do estado durante a renderização.

**Observe:** nenhum estado derivado adicional é necessário.

**Desfaça:** nenhuma alteração de código.

### 3.3 — Segurança no navegador

**Faça:** explique por que `localStorage` não deve guardar senhas e não substitui backend ou banco de dados.

**Observe:** scripts executados na mesma origem podem acessar esses dados.

**Desfaça:** nenhuma alteração de código.

---

## Antes de entregar

- [ ] Efeito de gravação com `[habits]` e `useState(loadHabits)` sem parênteses.
- [ ] Sem logs ou dependências experimentais.
- [ ] JSON inválido e valor não-array retornam aos três hábitos iniciais; `[]` continua vazio.
- [ ] Cadastro, alternância e título corretos após F5.
- [ ] No repositório do aluno, o commit `feat: persiste hábitos no navegador` muda **apenas** `src/App.jsx`.
- [ ] Se for necessário o checkpoint da Aula 07, apaguei a chave do Storage para voltar aos três hábitos.

[⬅️ Voltar para a Aula 06](./README.md)
