# 📜 Manifesto Zero

> **Arquitetura Zero** não é só uma estrutura de pastas.
> É uma forma de pensar sistemas como eles realmente são: simples, funcionais e previsíveis.

---

## Os 7 Princípios da Arquitetura Zero

### 1. **Todo sistema é um CRUD disfarçado**

Se você enxerga entradas, processamentos e saídas, então já tem a base do sistema.
A complexidade é só maquiagem.

---

### 2. **Frameworks são muletas**

Use o mínimo possível. Se for usar, saiba exatamente o que ele está fazendo por baixo dos panos.
A performance e o controle estão no código que você entende, não no que outros esconderam.

---

### 3. **Separar é governar**

Cada camada tem uma função clara:

* `interface`: pega do banco.
* `controller`: aplica regra.
* `view/api`: mostra o resultado.

Nada de misturar. Cada um no seu quadrado.

---

### 4. **Views são burras**

Views não pensam. Elas só mostram.
Toda a lógica deve estar antes — e quem manda na lógica são os controllers.

---

### 5. **Reutilizar é regra**

Se você escreveu algo que pode servir de novo, extraia.
Funções, helpers, constantes, configurações — tudo em `/utils`. Evite duplicação como se fosse bug.

---

### 6. **Hooks entram pela porta dos fundos**

Tudo que vem de fora (webhooks, sistemas terceiros, integrações) vai para `/receiver`.
Ali é o ponto de entrada. Nada entra direto. Tudo filtrado.

---

### 7. **O sistema existe para mostrar dados**

Você escreve código para apresentar dados, não para impressionar dev sênior.
A máquina precisa funcionar. Se for simples e funcional, você venceu.

---

> “A beleza está na clareza. A performance, na simplicidade. A escalabilidade, na separação.”

**Assinado:
Todo dev cansado de arquitetura inchada.**
