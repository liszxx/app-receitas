// main.dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:shared_preferences/shared_preferences.dart';
import 'dart:convert';
import 'package:http/http.dart' as http;

// ===================== MODELO DE USUÁRIO =====================
class UserModel {
  final String nome;
  final String email;
  final String telefone;

  UserModel({required this.nome, required this.email, required this.telefone});

  Map<String, dynamic> toJson() => {
        'nome': nome,
        'email': email,
        'telefone': telefone,
      };

  factory UserModel.fromJson(Map<String, dynamic> json) {
    return UserModel(
      nome: json['nome'],
      email: json['email'],
      telefone: json['telefone'],
    );
  }
}

// ===================== PROVIDER PARA CONTADOR =====================
class CounterProvider extends ChangeNotifier {
  int counter = 0;

  void increment() {
    counter++;
    notifyListeners(); // Atualiza todas as telas/listeners que escutam este provider
  }
}

// ===================== PROVIDER PARA TEMA =====================
class ThemeProvider extends ChangeNotifier {
  bool isDarkMode = false;

  Future<void> loadTheme() async {
    SharedPreferences prefs = await SharedPreferences.getInstance();
    isDarkMode = prefs.getBool('isDarkMode') ?? false;
    notifyListeners();
  }

  Future<void> toggleTheme() async {
    isDarkMode = !isDarkMode;
    SharedPreferences prefs = await SharedPreferences.getInstance();
    await prefs.setBool('isDarkMode', isDarkMode);
    notifyListeners();
  }
}

// ===================== INÍCIO DO APP =====================
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  runApp(
    MultiProvider(
      providers: [
        ChangeNotifierProvider(create: (_) => CounterProvider()),
        ChangeNotifierProvider(create: (_) => ThemeProvider()),
      ],
      child: const MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return Consumer<ThemeProvider>(
      builder: (context, themeProvider, _) {
        return MaterialApp(
          title: 'Exemplo Prova',
          debugShowCheckedModeBanner: false,
          theme: ThemeData.light(),
          darkTheme: ThemeData.dark(),
          themeMode: themeProvider.isDarkMode ? ThemeMode.dark : ThemeMode.light,
          home: const HomeScreen(),
        );
      },
    );
  }
}

// ===================== HOME SCREEN =====================
class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  // Variáveis de estado local
  Color bgColor = Colors.white;
  bool switchValue = false;
  bool checkValue = false;
  double sliderValue = 0;

  // Usuário fake para exemplo
  final UserModel userAtual = UserModel(nome: 'Elisa', email: 'elisa@email.com', telefone: '11999999999');

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Home Screen'),
        actions: [
          IconButton(
            icon: const Icon(Icons.brightness_6),
            onPressed: () {
              // Alterna o tema claro/escuro
              context.read<ThemeProvider>().toggleTheme();
              ScaffoldMessenger.of(context).showSnackBar(
                const SnackBar(content: Text('Tema alternado!')),
              );
            },
          )
        ],
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.stretch,
          children: [
            // ================== GESTUREDETECTOR ==================
            GestureDetector(
              onTap: () {
                setState(() {
                  bgColor = bgColor == Colors.white ? Colors.blue.shade100 : Colors.white;
                });
                ScaffoldMessenger.of(context).showSnackBar(
                  const SnackBar(content: Text('Cor de fundo alterada!')),
                );
              },
              child: Container(
                height: 100,
                color: bgColor,
                child: const Center(child: Text('Toque para alternar cor')),
              ),
            ),
            const SizedBox(height: 16),

            // ================== BOTÃO DE NAVEGAÇÃO COM PARAMETRO ==================
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (_) => ProfileScreen(user: userAtual),
                  ),
                );
              },
              child: const Text('Ver Perfil'),
            ),
            const SizedBox(height: 16),

            // ================== CHANGE NOTIFIER EXEMPLO ==================
            Text('Contador global usando ChangeNotifier:', style: const TextStyle(fontWeight: FontWeight.bold)),
            Consumer<CounterProvider>(
              builder: (context, counterProvider, _) {
                return Column(
                  children: [
                    Text('Valor: ${counterProvider.counter}', style: const TextStyle(fontSize: 18)),
                    ElevatedButton(
                      onPressed: counterProvider.increment,
                      child: const Text('Incrementar contador'),
                    ),
                  ],
                );
              },
            ),
            const SizedBox(height: 16),

            // ================== SLIDER ==================
            Text('Slider: ${sliderValue.toStringAsFixed(1)}'),
            Slider(
              value: sliderValue,
              min: 0,
              max: 100,
              divisions: 20,
              label: sliderValue.toStringAsFixed(1),
              onChanged: (value) {
                setState(() {
                  sliderValue = value;
                });
              },
            ),
            const SizedBox(height: 16),

            // ================== RADIO ==================
            Text('Escolha uma opção:'),
            RadioListTile<String>(
              title: const Text('Opção 1'),
              value: '1',
              groupValue: switchValue ? '2' : '1',
              onChanged: (value) {
                setState(() {
                  switchValue = value == '2';
                });
              },
            ),
            RadioListTile<String>(
              title: const Text('Opção 2'),
              value: '2',
              groupValue: switchValue ? '2' : '1',
              onChanged: (value) {
                setState(() {
                  switchValue = value == '2';
                });
              },
            ),
            const SizedBox(height: 16),

            // ================== SWITCH ==================
            SwitchListTile(
              title: const Text('Ativar opção'),
              value: switchValue,
              onChanged: (value) {
                setState(() {
                  switchValue = value;
                });
              },
            ),
            const SizedBox(height: 16),

            // ================== CHECKBOX ==================
            CheckboxListTile(
              title: const Text('Selecionar item'),
              value: checkValue,
              onChanged: (value) {
                setState(() {
                  checkValue = value ?? false;
                });
              },
            ),
            const SizedBox(height: 16),

            // ================== LISTVIEW FUTUREBUILDER (API SIMULADA) ==================
            const Text('Exemplo de API (GET):', style: TextStyle(fontWeight: FontWeight.bold)),
            SizedBox(
              height: 200,
              child: FutureBuilder<List<String>>(
                future: fetchFakeData(),
                builder: (context, snapshot) {
                  if (snapshot.connectionState == ConnectionState.waiting) {
                    return const Center(child: CircularProgressIndicator());
                  } else if (snapshot.hasError) {
                    return Center(child: Text('Erro: ${snapshot.error}'));
                  } else if (!snapshot.hasData || snapshot.data!.isEmpty) {
                    return const Center(child: Text('Nenhum dado encontrado'));
                  } else {
                    final items = snapshot.data!;
                    return ListView.builder(
                      itemCount: items.length,
                      itemBuilder: (context, index) {
                        return ListTile(
                          title: Text(items[index]),
                        );
                      },
                    );
                  }
                },
              ),
            ),
          ],
        ),
      ),
    );
  }

  // Simula chamada GET
  Future<List<String>> fetchFakeData() async {
    await Future.delayed(const Duration(seconds: 2));
    return ['Item 1', 'Item 2', 'Item 3', 'Item 4'];
  }
}

// ===================== PROFILE SCREEN =====================
class ProfileScreen extends StatelessWidget {
  final UserModel user;

  const ProfileScreen({super.key, required this.user});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Perfil do Usuário')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Nome: ${user.nome}', style: const TextStyle(fontSize: 18)),
            Text('Email: ${user.email}', style: const TextStyle(fontSize: 18)),
            Text('Telefone: ${user.telefone}', style: const TextStyle(fontSize: 18)),
          ],
        ),
      ),
    );
  }
}
