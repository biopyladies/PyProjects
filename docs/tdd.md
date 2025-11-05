# TDD (Test-Driven Development) - Desenvolvimento Orientado a Testes

O TDD é uma metodologia de desenvolvimento de software onde o ciclo de criação de uma funcionalidade se baseia em testes. Em vez de escrever o código e depois testá-lo, o processo é invertido.

O ciclo do TDD é conhecido como **Red-Green-Refactor**:

1.  **Red (Vermelho):** Escreva um teste automatizado para uma nova funcionalidade. Como a funcionalidade ainda não existe, o teste irá falhar (ficará "vermelho").
2.  **Green (Verde):** Escreva o código mais simples possível para fazer o teste passar (ficar "verde"). Nesta fase, o objetivo não é ter o código mais bonito ou eficiente, mas apenas o suficiente para satisfazer o teste.
3.  **Refactor (Refatorar):** Com a segurança de que o teste está passando, você pode agora melhorar o código (refatorar). Isso inclui remover duplicação, melhorar a clareza e a performance, sem alterar o comportamento da funcionalidade.

A grande vantagem do TDD, especialmente em ciência, é que ele garante que o código seja **confiável** e **verificável** desde o início, facilitando a manutenção e a colaboração.

## Exemplo Prático em Python: Calculando Conteúdo GC

Vamos usar o TDD para criar uma função que calcula o conteúdo GC de uma sequência de DNA.

### Passo 1: Red - Escrever o teste que falha

Primeiro, criamos um arquivo de teste (ex: `test_bio.py`) e escrevemos um teste para a nossa futura função `calculate_gc_content`.

```python
# test_bio.py
from bio import calculate_gc_content

def test_calculate_gc_content_simple():
    """Testa o cálculo de GC para uma sequência simples."""
    seq = "ATGCGC"
    assert calculate_gc_content(seq) == 50.0
```

Se rodarmos este teste, ele falhará, pois nem o arquivo `bio.py` nem a função `calculate_gc_content` existem.

### Passo 2: Green - Fazer o teste passar

Agora, criamos o arquivo `bio.py` e escrevemos o código mais simples possível para que o teste passe.

```python
# bio.py
def calculate_gc_content(seq):
    """Calcula o conteúdo GC de uma sequência de DNA."""
    if not seq:
        return 0.0
    
    gc_count = seq.count('G') + seq.count('C')
    total_bases = len(seq)
    
    return (gc_count / total_bases) * 100
```

Com este código, o teste `test_calculate_gc_content_simple` agora passa.

### Passo 3: Refatorar e adicionar mais testes

O código já está bom, mas podemos adicionar mais testes para cobrir outros cenários, como uma sequência vazia ou com letras minúsculas.

Primeiro, adicionamos novos testes ao `test_bio.py`:

```python
# test_bio.py (atualizado)
from bio import calculate_gc_content

def test_calculate_gc_content_simple():
    """Testa o cálculo de GC para uma sequência simples."""
    seq = "ATGCGC"
    assert calculate_gc_content(seq) == 50.0

def test_gc_content_empty_sequence():
    """Testa com uma sequência vazia."""
    assert calculate_gc_content("") == 0.0

def test_gc_content_no_gc():
    """Testa uma sequência sem G ou C."""
    assert calculate_gc_content("ATTATTA") == 0.0

def test_gc_content_with_lowercase():
    """Testa se a função ignora o case."""
    seq = "atgcgC"
    assert calculate_gc_content(seq) == 50.0
```

Os testes de sequência vazia e sem GC já passam, mas o de letras minúsculas falhará. Agora, refatoramos o código em `bio.py` para que ele também passe.

```python
# bio.py (refatorado)
def calculate_gc_content(seq):
    """Calcula o conteúdo GC de uma sequência de DNA, ignorando o case."""
    if not seq:
        return 0.0
    
    # Converte a sequência para maiúsculas para o cálculo
    seq_upper = seq.upper()
    
    gc_count = seq_upper.count('G') + seq_upper.count('C')
    total_bases = len(seq_upper)
    
    return (gc_count / total_bases) * 100
```

Agora, todos os testes passam e temos uma função mais robusta, desenvolvida de forma segura e incremental.
