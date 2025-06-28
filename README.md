// --- lib/main.dart ---


import 'package:flutter/material.dart';
import 'screens/profile_screen.dart';
import 'screens/stats_screen.dart';
import 'screens/quest_screen.dart';
import 'screens/special_quest_screen.dart';
import 'screens/memory_screen.dart';
import 'screens/title_screen.dart';

void main() {
  runApp(TheSystemApp());
}

class TheSystemApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'THE SYSTEM',
      theme: ThemeData.dark().copyWith(
        scaffoldBackgroundColor: Colors.black,
        primaryColor: Colors.blueAccent,
        textTheme: TextTheme(
          bodyText2: TextStyle(color: Colors.blue[100]),
        ),
      ),
      home: MainNavigation(),
      debugShowCheckedModeBanner: false,
    );
  }
}

class MainNavigation extends StatefulWidget {
  @override
  _MainNavigationState createState() => _MainNavigationState();
}

class _MainNavigationState extends State<MainNavigation> {
  int currentIndex = 0;

  final List<Widget> screens = [
    ProfileScreen(),
    StatsScreen(),
    QuestScreen(),
    SpecialQuestScreen(),
    MemoryScreen(),
    TitleScreen(),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: screens[currentIndex],
      bottomNavigationBar: BottomNavigationBar(
        currentIndex: currentIndex,
        onTap: (index) => setState(() => currentIndex = index),
        selectedItemColor: Colors.blueAccent,
        unselectedItemColor: Colors.blueGrey,
        backgroundColor: Colors.black,
        type: BottomNavigationBarType.fixed,
        items: [
          BottomNavigationBarItem(icon: Icon(Icons.person), label: 'Profile'),
          BottomNavigationBarItem(icon: Icon(Icons.bar_chart), label: 'Stats'),
          BottomNavigationBarItem(icon: Icon(Icons.list), label: 'Quests'),
          BottomNavigationBarItem(icon: Icon(Icons.flash_on), label: 'Special'),
          BottomNavigationBarItem(icon: Icon(Icons.memory), label: 'Memory'),
          BottomNavigationBarItem(icon: Icon(Icons.emoji_events), label: 'Titles'),
        ],
      ),
    );
  }
}


// --- lib/screens/profile_screen.dart ---

import 'package:flutter/material.dart';

class ProfileScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Profile')),
      body: Center(child: Text('This is the Profile screen.')),
    );
  }
}


// --- lib/screens/stats_screen.dart ---

import 'package:flutter/material.dart';

class StatsScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Stats')),
      body: Center(child: Text('This is the Stats screen.')),
    );
  }
}


// --- lib/screens/quest_screen.dart ---

import 'package:flutter/material.dart';

class QuestScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Quest')),
      body: Center(child: Text('This is the Quest screen.')),
    );
  }
}


// --- lib/screens/special_quest_screen.dart ---

import 'package:flutter/material.dart';

class SpecialQuestScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Special_quest')),
      body: Center(child: Text('This is the Special_quest screen.')),
    );
  }
}


// --- lib/screens/memory_screen.dart ---

import 'package:flutter/material.dart';

class MemoryScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Memory')),
      body: Center(child: Text('This is the Memory screen.')),
    );
  }
}


// --- lib/screens/title_screen.dart ---

import 'package:flutter/material.dart';

class TitleScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Title')),
      body: Center(child: Text('This is the Title screen.')),
    );
  }
}


// --- lib/models/user.dart ---

class User { String name; int level; String title; Stats stats; }

// --- lib/models/quest.dart ---

class Quest { String title; bool completed; DateTime dueDate; bool isSpecial; String subject; }

// --- lib/models/stats.dart ---

class Stats { int memory; int focus; int stamina; int confidence; }

// --- lib/models/memory.dart ---

class MemoryItem { String topic; DateTime lastReviewed; double memoryPercent; }

// --- lib/models/title.dart ---

class TitleItem { String name; String bonus; bool unlocked; }

// --- lib/services/ai_logic.dart ---


import '../models/quest.dart';
import '../models/memory.dart';
import '../models/user.dart';

class AILogicService {
  List<Quest> generateSuggestedTasks(List<MemoryItem> memoryList) {
    return memoryList
        .where((item) => item.memoryPercent < 50)
        .map((item) => Quest(
              title: "Revise \${item.topic}",
              dueDate: DateTime.now().add(Duration(days: 1)),
              isSpecial: false,
              completed: false,
              subject: "General",
            ))
        .toList();
  }

  double predictMemoryDecay(DateTime lastReviewed) {
    int days = DateTime.now().difference(lastReviewed).inDays;
    return 100 - (days * 12.5);
  }

  void updateStats(User user, Quest quest) {
    if (quest.subject == 'Maths' && quest.completed) {
      user.stats.focus += 1;
    }
    if (quest.isSpecial && quest.completed) {
      user.stats.confidence += 1;
    }
  }

  String suggestTitle(User user) {
    if (user.stats.memory >= 5) return "Shadow Recaller";
    if (user.stats.confidence >= 5) return "Speed Learner";
    if (user.level == 0) return "The Forgotten";
    return "No Title Yet";
  }

  Quest generatePunishmentQuest(String weakSubject) {
    return Quest(
      title: "Complete 10 questions from \$weakSubject",
      isSpecial: true,
      dueDate: DateTime.now().add(Duration(hours: 4)),
      completed: false,
      subject: weakSubject,
    );
  }
}
