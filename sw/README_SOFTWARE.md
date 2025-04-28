# User Interface Software

## Summary
Our User Interface is implemented as one python class that uses a library called PyQt5 as its core. The library essentially allows the user to freely create a variety of UI elements called Widgets. Using this library we were able to create a system for users to have full control over the device.

### virtual_keyboard.py
virtual_keyboard.py creates a class called VirtualKeyboard. This is used in a section of the UI, specifically in the second tab for WiFi Connectivity. This is essentially done by creating a Qwidget object that organizes a grid of push buttons that contain relevant keyboard inputs for entering WiFi credentials. On pressing a button it will respond by populating the relevant textbox for entering credentials. The relevant keyboard declaration and creation is highlight below.

```python
self.keys = [
            ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0'],
            ['q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p'],
            ['a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l', '←'],
            ['↑', 'z', 'x', 'c', 'v', 'b', 'n', 'm', '␣']
        ]

        keyboard_frame = QFrame()
        keyboard_frame.setStyleSheet("background-color: #333; border-radius: 10px; padding: 10px;")
        grid_layout = QGridLayout()

        for row_idx, row in enumerate(self.keys):
            for col_idx, key in enumerate(row):
                btn = QPushButton(key.upper() if self.caps_lock else key)
                btn.setFixedSize(50, 50)
                btn.setStyleSheet(self.get_button_style(key))
                btn.clicked.connect(lambda checked, k=key: self.key_pressed(k))
                grid_layout.addWidget(btn, row_idx, col_idx)
                self.buttons[key] = btn
```

### TabularUI.py
Our TabularUI.py script is in charge of setting up our user interface. Specifically we have our code organized as three seperate tabs, each of which has its own functionality. Tab 1 is in charge of general translation functionality. Tab 2 is in charge of WiFi connectivity. Tab 3 is used for settings control. All initialization and setup is done in the class method initUI(self).

``` python
def initUI(self):
        self.tabs = QTabWidget()
        self.tabs.setStyleSheet("""QTabWidget::pane { border: 1px solid #aaa; background: #ddd; }
                                  QTabBar::tab { padding: 6px; font-size: 10px; background: #eee; color: black; border: 1px solid #aaa; }
                                  QTabBar::tab:selected { background: #ccc; }""")
        
        # Create Tabs
        self.tab1 = QWidget()
        self.tab2 = QWidget()
        self.tab3 = QWidget()
        
        # Add tabs to the QTabWidget
        self.tabs.addTab(self.tab1, "Translation")
        self.tabs.addTab(self.tab2, "Wi-Fi")
        self.tabs.addTab(self.tab3, "Settings")
        
        # Set up layouts for each tab
        self.setupTab1()
        self.setupTab2()
        self.setupTab3()
        
        self.setCentralWidget(self.tabs)
```

1. First Tab (Translation)
The first tab is comprised of a few components to enable translation functionality. It consists of a status label, video label, a text editor. The class method setupTab1() sets up the Widgets such that the status label shows the current functional mode of the device, either "SPEECH" or "ASL". The video label will show the live camera feed when in ASL mode, and the text editor will give text translations in both user modes.

2. Second Tab (WiFi Connectivity)
The second tab setups various components to allow the device to connect to internet using the class method setupTab2(). It initializes the VirtualKeyboard class and then creates various textboxes to allow the user to enter relevant SSID and passwords for the internet they are trying to connect to.

3. Third Tab (Settings)
The third and final tab setups widgets for a volume indicator, language mode buttons, voice settings dropdown. This is created using the setupTab3() method.