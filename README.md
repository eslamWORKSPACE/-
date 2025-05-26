import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'متابعة الشغل',
      theme: ThemeData(
        primarySwatch: Colors.teal,
      ),
      home: HomeScreen(),
    );
  }
}

class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('الرئيسية'),
      ),
      body: ListView(
        children: [
          ListTile(
            title: Text('➕ إضافة مهمة يومية'),
            onTap: () {
              Navigator.push(context, MaterialPageRoute(builder: (_) => AddTaskScreen()));
            },
          ),
          ListTile(
            title: Text('📦 المصروفات والمشتريات'),
            onTap: () {
              Navigator.push(context, MaterialPageRoute(builder: (_) => ExpensesScreen()));
            },
          ),
        ],
      ),
    );
  }
}

class AddTaskScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('إضافة مهمة')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(decoration: InputDecoration(labelText: 'اسم المشروع')),
            TextField(decoration: InputDecoration(labelText: 'نوع الشغل')),
            TextField(decoration: InputDecoration(labelText: 'اسم الفني')),
            TextField(decoration: InputDecoration(labelText: 'الوصف')),
            ElevatedButton(onPressed: () {}, child: Text('حفظ المهمة'))
          ],
        ),
      ),
    );
  }
}

class ExpensesScreen extends StatefulWidget {
  @override
  _ExpensesScreenState createState() => _ExpensesScreenState();
}

class _ExpensesScreenState extends State<ExpensesScreen> {
  final List<Map<String, String>> expenses = [];
  final _typeController = TextEditingController();
  final _amountController = TextEditingController();
  final _noteController = TextEditingController();

  void _addExpense() {
    if (_typeController.text.isNotEmpty && _amountController.text.isNotEmpty) {
      setState(() {
        expenses.add({
          'type': _typeController.text,
          'amount': _amountController.text,
          'note': _noteController.text,
        });
        _typeController.clear();
        _amountController.clear();
        _noteController.clear();
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('المصروفات والمشتريات')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            TextField(
              controller: _typeController,
              decoration: InputDecoration(labelText: 'نوع المصروف (كلور، قطع غيار، مواصلات...)'),
            ),
            TextField(
              controller: _amountController,
              keyboardType: TextInputType.number,
              decoration: InputDecoration(labelText: 'المبلغ'),
            ),
            TextField(
              controller: _noteController,
              decoration: InputDecoration(labelText: 'ملاحظات (اختياري)'),
            ),
            ElevatedButton(onPressed: _addExpense, child: Text('إضافة')),
            SizedBox(height: 20),
            Expanded(
              child: ListView.builder(
                itemCount: expenses.length,
                itemBuilder: (context, index) {
                  final item = expenses[index];
                  return ListTile(
                    title: Text('${item['type']} - ${item['amount']} جنيه'),
                    subtitle: Text(item['note'] ?? ''),
                  );
                },
              ),
            )
          ],
        ),
      ),
    );
  }
}
