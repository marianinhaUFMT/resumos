# Tutorial completo: Flutter + MVVM + Riverpod

> Guia de referência para o TCC. Cobre os fundamentos de Flutter, o padrão MVVM, o Riverpod (versão 3.x, sintaxe atual com `riverpod_generator`), exemplos isolados de cada conceito e, por fim, a construção passo a passo de um app simples de lista de tarefas combinando os dois.

---

## Sumário

1. [Fundamentos do Flutter](#1-fundamentos-do-flutter)
2. [O que é MVVM](#2-o-que-e-mvvm)
3. [O que é Riverpod](#3-o-que-e-riverpod)
4. [Exemplo isolado: MVVM puro (sem Riverpod)](#4-exemplo-isolado-mvvm-puro-sem-riverpod)
5. [Exemplo isolado: Riverpod puro (sem MVVM)](#5-exemplo-isolado-riverpod-puro-sem-mvvm)
6. [Projeto prático: App de Tarefas com MVVM + Riverpod](#6-projeto-pratico-app-de-tarefas-com-mvvm-riverpod)
7. [Como os dados fluem (recapitulando)](#7-como-os-dados-fluem-recapitulando)
8. [Próximos passos e exercícios](#8-proximos-passos-e-exercicios)

---

## 1. Fundamentos do Flutter

Flutter é o SDK de UI multiplataforma do Google. Você escreve em **Dart** e o mesmo código compila para Android, iOS, Web e Desktop, usando o próprio motor de renderização do Flutter (não os widgets nativos da plataforma).

### 1.1 Tudo é widget

A unidade fundamental do Flutter é o **widget**. Um botão, um texto, um espaçamento, um layout inteiro — tudo é (ou é composto por) widgets, organizados em uma árvore (*widget tree*). Não existe "tela" no sentido tradicional; existe uma árvore de widgets que é reconstruída (*rebuild*) sempre que algo muda.

Dois tipos essenciais:

- **`StatelessWidget`**: não guarda estado mutável. Depende só dos parâmetros recebidos no construtor. Se nada muda de fora, ele nunca se reconstrói sozinho.
- **`StatefulWidget`**: tem um objeto `State` associado, que pode chamar `setState()` para forçar reconstrução quando algo interno muda.

```dart
class Greeting extends StatelessWidget {
  const Greeting({super.key, required this.name});
  final String name;

  @override
  Widget build(BuildContext context) {
    return Text('Olá, $name!');
  }
}
```

Todo widget implementa um método `build(BuildContext context)` que retorna a "foto" da UI naquele momento. O Flutter compara a árvore antiga com a nova e atualiza só o que mudou (*reconciliation*), de forma parecida com o Virtual DOM do React.

### 1.2 Programação declarativa

Você não diz "pegue esse texto na tela e mude para X" (imperativo). Você diz "quando o estado for X, o texto deve ser X" (declarativo) — e o framework cuida da atualização. É por isso que **gerenciamento de estado** é um tema tão central em Flutter: como esse "estado" que a UI declara depender é armazenado, alterado e propagado é o que muda entre Provider, Riverpod, Bloc, GetX, etc.

### 1.3 Estrutura básica de um projeto

```
meu_app/
├── android/, ios/, web/...   # código nativo de cada plataforma (gerado)
├── lib/                      # SEU código Dart vive aqui
│   └── main.dart             # ponto de entrada
├── test/                     # testes
├── pubspec.yaml              # dependências e metadados do projeto
```

O `pubspec.yaml` é o equivalente ao `package.json` do Node ou ao `requirements.txt` do Python — é onde você declara pacotes (como o Riverpod).

### 1.4 Ponto de entrada

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: const Text('Meu App')),
        body: const Center(child: Text('Olá, Flutter!')),
      ),
    );
  }
}
```

- `runApp()` pluga o widget raiz na árvore e inicia o motor de renderização.
- `MaterialApp` configura tema, rotas, etc. (existe também `CupertinoApp` para visual iOS).
- `Scaffold` é o "esqueleto" de uma tela: AppBar, corpo, FAB, drawer, etc.

### 1.5 Hot reload

Uma das grandes vantagens do Flutter: ao salvar o arquivo, o app já atualiza na tela em menos de 1 segundo, sem perder o estado atual (na maioria dos casos). Isso torna o ciclo de desenvolvimento muito rápido — importante lembrar disso quando você for codar o projeto lá na frente.

### 1.6 Por que gerenciamento de estado importa

Em apps pequenos, `setState()` dentro de um `StatefulWidget` resolve. Mas conforme o app cresce, alguns problemas aparecem:

- Como compartilhar o mesmo estado entre widgets distantes na árvore, sem passar parâmetro por 5 camadas (*prop drilling*)?
- Como separar a lógica de "buscar dados e decidir o que mostrar" da lógica de "desenhar a tela"?
- Como testar a lógica sem precisar montar widgets?

É exatamente aí que entram **arquitetura (MVVM)** e **gerenciamento de estado (Riverpod)** — são duas preocupações diferentes que se complementam, como você vai ver.

---

## 2. O que é MVVM

**MVVM = Model – View – ViewModel.** É um padrão arquitetural originado no ecossistema .NET/WPF (Microsoft, ~2005) para permitir *data binding* declarativo entre a UI e os dados. Hoje é amplamente usado também em Android (Jetpack), iOS (Combine/SwiftUI) e Flutter.

### 2.1 As três camadas

| Camada | Responsabilidade | Sabe sobre |
|---|---|---|
| **Model** | Dados puros + regras de negócio + acesso a dados (API, banco, cache) | Nada de UI |
| **View** | Desenhar a UI, capturar interação do usuário | Apenas o ViewModel |
| **ViewModel** | Buscar/transformar dados do Model, expor **estado observável** para a View, expor **comandos** (métodos) que a View pode chamar | Model. **Nunca** a View |

A regra de ouro é a **direção da dependência**: `View → ViewModel → Model`. Nunca o inverso. O ViewModel **não importa nada de UI** (no Flutter, isso significa: o arquivo do ViewModel não deveria importar `package:flutter/material.dart`). Isso é o que torna o ViewModel testável sem precisar montar um widget.

### 2.2 O mecanismo de "binding"

No WPF original, existia *data binding* automático de verdade (XAML). No Flutter não existe isso nativamente — por isso, na prática, usamos uma biblioteca de gerenciamento de estado (Riverpod, no seu caso) para **simular esse binding reativo**:

- A View **observa** (watch) o estado exposto pelo ViewModel → sempre que o estado muda, a View se reconstrói sozinha.
- A View **notifica** o ViewModel quando o usuário interage (tocou um botão, digitou algo) chamando um método do ViewModel.

```
Usuário toca botão
      │
      ▼
   View chama viewModel.incrementar()
      │
      ▼
ViewModel atualiza seu estado interno
      │
      ▼
ViewModel notifica os observadores
      │
      ▼
   View se reconstrói automaticamente
```

### 2.3 Model, com mais detalhe

"Model" geralmente é dividido em duas partes na prática:

- **Entidade / DTO**: uma classe de dados pura (ex: `class Task { id, title, isDone }`), sem lógica de UI nem de rede.
- **Repository / Service**: a classe responsável por *buscar e persistir* essas entidades (API REST, banco local, SharedPreferences, etc). O ViewModel depende do Repository, nunca fala direto com a API.

### 2.4 Por que usar MVVM (e não só "jogar tudo no widget")

- **Testabilidade**: você testa o ViewModel isoladamente (é uma classe Dart comum), sem precisar de `WidgetTester`.
- **Separação de responsabilidades**: um dev de UI mexe na View sem entender toda a lógica de negócio, e vice-versa.
- **Reuso**: o mesmo ViewModel pode alimentar telas diferentes (ex: versão mobile e versão tablet de uma tela).
- É o que seu orientador provavelmente quer ver documentado no TCC: uma justificativa clara de separação de camadas.

---

## 3. O que é Riverpod

**Riverpod** é uma biblioteca de gerenciamento de estado e injeção de dependências para Dart/Flutter, criada por Remi Rousselet (o mesmo autor do pacote `Provider`, do qual Riverpod é o "sucessor espiritual", corrigindo várias limitações dele).

Importante: **Riverpod não é uma arquitetura**. Ele não impõe MVVM, Clean Architecture ou qualquer outra coisa — ele só resolve o problema de "como armazenar estado e notificar quem está observando". A arquitetura (MVVM, no seu caso) é uma decisão em cima dele.

### 3.1 Conceito central: Provider

Um **provider** é uma declaração global (`final` no top-level do arquivo) que descreve **como criar um valor** — não o valor em si. O valor real só é criado na primeira vez que alguém o "observa", e fica guardado dentro de um `ProviderContainer` (não na árvore de widgets, ao contrário do `InheritedWidget`/Provider clássico).

```dart
final helloWorldProvider = Provider<String>((ref) => 'Olá, mundo');
```

Para a árvore de widgets conseguir acessar os providers, é preciso envolver o app inteiro com um `ProviderScope`:

```dart
void main() {
  runApp(const ProviderScope(child: MyApp()));
}
```

### 3.2 Tipos principais de provider

| Provider | Uso |
|---|---|
| `Provider` | Valor imutável / serviço / dependência (ex: um Repository) |
| `NotifierProvider` | Estado **síncrono** e mutável, com métodos que o alteram (ex: contador, lista local) |
| `AsyncNotifierProvider` | Estado **assíncrono** e mutável (ex: dados vindos de uma API, com loading/erro/sucesso automáticos) |
| `FutureProvider` | Um valor assíncrono somente leitura (uma chamada de API que não muda por métodos) |
| `StreamProvider` | Um valor vindo de uma `Stream` (ex: um socket, um listener do Firestore) |

Na prática, dentro do MVVM, os **ViewModels quase sempre serão `Notifier` ou `AsyncNotifier`**, e os **Repositories/Services quase sempre serão `Provider`**.

### 3.3 `ref.watch` vs `ref.read` vs `ref.listen`

- **`ref.watch(provider)`**: usa dentro do `build()` da View. Assina o provider — sempre que ele mudar, a View reconstrói.
- **`ref.read(provider)`**: lê o valor uma única vez, sem assinar. Use dentro de callbacks (`onPressed`, etc.), nunca dentro do `build()`.
- **`ref.listen(provider, (previous, next) {...})`**: executa um efeito colateral quando o provider muda (ex: mostrar um `SnackBar`), sem reconstruir a View inteira.

### 3.4 Code generation (`riverpod_generator`)

Escrever providers na mão é verboso. Por isso, hoje o padrão recomendado é usar a anotação `@riverpod` + geração de código, que cria o provider automaticamente a partir de uma função ou classe:

```dart
part 'meu_arquivo.g.dart'; // obrigatório

@riverpod
String helloWorld(Ref ref) {
  return 'Olá, mundo';
}
```

Isso gera um `helloWorldProvider` pronto pra usar. Você roda o gerador com:

```bash
dart run build_runner watch -d
```

> **Nota de versão:** desde o Riverpod 3.0 (lançado em setembro de 2025), o parâmetro do provider gerado é sempre do tipo `Ref` (antes era um tipo específico tipo `HelloWorldRef`). Se você ver tutoriais mais antigos usando `ExampleRef`, saiba que é sintaxe desatualizada.

### 3.5 `ConsumerWidget`

Para uma View conseguir usar `ref`, ela precisa estender `ConsumerWidget` (em vez de `StatelessWidget`) ou `ConsumerStatefulWidget`:

```dart
class MyView extends ConsumerWidget {
  const MyView({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final value = ref.watch(helloWorldProvider);
    return Text(value);
  }
}
```

### 3.6 Setup do projeto (versões atuais, checadas na documentação oficial)

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_riverpod: ^3.4.3
  riverpod_annotation: ^4.0.7

dev_dependencies:
  flutter_lints: ^5.0.0
  build_runner: ^2.4.13
  riverpod_generator: ^4.0.9
```

Ou, via terminal:

```bash
flutter pub add flutter_riverpod riverpod_annotation
flutter pub add -d build_runner riverpod_generator
```

---

## 4. Exemplo isolado: MVVM puro (sem Riverpod)

Antes de misturar tudo, veja o MVVM funcionando **sem nenhuma biblioteca externa** — só com `ChangeNotifier`, que já vem no Flutter. Isso deixa claro que MVVM é um *padrão*, não uma biblioteca.

**Model** (nesse exemplo, o estado em si já é simples o bastante para não precisar de uma classe separada):

**ViewModel** — repare que ele **não importa `material.dart`**:

```dart
import 'package:flutter/foundation.dart'; // só para ChangeNotifier, não é UI

class CounterViewModel extends ChangeNotifier {
  int _count = 0;
  int get count => _count; // estado exposto (somente leitura pra fora)

  void increment() {
    _count++;
    notifyListeners(); // avisa a View que algo mudou
  }
}
```

**View** — usa `ListenableBuilder` para "escutar" o ViewModel e reconstruir só aquele trecho da árvore:

```dart
import 'package:flutter/material.dart';
import 'counter_viewmodel.dart';

class CounterPage extends StatefulWidget {
  const CounterPage({super.key});

  @override
  State<CounterPage> createState() => _CounterPageState();
}

class _CounterPageState extends State<CounterPage> {
  final viewModel = CounterViewModel();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('MVVM puro')),
      body: Center(
        child: ListenableBuilder(
          listenable: viewModel,
          builder: (context, _) => Text('Contagem: ${viewModel.count}'),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: viewModel.increment, // View chama o "comando" do ViewModel
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

Isso **é** MVVM: a View nunca manipula o número diretamente, ela só chama `viewModel.increment()` e observa `viewModel.count`. O problema é que, num app real, esse jeito manual de "escutar" fica trabalhoso conforme cresce o número de ViewModels e telas — é aí que o Riverpod entra, cuidando desse encanamento pra você.

---

## 5. Exemplo isolado: Riverpod puro (sem MVVM)

Agora o oposto: Riverpod sozinho, sem nenhuma separação de camadas — só pra você ver que Riverpod, por si só, **não impõe arquitetura nenhuma**. Aqui a lógica fica direto dentro do provider e é usada direto no widget.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:riverpod_annotation/riverpod_annotation.dart';

part 'main.g.dart';

@riverpod
class Counter extends _$Counter {
  @override
  int build() => 0; // estado inicial

  void increment() => state++;
}

void main() {
  runApp(const ProviderScope(child: MyApp()));
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: const Text('Riverpod puro')),
        body: Center(
          child: Consumer(
            builder: (context, ref, _) {
              final count = ref.watch(counterProvider);
              return Text('Contagem: $count');
            },
          ),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () => ref.read(counterProvider.notifier).increment(),
          child: const Icon(Icons.add),
        ),
      ),
    );
  }
}
```

> Nesse exemplo específico o `FloatingActionButton` está fora de um `Consumer`, então na prática seria necessário também transformar o `MyApp` em `ConsumerWidget` para ter acesso ao `ref` ali. Deixei assim de propósito pra você notar o problema: **sem uma camada de ViewModel bem definida, a lógica de estado tende a se espalhar pelo widget**, misturando UI com regra de negócio. É exatamente esse espalhamento que o MVVM organiza.

Perceba: aqui não existe Model, nem Repository, nem separação de responsabilidade — é só estado + widget. Funciona para protótipos rápidos, mas em um TCC onde a arquitetura é parte da avaliação, você quer a organização do MVVM por cima disso.

---

## 6. Projeto prático: App de Tarefas com MVVM + Riverpod

Agora vamos juntar tudo. Vamos construir um **app de lista de tarefas simples** (dados em memória, sem backend — o foco é 100% entender a arquitetura, não integração de API), do zero.

### Passo 0 — Pré-requisitos

Confirme que o Flutter está instalado e funcionando:

```bash
flutter --version
flutter doctor
```

### Passo 1 — Criar o projeto

```bash
flutter create tarefas_mvvm_riverpod
cd tarefas_mvvm_riverpod
```

Isso gera um projeto padrão com um contador de exemplo. Vamos limpar e reconstruir.

### Passo 2 — Adicionar as dependências

```bash
flutter pub add flutter_riverpod riverpod_annotation
flutter pub add -d build_runner riverpod_generator
```

Isso atualiza automaticamente seu `pubspec.yaml`.

### Passo 3 — Estrutura de pastas

Dentro de `lib/`, apague o conteúdo de `main.dart` (vamos reescrever) e crie esta estrutura:

```
lib/
├── main.dart
├── data/
│   ├── models/
│   │   └── task.dart
│   └── repositories/
│       └── task_repository.dart
└── features/
    └── tasks/
        ├── viewmodel/
        │   └── task_list_viewmodel.dart
        └── view/
            ├── task_list_view.dart
            └── widgets/
                └── task_tile.dart
```

```bash
mkdir -p lib/data/models lib/data/repositories
mkdir -p lib/features/tasks/viewmodel lib/features/tasks/view/widgets
```

Essa organização é **feature-first**: em vez de ter uma pasta `models/`, uma `views/` e uma `viewmodels/` soltas na raiz misturando tudo de todas as telas, cada *feature* (aqui, "tasks") carrega suas próprias camadas dentro dela. Isso escala melhor conforme o projeto cresce (você teria `features/auth/`, `features/tasks/`, etc.).

### Passo 4 — O Model (`data/models/task.dart`)

A entidade pura — sem nenhuma lógica de UI ou de acesso a dados:

```dart
class Task {
  const Task({
    required this.id,
    required this.title,
    this.isDone = false,
  });

  final String id;
  final String title;
  final bool isDone;

  Task copyWith({String? title, bool? isDone}) {
    return Task(
      id: id,
      title: title ?? this.title,
      isDone: isDone ?? this.isDone,
    );
  }
}
```

`copyWith` é um padrão comum em Dart para criar uma cópia de um objeto imutável alterando só alguns campos — evita mutação direta do estado, o que facilita muito o trabalho do Riverpod (que compara estados por igualdade).

### Passo 5 — O Repository (`data/repositories/task_repository.dart`)

Essa classe representa "de onde os dados vêm". Aqui, simulada em memória (num projeto real, seria uma chamada HTTP, um banco Sqlite, etc.):

```dart
import 'package:riverpod_annotation/riverpod_annotation.dart';

import '../models/task.dart';

part 'task_repository.g.dart';

class TaskRepository {
  // "Banco de dados" fake, em memória.
  final List<Task> _tasks = [
    const Task(id: '1', title: 'Estudar arquitetura MVVM'),
    const Task(id: '2', title: 'Configurar Riverpod no projeto'),
  ];

  Future<List<Task>> getTasks() async {
    await Future.delayed(const Duration(milliseconds: 300)); // simula latência de rede
    return List.unmodifiable(_tasks);
  }

  void addTask(Task task) => _tasks.add(task);

  void removeTask(String id) => _tasks.removeWhere((task) => task.id == id);

  void updateTask(Task updated) {
    final index = _tasks.indexWhere((task) => task.id == updated.id);
    if (index != -1) _tasks[index] = updated;
  }
}

@riverpod
TaskRepository taskRepository(Ref ref) {
  return TaskRepository();
}
```

O `@riverpod` aqui transforma a função `taskRepository` em um provider (`taskRepositoryProvider`) que qualquer ViewModel pode acessar via `ref.watch`/`ref.read`. Isso é a "injeção de dependência" do Riverpod — o ViewModel não instancia `TaskRepository()` diretamente, ele pede pro Riverpod fornecer.

### Passo 6 — O ViewModel (`features/tasks/viewmodel/task_list_viewmodel.dart`)

Esse é o coração da arquitetura. Repare: **nenhum import de `flutter/material.dart`** — ele só conhece o Model e o Repository.

```dart
import 'package:riverpod_annotation/riverpod_annotation.dart';

import '../../../data/models/task.dart';
import '../../../data/repositories/task_repository.dart';

part 'task_list_viewmodel.g.dart';

@riverpod
class TaskListViewModel extends _$TaskListViewModel {
  @override
  Future<List<Task>> build() async {
    // build() roda automaticamente quando o provider é observado
    // pela primeira vez. Aqui buscamos o estado inicial.
    final repository = ref.watch(taskRepositoryProvider);
    return repository.getTasks();
  }

  Future<void> addTask(String title) async {
    final repository = ref.read(taskRepositoryProvider);
    final newTask = Task(
      id: DateTime.now().millisecondsSinceEpoch.toString(),
      title: title,
    );
    repository.addTask(newTask);
    state = await AsyncValue.guard(() => repository.getTasks());
  }

  Future<void> toggleDone(Task task) async {
    final repository = ref.read(taskRepositoryProvider);
    repository.updateTask(task.copyWith(isDone: !task.isDone));
    state = await AsyncValue.guard(() => repository.getTasks());
  }

  Future<void> removeTask(String id) async {
    final repository = ref.read(taskRepositoryProvider);
    repository.removeTask(id);
    state = await AsyncValue.guard(() => repository.getTasks());
  }
}
```

Pontos-chave:

- Estendemos `AsyncNotifier` (via `_$TaskListViewModel`, gerado automaticamente) porque `getTasks()` é assíncrono — isso dá de graça os estados de **loading**, **erro** e **dado carregado**, sem você escrever `bool isLoading` na mão.
- `AsyncValue.guard(...)` executa a função e automaticamente encapsula o resultado em sucesso ou erro — se `getTasks()` lançar uma exceção, o estado vira erro sozinho, sem `try/catch` manual.
- Os métodos (`addTask`, `toggleDone`, `removeTask`) são os **comandos** do ViewModel — é isso que a View vai chamar.

### Passo 7 — Rodar o gerador de código

Antes de continuar, gere os arquivos `.g.dart` (necessários por causa do `part '...'.g.dart'` que colocamos nos arquivos):

```bash
dart run build_runner build --delete-conflicting-outputs
```

Ou, para ele ficar rodando e regenerando a cada alteração salva (recomendado durante o desenvolvimento):

```bash
dart run build_runner watch -d
```

Isso cria `task_repository.g.dart` e `task_list_viewmodel.g.dart` automaticamente — **não edite esses arquivos na mão**.

### Passo 8 — A View: o widget da tarefa (`features/tasks/view/widgets/task_tile.dart`)

Um widget pequeno, "burro" (sem lógica de negócio), que só recebe dados e callbacks:

```dart
import 'package:flutter/material.dart';

import '../../../../data/models/task.dart';

class TaskTile extends StatelessWidget {
  const TaskTile({
    super.key,
    required this.task,
    required this.onToggle,
    required this.onDelete,
  });

  final Task task;
  final VoidCallback onToggle;
  final VoidCallback onDelete;

  @override
  Widget build(BuildContext context) {
    return CheckboxListTile(
      value: task.isDone,
      onChanged: (_) => onToggle(),
      title: Text(
        task.title,
        style: task.isDone
            ? const TextStyle(decoration: TextDecoration.lineThrough)
            : null,
      ),
      secondary: IconButton(
        icon: const Icon(Icons.delete_outline),
        onPressed: onDelete,
      ),
    );
  }
}
```

Repare que `TaskTile` **não sabe que o Riverpod existe**. Ele só recebe um `Task` e duas funções (`onToggle`, `onDelete`). Isso é bom: esse widget poderia ser reusado em outro contexto sem depender de nada de estado global.

### Passo 9 — A View principal (`features/tasks/view/task_list_view.dart`)

Aqui é onde a View efetivamente **observa** o ViewModel:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

import '../viewmodel/task_list_viewmodel.dart';
import 'widgets/task_tile.dart';

class TaskListView extends ConsumerWidget {
  const TaskListView({super.key});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final tasksAsync = ref.watch(taskListViewModelProvider);

    return Scaffold(
      appBar: AppBar(title: const Text('Minhas Tarefas')),
      body: tasksAsync.when(
        data: (tasks) {
          if (tasks.isEmpty) {
            return const Center(child: Text('Nenhuma tarefa ainda.'));
          }
          return ListView.builder(
            itemCount: tasks.length,
            itemBuilder: (context, index) {
              final task = tasks[index];
              return TaskTile(
                task: task,
                onToggle: () => ref
                    .read(taskListViewModelProvider.notifier)
                    .toggleDone(task),
                onDelete: () => ref
                    .read(taskListViewModelProvider.notifier)
                    .removeTask(task.id),
              );
            },
          );
        },
        loading: () => const Center(child: CircularProgressIndicator()),
        error: (error, stackTrace) => Center(child: Text('Erro: $error')),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () => _showAddTaskDialog(context, ref),
        child: const Icon(Icons.add),
      ),
    );
  }

  Future<void> _showAddTaskDialog(BuildContext context, WidgetRef ref) async {
    final controller = TextEditingController();
    await showDialog<void>(
      context: context,
      builder: (dialogContext) => AlertDialog(
        title: const Text('Nova tarefa'),
        content: TextField(
          controller: controller,
          autofocus: true,
          decoration: const InputDecoration(hintText: 'Digite a tarefa'),
        ),
        actions: [
          TextButton(
            onPressed: () => Navigator.pop(dialogContext),
            child: const Text('Cancelar'),
          ),
          FilledButton(
            onPressed: () {
              if (controller.text.trim().isNotEmpty) {
                ref
                    .read(taskListViewModelProvider.notifier)
                    .addTask(controller.text.trim());
              }
              Navigator.pop(dialogContext);
            },
            child: const Text('Adicionar'),
          ),
        ],
      ),
    );
  }
}
```

Pontos-chave:

- `ref.watch(taskListViewModelProvider)` dentro do `build()`: a View **assina** o estado — sempre que o ViewModel mudar, esse widget reconstrói sozinho.
- `.when(data: ..., loading: ..., error: ...)`: como o ViewModel é um `AsyncNotifier`, o estado já vem "embrulhado" em `AsyncValue`, e `.when` obriga você a tratar os três casos possíveis — nada de esquecer de mostrar um spinner.
- `ref.read(...).toggleDone(...)`: dentro de callbacks (não no `build()`), usamos `read`, não `watch` — só queremos *chamar um método*, não assinar mudanças ali.

### Passo 10 — Conectar tudo no `main.dart`

```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

import 'features/tasks/view/task_list_view.dart';

void main() {
  runApp(const ProviderScope(child: MyApp()));
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'MVVM + Riverpod Demo',
      theme: ThemeData(colorSchemeSeed: Colors.indigo, useMaterial3: true),
      home: const TaskListView(),
    );
  }
}
```

O `ProviderScope` **precisa** envolver o app inteiro — é o container onde o estado de todos os providers vive. Sem ele, `ref.watch`/`ref.read` lançam erro.

### Passo 11 — Rodar o app

Com o `build_runner watch -d` já rodando em um terminal (Passo 7), abra outro terminal e rode:

```bash
flutter run
```

Escolha o dispositivo/emulador quando solicitado. Você deve ver a lista com as duas tarefas iniciais, conseguir marcar como concluída (efeito de risco no texto), deletar e adicionar novas tarefas pelo botão "+".

---

## 7. Como os dados fluem (recapitulando)

Quando você toca o botão "Adicionar" no diálogo:

```
1. View chama: ref.read(taskListViewModelProvider.notifier).addTask(texto)
2. ViewModel.addTask() pede o Repository via ref.read(taskRepositoryProvider)
3. ViewModel manda o Repository adicionar a tarefa na "base" em memória
4. ViewModel busca a lista atualizada e faz: state = novoEstado
5. Riverpod detecta a mudança de estado e notifica quem está "assinado" (watch)
6. A View, que tinha feito ref.watch(taskListViewModelProvider), reconstrói
   automaticamente e mostra a nova lista
```

Note que em nenhum momento a `TaskListView` fala diretamente com `TaskRepository` — ela só conhece o `TaskListViewModel`. E o `TaskListViewModel` nunca soube da existência de `Scaffold`, `ListView` ou qualquer widget. Essa é a separação MVVM funcionando na prática, com o Riverpod fazendo o papel do "binding" reativo.

---

## 8. Próximos passos e exercícios

Para aprofundar depois que esse exemplo estiver rodando:

1. **Persistência real**: troque o `TaskRepository` em memória por uma implementação usando `shared_preferences` ou `sqflite` — o ViewModel e a View não precisam mudar (esse é o poder de depender de uma interface/abstração no Model).
2. **Testes de ViewModel**: como o `TaskListViewModel` não depende de widget nenhum, dá pra testá-lo com `ProviderContainer.test()` do próprio Riverpod, sem montar UI. Ótimo tópico pra citar no TCC como vantagem da arquitetura.
3. **Múltiplas features**: crie uma segunda pasta em `features/` (ex: `features/auth/`) seguindo a mesma estrutura, pra treinar a organização feature-first em um projeto com mais de uma tela.
4. **`riverpod_lint`**: instale o pacote de lint oficial do Riverpod — ele avisa em tempo real, no editor, quando você usa `watch` no lugar errado ou esquece de tratar um caso do `AsyncValue`.

Qualquer trecho que travar durante a montagem — erro de `build_runner`, import errado, provider não encontrado — me manda o erro que a gente debuga junto.