# Exercícios — Aula 05
## Eventos e formulários controlados

Comece após concluir os três passos do [README da Aula 05](./README.md). Faça os **níveis 1 e 2**; nível 3 opcional.

**Todos os exercícios são temporários:** **faça → observe → desfaça** antes de seguir para o próximo. Não entregue código de experimentos.

---

# Nível 1 — Fixação

### 1.1 — Sem o spread

**Faça:** em `HabitForm.jsx`, apague temporariamente a linha `...currentForm,` no `handleChange`. Digite título e meta e tente enviar.

**Observe:** atualizar um campo substitui o objeto, perdendo o outro. O texto pode continuar aparecendo, mas no envio `form.title.trim()` pode falhar com `Cannot read properties of undefined (reading 'trim')`. Pode aparecer aviso de campo controlado que se tornou não controlado.

**Desfaça:** restaure `...currentForm,` e teste os dois campos.

### 1.2 — `value` sem `onChange`

**Faça:** apague temporariamente `onChange={handleChange}` **somente** do input de Hábito.

**Observe:** o texto não acompanha a digitação; no desenvolvimento pode aparecer `You provided a value prop to a form field without an onChange handler`.

**Desfaça:** recoloque o handler e teste a digitação.

### 1.3 — Sem `preventDefault()`

**Faça:** retire temporariamente `event.preventDefault();` e envie um formulário preenchido.

**Observe:** o navegador deve executar o envio padrão e recarregar; a URL pode ganhar `?title=...&goal=...`. **Comportamento do navegador: não verificado em navegador real nesta revisão.**

**Desfaça:** restaure `event.preventDefault();` e confirme que o envio não recarrega.

---

# Nível 2 — Aplicação no projeto

### 2.1 — Só espaços

**Faça:** tente cadastrar um hábito com `   ` (três espaços) em ambos os campos.

**Observe:** `trim()` remove os espaços nas pontas; a validação rejeita e mostra “Preencha o hábito e a meta.”, sem criar cartão.

**Desfaça:** sem código para alterar; teste novamente com dados válidos.

### 2.2 — Títulos iguais, ids diferentes

**Faça:** cadastre “Ler” / “20 min” **duas vezes**.

**Observe:** aparecem **dois** cartões, cada qual com `id` único e sem aviso de `key`; concluir um não altera o outro.

**Desfaça:** pressione F5 para voltar aos três hábitos iniciais.

### 2.3 — Perda no F5

**Faça:** cadastre um hábito e marque-o como concluído; pressione F5.

**Observe:** o novo hábito desaparece e o estado volta ao array inicial. A persistência será feita na Aula 06.

**Desfaça:** sem código a restaurar; confira os três hábitos originais.

---

# Nível 3 — Desafio opcional

### 3.1 — Controlado × não controlado

**Faça:** leia a §5.6 e compare `value` com `defaultValue`. Um teste com campo não controlado deve ser temporário.

**Observe:** no campo controlado, state fornece o valor; no não controlado, o DOM guarda o valor atual.

**Desfaça:** deixe apenas os inputs controlados.

### 3.2 — `SyntheticEvent`

**Faça:** leia o Box da §5.6 e explique por que o handler lê `event.target` e copia `name`/`value`.

**Observe:** nenhuma mudança necessária no projeto.

**Desfaça:** nada.

---

## Antes de entregar

- [ ] O `HabitForm` está idêntico ao Passo 1 do README; restaurados spread, `onChange` e `preventDefault`.
- [ ] `App` contém apenas import, handler e painel novos, mantendo a Aula 04.
- [ ] O CSS foi **acrescentado**, não substituído.
- [ ] O código não guarda nenhum experimento intencional.
- [ ] Três hábitos iniciais; cadastro e alternância funcionando; console sem erros.
- [ ] Commit do aluno: `feat: adiciona formulário de hábitos` nos três arquivos da Aula 05.

[⬅️ Voltar para a Aula 05](./README.md)
