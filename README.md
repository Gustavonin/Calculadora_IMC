import 'package:flutter/material.dart';

void main() {
  runApp(CalculadoraIMCApp());
}

class CalculadoraIMCApp extends StatelessWidget {

  @override

 Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Calculadora de IMC',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: IMCCalculator(),
    );
  }
}

class IMCCalculator extends StatefulWidget {
  @override
  _IMCCalculatorState createState() => _IMCCalculatorState();
}

class _IMCCalculatorState extends State<IMCCalculator> {
  final TextEditingController _nameController = TextEditingController();
	
  final TextEditingController _weightController = TextEditingController();
	
  final TextEditingController _heightController = TextEditingController();
  String _result = '';

  void _calculateIMC() {
    final String name = _nameController.text;
		
    final double weight = double.tryParse(_weightController.text) ?? 0;
		
    final double height = double.tryParse(_heightController.text) ?? 0;

    if (weight > 0 && height > 0) {
      double imc = weight / (height * height);
      String classification;

      if (imc < 18.5) {
        classification = 'Baixo peso';
      } else if (imc < 25.0) {
        classification = 'Peso adequado';
      } else if (imc < 30.0) {
        classification = 'Sobrepeso';
      } else {
        classification = 'Obesidade';
      }

      setState(() {
        _result = '$name, seu IMC é ${imc.toStringAsFixed(2)}: $classification';
      });
    } else {
      setState(() {
        _result = 'Por favor, insira valores válidos.';
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Calculadora de IMC'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _nameController,
              decoration: InputDecoration(labelText: 'Nome'),
            ),
            TextField(
              controller: _weightController,
              decoration: InputDecoration(labelText: 'Peso (kg)'),
              keyboardType: TextInputType.number,
            ),
            TextField(
              controller: _heightController,
              decoration: InputDecoration(labelText: 'Altura (m)'),
              keyboardType: TextInputType.number,
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: _calculateIMC,
              child: Text('Calcular IMC'),
            ),
            SizedBox(height: 20),
            Text(
              _result,
              style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold),
              textAlign: TextAlign.center,
            ),
          ],
        ),
      ),
    );
  }
}
