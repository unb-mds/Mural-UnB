# Estudo de Typescript aplicado a React

    Este documento tem o objetivo de fornecer as anotações sobre o uso do TypeScript aplicado á web utilizando o React como framework.

## Pre-requisitos

- Git Instalado
- Node.js Instalado (versão LTS)
- Conhecimento básico de Javascript
- Ambiente de Desenvolvimento Integrado (IDE)

## Instalação

- Abra o git bash, ou o terminal, e verifique a sua versão do node.js
`$ node -v`

> Deve retornar a versão instalada mais recente, caso retorne um erro, no Linux rode o comando a baixo. No Windows, baixe o node.js no [site oficial](https://nodejs.org/en/download/)

```bash $ curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash - $ sudo apt-get install -y nodejs```

- Com o node.js instalado corretamente, abra o git bash ou o terminal e digite o comando:
`$ npm install -g typescript`

- Se for desejado instalar a linguagem localmente em um projeto, vá para o diretório do seu projeto e instale o Typescript como uma dependência:
`$ npm install --save-dev typescript`

- (Opicional) Inicialize a configuração do Typescript, dessa forma é possível configurar o comportamento do compilador
`$ npx tsc --init`

---

## Tipos básicos e Avançados

### 🔹 Tipos básicos

    A seguir, segue os tipos mais comuns, utilizados no dia a dia

#### **1.** String - Textos

`let nome: string = "Mario";`

#### **2.** Number - Inteiros e Decimais

`let idade: number = 15;`

#### **3.** Boolean - Verdadeiro ou Falso

`let ativo: boolean = true;`

#### **4.** Null e Unidentified

```typescript let vazio: null = null; let indefinido: unidentified = unidentified;```

#### **5.** Any - Aceita qualquer tipo

```typescript let dado: any = "texto"; dado = 10; \\válido```

#### **6.** Unknown - Similar ao any mas requer checagem de tipo antes de usar

```typescript let valor: unknown = "texto"; if(typeof valor == "string") {console.log(valor.toUpperCase());}```

#### **7.** Void - Usado em funções que não retornam valor

```typescript function: logar(): void {console.log("Executando...")};```

#### **8.** Object - Qualquer objeto não primitivo

```typescript let pessoa: object = { nome: "Ana", idade: 22 };```

#### **9.** Never - Funções que nunca retornam (lançam erro ou loop infinito)

```typescript function erro(msg: string): never {throw new Error(msg);}```

#### **10.** Array

```typescript let numeros: number[] = [1, 2, 3];  let nomes: Array<string> = ["Ana", "João"];```

#### **11.** Tupla - Array com tamanho e tipos fixos

```typescript let tupla: [string, number] = ["idade", 30];```

---

## 🔹🔹 Tipos Avançados

### 1. Interface - Contrato para objetos

```typescript interface Usuario {nome: string; idade: number;} let u: Usuario = { nome: "Carlos", idade: 30 };```

### 2. Enum – Conjunto de valores nomeados

```typescript enum Cores {Vermelho, Verde,Azul} let cor: Cores = Cores.Verde;```

### 3. Union (|) – Variável pode ter múltiplos tipos

```typescript let id: number | string; id = 123; id = "ABC123";```

### 4. Intersection (&) – Combina tipos

```typescript type Pessoa = { nome: string }; type Funcionario = { salario: number }; type Empregado = Pessoa & Funcionario; let joao: Empregado = { nome: "João", salario: 5000 };```

### 5. Type Aliases – Apelidos para tipos complexos

```typescript type ID = string | number; let userId: ID = 42;```

### 6. Generics – Tipos genéricos reutilizáveis

```typescript function identidade<T>(valor: T): T {return valor;} let numero = identidade<number>(10); let texto = identidade<string>("Oi");```

### 7. Literal types – Restringe valores possíveis

```typescript let direcao: "norte" | "sul" | "leste" | "oeste"; direcao = "norte";```

### 8. Optional e Nullable

```typescript function ola(nome?: string) {console.log("Olá " + (nome ?? "visitante"));}```

### 9. Type assertion` – forçar tipo

```typecript let valorDesconhecido: unknown = "texto"; let tamanho: number = (valorDesconhecido as string).length;```

### 10. Mapped type – Criar tipos a partir de outros

```typescript type UsuarioBase = { nome: string; idade: number }; type Parcial<T> = { [K in keyof T]?: T[K] }; let u1: Parcial<UsuarioBase> = { nome: "João" };```

### 11. Utility types – utilitários prontos do TypeScript

```typescript interface Pessoa {nome: string; idade: number; email?: string} let p1: Partial<Pessoa> = { nome: "Lucas" };       // todos opcionais let p2: Required<Pessoa> = { nome: "Ana", idade: 20, email: "a@b.com" }; // todos obrigatórios  let p3: Readonly<Pessoa> = { nome: "Marcos", idade: 30 }; // somente leitura  let p4: Pick<Pessoa, "nome"> = { nome: "Sofia" };  // pega apenas "nome"  let p5: Omit<Pessoa, "email"> = { nome: "Leo", idade: 22 }; // remove "email"```
