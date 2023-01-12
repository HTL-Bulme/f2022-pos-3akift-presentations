# Flutter 
#### by Lukas Zorn
---
### Überblick
- Einführung in Flutter
- Entwicklungsumgebung einrichten
- Basiskonzepte
- Features und -Erweiterungen
- Designoptionen
- Beispielanwendungen
- Fazit

---

## Einführung 
- Was ist Flutter?
- Warum Flutter?
--
### Was ist Flutter?

- Open-Source-Mobile-Application-Framework 
- by Google + Community
- 2018
- basierend auf Dart
- Plattform übergreifend
- Hot Reload 
- alternative zu react
--
### Warum Flutter?

- Schnelle Entwicklung
- Eine Codebase 
- Benutzerdefinierte Widgets
- Hohe Leistung
- Große Community
- 
note: 
Schnelle Entwicklung: Hot Reload >Änderungen schnell testen >  beschleunigt Entwicklungsprozess

Benutzerdefinierten Widgets: native Benutzeroberflächen > ohne dass Plattform-spezifischer Code

 Hohe Leistung > Maschinencode compiliert

Aktive Community > neue Features und Erweiterungen > Vielzahl von Ressourcen und Unterstützung.

---
## Flutter-Entwicklungsumgebung einrichten

-  Flutter installieren und einrichten
-  Flutter-Projekt erstellen und ausführen

--
### Installation

- Installationspaket von der Flutter-Website laden & ausführen
- evtl. Flutter zu $PATH hinzufügen
- Android-SDK und das Xcode-SDK laden
-  im Terminal: `$flutter doctor` 
--

### Projekt erstellen / ausführen

    
- `flutter create [project_name]`
- `$flutter run`  oder `$flutter run -d [device_id]` 
 
 
 

---
### Basiskonzepte

- Statisches Widget
- Dynamisches Widget 
- Widget-Bäume
- Layout
- Routing und Navigationssystem

--
   
### Statisches Widget

```Dart 
// Erstelle ein Text-Widget mit dem Inhalt "Hello World"
Text('Hello World')
```
--
### Dynamisches Widget 
```Dart 
// Erstelle eine Variable, um den Zähler zu speichern
int counter = 0;

// Erstelle ein FloatingActionButton-Widget,
//das den Zähler bei jedem Klick erhöht
FloatingActionButton(
  onPressed: () {
    setState(() {
      // Erhöhe den Zähler um 1
      counter++;
    });
  },
  // Zeige den aktuellen Wert des Zählers an
  child: Text('$counter'), 
)

```

--
   
### Widget-Bäume:

```Dart
// Erstelle ein Container-Widget mit zwei Spalten, die jeweils zwei Text-Widgets enthalten
Container(
  child: Row(
    children: [
      Column(
        children: [
          Text('Column 1'),
          Text('Column 2'),
        ],
      ),
      Column(
        children: [
          Text('Column 3'),
          Text('Column 4'),
        ],
      ),
    ],
  ),
)

```
note:   

Widget-Baum > Hierarchie der Widgets in der Anwendung 

mehrere Children immer nur ein  Elternelement. 

Widget-Klasse selbst ist das Wurzel-Widget des Baums.

--

### Layout 
   
```Dart
// Erstelle einen Container mit zwei Spalten, die die gleiche Breite haben und jeweils eine andere Farbe haben
Container(
  width: 100,
  height: 100,
  child: Flex(
  // Legen Sie fest, dass die Spalten horizontal angeordnet werden sollen
    direction: Axis.horizontal, 
    children: [
      Expanded(
        child: Container(
        // Erste Spalte hat die Farbe rot
          color: Colors.red, 
        ),
      ),
      Expanded(
        child: Container(
        // Zweite Spalte hat die Farbe grün
          color: Colors.green, 
        ),
      ),
    ],
  ),
)
```
note:
flexibles Layout-System > Benutzeroberflächen mit verschiedenen Bildschirmgrößen/Formfaktoren 

Box-Layout-Widgets: z.B. Container, Row, Column
ermöglichen, andere Widgets zu gruppieren und anzuordnen,

Flex-Layout-System> Größe/Verhältnis von Widgets anzupassen.

--
### Routing und Navigationssystem
   
   
```Dart
// Navigiere zur SecondRoute-Seite
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => SecondRoute(),
  ),
);
```
```Dart
// Navigiere zurück zur vorherigen Seite und übergebe den Wert "some data"
Navigator.pop(context, 'some data');

```
note: eingebautes Navigationssystem ermöglicht, zwischen verschiedenen Seiten (auch als "Routen" bezeichnet) in einer Anwendung zu navigieren

Navigator-Widgets > zu pushen und zu popen > Verwendung(Routen und RouteArguments)

---
### Flutter-Features und -Erweiterungen
- Integration von Native Code (z.B. Zugriff auf die Kamera, GPS, etc.)
-   Verwendung von externen Paketen und Bibliotheken
-   Internationalisierung und Lokalisierung

--

#### Integration von Native Code 
###### Kamera
```Dart 
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:image_picker/image_picker.dart';

class MyCameraApp extends StatefulWidget {
  @override
  _MyCameraAppState createState() => _MyCameraAppState();
}

class _MyCameraAppState extends State<MyCameraApp> {
  // Variablen für das Bild und den Image-Picker
  File _image;
  final picker = ImagePicker();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Kamera'),
      ),
      body: Center(
        child: _image == null
            ? Text('Kein Bild ausgewählt.')
            : Image.file(_image),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _getImage,
        tooltip: 'Bild auswählen',
        child: Icon(Icons.add_a_photo),
      ),
    );
  }

  // Funktion zum Öffnen des Image-Pickers und Auswählen eines Bilds
  void _getImage() async {
    final pickedFile = await picker.getImage(source: ImageSource.camera);
    setState(() {
      _image = File(pickedFile.path);
    });
  }
}

```

--
###### GPS
```Dart
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:location/location.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  Location _location = Location();
  LocationData _currentLocation;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('GPS'),
      ),
      body: Center(
        child: Text(_currentLocation != null
            ? 'Breitengrad: ${_currentLocation.latitude}, Längengrad: ${_currentLocation.longitude}'
            : 'Lade Standort...'),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _getLocation,
        tooltip: 'Standort abfragen',
        child: Icon(Icons.location_on),
      ),
    );
  }

  // Funktion zum Abfragen der aktuellen GPS-Position
  void _getLocation() async {
    try {
      // Frage die GPS-Berechtigung ab
      final status = await _location.requestPermission();
      if (status == PermissionStatus.granted) {
        // Hole die aktuelle GPS-Position
        _currentLocation = await _location.getLocation();
        setState(() {});
      }
    } on PlatformException catch (e) {
      if (e.code == 'PERMISSION_DENIED') {
        print('Permission denied');
      }
    }
  }
}

```
---
## Designoptionen
-   `MaterialApp` 
-   `CupertinoApp`
-   `WidgetsApp` 
-   `MaterialApp` with `ThemeData` 
-   `App`
note:  WidgetsApp -> eigene Navigation, Schriftarten und Farben
ThemeData > Eigene Themen 
App > keine Standardstruktur oder Designrichtlinien

--
 ***MaterialApp***

![[img/Pasted image 20230111183812.png]]
--

<br><br>
 ***CupertinoApp***

![[img/Pasted image 20230111181919.png]]

---

## Beispielanwendungen

-   Google Ads
-   Reflectly
- BMW-App
- Tencent
-   Alibaba


---
## Fazit 
- pro
- con
--
### pro
-  Ein Codebase für mehrere Plattformen
-  Schnelle Entwicklung
-  Nativ gefühlte Anwendungen
-  Breites Widget-Angebot
-  Integration von Native Code
-  Verwendung von externen Paketen und Bibliotheken
-  Internationalisierung und Lokalisierung
-  Performance

--

## con 

-  Hohe Lernkurve
-  Integration von Native Code kann Probleme mit sich bringen 
-  Schlechter Code != Performance 
-  manch externe Bibliotheken/ Pakete möglicherweise nicht vollständig kompatibel

---

# Herzlichen Dank für eure Aufmerksamkeit!
