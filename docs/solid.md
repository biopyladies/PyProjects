## SOLID

Os princípios SOLID são um conjunto de cinco diretrizes da programação orientada a objetos que visam criar software mais robusto, flexível e fácil de manter. Eles incluem o Princípio da Responsabilidade Única (S), o Princípio Aberto-Fechado (O), o Princípio da Substituição de Liskov (L), o Princípio da Segregação de Interfaces (I) e o Princípio da Inversão de Dependência (D).

## Princípio da Responsabilidade Única (S)

**Princípio** 

Uma classe deve ter **apenas um motivo para mudar**. Isso significa que uma classe deve ter apenas uma responsabilidade ou tarefa.

**Contexto** 

Imagine uma classe `BioSequence` que armazena dados de sequência (ex: "ATGC..."), analisa o conteúdo GC e também lê arquivos FASTA. Esta classe tem três responsabilidades (armazenamento, análise, parsing). Se o formato FASTA mudar, a classe muda. Se o algoritmo de análise mudar, a classe muda.

**❌ Exemplo Ruim (Violando S):**

```python
class BioSequence:
    def __init__(self, sequence):
        self.sequence = sequence

    def get_sequence(self):
        return self.sequence

    # Responsabilidade 2: Análise
    def calculate_gc_content(self):
        gc_count = self.sequence.count('G') + self.sequence.count('C')
        return (gc_count / len(self.sequence)) * 100

    # Responsabilidade 3: Parsing de Arquivo
    def read_fasta_file(self, filepath):
        with open(filepath, 'r') as f:
            # Lógica complexa de parsing FASTA...
            pass
        self.sequence = "..." # Define a sequência lida
```

**✅ Exemplo Bom (Seguindo S):**

Nós separamos as responsabilidades em classes distintas.

```python
# Responsabilidade 1: Armazenar o dado
class BioSequence:
    def __init__(self, sequence):
        self.sequence = sequence

    def get_sequence(self):
        return self.sequence

# Responsabilidade 2: Realizar análises
class SequenceAnalyzer:
    def calculate_gc_content(self, sequence: BioSequence):
        seq_str = sequence.get_sequence()
        gc_count = seq_str.count('G') + seq_str.count('C')
        return (gc_count / len(seq_str)) * 100

# Responsabilidade 3: Ler formatos
class FastaParser:
    def parse(self, filepath) -> BioSequence:
        sequence_data = ""
        with open(filepath, 'r') as f:
            # Lógica de parsing FASTA...
            sequence_data = "ATGC..." # Exemplo de sequência lida
        return BioSequence(sequence_data)
```

-----

## Princípio Aberto-Fechado (O)

**Princípio** 

Entidades de software (classes, módulos, funções) devem ser **abertas para extensão, mas fechadas para modificação**. Você deve poder adicionar novas funcionalidades sem alterar o código-fonte existente.

**Contexto** 

Imagine que você tem um analisador de sequências. Inicialmente, ele calcula o conteúdo GC. Depois, você quer adicionar a funcionalidade de transcrição (DNA para RNA) e, em seguida, de tradução (RNA para Proteína).

**❌ Exemplo Ruim (Violando O):**

Usar uma série de `if`/`elif` força a modificação da classe `SequenceAnalyzer` a cada nova análise.

```python
class SequenceAnalyzer:
    def __init__(self, sequence):
        self.sequence = sequence

    def analyze(self, analysis_type):
        if analysis_type == "gc":
            # ... lógica de cálculo GC ...
            return gc_result
        elif analysis_type == "transcribe":
            # ... lógica de transcrição ...
            return rna_result
        # Se quisermos adicionar "translate", TEMOS que editar esta classe.
```

**✅ Exemplo Bom (Seguindo O):**

Usamos o "Strategy Pattern" com classes abstratas. A classe `SequenceAnalyzer` é "fechada" e as novas análises são "extensões".

```python
from abc import ABC, abstractmethod

# 1. Criamos uma "abstração" de análise
class AnalysisStrategy(ABC):
    @abstractmethod
    def analyze(self, sequence):
        pass

# 2. Implementamos estratégias (extensões)
class GCContentAnalysis(AnalysisStrategy):
    def analyze(self, sequence):
        print("Calculando conteúdo GC...")
        # ... lógica ...
        return 45.5

class TranscriptionAnalysis(AnalysisStrategy):
    def analyze(self, sequence):
        print("Transcrevendo DNA para RNA...")
        # ... lógica ...
        return "AUGC..."

# 3. A classe principal é "fechada" para modificações
# Ela apenas usa a abstração.
class SequenceAnalyzer:
    def __init__(self, strategy: AnalysisStrategy):
        self.strategy = strategy

    def perform_analysis(self, sequence):
        return self.strategy.analyze(sequence)

# Uso:
dna_seq = "ATGC..."
gc_analyzer = SequenceAnalyzer(GCContentAnalysis())
gc_analyzer.perform_analysis(dna_seq)

rna_analyzer = SequenceAnalyzer(TranscriptionAnalysis())
rna_analyzer.perform_analysis(dna_seq)
```

-----

## Princípio da Substituição de Liskov (L)

**Princípio** 

Subtipos (classes filhas) devem ser substituíveis por seus tipos base (classes mãe) sem quebrar a aplicação. Se `S` é um subtipo de `T`, então objetos do tipo `T` podem ser substituídos por objetos do tipo `S` sem alterar nenhuma das propriedades desejáveis do programa.

**Contexto** 

Temos uma hierarquia de classes. `BioSequence` é a classe base. `DNASequence` e `ProteinSequence` são classes filhas. Uma função que espera uma `BioSequence` deve funcionar perfeitamente se receber uma `DNASequence` ou uma `ProteinSequence`.

**❌ Exemplo Ruim (Violando L):**

A classe filha `ProteinSequence` quebra o "contrato" da classe mãe. A classe `DNASequence` tem um método `get_complement()` que faz sentido. A `ProteinSequence` herda isso, mas uma proteína não tem um complemento.

```python
class DNASequence:
    def __init__(self, seq):
        self.seq = seq
        
    def get_complement(self):
        # Lógica para A->T, T->A, C->G, G->C
        return "Complemento de DNA"

# Hierarquia errada! Proteína não é um tipo de DNA.
class ProteinSequence(DNASequence): 
    def get_complement(self):
        # Quebra o contrato. O que fazer?
        raise NotImplementedError("Proteínas não têm complemento!")

def print_complements(sequences: list[DNASequence]):
    for seq in sequences:
        # Esta linha vai quebrar se recebermos uma ProteinSequence
        print(seq.get_complement())

# Isso vai falhar!
# print_complements([DNASequence("ATGC"), ProteinSequence("MKT")])
```

**✅ Exemplo Bom (Seguindo L):**

Corrigimos a hierarquia. Criamos uma abstração correta (`NucleotideSequence`) que *tem* um complemento, enquanto `ProteinSequence` segue seu próprio caminho.

```python
from abc import ABC, abstractmethod

class BioSequence(ABC):
    def __init__(self, seq):
        self.seq = seq

# Uma abstração para sequências que TÊM complementos
class NucleotideSequence(BioSequence):
    @abstractmethod
    def get_complement(self):
        pass

class DNASequence(NucleotideSequence):
    def get_complement(self):
        # Lógica para A->T, T->A, C->G, G->C
        return "Complemento de DNA"

class RNASequence(NucleotideSequence):
    def get_complement(self):
        # Lógica para A->U, U->A, C->G, G->C
        return "Complemento de RNA"

# Proteína é uma BioSequence, mas não uma NucleotideSequence
class ProteinSequence(BioSequence):
    pass # Não tem o método get_complement(), o que está correto.

# Esta função agora aceita APENAS o que pode ser complementado
def print_complements(sequences: list[NucleotideSequence]):
    for seq in sequences:
        print(seq.get_complement())

# Isso funciona perfeitamente:
print_complements([DNASequence("ATGC"), RNASequence("AUGC")])
```

-----

## Princípio da Segregação de Interfaces (I)

**Princípio** 

Clientes (classes que usam uma interface) não devem ser forçados a depender de métodos que não usam. É melhor ter muitas interfaces pequenas e específicas do que uma única interface "gorda" e genérica.

**Contexto** 

Imagine uma interface "geral" de operações biológicas: `ISequenceActions` que define `transcribe()`, `translate()` e `align()`.

**❌ Exemplo Ruim (Violando I):**

Uma classe `ProteinSequence` é forçada a implementar `transcribe()` (transcrição), o que não faz sentido para uma proteína. Ela depende de um método que não usa.

```python
from abc import ABC, abstractmethod

# Interface "Gorda"
class ISequenceActions(ABC):
    @abstractmethod
    def transcribe(self): pass # DNA -> RNA
    
    @abstractmethod
    def translate(self): pass # RNA -> Proteína
    
    @abstractmethod
    def align(self): pass # DNA, RNA ou Proteína

# Esta classe é forçada a implementar métodos inúteis para ela
class ProteinSequence(ISequenceActions):
    def transcribe(self):
        raise NotImplementedError("Proteínas não transcrevem")

    def translate(self):
        raise NotImplementedError("Proteínas já são traduzidas")

    def align(self):
        print("Alinhando proteína...")
        return "Resultado do Alinhamento"
```

**✅ Exemplo Bom (Seguindo I):**

Segregamos (separamos) as interfaces por capacidade.

```python
from abc import ABC, abstractmethod

# Interface 1
class ITranscribable(ABC):
    @abstractmethod
    def transcribe(self): pass

# Interface 2
class ITranslatable(ABC):
    @abstractmethod
    def translate(self): pass

# Interface 3
class IAlignable(ABC):
    @abstractmethod
    def align(self): pass

# Agora as classes implementam APENAS o que precisam

class DNASequence(ITranscribable, IAlignable):
    def transcribe(self):
        print("Transcrevendo DNA...")
    def align(self):
        print("Alinhando DNA...")

class RNASequence(ITranslatable, IAlignable):
    def translate(self):
        print("Traduzindo RNA...")
    def align(self):
        print("Alinhando RNA...")

# Esta classe só implementa o que é relevante para ela.
class ProteinSequence(IAlignable):
    def align(self):
        print("Alinhando Proteína...")
```

-----

## Princípio da Inversão de Dependência (D)

**Princípio**

1.  Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender de abstrações (interfaces).
2.  Abstrações não devem depender de detalhes. Detalhes (classes concretas) devem depender de abstrações.

**Contexto** 

Um módulo de "alto nível" é um `AnalysisPipeline` que executa uma análise. Um módulo de "baixoível" é um `FastaFileReader` (que lê um formato de arquivo específico).

**❌ Exemplo Ruim (Violando D):**

O `AnalysisPipeline` (alto nível) depende *diretamente* da classe concreta `FastaFileReader` (baixo nível). Se quisermos ler um arquivo GenBank, teremos que modificar o `AnalysisPipeline`.

```python
# Módulo de baixoível (detalhe)
class FastaFileReader:
    def read(self, filepath):
        print(f"Lendo arquivo FASTA: {filepath}")
        return "ATGC..." # Dados da sequência

# Módulo de alto nível
class AnalysisPipeline:
    def __init__(self, filepath):
        # Dependência FORTE e DIRETA do módulo de baixo nível!
        self.reader = FastaFileReader() 
        self.filepath = filepath

    def run_pipeline(self):
        sequence = self.reader.read(self.filepath)
        print(f"Analisando sequência: {sequence[:10]}...")
```

**✅ Exemplo Bom (Seguindo D):**

Ambos os módulos passam a depender de uma "abstração" (`ISequenceReader`). O `AnalysisPipeline` não sabe *como* o arquivo é lido, apenas que ele *pode* ser lido. Isso é chamado de **Injeção de Dependência**.

```python
from abc import ABC, abstractmethod

# 1. A Abstração (Interface)
class ISequenceReader(ABC):
    @abstractmethod
    def read(self, filepath) -> str:
        pass

# 2. Módulos de baixo nível (detalhes) dependem da abstração
class FastaFileReader(ISequenceReader):
    def read(self, filepath) -> str:
        print(f"Lendo arquivo FASTA: {filepath}")
        return "ATGC..."

class GenBankFileReader(ISequenceReader):
    def read(self, filepath) -> str:
        print(f"Lendo arquivo GenBank: {filepath}")
        return "ATGC..."

# 3. Módulo de alto nível depende da ABSTRAÇÃO
class AnalysisPipeline:
    def __init__(self, reader: ISequenceReader):
        # A dependência é "injetada"
        self.reader = reader

    def run_pipeline(self, filepath):
        sequence = self.reader.read(filepath)
        print(f"Analisando sequência: {sequence[:10]}...")

# Agora o pipeline é flexível:
fasta_reader = FastaFileReader()
pipeline1 = AnalysisPipeline(fasta_reader)
pipeline1.run_pipeline("meu_arquivo.fasta")

genbank_reader = GenBankFileReader()
pipeline2 = AnalysisPipeline(genbank_reader)
pipeline2.run_pipeline("meu_arquivo.gb")
```


# Referências

### Vídeos do Youtube

- [Princípios SOLID by Renato Augusto](https://youtube.com/playlist?list=PLNHxHgB-_LTtxnX6ILHDBpY6hCuzBwlrW&si=57jIyVVVSjZjbr6K)

- [Princípios SOLID by Felipe Deschamps](https://www.youtube.com/watch?v=2yqHlJ2HbTo)

- [Princípios SOLID by Augusto Galego](https://www.youtube.com/watch?v=2yqHlJ2HbTo)

### Artigos Online

- [SOLID by Alura](https://www.alura.com.br/artigos/solid?srsltid=AfmBOooNU4-YkjcJSKXW-kcPnBVxJrnCbSmlGQtCbx7Abq0kfAAMxTsG)

- [SOLID by freeCodeCamp](https://www.freecodecamp.org/portuguese/news/os-principios-solid-da-programacao-orientada-a-objetos-explicados-em-bom-portugues/)