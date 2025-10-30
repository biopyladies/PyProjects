# Documentação

Uma docstring em Python é a forma padrão de documentar o que um módulo, função, classe ou método faz. É uma string que aparece logo na primeira linha da definição e fica acessível através do atributo __doc__.

# Boas Práticas de Documentação

1. Clareza e Objetividade: 

A primeira linha deve ser um resumo curto e direto.

2. Descreva os Parâmetros (Args): 

Liste cada parâmetro, seu tipo e o que ele representa.

3. Descreva o Retorno (Returns): 

Especifique o que a função retorna e seu tipo.


### Adote um Estilo Padrão

Usar um formato consistente (como Google Style) torna a documentação legível e compatível com ferramentas que geram documentação automaticamente.

Exemplo Prático de Docstring (Google Style)

```python
def calculate_gc_content(seq: str) -> float:
    """Calcula a porcentagem de bases G e C em uma sequência de DNA.

    Args:
        seq (str): A sequência de DNA para análise.

    Returns:
        float: A porcentagem de conteúdo GC. Retorna 0.0 se a sequência
               for vazia.
    """
    if not seq:
        return 0.0

    seq_upper = seq.upper()
    gc_count = seq_upper.count('G') + seq_upper.count('C')
    
    return (gc_count / len(seq_upper)) * 100
```

Essa prática não só ajuda outras pessoas a entenderem seu código, mas também permite que ferramentas automáticas criem documentações e que IDEs ofereçam ajuda inteligente enquanto você programa.

# Referências

- [Google Style](https://google.github.io/styleguide/pyguide.html)