# Proposta de Dinâmica de Projeto

## Objetivo
Aplicar conceitos de bioinformática
Aplicar conceitos de boas práticas de programação

## Desafios
Implementar um programa que resolva um problema comum na bioinformática
Otimizar o código seguindo a notação Big O
Melhorar o código seguindo os princípios SOLID e Padrões de Projeto
Testar as funcionalidades do programa seguindo a metodologia TDD
Automatizar a execução do programa
Documentar a implementação
Disponibilizar implementação em um repositório do github

## Problema
### Nível Básico: criar um pacote que lide com criação e manipulação de sequências de DNA, RNA e Proteínas.

### Nível Intermediário: criar um pacote que permita a manipulação de arquivos e sequências de DNA, RNA e proteínas.

### Nível Avançado: criar um pacote que permita a integração de APIs de bancos de dados de sequências de DNA, RNA e Proteínas.

##  Exemplos

(NÃO PRECISA SEGUIR ESSE FORMATO, use sua criatividade)

Dado uma sequência de DNA (organismo de sua escolha): 

```python
sequencia = input("Digite a sequência de nucleotídeos: ").upper().strip()
sequencia = sequencia.replace(" ", "")

bases_validas = {'A', 'T', 'C', 'G'}

tamanho = len(sequencia)
    count_g = sequencia.count('G')
    count_c = sequencia.count('C')
    count_gc = count_g + count_c
    percentual_gc = (count_gc / tamanho) * 100

print(f"Sequência: {sequencia}")
    print(f"Tamanho: {tamanho} nucleotídeos")
    print(f"Bases G: {count_g}")
    print(f"Bases C: {count_c}")
    print(f"Total GC: {count_gc}")
    print(f"Percentual GC: {percentual_gc:.2f}%")
```

## Material de apoio
[BioPython](https://biopython.org/)
[ETE Toolkit] (https://etetoolkit.org/)

