# Guia GitHub — passo a passo da turma

Este guia foi escrito para quem ainda não trabalha com Git/GitHub no dia a dia.

## 1. Faça o fork

Na página do repositório da turma:

1. clique em **Fork**;
2. confirme a criação na sua conta;
3. abra o repositório que foi criado no seu GitHub.

Você fará isso **uma única vez**.

---

## 2. Clone o seu fork

No seu fork, copie a URL HTTPS.

No terminal:

```bash
git clone URL-DO-SEU-FORK
cd iteam-react-modulo-04
```

Exemplo:

```bash
git clone https://github.com/seu-usuario/iteam-react-modulo-04.git
cd iteam-react-modulo-04
```

---

## 3. Configure o repositório da turma como `upstream`

Execute uma única vez:

```bash
git remote add upstream https://github.com/virgiliojr94/iteam-react-modulo-04.git
```

Confira:

```bash
git remote -v
```

Você deverá enxergar:

- `origin` → seu fork;
- `upstream` → repositório da turma.

---

## 4. Crie sua pasta

Use seu nome completo em minúsculas, sem acentos, trocando espaços por hífens.

Exemplo:

```text
alunos/maria-de-souza-silva/
```

Dentro dela:

```text
alunos/maria-de-souza-silva/
├── README.md
├── evidencias/
└── my-daily-habits/
```

Copie `templates/ALUNO.md` para o seu `README.md`.

---

# Rotina de TODA aula

## Antes de começar

Baixe o que o professor liberou:

```bash
git pull --no-rebase upstream main
```

Depois envie essa atualização também para o seu fork:

```bash
git push origin main
```

A nova atividade aparecerá em:

```text
atividades/aula-XX/README.md
```

## Durante a atividade

Trabalhe somente dentro da sua pasta:

```text
alunos/seu-nome-completo/
```

O projeto React permanece em:

```text
alunos/seu-nome-completo/my-daily-habits/
```

## Ao terminar

Confira o que mudou:

```bash
git status
```

Registre:

```bash
git add .
git commit -m "atividade XX: resumo curto"
git push origin main
```

O Pull Request aberto no primeiro dia será atualizado automaticamente.

---

## Se algo der errado

Não apague arquivos aleatoriamente.

Execute primeiro:

```bash
git status
```

e mostre ao professor:

- o comando executado;
- a mensagem completa;
- o que você esperava que acontecesse.
