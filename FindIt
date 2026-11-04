import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

void main() => runApp(
      ChangeNotifierProvider(
        create: (_) => AppData(),
        builder: (context, child) => const FindItApp(),
      ),
    );

const Color blue = Color(0xFF2563EB);

class Item {
  String title, category, location, description, type, reporter;
  Item(this.title, this.category, this.location, this.description, this.type,
      this.reporter);
}

class AppData extends ChangeNotifier {
  final List<Item> _items = [
    Item('Black Wallet', 'Accessories', 'College Library',
        'Black wallet found near the library entrance.', 'Found', 'Rahul'),
    Item('Student ID Card', 'ID Cards', 'Canteen',
        'Student ID card found on a canteen table.', 'Found', 'Priya'),
    Item('Blue Water Bottle', 'Personal Items', 'Block A',
        'Blue water bottle with silver cap.', 'Lost', 'Arjun'),
  ];
  String _currentUser = '';

  List<Item> get items => List.unmodifiable(_items);
  String get currentUser => _currentUser;

  void setUser(String name) {
    _currentUser = name;
    notifyListeners();
  }

  void addItem(Item item) {
    _items.insert(0, item);
    notifyListeners();
  }
}

class FindItApp extends StatelessWidget {
  const FindItApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'FindIt',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: blue),
        useMaterial3: true,
      ),
      home: const LoginScreen(),
    );
  }
}

class LoginScreen extends StatefulWidget {
  const LoginScreen({super.key});

  @override
  State<LoginScreen> createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  final TextEditingController name = TextEditingController();
  final TextEditingController email = TextEditingController();
  final TextEditingController password = TextEditingController();
  bool register = false;

  void enter() {
    if (email.text.trim().isEmpty ||
        password.text.isEmpty ||
        (register && name.text.trim().isEmpty)) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Please complete all fields')),
      );
      return;
    }
    context.read<AppData>().setUser(
        register ? name.text.trim() : email.text.split('@').first);
    Navigator.pushReplacement(
      context,
      MaterialPageRoute(builder: (_) => const MainScreen()),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(26),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              const Icon(Icons.search, size: 80, color: blue),
              const Text('FindIt',
                  textAlign: TextAlign.center,
                  style: TextStyle(fontSize: 34, fontWeight: FontWeight.bold)),
              const Text('Campus Lost & Found', textAlign: TextAlign.center),
              const SizedBox(height: 35),
              Text(register ? 'Create Account' : 'Welcome Back!',
                  style: const TextStyle(
                      fontSize: 25, fontWeight: FontWeight.bold)),
              if (register) ...[
                const SizedBox(height: 18),
                TextField(
                    controller: name,
                    decoration: const InputDecoration(
                        labelText: 'Full Name',
                        prefixIcon: Icon(Icons.person_outline))),
              ],
              const SizedBox(height: 18),
              TextField(
                  controller: email,
                  decoration: const InputDecoration(
                      labelText: 'Email Address',
                      prefixIcon: Icon(Icons.email_outlined))),
              const SizedBox(height: 18),
              TextField(
                  controller: password,
                  obscureText: true,
                  decoration: const InputDecoration(
                      labelText: 'Password',
                      prefixIcon: Icon(Icons.lock_outline))),
              const SizedBox(height: 25),
              FilledButton(
                  onPressed: enter,
                  child: Text(register ? 'Register' : 'Login')),
              TextButton(
                  onPressed: () => setState(() => register = !register),
                  child: Text(register
                      ? 'Already registered? Login'
                      : 'Create an account')),
            ],
          ),
        ),
      ),
    );
  }
}

class MainScreen extends StatefulWidget {
  const MainScreen({super.key});

  @override
  State<MainScreen> createState() => _MainScreenState();
}

class _MainScreenState extends State<MainScreen> {
  int tab = 0;
  String filter = 'All';

  @override
  Widget build(BuildContext context) {
    final List<Widget> screens = [
      buildHome(),
      const AddReport(),
      const MyReports(),
      const Profile(),
    ];

    return Scaffold(
      body: IndexedStack(index: tab, children: screens),
      bottomNavigationBar: NavigationBar(
        selectedIndex: tab,
        onDestinationSelected: (i) => setState(() => tab = i),
        destinations: const [
          NavigationDestination(
              icon: Icon(Icons.home_outlined), label: 'Home'),
          NavigationDestination(
              icon: Icon(Icons.add_circle_outline), label: 'Report'),
          NavigationDestination(
              icon: Icon(Icons.list_alt_outlined), label: 'Mine'),
          NavigationDestination(
              icon: Icon(Icons.person_outline), label: 'Profile'),
        ],
      ),
    );
  }

  Widget buildHome() {
    return Consumer<AppData>(builder: (context, data, child) {
      final visible = data.items
          .where((i) => filter == 'All' || i.type == filter)
          .toList();
      return Scaffold(
        appBar: AppBar(
            title: const Text('FindIt',
                style: TextStyle(fontWeight: FontWeight.bold))),
        body: ListView(padding: const EdgeInsets.all(18), children: [
          Text('Hello, ${data.currentUser}!',
              style: const TextStyle(fontSize: 25, fontWeight: FontWeight.bold)),
          const SizedBox(height: 20),
          Wrap(
              spacing: 8,
              children: ['All', 'Lost', 'Found'].map<Widget>((f) => ChoiceChip(
                  label: Text(f),
                  selected: filter == f,
                  onSelected: (_) => setState(() => filter = f))).toList()),
          const SizedBox(height: 15),
          ...visible.map<Widget>((i) => ItemCard(item: i)).toList(),
        ]),
      );
    });
  }
}

class ItemCard extends StatelessWidget {
  final Item item;
  const ItemCard({super.key, required this.item});

  @override
  Widget build(BuildContext context) {
    return Card(
      margin: const EdgeInsets.only(bottom: 12),
      child: ListTile(
        leading: const Icon(Icons.inventory_2_outlined, color: blue),
        title: Text(item.title, style: const TextStyle(fontWeight: FontWeight.bold)),
        subtitle: Text('${item.location}\n${item.category}'),
        isThreeLine: true,
        trailing: Text(item.type,
            style: TextStyle(
                fontWeight: FontWeight.bold,
                color: item.type == 'Lost' ? Colors.red : Colors.green)),
        onTap: () => Navigator.push(context,
            MaterialPageRoute(builder: (_) => DetailsScreen(item: item))),
      ),
    );
  }
}

class AddReport extends StatefulWidget {
  const AddReport({super.key});

  @override
  State<AddReport> createState() => _AddReportState();
}

class _AddReportState extends State<AddReport> {
  final TextEditingController title = TextEditingController();
  final TextEditingController location = TextEditingController();
  final TextEditingController desc = TextEditingController();
  String category = 'Accessories';
  String type = 'Lost';

  void submit() {
    if (title.text.trim().isEmpty) return;
    context.read<AppData>().addItem(Item(title.text.trim(), category,
        location.text.trim(), desc.text.trim(), type, context.read<AppData>().currentUser));
    ScaffoldMessenger.of(context)
        .showSnackBar(const SnackBar(content: Text('Report submitted')));
    title.clear();
    location.clear();
    desc.clear();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Add New Report')),
      body: ListView(padding: const EdgeInsets.all(20), children: [
        SegmentedButton<String>(
            segments: const [
              ButtonSegment(value: 'Lost', label: Text('Lost')),
              ButtonSegment(value: 'Found', label: Text('Found')),
            ],
            selected: {type},
            onSelectionChanged: (v) => setState(() => type = v.first)),
        const SizedBox(height: 20),
        TextField(controller: title, decoration: const InputDecoration(labelText: 'Item Name', border: OutlineInputBorder())),
        const SizedBox(height: 15),
        DropdownButtonFormField<String>(
            initialValue: category,
            decoration: const InputDecoration(labelText: 'Category', border: OutlineInputBorder()),
            items: ['Accessories', 'ID Cards', 'Personal Items', 'Other']
                .map<DropdownMenuItem<String>>((v) => DropdownMenuItem(value: v, child: Text(v))).toList(),
            onChanged: (v) => setState(() => category = v!)),
        const SizedBox(height: 15),
        TextField(controller: location, decoration: const InputDecoration(labelText: 'Location', border: OutlineInputBorder())),
        const SizedBox(height: 15),
        TextField(controller: desc, maxLines: 3, decoration: const InputDecoration(labelText: 'Description', border: OutlineInputBorder())),
        const SizedBox(height: 20),
        FilledButton(onPressed: submit, child: const Text('Submit Report')),
      ]),
    );
  }
}

class MyReports extends StatelessWidget {
  const MyReports({super.key});
  @override
  Widget build(BuildContext context) {
    return Consumer<AppData>(builder: (context, data, child) {
      final mine = data.items.where((i) => i.reporter == data.currentUser).toList();
      return Scaffold(
          appBar: AppBar(title: const Text('My Reports')),
          body: mine.isEmpty
              ? const Center(child: Text('No reports submitted yet'))
              : ListView(
                  padding: const EdgeInsets.all(16),
                  children: mine.map<Widget>((i) => ItemCard(item: i)).toList()));
    });
  }
}

class Profile extends StatelessWidget {
  const Profile({super.key});
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Profile')),
      body: Center(
        child: Column(mainAxisAlignment: MainAxisAlignment.center, children: [
          const CircleAvatar(radius: 50, child: Icon(Icons.person, size: 60)),
          const SizedBox(height: 20),
          Text(context.watch<AppData>().currentUser, style: const TextStyle(fontSize: 24, fontWeight: FontWeight.bold)),
          const SizedBox(height: 30),
          OutlinedButton.icon(
              icon: const Icon(Icons.logout),
              onPressed: () => Navigator.pushReplacement(context, MaterialPageRoute(builder: (_) => const LoginScreen())),
              label: const Text('Logout')),
        ]),
      ),
    );
  }
}

class DetailsScreen extends StatelessWidget {
  final Item item;
  const DetailsScreen({super.key, required this.item});
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Item Details')),
      body: Padding(
        padding: const EdgeInsets.all(20),
        child: Column(crossAxisAlignment: CrossAxisAlignment.start, children: [
          Text(item.title, style: const TextStyle(fontSize: 28, fontWeight: FontWeight.bold)),
          const SizedBox(height: 10),
          Chip(label: Text(item.type, style: TextStyle(color: item.type == 'Lost' ? Colors.red : Colors.green))),
          const SizedBox(height: 15),
          ListTile(
            leading: const Icon(Icons.place),
            title: const Text('Location'),
            subtitle: Text(item.location),
          ),
          ListTile(
            leading: const Icon(Icons.category),
            title: const Text('Category'),
            subtitle: Text(item.category),
          ),
          const Padding(
            padding: EdgeInsets.all(16),
            child: Text('Description', style: TextStyle(fontWeight: FontWeight.bold)),
          ),
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 16),
            child: Text(item.description, style: const TextStyle(fontSize: 16)),
          ),
        ]),
      ),
    );
  }
}
