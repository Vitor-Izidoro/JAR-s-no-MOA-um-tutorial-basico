# JAR-s-no-MOA-um-tutorial-basico
Um tutorial básico para ter uma base para a utilização de JAR's

###  **O que é um JAR?**

Um **JAR (Java ARchive)** é um arquivo compactado no formato **.jar**, que contém **código compilado** (`.class`), **recursos** (como arquivos de configuração) e **metadados** (como um `MANIFEST.MF`).  

No caso do **MOA**, os JARs que você usa (`moa-master.jar`, etc.) contêm o código-fonte compilado das classes do **Massive Online Analysis**, permitindo que você rode treinamentos e experimentos sem precisar recompilar tudo.  

---

###  **Tipos de JARs e seu papel no MOA**

 **JAR Principal (`moa.jar`)**  
   - Contém todas as classes essenciais do MOA.  
   - Inclui classificadores (`moa.classifiers`), avaliadores (`moa.evaluation`), geradores de fluxo de dados (`moa.streams`), etc.  
   - Você pode rodar comandos como:

     ```bash
     java -cp "moa.jar" moa.DoTask "EvaluatePrequential -l (trees.HoeffdingTree) -s (ArffFileStream -f dataset.arff)"
     ```

 **JAR Personalizado (`moaPIBIC.jar`)**  
   - Criado para **adicionar ou modificar funcionalidades** do MOA.  
   - Pode conter novos classificadores, métodos de detecção de conceito, etc.  
   - Exemplo: pode incluir a classe `drift.InstanceSelectedClassifier3`, que talvez não exista no `moa.jar`.  
   - Para rodá-lo junto com o MOA, você precisa incluí-lo no `classpath`:

     ```bash
     java -cp "moa.jar;moaPIBIC.jar" moa.DoTask "EvaluatePrequential -l (drift.InstanceSelectedClassifier3) ..."
     ```

 **JAR de Dependências**  
   - O MOA pode precisar de bibliotecas externas, como **WEKA** (`weka.jar`) ou **Apache Commons Math** (`commons-math.jar`).  
   - Se o MOA exigir um JAR extra, você deve adicioná-lo no `-cp`:

     ```bash
     java -cp "moa.jar;weka.jar" moa.DoTask "..."
     ```

---

### ⚙ **Como um JAR funciona internamente?**

Um JAR é basicamente um **arquivo ZIP** com uma estrutura específica. Se você quiser ver o que tem dentro de um JAR, pode usar:

```bash
jar tf moa.jar
```
Isso mostrará algo como:
```
META-INF/MANIFEST.MF
moa/DoTask.class
moa/classifiers/meta/OzaBag.class
moa/streams/ArffFileStream.class
...

```
Por que usar um JAR no MOA?

 Facilita a distribuição – Você pode compartilhar um único arquivo .jar em vez de várias classes.
 
 Evita recompilação – O código já está compilado, pronto para execução.
 
 Permite extensões – Você pode criar um JAR personalizado (moaPIBIC.jar) para modificar o MOA sem alterar o original.
