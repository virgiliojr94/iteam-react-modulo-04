# Como entregar as atividades

A entrega do Módulo 04 acontece no **seu repositório pessoal do My Daily Habits**.

Este repositório do curso serve apenas como mapa, apoio e fonte dos exercícios. Você não entrega código aqui.

---

## O que o professor precisa receber

Envie ao professor o link do seu repositório:

```text
https://github.com/seu-usuario/my-daily-habits
```

Esse mesmo repositório será usado durante todo o módulo.

---

## Uma entrega por aula

Cada aula termina com um commit que representa a pequena evolução daquele dia.

Exemplos:

```text
feat: cria primeira versão do My Daily Habits
refactor: organiza hábitos em componentes
feat: adiciona comunicação entre componentes
feat: adiciona estado aos hábitos
```

Não precisa criar outro repositório para a aula seguinte. Continue sempre no mesmo projeto.

---

## Antes de entregar

Primeiro confirme que o projeto funciona:

```bash
npm run dev
```

Depois confira o que mudou:

```bash
git status
```

Registre a entrega:

```bash
git add .
git commit -m "feat: descreva a entrega da aula"
git push
```

Por fim, abra seu repositório no navegador e confirme que o commit está visível.

---

## Critério mínimo de entrega

O professor observará:

- se o projeto continua executando;
- se a funcionalidade pedida na aula está presente;
- se o commit correspondente à aula aparece no GitHub;
- se o repositório enviado é o seu `my-daily-habits`;
- se você consegue explicar, de forma simples, o que alterou.

Não é necessário criar relatório ou pasta de evidência.

---

## Se ainda tiver dúvida com Git

Consulte [`GUIA-GITHUB.md`](./GUIA-GITHUB.md).

Ele mostra apenas o fluxo usado neste módulo: `clone`, `status`, `add`, `commit` e `push` no seu repositório pessoal.
