Aqui está o guia atualizado, incluindo o `.gitignore`.

## Estrutrando seu Projeto

Organizar seus arquivos desde o início economiza muito tempo. Uma boa estrutura de pastas permite que qualquer pessoa (incluindo você no futuro) entenda rapidamente onde encontrar cada coisa.

Exemplo de uma estrutura de diretórios simples e útil:

```bash
seu-projeto/
├── README.md
├── TODO.md
├── .gitignore
├── docs/
├── code/
└── tests/
```

-----

### README.md

Este é o **"cartão de visitas"** do seu projeto. É o primeiro arquivo que as pessoas devem ler.

  * **Para que serve:** Explica **o que** o projeto faz, **por que** ele existe, **como** instalá-lo e **como** usá-lo. Essencial para qualquer projeto.

### TODO.md

Este é o seu **bloco de notas** de tarefas.

  * **Para que serve:** É uma lista simples de "coisas a fazer" (bugs para corrigir, ideias para implementar, melhorias pendentes). Ajuda a manter o foco e a rastrear o progresso sem a necessidade de uma ferramenta complexa.

### .gitignore

Este é o **"porteiro"** do seu controle de versão (como o Git).

  * **Para que serve:** Este arquivo diz ao Git quais arquivos ou pastas ele deve **ignorar completamente**. É fundamental para evitar que arquivos sensíveis (como senhas ou chaves de API), arquivos de configuração local ou arquivos temporários desnecessários sejam enviados para o repositório.

### docs/

Uma pasta para guardar toda a **documentação detalhada**.

  * **Para que serve:** Enquanto o `README.md` é um resumo, a pasta `docs/` guarda guias mais longos, tutoriais, diagramas de arquitetura, anotações de pesquisa ou qualquer documentação que seja muito extensa para o arquivo principal.

### code/

Esta pasta contém o **"cérebro"** do seu projeto: o código-fonte.

  * **Para que serve:** É onde você guarda os scripts, arquivos de lógica e tudo o que faz seu projeto funcionar. Separar o código em uma pasta dedicada (em vez de deixá-lo solto na raiz) mantém o projeto limpo e organizado.
  * *(Nota: Muitas vezes você verá esta pasta nomeada como `src/`, que significa "source" ou fonte).*

### tests/

Uma pasta dedicada para todos os seus **arquivos de teste**.

  * **Para que serve:** Testes são códigos que verificam se o seu código principal na pasta `code/` está funcionando como esperado. Mantê-los separados garante que o código de produção (o que o usuário final roda) não se misture com o código de verificação.

-----


