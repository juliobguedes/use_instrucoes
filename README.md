# Instruções - UseOCL

UseOCL é uma ferramenta de especificação e validação de Sistemas de Informação baseado num subconjunto de Unified Modelling Language (UML) e Object Constraint Language (OCL). A documentação oficial pode ser vista [neste site](https://scribestools.readthedocs.io/en/latest/useocl/), embora ao longo deste readme serão dadas as instruções necessárias para o(s) exercício(s).

UseOCL pode ser baixado [neste link](https://sourceforge.net/projects/useocl/files/USE/4.2.0/use-4.2.0.zip/download). Para executar o programa, entre na pasta `/bin/` pelo terminal e insira o comando `./use`; caso queira carregar algum diagrama de classes no programa, executa-se `./use caminho/para/o/arquivo.use`.

Inicialmente, é necessário entender que a ferramenta é composta por uma GUI (Graphic User Interface) e uma CLI (Command Line Interface) que funcionam em sincronia, mas arquivos também serão necessários para o uso da ferramenta. Assim como vocês devem ter visto em aula, OCL é especificado baseado em um diagrama de classes e, para isso, nosso sistema deve ser especificado em um arquivo `.use`. A especificação é extremamente simples, e para o exercício dado em sala, é necessário que vocês criem tanto o diagrama de classes quanto as constraints `inv`, `pre` e `post`. 

## Para especificar o diagrama

Cada arquivo `.use` deve iniciar-se informando qual modelo ali estará especificado. Isto pode ser feito adicionando uma linha com `model nomeModelo`. Cada classe é especificada na seguinte sintaxe:

```use
class NomeDaClasse
-- Esses dois traços representam um comentário
attributes
    atributo1: String
    atributo2: Integer
    atributo3: Real
operations
    comParam(param1: String): String
    semParam(): Integer
    semRetorno()
end -- Fim da classe
```

Muitas vezes uma classe herda de outra classe. Isso pode ser especificado da seguinte forma:

```use
class SegundaClasse:NomeDaClasse
-- Segue declarando o restante dos atributos e operações da mesma forma que ao declarar uma classe sem herança.
end
```

Muitas vezes as classes tem associações entre si, ou até mesmo classes de associação. As associações e classes de associação podem ser especificadas da seguinte forma:

```use
association nomeDaAssociacao between -- associação sem classe
    Classe1[1..*] role papelDaClasse1
    Classe2[*] role papelDaClasse2
end

associationclass ClasseAssociacao between -- associationclass
    Classe3[1] role papelDaClasse3
    Classe4[1..10] role papelDaClasse4
attributes
    atributoDaAssociacao: String
operations
    metodoDaAssociacao(): Integer
end
```

## Para especificar as constraints

Infelizmente, UseOCL não dá suporte às invariantes `def`, `body` ou `derive`, e por isso, serão cobradas apenas `inv`, `pre` e `post`. As constraints devem ser especificadas após o `model` ter sido finalizado e, para separar essa divisão, usa-se a palavra reservada `constraints`:

```use
model diagramaDeClasses

-- Declaração de diversas classes e associações
-- Quando as declarações de classe do modelo tiverem encerrado:

constraints

-- A partir desse ponto, declaram-se as constraints
```

De resto, a sintaxe é idêntica àquela vista em sala. Por exemplo:

```use
model Cafe

class CanecaCafe
attributes
    qtdAtual: Integer
    capacidadeMax: Integer
end

constraints

context CanecaCafe
    inv: self.qtdAtual >= 0 -- A quantidade de café atual não pode ser negativa
    inv: self.capacidadeMax >= 0 -- A capacidade máxima de café não pode ser negativa
    inv: self.qtdAtual <= self.capacidadeMax -- A quantidade atual de café não pode ser maior que a capacidade máxima
```

## Testes de constraints

Para testar se as constraints estão sendo respeitadas, é possível criar objetos, definir propriedades, para em seguida checar as invariantes. Considerando o último exemplo dado acima, temos:

```use
!create minhaCaneca : CanecaCafe
!set minhaCaneca.qtdAtual := 100
!set minhaCaneca.capacidadeMax := 500
check
-- O retorno do check das invariantes retornará OK

!create canecaNova : CanecaCafe
!set canecaNova.qtdAtual := 300
!set canecaNova.capacidadeMax := 250
check
-- O retorno do check das invariantes retornará OK para as duas primeiras invariantes, mas failed para terceira

```

## Sobre a submissão



## Créditos

A ferramenta UseOCL foi desenvolvida por:

Mark Richters, Martin Gogolla, Joern Bohling, Fabian Buettner, Fabian Gutsche, Hanna Bauerdick, Antje Werner, Jens Bruening, Mirco Kuhlmann, Duc-Hanh Dang, Lars Hamann, Daniel Gent e Frank Hilken.
