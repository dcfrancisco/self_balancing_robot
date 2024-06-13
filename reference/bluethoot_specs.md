To create a Flutter Bluetooth remote control for your robot, you'll need to map out all the interfaces, pins, and Bluetooth values that your robot uses. Here’s a breakdown of the required information:

### Pin Definitions

#### Ultrasonic Module
- **Trigger Pin (TRIG)**: Pin 3
- **Echo Pin (ECHO)**: Pin 5

#### RGB LED Control
- **Red LED Pin (RLED)**: Pin A0
- **Green LED Pin (GLED)**: Pin A1
- **Blue LED Pin (BLED)**: Pin A2

#### Buzzer
- **Buzzer Pin (BUZZER)**: Pin 11

#### MPU6050 Gyroscope
- **SCL Pin (MPU_SCL)**: Pin 5
- **SDA Pin (MPU_SDA)**: Pin 4

#### TB6612 Motor Driver
- **Standby Pin (TB6612_STBY)**: Pin 8
- **PWM A Pin (TB6612_PWMA)**: Pin 10
- **PWM B Pin (TB6612_PWMB)**: Pin 9
- **AIN1 Pin (TB6612_AIN1)**: Pin 12
- **AIN2 Pin (TB6612_AIN2)**: Pin 13
- **BIN1 Pin (TB6612_BIN1)**: Pin 7
- **BIN2 Pin (TB6612_BIN2)**: Pin 6

#### Motor Encoders
- **Motor Encoder 1 (MOTOR1)**: Pin 2
- **Motor Encoder 2 (MOTOR2)**: Pin 4

### Bluetooth Communication

#### RX Package Structure
- **Packet Header (Byte 0)**: 0xA5
- **X-axis Value (Byte 1)**: x_axis
- **Y-axis Value (Byte 2)**: y_axis
- **Klaxon (Byte 3)**: klaxon
- **Counterclockwise Rotation (Byte 4)**: N_Button
- **Clockwise Rotation (Byte 5)**: S_Button
- **Mode 1 Button (Byte 6)**: mode1_Button
- **Mode 2 Button (Byte 7)**: mode2_Button
- **Mode 3 Button (Byte 8)**: mode3_Button
- **Packet Tail (Byte 10)**: 0x5A

#### TX Package Structure
- **Packet Header (Byte 0)**: 0xA5
- **Data (Byte 1)**: dat
- **Check Sum (Byte 2)**: TX_package[1]
- **Packet Tail (Byte 3)**: 0x5A

### Flutter Bluetooth Remote Control

#### Required Controls
- **Joystick or D-pad**: To control `x_axis` and `y_axis`.
- **Buttons**: For `klaxon`, `N_Button`, `S_Button`, `mode1_Button`, `mode2_Button`, and `mode3_Button`.

#### Data Mapping
1. **Joystick**: Maps to `x_axis` and `y_axis` values.
2. **Buttons**:
   - **Klaxon**: `klaxon`
   - **Counterclockwise Rotation**: `N_Button`
   - **Clockwise Rotation**: `S_Button`
   - **Mode 1**: `mode1_Button`
   - **Mode 2**: `mode2_Button`
   - **Mode 3**: `mode3_Button`

### Example Code for Flutter Bluetooth Communication

Here’s an example of how you can set up the Bluetooth communication in Flutter:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_bluetooth_serial/flutter_bluetooth_serial.dart';

class BluetoothControlScreen extends StatefulWidget {
  @override
  _BluetoothControlScreenState createState() => _BluetoothControlScreenState();
}

class _BluetoothControlScreenState extends State<BluetoothControlScreen> {
  BluetoothConnection? connection;
  String x_axis = '0';
  String y_axis = '0';
  String klaxon = '0';
  String n_button = '0';
  String s_button = '0';
  String mode1_button = '0';
  String mode2_button = '0';
  String mode3_button = '0';

  void connectToBluetooth() async {
    BluetoothDevice device = await selectBluetoothDevice();
    BluetoothConnection.toAddress(device.address).then((_connection) {
      connection = _connection;
      setState(() {});
    });
  }

  Future<BluetoothDevice> selectBluetoothDevice() async {
    List<BluetoothDevice> devices = await FlutterBluetoothSerial.instance.getBondedDevices();
    return devices.first; // Select the first device for simplicity
  }

  void sendData(String data) {
    if (connection != null && connection!.isConnected) {
      connection!.output.add(Uint8List.fromList(data.codeUnits));
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Bluetooth Control'),
      ),
      body: Column(
        children: [
          // Add your joystick or D-pad here to control x_axis and y_axis
          // Add your buttons here and map their onPressed to set the respective values
          ElevatedButton(
            onPressed: () {
              klaxon = '1';
              sendData('klaxon:1');
            },
            child: Text('Klaxon'),
          ),
          // Add more buttons for N_Button, S_Button, mode1_Button, mode2_Button, mode3_Button
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: connectToBluetooth,
        child: Icon(Icons.bluetooth),
      ),
    );
  }
}
```

### Conclusion
This breakdown should help you configure your Flutter Bluetooth remote control to interact with your robot. The provided Flutter code is a basic example and should be extended to include joystick or D-pad functionality, as well as additional button mappings.


Here is the information formatted into a markdown table:

### Pin Definitions

| Component                   | Description                    | Pin       |
|-----------------------------|--------------------------------|-----------|
| Ultrasonic Module           | Trigger Pin                    | `TRIG 3`  |
| Ultrasonic Module           | Echo Pin                       | `ECHO 5`  |
| RGB LED                     | Red LED Pin                    | `RLED A0` |
| RGB LED                     | Green LED Pin                  | `GLED A1` |
| RGB LED                     | Blue LED Pin                   | `BLED A2` |
| Buzzer                      | Buzzer Pin                     | `BUZZER 11`|
| MPU6050 Gyroscope           | SCL Pin                        | `MPU_SCL 5`|
| MPU6050 Gyroscope           | SDA Pin                        | `MPU_SDA 4`|
| TB6612 Motor Driver         | Standby Pin                    | `TB6612_STBY 8`|
| TB6612 Motor Driver         | PWM A Pin                      | `TB6612_PWMA 10`|
| TB6612 Motor Driver         | PWM B Pin                      | `TB6612_PWMB 9`|
| TB6612 Motor Driver         | AIN1 Pin                       | `TB6612_AIN1 12`|
| TB6612 Motor Driver         | AIN2 Pin                       | `TB6612_AIN2 13`|
| TB6612 Motor Driver         | BIN1 Pin                       | `TB6612_BIN1 7`|
| TB6612 Motor Driver         | BIN2 Pin                       | `TB6612_BIN2 6`|
| Motor Encoder               | Motor Encoder 1                | `MOTOR1 2`|
| Motor Encoder               | Motor Encoder 2                | `MOTOR2 4`|

### Bluetooth Communication

| Variable           | Description                     | Byte Index   |
|--------------------|---------------------------------|--------------|
| Packet Header      | Start of the packet             | `0xA5`       |
| X-axis Value       | Control variable for X-axis     | `1`          |
| Y-axis Value       | Control variable for Y-axis     | `2`          |
| Klaxon             | Control variable for the horn   | `3`          |
| N_Button           | Rotate counterclockwise         | `4`          |
| S_Button           | Rotate clockwise                | `5`          |
| mode1_Button       | Mode 1 activation               | `6`          |
| mode2_Button       | Mode 2 activation               | `7`          |
| mode3_Button       | Mode 3 activation               | `8`          |
| Packet Tail        | End of the packet               | `0x5A`       |

### TX Package Structure

| Variable           | Description                     | Byte Index   |
|--------------------|---------------------------------|--------------|
| Packet Header      | Start of the packet             | `0xA5`       |
| Data               | Data to be sent                 | `1`          |
| Check Sum          | Check sum of the data           | `2`          |
| Packet Tail        | End of the packet               | `0x5A`       |

This table provides a clear overview of the pin definitions and the Bluetooth communication structure required for the Flutter Bluetooth remote control.


Based on the provided code and the functionality described, the following buttons are needed for the Bluetooth remote control:

1. **Joystick or D-pad**: To control `x_axis` and `y_axis` (for forward, backward, left, and right movement).
2. **Klaxon Button**: To control the horn.
3. **N_Button**: For counterclockwise rotation.
4. **S_Button**: For clockwise rotation.
5. **Mode1_Button**: To activate Mode 1 (Control through Bluetooth).
6. **Mode2_Button**: To activate Mode 2 (Obstacle avoidance mode).
7. **Mode3_Button**: To activate Mode 3 (Follow mode).

### Control Summary

| Control          | Description                                  |
|------------------|----------------------------------------------|
| Joystick/D-pad   | Controls `x_axis` and `y_axis` for movement. |
| Klaxon Button    | Toggles the horn on/off.                     |
| N_Button         | Activates counterclockwise rotation.         |
| S_Button         | Activates clockwise rotation.                |
| Mode1_Button     | Activates Mode 1 (Bluetooth control).        |
| Mode2_Button     | Activates Mode 2 (Obstacle avoidance).       |
| Mode3_Button     | Activates Mode 3 (Follow mode).              |

### Detailed Button Functions

1. **Joystick/D-pad**:
    - Controls `x_axis` and `y_axis` for:
        - Forward: `y_axis > 30`
        - Backward: `y_axis < -30`
        - Left: `x_axis > 30`
        - Right: `x_axis < -30`

2. **Klaxon Button**:
    - Sets `klaxon > 0` for turning the horn on and `klaxon = 0` for turning it off.

3. **N_Button**:
    - `N_Button > 0` triggers counterclockwise rotation.

4. **S_Button**:
    - `S_Button > 0` triggers clockwise rotation.

5. **Mode1_Button**:
    - Activates Mode 1 with `mode1_var = 1`.

6. **Mode2_Button**:
    - Activates Mode 2 with `mode2_var = 1`.

7. **Mode3_Button**:
    - Activates Mode 3 with `mode3_var = 1`.

### Example Layout for Flutter Bluetooth Remote Control

Here’s an example of how you might structure the Flutter UI to include all these controls:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_bluetooth_serial/flutter_bluetooth_serial.dart';

class BluetoothControlScreen extends StatefulWidget {
  @override
  _BluetoothControlScreenState createState() => _BluetoothControlScreenState();
}

class _BluetoothControlScreenState extends State<BluetoothControlScreen> {
  BluetoothConnection? connection;
  String x_axis = '0';
  String y_axis = '0';
  bool klaxon = false;
  bool n_button = false;
  bool s_button = false;
  bool mode1_button = false;
  bool mode2_button = false;
  bool mode3_button = false;

  void connectToBluetooth() async {
    BluetoothDevice device = await selectBluetoothDevice();
    BluetoothConnection.toAddress(device.address).then((_connection) {
      connection = _connection;
      setState(() {});
    });
  }

  Future<BluetoothDevice> selectBluetoothDevice() async {
    List<BluetoothDevice> devices = await FlutterBluetoothSerial.instance.getBondedDevices();
    return devices.first; // Select the first device for simplicity
  }

  void sendData(String data) {
    if (connection != null && connection!.isConnected) {
      connection!.output.add(Uint8List.fromList(data.codeUnits));
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Bluetooth Control'),
      ),
      body: Column(
        children: [
          // Joystick or D-pad for x_axis and y_axis
          Joystick(
            mode: JoystickMode.all,
            listener: (details) {
              setState(() {
                x_axis = details.x.toString();
                y_axis = details.y.toString();
              });
              sendData('x:$x_axis,y:$y_axis');
            },
          ),
          // Klaxon Button
          ElevatedButton(
            onPressed: () {
              setState(() {
                klaxon = !klaxon;
              });
              sendData('klaxon:${klaxon ? 1 : 0}');
            },
            child: Text('Klaxon'),
          ),
          // N_Button
          ElevatedButton(
            onPressed: () {
              setState(() {
                n_button = true;
                s_button = false;
              });
              sendData('N_Button:1');
            },
            child: Text('Rotate Counterclockwise'),
          ),
          // S_Button
          ElevatedButton(
            onPressed: () {
              setState(() {
                s_button = true;
                n_button = false;
              });
              sendData('S_Button:1');
            },
            child: Text('Rotate Clockwise'),
          ),
          // Mode Buttons
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: [
              ElevatedButton(
                onPressed: () {
                  setState(() {
                    mode1_button = true;
                    mode2_button = false;
                    mode3_button = false;
                  });
                  sendData('mode1:1');
                },
                child: Text('Mode 1'),
              ),
              ElevatedButton(
                onPressed: () {
                  setState(() {
                    mode1_button = false;
                    mode2_button = true;
                    mode3_button = false;
                  });
                  sendData('mode2:1');
                },
                child: Text('Mode 2'),
              ),
              ElevatedButton(
                onPressed: () {
                  setState(() {
                    mode1_button = false;
                    mode2_button = false;
                    mode3_button = true;
                  });
                  sendData('mode3:1');
                },
                child: Text('Mode 3'),
              ),
            ],
          ),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: connectToBluetooth,
        child: Icon(Icons.bluetooth),
      ),
    );
  }
}

class Joystick extends StatelessWidget {
  final Function listener;
  final JoystickMode mode;

  Joystick({required this.listener, required this.mode});

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 200,
      width: 200,
      child: Center(
        child: Text('Joystick Here'), // Placeholder for actual joystick widget
      ),
    );
  }
}

enum JoystickMode { all, horizontal, vertical }
```

This example assumes you have a joystick or D-pad widget for controlling `x_axis` and `y_axis`. You can replace the placeholder with an actual joystick widget suitable for Flutter.


Yes, you can use a selector like a `DropdownButton` or `SegmentedControl` in Flutter for the mode buttons. Here’s an example using a `DropdownButton` for mode selection:

### Flutter UI with DropdownButton for Mode Selection

```dart
import 'package:flutter/material.dart';
import 'package:flutter_bluetooth_serial/flutter_bluetooth_serial.dart';

class BluetoothControlScreen extends StatefulWidget {
  @override
  _BluetoothControlScreenState createState() => _BluetoothControlScreenState();
}

class _BluetoothControlScreenState extends State<BluetoothControlScreen> {
  BluetoothConnection? connection;
  String x_axis = '0';
  String y_axis = '0';
  bool klaxon = false;
  bool n_button = false;
  bool s_button = false;
  String selectedMode = 'Mode 1'; // Default selected mode

  void connectToBluetooth() async {
    BluetoothDevice device = await selectBluetoothDevice();
    BluetoothConnection.toAddress(device.address).then((_connection) {
      connection = _connection;
      setState(() {});
    });
  }

  Future<BluetoothDevice> selectBluetoothDevice() async {
    List<BluetoothDevice> devices = await FlutterBluetoothSerial.instance.getBondedDevices();
    return devices.first; // Select the first device for simplicity
  }

  void sendData(String data) {
    if (connection != null && connection!.isConnected) {
      connection!.output.add(Uint8List.fromList(data.codeUnits));
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Bluetooth Control'),
      ),
      body: Column(
        children: [
          // Joystick or D-pad for x_axis and y_axis
          Joystick(
            mode: JoystickMode.all,
            listener: (details) {
              setState(() {
                x_axis = details.x.toString();
                y_axis = details.y.toString();
              });
              sendData('x:$x_axis,y:$y_axis');
            },
          ),
          // Klaxon Button
          ElevatedButton(
            onPressed: () {
              setState(() {
                klaxon = !klaxon;
              });
              sendData('klaxon:${klaxon ? 1 : 0}');
            },
            child: Text('Klaxon'),
          ),
          // N_Button
          ElevatedButton(
            onPressed: () {
              setState(() {
                n_button = true;
                s_button = false;
              });
              sendData('N_Button:1');
            },
            child: Text('Rotate Counterclockwise'),
          ),
          // S_Button
          ElevatedButton(
            onPressed: () {
              setState(() {
                s_button = true;
                n_button = false;
              });
              sendData('S_Button:1');
            },
            child: Text('Rotate Clockwise'),
          ),
          // Mode Selector
          DropdownButton<String>(
            value: selectedMode,
            items: [
              DropdownMenuItem(value: 'Mode 1', child: Text('Mode 1')),
              DropdownMenuItem(value: 'Mode 2', child: Text('Mode 2')),
              DropdownMenuItem(value: 'Mode 3', child: Text('Mode 3')),
            ],
            onChanged: (value) {
              setState(() {
                selectedMode = value!;
              });
              switch (selectedMode) {
                case 'Mode 1':
                  sendData('mode1:1');
                  break;
                case 'Mode 2':
                  sendData('mode2:1');
                  break;
                case 'Mode 3':
                  sendData('mode3:1');
                  break;
              }
            },
          ),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: connectToBluetooth,
        child: Icon(Icons.bluetooth),
      ),
    );
  }
}

class Joystick extends StatelessWidget {
  final Function listener;
  final JoystickMode mode;

  Joystick({required this.listener, required this.mode});

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 200,
      width: 200,
      child: Center(
        child: Text('Joystick Here'), // Placeholder for actual joystick widget
      ),
    );
  }
}

enum JoystickMode { all, horizontal, vertical }
```

### Explanation

- **Joystick/D-pad**: This control uses a placeholder for the actual joystick widget, which will control the `x_axis` and `y_axis` values.
- **Klaxon Button**: Toggles the horn on and off.
- **N_Button and S_Button**: Controls for counterclockwise and clockwise rotation.
- **Mode Selector (DropdownButton)**: This control lets you select between different modes. When a mode is selected, the corresponding data is sent to the robot.

You can replace the placeholder for the joystick with an actual joystick implementation compatible with Flutter, such as the [flutter_joystick](https://pub.dev/packages/flutter_joystick) package or a custom implementation.

This setup ensures all necessary controls are easily accessible and the mode selection is simplified using a dropdown menu.



To implement a game controller-like interface where the user can only select one mode at a time (similar to weapon selection), you can use a `ToggleButtons` widget. This provides a set of toggle buttons where only one can be active at a time.

### Flutter UI with ToggleButtons for Mode Selection

Here’s an example of how to implement this:

```dart
import 'package:flutter/material.dart';
import 'package:flutter_bluetooth_serial/flutter_bluetooth_serial.dart';

class BluetoothControlScreen extends StatefulWidget {
  @override
  _BluetoothControlScreenState createState() => _BluetoothControlScreenState();
}

class _BluetoothControlScreenState extends State<BluetoothControlScreen> {
  BluetoothConnection? connection;
  String x_axis = '0';
  String y_axis = '0';
  bool klaxon = false;
  bool n_button = false;
  bool s_button = false;
  List<bool> isSelected = [true, false, false]; // Default selected mode

  void connectToBluetooth() async {
    BluetoothDevice device = await selectBluetoothDevice();
    BluetoothConnection.toAddress(device.address).then((_connection) {
      connection = _connection;
      setState(() {});
    });
  }

  Future<BluetoothDevice> selectBluetoothDevice() async {
    List<BluetoothDevice> devices = await FlutterBluetoothSerial.instance.getBondedDevices();
    return devices.first; // Select the first device for simplicity
  }

  void sendData(String data) {
    if (connection != null && connection!.isConnected) {
      connection!.output.add(Uint8List.fromList(data.codeUnits));
    }
  }

  void handleModeChange(int index) {
    setState(() {
      for (int i = 0; i < isSelected.length; i++) {
        isSelected[i] = i == index;
      }
    });
    switch (index) {
      case 0:
        sendData('mode1:1');
        break;
      case 1:
        sendData('mode2:1');
        break;
      case 2:
        sendData('mode3:1');
        break;
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Bluetooth Control'),
      ),
      body: Column(
        children: [
          // Joystick or D-pad for x_axis and y_axis
          Joystick(
            mode: JoystickMode.all,
            listener: (details) {
              setState(() {
                x_axis = details.x.toString();
                y_axis = details.y.toString();
              });
              sendData('x:$x_axis,y:$y_axis');
            },
          ),
          // Klaxon Button
          ElevatedButton(
            onPressed: () {
              setState(() {
                klaxon = !klaxon;
              });
              sendData('klaxon:${klaxon ? 1 : 0}');
            },
            child: Text('Klaxon'),
          ),
          // N_Button
          ElevatedButton(
            onPressed: () {
              setState(() {
                n_button = true;
                s_button = false;
              });
              sendData('N_Button:1');
            },
            child: Text('Rotate Counterclockwise'),
          ),
          // S_Button
          ElevatedButton(
            onPressed: () {
              setState(() {
                s_button = true;
                n_button = false;
              });
              sendData('S_Button:1');
            },
            child: Text('Rotate Clockwise'),
          ),
          // Mode Selector using ToggleButtons
          ToggleButtons(
            children: <Widget>[
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: Text('Mode 1'),
              ),
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: Text('Mode 2'),
              ),
              Padding(
                padding: const EdgeInsets.all(8.0),
                child: Text('Mode 3'),
              ),
            ],
            isSelected: isSelected,
            onPressed: handleModeChange,
          ),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: connectToBluetooth,
        child: Icon(Icons.bluetooth),
      ),
    );
  }
}

class Joystick extends StatelessWidget {
  final Function listener;
  final JoystickMode mode;

  Joystick({required this.listener, required this.mode});

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 200,
      width: 200,
      child: Center(
        child: Text('Joystick Here'), // Placeholder for actual joystick widget
      ),
    );
  }
}

enum JoystickMode { all, horizontal, vertical }
```

### Explanation

- **Joystick/D-pad**: Placeholder for the joystick widget to control `x_axis` and `y_axis`.
- **Klaxon Button**: Toggles the horn on and off.
- **N_Button and S_Button**: Controls for counterclockwise and clockwise rotation.
- **Mode Selector (ToggleButtons)**: Three toggle buttons allow the user to select one mode at a time. Only one button can be active at a time, similar to weapon selection in a game.

### UI Overview
- The `ToggleButtons` widget provides a clean and simple way to allow the user to select between modes. When a button is pressed, it sends the corresponding mode data.
- You can replace the placeholder joystick implementation with a suitable joystick widget for Flutter, like `flutter_joystick`.

This setup mimics a game controller interface, providing a more intuitive control experience for the user.


You can achieve a radio button-like functionality with regular buttons by ensuring only one button can be active at a time. Here's an example using three buttons where only one can be active at any given time, similar to how radio buttons work:

### Flutter UI with Buttons Acting as Radio Buttons

```dart
import 'package:flutter/material.dart';
import 'package:flutter_bluetooth_serial/flutter_bluetooth_serial.dart';

class BluetoothControlScreen extends StatefulWidget {
  @override
  _BluetoothControlScreenState createState() => _BluetoothControlScreenState();
}

class _BluetoothControlScreenState extends State<BluetoothControlScreen> {
  BluetoothConnection? connection;
  String x_axis = '0';
  String y_axis = '0';
  bool klaxon = false;
  bool n_button = false;
  bool s_button = false;
  int selectedMode = 0; // Default selected mode index (0 for Mode 1)

  void connectToBluetooth() async {
    BluetoothDevice device = await selectBluetoothDevice();
    BluetoothConnection.toAddress(device.address).then((_connection) {
      connection = _connection;
      setState(() {});
    });
  }

  Future<BluetoothDevice> selectBluetoothDevice() async {
    List<BluetoothDevice> devices = await FlutterBluetoothSerial.instance.getBondedDevices();
    return devices.first; // Select the first device for simplicity
  }

  void sendData(String data) {
    if (connection != null && connection!.isConnected) {
      connection!.output.add(Uint8List.fromList(data.codeUnits));
    }
  }

  void handleModeChange(int modeIndex) {
    setState(() {
      selectedMode = modeIndex;
    });
    switch (modeIndex) {
      case 0:
        sendData('mode1:1');
        break;
      case 1:
        sendData('mode2:1');
        break;
      case 2:
        sendData('mode3:1');
        break;
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Bluetooth Control'),
      ),
      body: Column(
        children: [
          // Joystick or D-pad for x_axis and y_axis
          Joystick(
            mode: JoystickMode.all,
            listener: (details) {
              setState(() {
                x_axis = details.x.toString();
                y_axis = details.y.toString();
              });
              sendData('x:$x_axis,y:$y_axis');
            },
          ),
          // Klaxon Button
          ElevatedButton(
            onPressed: () {
              setState(() {
                klaxon = !klaxon;
              });
              sendData('klaxon:${klaxon ? 1 : 0}');
            },
            child: Text('Klaxon'),
          ),
          // N_Button
          ElevatedButton(
            onPressed: () {
              setState(() {
                n_button = true;
                s_button = false;
              });
              sendData('N_Button:1');
            },
            child: Text('Rotate Counterclockwise'),
          ),
          // S_Button
          ElevatedButton(
            onPressed: () {
              setState(() {
                s_button = true;
                n_button = false;
              });
              sendData('S_Button:1');
            },
            child: Text('Rotate Clockwise'),
          ),
          // Mode Selector Buttons
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: [
              ElevatedButton(
                onPressed: () => handleModeChange(0),
                style: ElevatedButton.styleFrom(
                  primary: selectedMode == 0 ? Colors.blue : Colors.grey,
                ),
                child: Text('Mode 1'),
              ),
              ElevatedButton(
                onPressed: () => handleModeChange(1),
                style: ElevatedButton.styleFrom(
                  primary: selectedMode == 1 ? Colors.blue : Colors.grey,
                ),
                child: Text('Mode 2'),
              ),
              ElevatedButton(
                onPressed: () => handleModeChange(2),
                style: ElevatedButton.styleFrom(
                  primary: selectedMode == 2 ? Colors.blue : Colors.grey,
                ),
                child: Text('Mode 3'),
              ),
            ],
          ),
        ],
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: connectToBluetooth,
        child: Icon(Icons.bluetooth),
      ),
    );
  }
}

class Joystick extends StatelessWidget {
  final Function listener;
  final JoystickMode mode;

  Joystick({required this.listener, required this.mode});

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 200,
      width: 200,
      child: Center(
        child: Text('Joystick Here'), // Placeholder for actual joystick widget
      ),
    );
  }
}

enum JoystickMode { all, horizontal, vertical }
```

### Explanation

- **Joystick/D-pad**: Placeholder for the joystick widget to control `x_axis` and `y_axis`.
- **Klaxon Button**: Toggles the horn on and off.
- **N_Button and S_Button**: Controls for counterclockwise and clockwise rotation.
- **Mode Selector Buttons**: Three buttons representing the different modes. Only one button can be active at a time, similar to radio buttons. The active button is highlighted in blue, while inactive buttons are grey.

### UI Overview
- The `ElevatedButton` widget is used to create mode selection buttons.
- The `style` property of `ElevatedButton` is used to change the button color based on the selected mode.
- When a button is pressed, the `handleModeChange` function updates the `selectedMode` state and sends the corresponding mode data.

This setup provides a clear and intuitive way for users to select modes, similar to how radio buttons work, ensuring only one mode can be active at any time.


```dart
        child: Text('Joystick Here'), // Placeholder for actual joystick widget
      ),
    );
  }
}

enum JoystickMode { all, horizontal, vertical }
```

### Explanation

- **Joystick/D-pad**: Placeholder for the joystick widget to control `x_axis` and `y_axis`.
- **Klaxon Button**: Toggles the horn on and off.
- **N_Button and S_Button**: Controls for counterclockwise and clockwise rotation.
- **Mode Selector Buttons**: Three buttons representing the different modes. Only one button can be active at a time, similar to radio buttons. The active button is highlighted in blue, while inactive buttons are grey.

### UI Overview
- The `ElevatedButton` widget is used to create mode selection buttons.
- The `style` property of `ElevatedButton` is used to change the button color based on the selected mode.
- When a button is pressed, the `handleModeChange` function updates the `selectedMode` state and sends the corresponding mode data.

This setup provides a clear and intuitive way for users to select modes, ensuring only one mode can be active at any time.


```dart