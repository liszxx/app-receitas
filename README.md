import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'contador_store.dart';

class HomeScreen extends StatefulWidget {
  const HomeScreen({super.key});

  @override
  State<HomeScreen> createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  // Q1
  bool mostrarDetalhes = false;

  // Q2
  int contadorLocal = 0;

  // Q3
  bool statusAtivo = false;

  // Q4
  final ValueNotifier<int> valorNotifier = ValueNotifier<int>(0);

  @override
  Widget build(BuildContext context) {
    final contadorStore = context.watch<ContadorStore>(); // Q5

    return Scaffold(
      appBar: AppBar(
        title: const Text("Prova – Card 4"),
      ),
      body: SingleChildScrollView(
        padding: const EdgeInsets.all(16),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [

            //----------------------------------------------------------------------
            // Q1 – SWITCH PARA MOSTRAR/OCULTAR DETALHES
            //----------------------------------------------------------------------
            const Text(
              "Q1 – Mostrar detalhes com Switch",
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            SwitchListTile(
              title: const Text("Exibir detalhes"),
              value: mostrarDetalhes,
              onChanged: (v) {
                setState(() {
                  mostrarDetalhes = v;
                });
              },
            ),
            if (mostrarDetalhes)
              const Padding(
                padding: EdgeInsets.only(bottom: 16),
                child: Text(
                  "Aqui estão os detalhes adicionais...",
                  style: TextStyle(fontSize: 16),
                ),
              ),

            const Divider(),

            //----------------------------------------------------------------------
            // Q2 – CONTADOR LOCAL + E -
            //----------------------------------------------------------------------
            const Text(
              "Q2 – Contador com + e -",
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            Row(
              children: [
                ElevatedButton(
                  onPressed: () {
                    setState(() => contadorLocal--);
                  },
                  child: const Text("-1"),
                ),
                Padding(
                  padding: const EdgeInsets.symmetric(horizontal: 16),
                  child: Text(
                    "$contadorLocal",
                    style: const TextStyle(fontSize: 22),
                  ),
                ),
                ElevatedButton(
                  onPressed: () {
                    setState(() => contadorLocal++);
                  },
                  child: const Text("+1"),
                ),
              ],
            ),

            const Divider(),

            //----------------------------------------------------------------------
            // Q3 – STATUS VISUAL (ATIVO/INATIVO)
            //----------------------------------------------------------------------
            const Text(
              "Q3 – Status visual",
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),
            GestureDetector(
              onTap: () {
                setState(() => statusAtivo = !statusAtivo);
              },
              child: Container(
                height: 80,
                width: double.infinity,
                alignment: Alignment.center,
                decoration: BoxDecoration(
                  color: statusAtivo ? Colors.green : Colors.red,
                ),
                child: Text(
                  statusAtivo ? "Ativo" : "Inativo",
                  style: const TextStyle(color: Colors.white, fontSize: 22),
                ),
              ),
            ),

            const Divider(),

            //----------------------------------------------------------------------
            // Q4 – VALUENOTIFIER
            //----------------------------------------------------------------------
            const Text(
              "Q4 – ValueNotifier",
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),

            ValueListenableBuilder<int>(
              valueListenable: valorNotifier,
              builder: (context, valor, _) {
                return Text(
                  "Valor atual: $valor",
                  style: const TextStyle(fontSize: 20),
                );
              },
            ),

            Row(
              children: [
                ElevatedButton(
                  onPressed: () {
                    valorNotifier.value++;
                  },
                  child: const Text("Incrementar"),
                ),
                const SizedBox(width: 12),
                ElevatedButton(
                  onPressed: () {
                    valorNotifier.value = 0;
                  },
                  child: const Text("Zerar"),
                ),
              ],
            ),

            const Divider(),

            //----------------------------------------------------------------------
            // Q5 – CHANGENOTIFIER (GLOBAL)
            //----------------------------------------------------------------------
            const Text(
              "Q5 – ChangeNotifier compartilhado",
              style: TextStyle(fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 8),

            Text(
              "Valor global: ${contadorStore.valor}",
              style: const TextStyle(fontSize: 22),
            ),

            ElevatedButton(
              onPressed: () {
                contadorStore.incrementar();
              },
              child: const Text("Somar +1 Global"),
            ),

            const SizedBox(height: 40),
          ],
        ),
      ),
    );
  }
}
